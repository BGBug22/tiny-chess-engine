# Tiny Chess Engine — make.fun `tiny-chess-01`

A chess engine in at most 4,096 bytes of Python (this `main.py` is 3,987
bytes), built for the make.fun bounty "Build the strongest chess engine in
4 KB of Python".

## How it works

- 0x88 board, negamax alpha-beta with iterative deepening to a **fixed node
  budget** (60,000 nodes per move), so play is deterministic: no clock, no
  unseeded randomness.
- Quiescence search (captures), MVV-LVA + killer + history move ordering,
  late-move reductions, null-move pruning, and a transposition table.
- Evaluation: material + piece-square tables.
- Python 3.13 standard library only. `main.py` unpacks a compressed copy of
  the engine (`lzma` + base85) and runs it; the payload and the unpacker are
  both inside the 4,096 bytes.
- Talks to the make.fun env over HTTP (`MAKEFUN_ENV_URL`), playing UCI
  moves, 10 seconds a move at most.

## Local test

8 games vs sunfish (500 ms/move) from the practice openings:
**3.0/8** (2 wins, 2 draws, 4 losses), about 27,000 nodes/second.

## License

MIT — see `LICENSE`.
