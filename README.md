# Trend Watcher

Standalone agent for Bull or Bear. Finds 3 trending, healthy-debate news
topics per pass (India-focused, finance/policy leaning), scores them, and
emails a shortlist. Runs on a schedule as a Claude Code cloud routine — see
`TREND_WATCHER/README.md` for the full spec.

This repo is deliberately independent of Bull or Bear's main content
pipeline repo — no shared brand files, no shared QA gate.
