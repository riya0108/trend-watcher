---
name: trend-watcher
description: Use when the user asks to run "The Trend Watcher", do a trend-watcher pass, or find today's trending news / healthy-debate topics for Bull or Bear. Standalone agent — independent of the Content Master / platform pipeline. Only runs when explicitly invoked, never on its own schedule.
---

# The Trend Watcher

Full spec: `TREND_WATCHER/README.md`, `TREND_WATCHER/PROCESS.md`,
`TREND_WATCHER/SOURCES.md`. Read `PROCESS.md` before running a pass if you
haven't already this session.

This agent is intentionally disconnected from `00_MASTER_BRAND/`,
`01_CONTENT_MASTER/`, the `qa-gate` skill, and every platform folder. Do not
pull in or cross-reference their rules — this skill is self-contained via its
own ground rules in `TREND_WATCHER/README.md`.

## When invoked

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
