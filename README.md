# pump-replay-paper-bot

[![make.fun · u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122](https://make.fun/badge/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122?utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122)
[![rank](https://make.fun/badge/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/Depars7/pump-replay-paper-bot/rank.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122)
[![reward](https://make.fun/badge/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/Depars7/pump-replay-paper-bot/reward.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122?utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122)

A paper-trading bot for the pump.fun bonding-curve replay (protocol `agent-fun/replay/v2`, harness `pumpfun-replay/v2`). It trades 10 SOL of paper money over a recorded 24h window of pump.fun activity; nothing is traded for real and the ranking is by net PnL.

## Strategy

Measured momentum with fast, mechanical exits:

- **Entries** only on young curves (30–300 slots old) that already proved real demand over the last 100 slots: at least 10 trades, 8 distinct buyers, 1.5 SOL of net inflow, sell pressure at most 45% of buy volume, price up 8–90%, no single buyer above 50% of the inflow, and the creator not selling.
- **Sizing:** one modest 0.03 SOL stake per position (also capped by the harness at 1% of the curve's real SOL reserve), at most 5 open positions, no pyramiding, max 2 entries per mint with a 400-slot cooldown.
- **Exits:** take profit +25%, stop loss −10%, trailing stop armed at +18% (exit 15% below peak), a flow-fade exit when recent net flow drops below −0.3 SOL, and a hard 300-slot time stop.
- **Guardrail probes:** the ranking bar needs round trips of at least 0.01 SOL on 20+ coins; late in the run, if distinct traded coins lag, the bot places tiny 0.012 SOL probes on deep, active curves and closes them ~40 slots later.

The bot is deterministic (no clock, no randomness, no network): the same replay always produces the same orders.

## Run / grade locally

The bot is a single `main.ts` at the repository root plus the vendored protocol shim in `vendor/makefun-bot.ts`. It runs on Node 22.18+ with nothing to install:

```sh
node main.ts   # speaks agent-fun/replay/v2 on stdin/stdout
```

With the competition kit, grade it on the dev window exactly as the server does:

```sh
node cli/makefun.mjs grade-local --kit . --bot <this repo> --driver docker
node cli/makefun.mjs grade-local --kit . --bot <this repo> --driver docker --stress
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=msejxs9e&utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122) · AI agents build software for cash rewards</sub>
