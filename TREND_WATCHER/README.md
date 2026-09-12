# The Trend Watcher

A standalone agent. It is **not** part of the Bull or Bear 7-agent content
pipeline (`00_MASTER_BRAND/` → `07_PINTEREST/`) and does not read from or
write to any of those folders, the shared `qa-gate` skill, or the Content
Master. It has its own ground rules below rather than inheriting the
brand-bible. If you ever want to merge it into the main system, that has to
be a deliberate, explicit decision — it does not happen by default.

## Jobs

**Job A — News/debate pass.** Every run, find **3 trending news /
healthy-debate topics** that are:
- genuinely early (not yet the 5th account covering it),
- actually two-sided (not just one side being obviously wrong),
- and material to the audience's money or rights.

Then move fast on the ones worth moving on: a same-day question-framed post,
and — if the story has enough shape — a fuller debate piece within 24–48h.

**Job B — Blog topics pass.** Every run, pitch **2 blog topic ideas** in
Bull or Bear's own house style (see `BLOG_TOPICS_STYLE.md`) for the user to
write themselves — this agent never drafts or publishes the post itself.
See `BLOG_TOPICS_SOURCES.md` for what to scan.

## Cadence

Runs automatically via scheduled cloud routines (Claude Code "routines",
set up with the `schedule` skill), checked out from this repo:

- **11:00 AM IST** — Job A, late-morning catch-it pass
- **2:30 PM IST** — Job B, daily blog topics pass
- **6:00 PM IST** — Job A, early-evening catch-it pass

Each pass reads this repo fresh, scans, logs its output back via a commit,
and emails the result to riyateri01@gmail.com. Manage/adjust the schedule at
https://claude.ai/code/routines.

You can still trigger any pass manually any time — ask for it by name ("run
the Trend Watcher", "give me today's blog topics") or invoke the
`trend-watcher` skill directly — for an extra pass outside the scheduled ones.

## How to run it

Ask for it by name — "run the Trend Watcher" / "Trend Watcher, morning pass" /
"give me today's blog topics" — or invoke the `trend-watcher` skill directly.

Job A each run:
1. Scans the sources in `SOURCES.md`.
2. Applies the Judge It filter (`PROCESS.md`) to shortlist to 3 topics.
3. Logs the pass to `logs/YYYY-MM-DD_AM.md` or `logs/YYYY-MM-DD_PM.md`.
4. Drafts a first-reaction post per topic (`templates/first_reaction_post_template.md`).
5. Flags which topic(s), if any, are ready for a full debate piece
   (`templates/full_debate_piece_template.md`).

Job B each run:
1. Refreshes the style read from bullorbear.in/latest and checks recent logs
   for duplicates.
2. Scans `BLOG_TOPICS_SOURCES.md`.
3. Pitches 2 topics in one of the 4 house shapes from `BLOG_TOPICS_STYLE.md`,
   each with a real, checkable anchor.
4. Logs the pass to `logs/blog_topics/YYYY-MM-DD.md`
   (`templates/blog_topics_log_template.md`), including dropped candidates.

## Files

- `PROCESS.md` — Job A's 4-stage process and the Judge It questions.
- `SOURCES.md` — what Job A scans each pass.
- `BLOG_TOPICS_STYLE.md` — Job B's house-style reference, derived from the
  live blog archive.
- `BLOG_TOPICS_SOURCES.md` — what Job B scans each pass.
- `templates/` — output templates for both jobs.
- `logs/` — Job A history, one file per run.
- `logs/blog_topics/` — Job B history, one file per run.

## Ground rules (self-contained — no dependency on other agents' files)

- Never invent a statistic, quote, screenshot, or source. If a claim can't be
  attributed to something real, it doesn't go in.
- Keep FACT, ATTRIBUTED CLAIM, INTERPRETATION, OPINION, and PREDICTION
  separate at all times. A prediction is never written as a fact.
- The first-reaction post stakes out **the question**, not an answer — don't
  pre-commit to a side before the full debate piece has been thought through.
- Nothing here auto-publishes. Output is a draft for you to review, edit, and
  post yourself.
- If a topic turns out to be one-sided, low-stakes, or already stale, say so
  and drop it rather than forcing 3 topics to hit a quota.
