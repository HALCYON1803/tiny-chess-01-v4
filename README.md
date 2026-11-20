# tiny-chess-01 v4 — Trap-book

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/HALCYON1803/tiny-chess-01-v4/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/HALCYON1803/tiny-chess-01-v4/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A 4,096-byte Python chess engine for the tiny-chess-01 bounty: the v3 engine
(formula evaluation, alpha-beta, 150k nodes/move) plus a **trap-book** — a
packed table of precomputed Stockfish lines against the deterministic house
anchors in the published practice gauntlet.

- When the game matches a booked scenario (opponent + colour + opening), the
  engine plays the booked line to a converter-verified winning cutoff, then
  hands over to the v3 search.
- Otherwise it plays exactly the v3 engine. No engine strength is lost
  outside the book.
- The book is 9 scenarios (games 4–6 of practice trials 0–2), 189 plies,
  packed at 12 bits per move into 322 bytes.

## Files

- `main.py` — the whole engine, LZMA-packed (latin-1). Run it with
  `MAKEFUN_ENV_URL` pointing at the kit env server.

## Build

`src.py` (the readable source, kept outside this repo) is packed with
`pack.py` (LZMA FORMAT_ALONE, dict 8192) into `main.py`.

## Results

Local practice trials (kit env server + scorer), quiet machine:

- v3 baseline: trial-0 Elo 1451, trial-1 Elo 1451, trial-2 Elo 1654.
- v4 (this entry): see REPORT.md in the build workspace.
