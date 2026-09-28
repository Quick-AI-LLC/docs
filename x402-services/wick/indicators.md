# Indicators

Wick's v1 indicators are the **same mathematical engine** as [Quick Signal TA](../quick-signal-ta.md) — the technical-analysis API at signal.quickai.build.

That engine has been callable by agents on Base via x402 since May 2026. Usage is real, small, and older than Wick. On Signal the output is JSON for an agent; on Wick it is human-readable and editable — colors, overlays, and panes through the on-site presentation controls. The same architecture also sits in the public [AI Technical Readout](https://github.com/Quick-AI-LLC/AI-Technical-Readout) repo.

## The v1 menu

The control header reads: `ADD INDICATOR (MAX 2 · ONE PANE)`.

**Overlays** (drawn on the price pane):

- SMA 20 · SMA 50 · SMA 200
- EMA 20 · EMA 50 · EMA 200
- Bollinger Bands

**Panes** (drawn on their own pane below):

- RSI (14) · MACD (12, 26, 9) · Stochastic (14, 3, 3) · ATR (14)

## What a Signal user should know

Signal has three indicators Wick v1 does **not** mount: **VWMA (20), Fractals, and Volume.**

Volume is not missing — **Delta is the volume pane.** On Wick, Delta occupies the pane a Signal user expects volume in. See [Delta](delta.md) for why.

## Rules

- **Max two indicators**, and only **one of them may be a pane**. Two RSIs (or RSI + MACD) is a conflict.
- Intended pair: **overlay + pane** (e.g. EMA 20 + RSI).

## Derivation

Indicators are computed from the Wick series — not drawn by hand, not a visual theme. Same math as Signal; different input (Arc pool snapshots vs Signal's CEX fallback chain). For the full endpoint and schema, follow the [Quick Signal TA](../quick-signal-ta.md) page.