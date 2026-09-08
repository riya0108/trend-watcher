# The Trend Watcher

A standalone agent. It is **not** part of the Bull or Bear 7-agent content
pipeline (`00_MASTER_BRAND/` → `07_PINTEREST/`) and does not read from or
write to any of those folders, the shared `qa-gate` skill, or the Content
Master. It has its own ground rules below rather than inheriting the
brand-bible. If you ever want to merge it into the main system, that has to
be a deliberate, explicit decision — it does not happen by default.

## Job

Every run, find **3 trending news / healthy-debate topics** that are:
- genuinely early (not yet the 5th account covering it),
- actually two-sided (not just one side being obviously wrong),
- and material to the audience's money or rights.

Then move fast on the ones worth moving on: a same-day question-framed post,
and — if the story has enough shape — a fuller debate piece within 24–48h.

## Cadence

Runs automatically twice a day via a scheduled cloud routine (Claude Code
"routines", set up with the `schedule` skill), checked out from this repo:

- **11:00 AM IST** — late-morning catch-it pass
- **6:00 PM IST** — early-evening catch-it pass

Each pass reads this repo fresh (this README, `PROCESS.md`, `SOURCES.md`),
scans, scores, logs its output back to `logs/` via a commit, and emails the
shortlist to riyateri01@gmail.com. Manage/adjust the schedule at
https://claude.ai/code/routines.

You can still trigger a pass manually any time — ask for it by name ("run
the Trend Watcher") or invoke the `trend-watcher` skill directly — for an
extra pass outside the two scheduled ones.

## How to run it

Ask for it by name — "run the Trend Watcher" / "Trend Watcher, morning pass" —
or invoke the `trend-watcher` skill directly. Each run:

1. Scans the sources in `SOURCES.md`.
2. Applies the Judge It filter (`PROCESS.md`) to shortlist to 3 topics.
3. Logs the pass to `logs/YYYY-MM-DD_AM.md` or `logs/YYYY-MM-DD_PM.md`.
4. Drafts a first-reaction post per topic (`templates/first_reaction_post_template.md`).
5. Flags which topic(s), if any, are ready for a full debate piece
   (`templates/full_debate_piece_template.md`).

## Files

- `PROCESS.md` — the 4-stage process and the Judge It questions.
- `SOURCES.md` — what to scan each pass. Fill in your actual accounts/lists here.
- `templates/` — the three output templates.
- `logs/` — one file per run, dated, kept as a running history so the same
  story doesn't get "caught" twice and so you can see how fast it moved.

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
