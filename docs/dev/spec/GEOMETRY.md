# Geometry and rotation contract

This child owns detailed R01 and R10 geometry. [SPEC](../SPEC.md) owns game lifecycle, timing and acceptance integration.

## Coordinates and shape masks

x increases rightward; y increases downward. Visible columns are 0–9 and rows 0–19. The active origin and masks use integer cells. Every transient cell must have x in 0–9 and y in −2–19. Settled storage contains visible cells only. Origin spawn is x=3, y=−1 for all types; the O mask below centers its footprint in columns 4–5.

Spawn orientation is 0. For three-cell matrices, rows are separated by `/`; dots denote empty cells. JLSTZ use 3×3 matrices: J=`J../JJJ/...`, L=`..L/LLL/...`, S=`.SS/SS./...`, Z=`ZZ./.ZZ/...`, T=`.T./TTT/...`. I uses 4×4 `..../IIII/..../....`. O uses a fixed 4×4 `.OO./.OO./..../....` footprint for every orientation. Each mask has exactly four cells.

For I rotate the 4×4 matrix; for JLSTZ rotate the 3×3 matrix. A clockwise rotation transforms local (x,y) to (N−1−y,x). Orientations are 0, R, 2, L in clockwise order. Counterclockwise reverses that order. O changes no footprint and requires no kick offsets.

Normal placement rejects any occupied visible cell. Hidden cells do not collide with settled storage. Normal downward landing repeatedly tests one row below until blocked; a pose partly above the board may be grounded but cannot legally lock. Ghost requires a fully visible landing. Penetrator transit/landing follow R11, using the same shape cells and bounds.

## Ordered kick offsets

Offsets below use the y-down convention above and add to the active origin after rotation. Try left to right; first legal candidate wins. No 180-degree command is defined. Legal includes the appropriate normal/penetrator placement rules.

| JLSTZ transition | Candidate (dx,dy) sequence |
| --- | --- |
| 0→R | (0,0), (−1,0), (−1,−1), (0,2), (−1,2) |
| R→0 | (0,0), (1,0), (1,1), (0,−2), (1,−2) |
| R→2 | (0,0), (1,0), (1,1), (0,−2), (1,−2) |
| 2→R | (0,0), (−1,0), (−1,−1), (0,2), (−1,2) |
| 2→L | (0,0), (1,0), (1,−1), (0,2), (1,2) |
| L→2 | (0,0), (−1,0), (−1,1), (0,−2), (−1,−2) |
| L→0 | (0,0), (−1,0), (−1,1), (0,−2), (−1,−2) |
| 0→L | (0,0), (1,0), (1,−1), (0,2), (1,2) |

| I transition | Candidate (dx,dy) sequence |
| --- | --- |
| 0→R | (0,0), (−2,0), (1,0), (−2,1), (1,−2) |
| R→0 | (0,0), (2,0), (−1,0), (2,−1), (−1,2) |
| R→2 | (0,0), (−1,0), (2,0), (−1,−2), (2,1) |
| 2→R | (0,0), (1,0), (−2,0), (1,2), (−2,−1) |
| 2→L | (0,0), (2,0), (−1,0), (2,−1), (−1,2) |
| L→2 | (0,0), (−2,0), (1,0), (−2,1), (1,−2) |
| L→0 | (0,0), (1,0), (−2,0), (1,2), (−2,−1) |
| 0→L | (0,0), (−1,0), (2,0), (−1,−2), (2,1) |

The baseline delivery may test only offset (0,0), as scoped by PLAN. Full-target behavior uses every listed candidate. Bounds apply to occupied shape cells, not empty matrix padding.

## Objective checks

Four clockwise or four counterclockwise rotations restore every mask. Opposite rotations without kicks restore a pose. For every transition, verify first-success ordering with fixtures in which earlier candidates fail, all-fail rejection, wall/floor contact, hidden top bound and occupied-cell obstruction. Validate I separately from JLSTZ. Validate penetration with transit overlap and with no legal final landing. O repeated rotation preserves cells and existing grounding deadline.
