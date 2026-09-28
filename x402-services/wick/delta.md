# Delta

**Delta is Wick's house metric.** It replaces the volume bars on the chart. On every pool Wick monitors, Delta transposes the incremental reads of pool state onto the pane below the price.

Delta is derived from successive USDC-pool snapshots. That is all you need to read it — the exact expression lives in the product spec, not the docs.

## Why not volume?

CEX volume is matched prints on an order book. The exchange hides its inventory. A volume bar tells you *that someone traded* — not *what happened to the book*.

On a decentralized pool, that distinction collapses. A swap **is** the inventory change. Price and liquidity move in the same event. A volume bar on an AMM chart is a borrowed CEX glyph — it describes a mechanism that does not exist here.

## Why USDC-quoted pairs make this work

Every pool Wick monitors is quoted in USDC. That matters more than it sounds:

- A change in the USDC reserve and a change in price are **both visible in the same unit**.
- Delta records both in that unit. You are not translating between two quote assets to see what actually moved.

That is the entire point of the single-stable catalog — not a nice-to-have, the reason the metric works.

## How to read the pane

- **Up / down color lock** — Wick green / grey for direction.
- **Cyan / amber accents** on the site for emphasis.
- **Most Momentum** is a Delta board — it ranks by *change in the pool*, not by *traded volume*.

## What Delta is not

- Not order-flow from a CEX.
- Not "buy volume vs sell volume" in the tape sense.
- Not a promise of future returns.

## One comparison

| | CEX tape | Wick on Arc LPs |
|---|---|---|
| Native event | matched order | swap against a USDC pool |
| What volume shows | prints | a borrowed glyph |
| What Delta shows | — | change in the USDC-quoted pool (price and liquidity, same unit) |