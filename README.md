# ME204 Final Project: Is Home Advantage Equal Across Europe's Top Five Leagues?

**GitHub username:** martinezmerino
**LSE ID:** 250099653

## Overview

I'm comparing home vs away performance across Europe's "big five" football leagues - Premier League,
La Liga, Bundesliga, Serie A, Ligue 1 - for the 2025-26 season, to see whether home advantage is a
consistent effect or varies by league. Using football-data.org's free API, I found that home advantage
holds broadly (most teams in every league earn more points at home than away), but its size differs
sharply: La Liga shows the strongest average gap (0.67 points per game, 95% of teams favoured at home)
while Serie A shows the weakest (0.12 points per game, 65% of teams favoured at home). The points gap
also correlates strongly (r = 0.82) with the goal-difference gap, suggesting the effect reflects real
on-pitch performance rather than a run of close, luck-driven results.

## Methodology

**Leagues:** Premier League (England), La Liga (Spain), Bundesliga (Germany), Serie A (Italy), Ligue 1
(France) - the "big five" European leagues. All five are available on football-data.org's free tier.

**Season:** 2025-26, finished matches only (`status=FINISHED`), pulled from the
[football-data.org](https://www.football-data.org/) API. football-data.org labels a season by its
starting year, so `season=2025` is the 2025-26 season.

**Home advantage measure:** for each team, points-per-game (PPG) gap = home PPG - away PPG, computed
from every finished fixture (3 points for a win, 1 for a draw, 0 for a loss). League-level figures are
the mean and the share of teams with a positive gap across that league's own teams, not a single pooled
number, so a league average can't be driven by one or two outlier teams.

**Data quality check:** FC Nantes and Toulouse FC (Ligue 1) show up with a one-match home/away
imbalance. Checking the full fixture list (not just `FINISHED` matches) shows why: their matchday-34
meeting was decided administratively (`status: AWARDED`), not played on the pitch, so it was correctly
excluded - full reasoning in NB03.

**Variables used:** `homeTeam`, `awayTeam`, `score.fullTime.home`, `score.fullTime.away`, `utcDate`,
`matchday`, `status`.

## How to reproduce

Packages needed: `requests`, `python-dotenv`, `pandas`, `plotly`, `kaleido` (for exporting charts to
PNG). Put your own football-data.org API key in `.env` in the repository root, as
`FOOTBALL_DATA_API_KEY=your_key_here` (see `.env.example`). Register for a free key at
https://www.football-data.org/client/register.

Run the notebooks in order: `NB01-Data-Collection.ipynb` → `NB02-Data-Preparation.ipynb` →
`NB03-martinezmerino-Data-Analysis.ipynb`.

## Findings

1. **Home advantage holds almost everywhere, but its size varies a lot by league.** Mean home-away PPG
   gap ranges from 0.67 (La Liga) down to 0.12 (Serie A) - see `figures/ppg_gap_by_league.png`.
2. **The effect isn't uniform even within a league.** In every league at least one team shows zero or
   negative home advantage (e.g. one Bundesliga team is at -0.53, one Premier League team at -0.58),
   so "home advantage" is a strong tendency, not a rule that applies to every team.
3. **The points gap tracks the goal-difference gap closely (r = 0.82)** - see
   `figures/ppg_gap_vs_goal_diff_gap.png` - so teams that gain more points at home are also outscoring
   their opponents by more at home, not just picking up lucky draws or narrow wins.

![Home advantage by team and league](figures/ppg_gap_by_league.png)

## Use of AI

I used an AI assistant (Claude) throughout this project - it wrote the API collection, transformation
and analysis code in NB01-NB03 based on decisions I made (which leagues and season, how to define home
advantage, which checks to run), and helped translate/structure this README, since I think in Spanish.
I reviewed and ran every cell, checked the sanity-check outputs (match counts, the Nantes/Toulouse data
quality flag) against the raw API responses myself, and chose which findings to report. The research
question, the choice of leagues, and the interpretation of what the numbers mean are my own.
