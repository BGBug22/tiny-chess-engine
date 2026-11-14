# Tiny Chess Engine — make.fun `tiny-chess-01`

A chess engine in at most 4,096 bytes of Python (this `main.py` is 3,560
bytes), built for the make.fun bounty "Build the strongest chess engine in
4 KB of Python".

## How it works

- 0x88 board, negamax alpha-beta with PVS and iterative deepening to a
  **fixed node budget** (80,000 nodes per move), so play is deterministic:
  no clock, no unseeded randomness.
- Quiescence search (captures only), MVV-LVA + killer + history move
  ordering, late-move reductions, null-move pruning, a transposition table,
  and a check extension.
- Evaluation: material + piece-square tables.
- Repetition-history scoring: a position already seen in the game scores as
  a draw, so the engine avoids repeating moves instead of shuffling.
- Python 3.13 standard library only. `main.py` unpacks a compressed copy of
  the engine (`lzma` + base85) and runs it; the payload and the unpacker are
  both inside the 4,096 bytes.
- Talks to the make.fun env over HTTP (`MAKEFUN_ENV_URL`), playing UCI
  moves, 10 seconds a move at most.

## Results

Official practice gauntlet (trial-0, 8 games against the kit's calibrated
house engines, run 2026-11-14): **Elo 1173** — above the 1,154 floor for
the final. 2 wins, 1 draw, 5 losses. All losses are checkmates in the late
games; the next work is search depth, not new evaluation terms.

Engine version: v3 step 6 — v1 plus PVS, a check extension, cached king
squares (1.47x faster, letting the node budget grow to 80,000), and
repetition-history scoring.

## License

MIT — see `LICENSE`.
