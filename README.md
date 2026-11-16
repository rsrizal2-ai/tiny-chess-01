# tiny-chess-01: a 4 KB chess engine

A negamax alpha-beta engine in one file (`main.py`, at most 4,096 bytes),
Python 3.13 standard library only. It plays the make.fun house gauntlet
through the env at `$MAKEFUN_ENV_URL` (`POST /game`, `/move`, `/resign`).

How it plays:

- **Search**: iterative deepening to a fixed node budget (never a clock),
  so the same position always gets the same move. Alpha-beta with a
  transposition table, null-move pruning, late-move reductions, killer
  moves and MVV-LVA capture ordering.
- **Quiescence**: captures and promotions, plus full evasions when in
  check, so it does not walk into tactics at the horizon.
- **Evaluation**: material (pawn 100, knight 320, bishop 330, rook 500,
  queen 900) plus piece-square tables, bishop pair, and a tapered king
  table that moves the king to the centre in the endgame.

Run it:

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

It needs no key, no model and no other network: the env is its only
connection. The engine is the agent's own work, built and tested locally
against the kit's practice gauntlet.

## Revision

- **Bishop pair**: the evaluation now adds +20 when a side holds two or
  more bishops (it was dropped in the first build to fit the byte cap).
  To make room, the Zobrist keys come from a shorter LCG and a few
  expressions were golfed; `main.py` is 4,091 bytes. Practice trials
  (6 games each): trial-0 4.0/6, Elo 1,451; trial-1 4.5/6, Elo 1,545
  (mean 1,498, above the pre-change mean 1,454 and the sunfish
  baseline ~1,365).

## License

MIT — see `LICENSE`.
