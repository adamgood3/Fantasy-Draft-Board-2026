# 2026 Fantasy Football Draft Board

A data-driven draft board and live draft assistant for a 12-team, superflex, full-PPR fantasy football league — built as a personal data science project.

## What it does

- Pulls real 2026 season-long PPR projections and cleans them with pandas
- Calculates **VORP** (Value Over Replacement Player) for every player, with replacement-level baselines tuned specifically for a superflex league
- Estimates a real **floor and ceiling** for each player's weekly output, using actual 2025 game-log variance (via `nflreadpy`) applied to the 2026 projection
- Visualizes positional scarcity — how value declines round over round, by position
- An interactive HTML draft board (works fully offline, no server needed) with:
  - Search, team filter, and position tabs
  - Per-player notes and a "target" / "avoid" flagging system
  - Live sync to a real Sleeper.com draft via their public API — auto-marks players drafted in real time
  - A **draft grade** system that scores your actual picks across four components: value vs. real market ADP, positional scarcity timing, team value, and roster balance

## Repo structure

- **/notebooks** — Python/Colab notebooks: data cleaning, VORP, floor/ceiling, backtesting
- **/data** — Raw and processed CSVs (projections, real ADP, real 2025 outcomes)
- **/draft-board** — The interactive HTML draft board
- **/docs** — Full project writeup (methodology, every decision explained) + in-season strategy notes

## Methodology, briefly

VORP is calculated by subtracting each position's replacement-level projected points (the player at QB20 / RB31 / WR31 / TE14) from every player's own projection — making players comparable across positions, not just within one. Floor/ceiling uses each player's real 2025 weekly variance (10th/90th percentile weeks, as a ratio of their season average) applied to their 2026 projected pace, with a position-average fallback for rookies with no 2025 history.

The draft strategy itself (RB early with a floor bias, pivot to WR around round 3, wait on QB until several are off the board, TE late) was backtested against real 2025 outcomes — see /docs for the full writeup, including a methodology flaw that was caught and corrected mid-project (an early "backtest" accidentally used omniscient hindsight; the corrected version uses only pre-season-knowable information and still shows the strategy beating a naive best-value approach in all 12 simulated draft slots).

## Full writeup

See /docs/project_writeup.pdf for a complete, detailed walkthrough of every step — data cleaning, VORP methodology, floor/ceiling calculation, the draft board's full feature history (including real bugs caught and fixed along the way), and the backtest.

## Tech used

Python, pandas, nflreadpy, matplotlib, vanilla HTML/CSS/JS (no framework) for the draft board, Sleeper's public API for live draft sync.
## Results

**Positional value decline by round** — confirms RB scarcity is far steeper than WR, directly informing the draft strategy:

![Round decline chart](./docs/images/round_decline_chart.png)

**A real draft grade** — the board's 4-component grading system evaluating an actual mock draft:

![Draft grade example](./docs/images/draft_grade_example.png)
