# Changelog

All notable changes to the scoring logic, thresholds, and dashboard behavior
go here. Format: date, what changed, why.

## 2026-09-01 — Initial versioned release

- Composite score formula:
  `RS×0.5 + CIR×100×0.25 + RSavgInBase×0.15 − overheadSupply%×0.3 − max(0, extension%−8)×0.5`
- Flags: Extended (>10% past pivot), Weak close (CIR<0.6), Old base (>30wk),
  Heavy overhead supply (>20%), RS jumped late (RS−RSavg>30pts), Low volume (<1x avg)
- Added 3-tier Confidence (High/Medium/Low) combining score + flag severity —
  not a raw score-only cutoff, so a high score with a structural red flag
  (old base, heavy supply, RS jumped late) is still Low.
- Added strict "Top 5 to Watch" tab: High confidence AND zero flags AND
  extension ≤8% only. Shows fewer than 5 rather than backfilling with
  compromised picks.
- Added manual "Squat" and "Failed Poke" optional column mappings — not in
  BananaPatterns' export, user adds these columns themselves. Same severity
  tier as Old base / Heavy overhead supply (hard-excludes from High
  confidence and the Top 5 Watchlist).
- Mobile-first layout: stacked cards / horizontally scrollable table,
  single-row stat cards on desktop (wrap 2-per-row on mobile), tap-friendly
  upload.
