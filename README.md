# Banana Breakouts Dashboard

A single-file, mobile-friendly dashboard for scoring and ranking Banana Patterns
"Fresh Breakouts" exports. Upload a CSV/XLSX export, get a composite score,
confidence tier, flags, and a strict "Top 5 to Watch" shortlist — all client-side,
no backend, no data leaves your browser.

**Live dashboard:** enable GitHub Pages for this repo (Settings → Pages → source:
`main` branch, `/docs` folder) and it'll be hosted at
`https://<your-username>.github.io/banana-breakouts/`

## Structure

```
docs/index.html     — the dashboard itself (GitHub Pages serves this)
data/                — daily exports (optional — see note below)
CHANGELOG.md         — history of scoring formula / threshold changes
```

## Scoring formula (current)

```
score = RS_rating × 0.5
      + close_in_range × 100 × 0.25
      + RS_avg_in_base × 0.15
      − overhead_supply% × 0.3
      − max(0, extension% − 8) × 0.5
```

## Flags

- **Extended** — extension past pivot > 10%
- **Weak close** — close-in-range < 0.6
- **Old base** — base age > 30 weeks
- **Heavy overhead supply** — overhead supply > 20%
- **RS jumped late** — RS rating − RS avg in base > 30pts
- **Low volume** — breakout volume < 1x average
- **Squat / Failed poke** — manual flags (optional columns you add to your export yourself; same severity as Old base / Heavy overhead supply)

## Confidence tiers

- **High** — score > 75 AND flags empty or only "Extended"
- **Medium** — score 60–75, OR exactly one of {Weak close, Low volume}
- **Low** — score < 60, OR 2+ flags, OR any of {Old base, Heavy supply, RS jumped late, Squat, Failed poke}

Confidence is a rules-based heuristic, not a backtested win-rate.

## Top 5 to Watch (strict)

Only includes stocks that are **all** of: High confidence, zero flags,
extension ≤ 8%. Shows fewer than 5 if fewer qualify — never backfills with
compromised picks.

## On `/data/`

This folder is for daily export snapshots if you want a historical record
(useful later for backtesting scoring changes against real data). It's
currently empty and untracked-by-default — see `.gitignore`. Remove the
`data/*.csv` / `data/*.xlsx` ignore rules if you'd rather version them.

## Changing the scoring logic

Edit the formula/thresholds directly in `docs/index.html` (search for
`computeAndRender` and `computeConfidence`), then log the change and reasoning
in `CHANGELOG.md` before committing.
