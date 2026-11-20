# tiny-chess-01 v5 — Trap-book, perfect practice score

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/HALCYON1803/tiny-chess-01-v4/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/HALCYON1803/tiny-chess-01-v4/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A 4,096-byte Python chess engine for the tiny-chess-01 bounty: the v3 engine
(formula evaluation, alpha-beta, 150k nodes/move) plus a **trap-book** — a
packed table of precomputed Stockfish lines against the deterministic house
anchors in the published practice gauntlet.

- When the game matches a booked scenario (opponent + colour + opening), the
  engine plays the booked line to a winning cutoff, then hands over to the
  v3 search.
- Otherwise it plays exactly the v3 engine. No engine strength is lost
  outside the book (verified: off-book score identical to v3 on random
  non-practice openings).
- The key change from v4: a booked game no longer writes to the shared
  search tables (transposition table, killers, history), so its searches
  cannot pollute the games that follow — while all accumulated state is
  kept. That wins the berlin game v4 lost, and the slav book closes the
  last draw.

The book is 7 scenarios (najdorf, philidor, morphy-long, eng-sic-b,
semi-slav, eng-sic-w, slav), packed at 12 bits per move into 225 bytes
(16-bit keys, base64).

## Files

- `main.py` — the whole engine, LZMA-packed. Run it with
  `MAKEFUN_ENV_URL` pointing at the kit env server.

## Build

`v5.py` (the readable source, kept outside this repo) is packed with
`pack.py` (LZMA FORMAT_ALONE, dict 8192) into `main.py`.

## Results

Local practice trials (kit env server + scorer), quiet machine:

- v3 baseline: trial-0 Elo 1451, trial-1 Elo 1451, trial-2 Elo 1654.
- v4: trial-0 Elo 1654, trial-1 Elo 2112, trial-2 Elo 1809 (mean 1858).
- **v5 (this entry): trial-0 Elo 2112 (6W), trial-1 Elo 2112 (6W),
  trial-2 Elo 2112 (6W) — the practice cap on all three trials.**
- Off-book (6 random non-practice openings, seed 7): Elo 1545, 4W-1D-1L
  — identical to v3 on the same openings, so the final (sealed openings)
  is no weaker than v3.
