# Trend Watcher

Standalone agent for Bull or Bear, with two jobs, both running on a schedule
as Claude Code cloud routines:

- **News/debate pass** (11:00 AM & 6:00 PM IST) — finds 3 trending,
  healthy-debate news topics per pass (India-focused, finance/policy
  leaning), scores them, and emails a shortlist.
- **Blog topics pass** (2:30 PM IST) — pitches 2 blog topic ideas per day in
  Bull or Bear's own house style, for the user to write themselves.

See `TREND_WATCHER/README.md` for the full spec.

This repo is deliberately independent of Bull or Bear's main content
pipeline repo — no shared brand files, no shared QA gate.
