# tiny-chess

A strong chess engine in 4 KB of Python, built by an AI agent for make.fun's tiny-chess-01 bounty.

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/litao54089558/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/litao54089558/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01)

## Engine

`main.py` is a packed (LZMA) 0x88-board engine: alpha-beta with iterative
deepening, a transposition table, MVV-LVA + killer move ordering, null-move
pruning, late-move reductions and quiescence search. The readable source is
`engine.py` (same program, unpacked); `main.py` is produced from it with the
kit's `makefun compress` command.

- Board: 0x88, incremental material + piece-square evaluation (kings blended
  middlegame/endgame), bishop pair via the piece-square tables.
- Search: iterative deepening to a fixed node budget (NB=55000) per iteration,
  TT with mate-score adjustment, killers, NMP (R=2) and LMR for late quiet
  moves. Fixed node budget, no wall clock in the search, deterministic.
- Protocol: talks to the make.fun chess env over HTTP only; no model calls.

## Practice results (6-game trials, NB=55000 unless noted)

- trial-0 (source): 4.5/6, Elo 1545 — W vs depth-2/3/4 and nodes-100k,
  D vs nodes-250k, L vs nodes-500k.
- trial-0 (source, NB=70000): 4.5/6, Elo 1545 (same score; NB=55000 kept
  for wall-time safety in the 8-game final).
- trial-1 (source): 3.0/6, Elo 1279 — W vs depth-2/3/4, L vs nodes-100k/250k/500k.
- trial-2 (packed `main.py`): 3.0/6, Elo 1279 — W vs depth-2/4 and nodes-100k,
  L vs depth-3 and nodes-250k/500k.
- Mean across the three instances at NB=55000: Elo ~1368. The house baseline
  (sunfish 4K) averages ~1325-1365 on the same practice trials, so this entry
  is at rough parity with the baseline; the prize depends on the scored
  instances.

Per-move time: mean ~2.8s, max ~7.3s (limit 10s). Trial wall: ~785-921s
for 6 games (limit 1800s); the 8-game final projects to ~1250-1350s.

## Files

- `main.py` — the packed entrypoint (3114 bytes, <= 4096)
- `engine.py` — the readable source `main.py` was packed from (not committed)
- `LICENSE` — MIT
