# Wick Gitbook outline — for Hermes review

**Repo:** `Quick-AI-LLC/docs` (public). Gitbook source of record is `x402-services/` (`SUMMARY.md` is the sidebar).
**Existing:** `x402-services/wick/overview.md` is a stub. `Quick Signal TA` already documents the indicator engine Wick reuses.
**This file:** product outline only. Not live Gitbook pages. Do not add the planned child pages to `SUMMARY.md` until the copy exists — dead sidebar links break the book.
**Audience split (same as the product roadmap):** end users first (traders / whales / holders). Teams second. Agents / desks third.
**Date:** 26 Sep 2026. Nick source notes + Grok structure. Hermes custodian.

---

## 0. What the site does not say (the job of this book)

wick.green shows charts, cards, listings, and data leaves. It does not explain:

1. What **Delta** is, why it replaces volume, why it is stronger in a USDC-quoted LP world than on a CEX tape.
2. The two-step process: **catalog** (Wick's data, always ours) → **integrate / List on Wick** (one-time fee, interface presence).
3. That catalog data stays callable after an interface delist.
4. How delist is a *quality filter for users*, not a punishment or a data wipe — and that a listing fee is not a lifetime hostage.
5. That token-card fields are a **point-in-time capture**, corrected via Discord `#requests`.
6. That v1 indicators are the Signal x402 engine, human-readable.

If a page does not close one of those gaps, it does not belong in v1 of this book.

---

## 1. Proposed sidebar (`SUMMARY.md` Wick block, after review)

Keep the current one-liner until pages land. Target tree:

```
## Wick
- [Overview](wick/overview.md)
- [Delta](wick/delta.md)
- [Catalog and listing](wick/catalog-and-listing.md)
- [Delisting and data retention](wick/delisting.md)
- [Token cards](wick/token-cards.md)
- [Indicators](wick/indicators.md)
- [Using the chart](wick/using-the-chart.md)
- [Data access](wick/data-access.md)
- [FAQ](wick/faq.md)
```

Optional later (not v1 of the book): roadmap image page, API reference once the Q1 warehouse door exists, glossary if FAQ outgrows itself.

Voice: same register as `quick-signal-ta.md` — short, declarative, tables where numbers live, no lab-handoff tone. Internal strike counts and box IPs stay out of Gitbook.

---

## 2. Page-by-page outline

### 2.1 `wick/overview.md` — replace the stub

**Job:** one screen that orients a holder and a team lead. Link out; do not teach Delta here.

Draft beats:

- One-liner: Wick is the Arc-native charting terminal and the indexer of record for founding USDC pools on Circle's Arc L1 (chain 5042). Live: wick.green.
- Who it is for: people who chart Arc assets. Not a catch-all multi-chain scanner.
- What you get on the site vs what you get in this book (site = the workbench; docs = the rules).
- Two statuses, said once:
  - **Cataloged** — Wick captured the pool. The series is Wick's. Callable.
  - **Listed / Integrated** — the team paid the one-time fee. The asset is first-class on the interface (search, cards, rails, share).
- North star, one sentence: holders should prefer Wick to a busy catch-all terminal; listing follows that preference.
- Links: Delta, Catalog and listing, Using the chart, Discord.

Keep it under ~400 words. The stub's "coming soon" line dies.

---

### 2.2 `wick/delta.md` — the page the site cannot do

**Job:** make a DexScreener-native trader understand why the bottom pane is not volume.

Draft beats:

- **What it is.** Delta is Wick's house metric. It replaces the volume bars on the chart. Incremental reads of pool state are transposed onto the pane. (Do not dump the formula on this page. "Derived from successive USDC-pool snapshots" is enough for v1. Link a later precision note if Hermes wants the exact expression from `ARC-Charting-v0`.)
- **Why not volume.** CEX volume is matched prints on an order book. The exchange hides inventory. A volume bar says *that someone traded*, not *what happened to the book*.
- **Why LP is different.** On a decentralized pool, a swap *is* the inventory change. Price and liquidity move in the same event. A volume bar on an AMM chart is a borrowed CEX glyph.
- **Why USDC-quoted pairs make this work.** Every pool Wick monitors is token/USDC. A change in the USDC reserve and a change in price are both visible in the same unit. Delta records both. That is the point of a single-stable catalog — not a nice-to-have.
- **How to read the pane.** Up/down color lock (Wick green `#37DB52` / grey). Cyan / amber accents already on the site. Most Momentum is a Δ board, not a volume leaderboard. (Honesty caveat stays internal until PR #3 is live — Gitbook should not advertise a board that can still lie.)
- **What Delta is not.** Not order-flow from a CEX. Not "buy volume vs sell volume" in the tape sense. Not a promise of future returns.

Tone: teach, do not evangelize. One comparison table is enough:

| | CEX tape | Wick on Arc LPs |
|---|---|---|
| Native event | matched order | swap against a USDC pool |
| What volume shows | prints | a borrowed glyph |
| What Delta shows | — | change in the USDC-quoted pool (price and liquidity, same unit) |

---

### 2.3 `wick/catalog-and-listing.md` — process + price

**Job:** teams stop confusing "we're on the indexer" with "we're Listed on Wick." Holders understand why some assets are quieter.

Draft beats:

- **Step 1 — Catalog.** Wick indexes the pool. The series (15-minute snapshots, portable JSONL) is Wick's, from first capture. Catalog is not an endorsement and not a listing. Token-card fields are captured at this moment (see Token cards).
- **Step 2 — Integrate / List on Wick.** One-time fee. Bound receiving address → inbound USDC → verify → flip to Integrated. Interface presence: chart is first-class, cards, rails, share path.
- **Catalog data does not depend on the fee.** Once cataloged, the series stays available via x402 and other call methods until the LP is actually dead and the file is moved to the rugged / no-activity store (see Delisting). Paying does not buy the history. The history is already Wick's.
- **Pricing, locked 26 Sep 2026** (corrects the older "double by 2028" line in the lab master doc):

  | Window | One-time List on Wick |
  |---|---|
  | Through 31 Dec 2026 | $750 USDC |
  | Calendar 2027 | $1,500 USDC |
  | Calendar 2028 | $2,250 USDC |
  | Calendar 2029 | $3,000 USDC |
  | Each year after | + $750 |

  Rationale, one sentence for teams: a doubling schedule prices organic teams out by year 4. A linear step keeps year 4 under a panic number and year 5 still in reach.
- **What the fee is not.** Not a subscription. Not a data-retention fee. Not a promise the asset stays on the *interface* if the pool dies (quality filter, next page).
- **How to start (v1, concierge).** Point at Discord `#requests` and wick.green/Listings how-to once that leaf copy is honest. Self-serve checkout is a 2027 item — do not document a flow that does not exist.
- **Genesis / founding set.** Early Arc pools were cataloged because Wick is the indexer of record, not because those teams paid. Paid listing is the path for everyone after.

Hermes flag: master doc still says `$750 EOY'26 → $3k 2028`. This outline is the correction. Align the master doc when this lands.

---

### 2.4 `wick/delisting.md` — assuage the team, protect the user

**Job:** a team lead who thinks "$750–$1,500 is a hostage" leaves the page less scared. A holder understands why a dead ticker vanished from the rail.

Two different actions. Never use one word for both.

**A. Interface delist** (quality filter)

- Purpose: Wick users do not have dead or rugged assets thrust in front of them forever.
- Applies to **Listed / Integrated** assets.
- Public wording: the listing remains while the pool is alive. If activity collapses and stays collapsed across several weeks, the asset leaves the interface.
- Internal rule (do **not** print the numbers unless Nick/Hermes explicitly want transparency): `<1% Delta` for **4 consecutive weeks**, sampled **once per week at a random time that day**.
- If a Listed token rugs: evidence stays on the interface for about a month, then the asset is interface-delisted and becomes **data-retrieval only**.
- Reassurance for teams: this is not "we took your money and we can yeet you on a mood." A living pool stays. A dead pool does not occupy the rail. The fee bought interface presence for a living market.

**B. Catalog purge / archive** (longer fuse)

- Purpose: the warehouse does not pretend a corpse is a series.
- Applies to **cataloged** assets, listed or not.
- Public wording: data remains callable until the LP is gone — no meaningful activity, pool effectively dead — and is then moved to a rugged / no-activity store. Retrieval still exists. It is no longer a live catalog row.
- Longer parameters than interface delist. Do not publish the purge constants in v1 unless ops wants them load-bearing.

**C. What holders still get after an interface delist**

- History remains callable (x402 / other methods).
- The rug, if there was one, is not memory-holed. It just stops being a default rail item.

Suggested callout box for Gitbook:

> Listing is a quality surface, not a data lease. Pay once. Stay on the interface while the market is real. If the pool dies, the chart leaves the rail and the series stays in the warehouse.

---

### 2.5 `wick/token-cards.md`

**Job:** stop "Wick has the wrong Twitter" from becoming a support fire — and stop Nick from becoming a link janitor.

Draft beats:

- Cards are filled at **catalog time** from the team-facing sheet (`Wick-Token-Card-Information.xlsx` is the internal master; do not link the xlsx in Gitbook).
- Accurate *at capture*. Teams change X handles and sites. Wick does not scrape those on a timer.
- Supply is one of two honest states: a fixed-cap number, or the word `mintable` for wrapped / on-demand assets. No infinity glyph.
- Corrections: join Discord https://discord.gg/Xyxs4rt8W5 and post in `#requests`. Holder or team. That is the ticket.
- What Wick will not do from `#requests`: rewrite history, invent a website, or take a social that does not belong to the CA.

---

### 2.6 `wick/indicators.md`

**Job:** the "this is cool" page. Connect Wick to Signal without making Wick look like a reskin of a Base API.

Draft beats:

- v1 indicators on the canvas are the **same mathematical engine** as [Quick Signal TA](../quick-signal-ta.md) (`signal.quickai.build`).
- That engine has been callable by agents on Base via x402 since May 2026. Usage is real, small, and older than Wick.
- On Wick the output is human-readable and editable (colors, overlays vs panes — site already has the presentation registry). On Signal it is JSON for an agent.
- Same architecture also sits in the public [AI Technical Readout](https://github.com/Quick-AI-LLC/AI-Technical-Readout) repo.
- List the v1 set that actually ships on the canvas (do not paste Signal's ten if the chart only mounts a subset). Hermes + ZC confirm the mounted set before this page goes live. Signal's ten, for reference: Bollinger, SMA 20/50/200, EMA 20/50/200, VWMA 20, RSI, MACD, ATR, Stochastic, Fractals, Volume. Wick substitutes **Delta for Volume** on the pane. Overlay vs pane collision rules stay as in v1 requirements (max one extra pane-type).
- One sentence on derivation: computed from the Wick series (or the Signal OHLCV path on Base), not drawn by hand, not a visual theme.

Do not fork Signal's endpoint docs into this page. Link them.

---

### 2.7 `wick/using-the-chart.md` — the generic "docs people want"

**Job:** a first session without a tour video.

Draft beats:

- Open wick.green. Search is the full catalog. Right rail is a shortlist (Most Momentum / Newly Added), not the universe.
- Pan / zoom: wheel zoom, left-drag pan. Right-click is Wick-specific (context menu).
- Pane stack: price (overlays) → Delta → optional indicator pane.
- Applied Technicals: lines, zones, shapes. Color + interior opacity. v1 persistence and permalinks are Q4 work — document what is live *today*, and do not promise a share URL until it ships.
- Publish bar: note + PNG + PDF + Share to X, as grouped on the site.
- Cards under the chart: supply / website / CA / docs / socials. How to correct them (link Token cards).
- What "undetermined" / `mintable` means, one line.

This page will rot if it is written ahead of the Q4 permalink drop. Hermes should date-stamp "as of" at the top.

---

### 2.8 `wick/data-access.md`

**Job:** holders and desks know the series exists off-canvas. Do not publish an API that is not live.

Draft beats:

- Catalog data is Wick's and remains callable after interface delist.
- **Today:** site leaves (`/data/assets`, listings, per-token pages). No public guaranteed machine API yet.
- **Next:** x402 read API (Security-Manager first), same warehouse, metered. Point at the existing x402 / Bazaar guide for *how* 402 works; do not invent Wick routes.
- Agents: Signal remains the TA ASO on Base; Wick becomes the Arc series ASO when the door opens. Two products, one lab.
- Honesty line: scraping the chart is not the interface. Wait for the door.

---

### 2.9 `wick/faq.md`

Seed questions (answer in one short paragraph each):

- Is Wick a DexScreener for Arc? (No. Arc-native terminal. Quieter on purpose.)
- Does cataloging cost anything? (No.)
- Does listing expire? (Fee is one-time. Interface presence lasts while the market is real.)
- If you delist us do we lose the data? (No. Interface ≠ warehouse.)
- What if we rug / the LP dies? (Interface drops after the inactivity window. Series remains retrievable.)
- Why is our Twitter wrong? (`#requests`.)
- Why is the bottom pane not volume? (Delta page.)
- Why USDC pairs only? (That is how Delta is in one unit; that is the founding catalog.)
- Can I get this as an API? (Data access page. Not yet a published Wick route.)
- Where is Discord? https://discord.gg/Xyxs4rt8W5

---

## 3. Copy rules for Hermes

- **Public vs internal.** Interface-delist constants (`<1% Δ`, 4 weeks, random weekday sample) stay in this outline and in ops docs unless Nick decides the public page should show the exact fuse. Default: "several weeks of near-zero activity."
- **Pricing.** Use the linear table in §2.3. Retire the doubling language everywhere it still lives (`WICK-DEVELOPMENT-MASTER.md` §6).
- **Do not document vapor.** Permalinks, watchlists, x402 Wick routes, self-serve checkout — roadmap items. "As of [date]" on any page that describes the live canvas.
- **Discord is the support desk.** `#requests` for card fixes and listing intent. Do not invent a second form.
- **Do not paste lab handoffs** (`HANDOFF-v1-UI-ROUND6`, box IPs, PR numbers) into Gitbook.
- **Link Signal, do not clone it.** Indicator math lives in one place.
- **End-user first.** Team pricing sits on Catalog and listing, not on Overview.

---

## 4. Suggested write order

1. Overview stub replacement (unblocks the sidebar).
2. Delta (the gap that makes Wick make sense).
3. Catalog and listing + Delisting (the pair teams will screenshot).
4. Token cards (short; unblocks support).
5. Indicators (short; links Signal).
6. Using the chart (write last so it matches the live canvas).
7. Data access + FAQ.

Nick hammers thesis/price/delist wording with Hermes. Hermes owns Gitbook publish. Grok can draft page bodies off this outline when asked — not before the three locked pages (Delta, Catalog, Delist) are verbally signed.

---

## 5. Open calls for Nick / Hermes

1. Print the exact 4-week / `<1% Δ` fuse on the public delist page, or keep it "several weeks"?
2. Confirm 2028 = $2,250 and 2029 = $3,000 as the published table.
3. Confirm which indicators are actually mounted on the v1 canvas vs Signal's full ten.
4. Genesis assets: say "indexer of record, cataloged without a fee" or stay quiet?
5. Data-access page: ship as "not yet" or wait until the Q1 door exists and skip the page in v1 of the book?

_Outline only. Not a Gitbook page._
