---
name: trend-watcher
description: Use when the user asks to run "The Trend Watcher", do a trend-watcher pass, find today's trending news / healthy-debate topics, or get blog topic ideas for Bull or Bear. Standalone agent — independent of the Content Master / platform pipeline. Has two jobs, both also running on their own cloud-routine schedule (see README.md) in addition to on-demand invocation.
---

# The Trend Watcher

Full spec: `TREND_WATCHER/README.md`, `TREND_WATCHER/PROCESS.md`,
`TREND_WATCHER/SOURCES.md`. Read `PROCESS.md` before running a news pass, or
`BLOG_TOPICS_STYLE.md` + `BLOG_TOPICS_SOURCES.md` before running a blog-topics
pass, if you haven't already this session.

This agent is intentionally disconnected from `00_MASTER_BRAND/`,
`01_CONTENT_MASTER/`, the `qa-gate` skill, and every platform folder. Do not
pull in or cross-reference their rules — this skill is self-contained via its
own ground rules in `TREND_WATCHER/README.md`.

Two distinct jobs live here — check which one is being asked for:

- **Job A — News/debate pass** (AM 11:00 IST, PM 18:00 IST): §"When invoked
  (news pass)" below.
- **Job B — Blog topics pass** (14:30 IST daily): §"When invoked (blog
  topics pass)" below.

## When invoked (news pass)

1. **Determine the pass.** Ask which slot this is (late-morning / early-evening
   IST) if not stated, purely for labeling the log file — don't block on it if
   the user just says "run it."

2. **Catch it.** Check `TREND_WATCHER/SOURCES.md`. If it has real
   accounts/queries filled in, use those. If it's still blank, say so and fall
   back to general web search for "India trending news today" + "India
   markets/policy news today" — load `WebSearch` (and `WebFetch` if a
   specific article needs checking) via `ToolSearch` first if not already
   available. Look for stories that are financial, policy, or rights-related
   and genuinely contested.

3. **Judge it.** For each candidate, first try the reframe (`PROCESS.md`
   §Stage 2A — flat fact → concrete-stakes hook), then score the reframed
   version on the 5 signals (🔥 Trend, 😮 Surprise, 💰 Impact, ⚔️ Conflict,
   🎥 Visual). Only 4/5 or 5/5 qualifies. Separately check timing (still
   early vs. already saturated — §Stage 2C) to route each qualifier to
   Stage 3+4 or Stage 4-only. Narrow to the 3 strongest qualifiers. If fewer
   than 3 survive, say so — do not pad with weak topics to hit the number.

4. **Log the pass.** Write `TREND_WATCHER/logs/{{YYYY-MM-DD}}_{{AM|PM}}.md`
   using `templates/daily_scan_log_template.md`. Check prior logs from the
   last 1–2 days first so a story already caught and actioned isn't
   re-presented as new — carry it forward instead if it's still inside its
   deadline window.

5. **Draft first reactions.** For each of the 3 topics, fill
   `templates/first_reaction_post_template.md` — a question-framed X/Threads
   post, not an answer. Compute the actual post-by deadline from the story's
   break time (best estimate), not from scan time.

6. **Flag full debate candidates.** For any topic that already has enough
   shape (multiple confirmed facts, named positions on both sides), draft
   `templates/full_debate_piece_template.md`. If a topic is too thin still,
   say so rather than forcing the truth-layer table with guesses.

7. **Ground rules apply throughout** (from `TREND_WATCHER/README.md`): never
   invent a stat/quote/source; keep FACT vs ATTRIBUTED CLAIM vs
   INTERPRETATION vs OPINION vs PREDICTION vs UNKNOWN separate; nothing
   auto-publishes; the user edits and posts everything themselves.

8. **Output to the user:** the 3 topics with their Judge-It verdicts, the
   first-reaction drafts, and any full-debate drafts — in chat, plus
   confirmation the log file was saved.

## When invoked (blog topics pass)

Job: give exactly **2 blog topic pitches**, in Bull or Bear's own house
style, that the user could actually go write. Never pad to 2 with a weak
idea — 1 strong pitch beats 2 mediocre ones, but say so explicitly if you're
short.

1. **Refresh the style read.** Pull the first 1-2 pages of
   https://www.bullorbear.in/latest (WebFetch may 403 — use a browser tool
   or WebSearch `site:bullorbear.in` as fallback) to confirm
   `BLOG_TOPICS_STYLE.md` still matches current output and to build the
   duplicate-check list. Also check the last ~2 weeks of
   `logs/blog_topics/` so yesterday's pitches aren't repeated verbatim.

2. **Scan for angles**, per `BLOG_TOPICS_SOURCES.md`: the blog's own
   category pages, the X accounts in `SOURCES.md` (Raj Shamani / Vaibhav
   Sisinty framing lenses included), and the Staying Ahead "Daily AI
   Updates" community for a global-AI story worth reframing.

3. **Pitch it in one of the 4 house shapes** from `BLOG_TOPICS_STYLE.md`
   (everyday-friction "why", universal-behavior "why we", dated India money
   explainer, or news/scandal explainer) — reframe raw facts around
   concrete stakes exactly like Stage 2A in `PROCESS.md`, never invent the
   underlying fact.

4. **Every pitch needs a real, checkable anchor** — a study, price move,
   report, or named event. If you can't find one, drop the idea and say why
   rather than shipping an ungrounded pitch.

5. **Log the pass** to `TREND_WATCHER/logs/blog_topics/{{YYYY-MM-DD}}.md`
   using `templates/blog_topics_log_template.md` — include the 2 pitches
   *and* the "also considered / dropped this pass" list. The user has
   explicitly said they want the dropped-candidates list kept, same as the
   news pass.

6. **Output to the user:** the 2 topics (title, category, hook, why-now,
   real anchor) in chat, plus the dropped list, plus confirmation the log
   was saved.

7. When run as the scheduled 14:30 IST cloud routine: also commit + push
   the log to this repo's default branch, and email the 2 pitches (+
   dropped list) to riyateri01@gmail.com via the Gmail connector — same
   delivery pattern as the AM/PM news passes. Never skip the email, even if
   only 1 topic qualifies.
