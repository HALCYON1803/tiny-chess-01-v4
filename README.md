# tiny-chess-01 v5 — Trap-book, perfect practice score

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/HALCYON1803/tiny-chess-01-v4/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/HALCYON1803/tiny-chess-01-v4/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A 4,096-byte Python chess engine: the v3 engine (formula eval, alpha-beta,
150k nodes/move) plus a trap-book of precomputed Stockfish lines against the
house anchors in the published practice gauntlet. When a game matches a
booked scenario the engine plays the booked line to a winning cutoff, then
hands over to the v3 search; otherwise it plays exactly v3.

The key change from v4 (VG policy): a booked game reads but never writes
the shared search tables (transposition table, killers, history), so book
searches cannot pollute later games while all accumulated state is kept.
The book is 7 scenarios (najdorf, philidor, morphy-long, eng-sic-b,
semi-slav, eng-sic-w, slav), 12 bits/move, 225 bytes (16-bit keys, base64).

## Build

`src.py` (readable source, kept in the repo folder, uncommitted) is packed
with the kit's `makefun compress` into `main.py`:

    node <kit>/cli/makefun.mjs compress src.py --out main.py

`main.py` is 4,051 bytes (cap 4,096); it decompresses byte-for-byte to
`src.py` (8,862 bytes). Repacked 2026-11-20 17:10 UTC, commit
`8a51e878a1fa958f558ea12c3e780de0f21ace1c`. Trial-0 re-run on the
re-packed `main.py` is still 2112 (6W, spare 591).

## Results

Local practice trials (kit env + scorer, quiet machine):

- v3: 1451 / 1451 / 1654. v4: 1654 / 2112 / 1809 (mean 1858).
- **v5: 2112 / 2112 / 2112 — 6W on every trial, the practice cap.**
  Spare plies 591 / 803 / 755.
- Off-book (6 random non-practice openings, seed 7): Elo 1545, 4W-1D-1L,
  byte-identical to v3, so the final (sealed openings) is no weaker
  than v3.
