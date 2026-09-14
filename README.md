# LPL EDGE v1 dataset

A 0–100 composite measure of franchise strength in the Lanka Premier
League, same methodological family as MLC / PSL / BPL EDGE — six
independently interpretable components, season-adjusted, frozen
calibration for out-of-sample scoring.

Built from the uploaded Cricsheet JSON archive.

![LPL EDGE heatmap](visuals/lpl_edge_heatmap.png)

## Calibration check: clean this time

Verified the 0–100 transform the same way as the BPL audit — reconstructed
`EDGE` from `EDGE_z` using `(rank − 0.5)/N_hist × 100` for pre-2026 rows
and `percentileofscore` against that frozen distribution for 2026. Both
matched the stored values exactly. LPL's final season is labeled `2026`
cleanly (not a split-year label like BPL's `2025/26`), and the calibration
correctly held it out. No correction needed here.

## Coverage — heatmap again, and why

6 seasons (2020/21–2026, no 2025 edition in this data), 30 team-season
rows, **19 distinct team names**. Franchise turnover here sits between
MLC/PSL's stability and BPL's near-total churn: Jaffna Kings (5 seasons)
and Galle Gladiators (3) show real continuity, but 11 of the 19 names
appear in exactly one season. Same reasoning as BPL: a line-trajectory
chart would imply continuity the data doesn't support for most of these
franchises, so this is a heatmap — one row per team name (ordered by
seasons played), blank cells are genuine non-participation, not zero
scores.

6 matches excluded entirely from EDGE calculations (ties and no-results):

| Season | Match | Result |
|---|---|---|
| 2020/21 | Kandy Tuskers vs Colombo Kings | tie |
| 2020/21 | Dambulla Viiking vs Jaffna Stallions | no result |
| 2021/22 | Dambulla Giants vs Galle Gladiators | no result |
| 2023 | Galle Titans vs Dambulla Aura | tie |
| 2024 | Galle Marvels vs Dambulla Sixers | tie |
| 2026 | Dambulla Sixers vs Jaffna Kings | no result |

Full list with match IDs: `excluded_matches.csv`.

## Method

Identical to PSL EDGE v1 and BPL EDGE v1 (same six components, shrinkage
constants, z-scoring and sign-inversion rules) — see PSL's README for the
full formula writeup. Held-out calibration season: `2026`.

## Files

- `team_match_edge.csv` — audit-level team-match inputs and derived rates.
- `team_season_edge_v1.csv` — final component scores and EDGE v1 (calibration verified, unchanged from upload).
- `excluded_matches.csv` — matches excluded because no winner/outcome was recorded.
- `visuals/lpl_edge_heatmap.png` — franchise-season EDGE heatmap.

## What this package does not do

Same non-goals as the rest of the EDGE family: no recency weighting, no
opponent adjustment or in-season rolling updates, no weights fit to
observed wins, no roster/player-availability effects, and — as with BPL —
no attempt to link rebranded or renamed franchises into a single lineage.
Each team name is scored as its own entity.
