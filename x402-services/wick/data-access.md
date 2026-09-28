# Data access

This page is the policy around Wick's data. The catalog data is Wick's and remains callable after an interface delist.

## For humans today

The data lives on wick.green — the site's **Data** surface, the **Listings**, and per-token pages. That is the human access path.

## For agents and desks (next)

An **x402 read API** over the same warehouse is the planned path — metered, and the Security-Manager is first in line. It is not published yet, so it is not documented here. The short version:

- Signal remains the technical-analysis service on Base.
- Wick becomes the Arc-series data source when that door opens. Two products, one lab.

For how x402 requests work in general, see the [x402 & Bazaar integration](../x402-bazaar.md) guide.

## Honesty line

Scraping the chart is not the interface. When a machine API ships, it will be a documented route — not something you reverse-engineer from the rendered page.