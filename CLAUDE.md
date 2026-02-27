# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A live dashboard for the **Ripple Institute** — an EdTech venture launching an IECMHC certification program. Tracks progress toward two milestones:
- **100 LOIs by end of March 2026** — spin-off decision point
- **250 LOIs by June 2026** — founding cohort launch

## Architecture

Zero build step. Two files, served as a static site:

- `index.html` — entire app (HTML + embedded CSS + JS), dark theme with glassmorphism
- `data.json` — team-editable data file driving all metrics and calculations

Business context is in `ripple.md`.

## Running Locally

```bash
python3 -m http.server 8000
# Open http://localhost:8000
```

Must be served via HTTP (not `file://`) because the JS fetches `data.json`.

## How Data Updates Work

Edit `data.json` and refresh the page. Key fields:
- `actuals.lois.b2c` / `actuals.lois.b2b` — LOI counts by type
- `actuals.pipeline` — funnel stages (leads, qualified, inConversation)
- `actuals.weeklyLOIsHistory` — append one entry per week for pace sparkline
- `lastUpdated` — shown in footer

## Dashboard Metrics

1. **Dual Countdown** (header) — days to March deadline + June launch
2. **LOIs Signed** (hero) — SVG progress ring, auto-targets 100 before March, 250 after
3. **Pipeline** — funnel bars (leads → qualified → in-conversation)
4. **Revenue Committed** — `(b2c × $2,500) + (b2b × $300)` vs Y1 $483K target
5. **Weekly Pace** — sparkline + current vs required LOIs/week, color-coded (green/amber/red)

## Key Calculations

- **Active target**: March target (100) if before March deadline, else June target (250)
- **Pace**: Average of last 3 weeks of `weeklyLOIsHistory` vs `remaining / weeksLeft`
- **Pace color**: green (ratio ≥ 1.0), amber (≥ 0.8), red (< 0.8)
- **Revenue**: `b2c × 2500 + b2b × 300`, percentage against `targets.y1Revenue`
