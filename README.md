# Tiny Chess Engine — make.fun `tiny-chess-01`

A chess engine in at most 4,096 bytes of Python (this `main.py` is 3,570
bytes, sha256 `734d2f3717f9046c9aa1e670d691085d18be72a4c6c774eb8f340da3462a51cc`),
built for the make.fun bounty "Build the strongest chess engine in
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
- Null-move pruning without an eval margin: the null-move cutoff no longer
  requires the static evaluation to be 50 points above beta (Bagus's idea).
- Python 3.13 standard library only. `main.py` unpacks a compressed copy of
  the engine (`lzma` + base85) and runs it; the payload and the unpacker are
  both inside the 4,096 bytes.
- Talks to the make.fun env over HTTP (`MAKEFUN_ENV_URL`), playing UCI
  moves, 10 seconds a move at most.

## Results

Official practice gauntlet (trial-0, 8 games against the kit's calibrated
house engines, run 2026-11-15): **Elo 1253** — above the 1,154 floor for
the final. 2 wins, 2 draws, 4 losses. That is +80 over step 6 (Elo 1173
on the same gauntlet); the only change from step 6 is dropping the
eval-margin guard on null-move pruning.

Engine version: v3 step 11 — v1 plus PVS, a check extension, cached king
squares (1.47x faster, letting the node budget grow to 80,000),
repetition-history scoring, and null-move pruning without an eval margin.

## License

MIT — see `LICENSE`.
