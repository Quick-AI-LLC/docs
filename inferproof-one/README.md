# InferProof One (IP1)

**Time-bounded on-chain usage certificate for paid AI inference.**

1,000,000 paid OpenRouter tokens = 1.0 IP1. One-year mining window. Daily settlement on Base. Capacity gated by one-time Minecart Size payments.

- **Live app:** https://inferproof.one  
- **X:** https://x.com/inferproofone  
- **Operator:** Quick AI LLC (Idaho)

This folder is the public documentation set for InferProof One. The implementation repository remains private.

## Core documents

| Document | Description |
|----------|-------------|
| [Blue Paper](./blue-paper.md) | Formal conceptual design, philosophy, mechanics, and operational posture |
| [Mining Rules](./mining-rules.md) | Full product terms users accept (demo, settlement, payments, upgrades, scope) |
| [Privacy Notice](./privacy-notice.md) | InferProof-specific data practices |
| [Anomaly Review Policy](./anomaly-review.md) | Integrity monitoring posture and response principles |

## Short summary

Users register via OAuth, paste a non-spendable OpenRouter management key, bind a Base wallet, and mine IP1 from paid inference they already purchase. Free/unbilled activity is ineligible. Every account receives a one-time 25 IP1 demo allowance under the same rules. After the demo, a paid Minecart Size sets the daily ceiling. Day N usage is compiled on N+1, reviewed, and minted on N+2. The operator prioritizes record integrity over continuous mining; days may be delayed or stricken.

The apparatus is a pure historical certificate, not a consensus mechanism or speculative emission schedule. After the one-year window, operational control ends and governance passes to a restricted DAO formed from holders.

## Related

- High-level product note (x402 stack context): [x402-services/inferproof-one.md](../x402-services/inferproof-one.md)
- Quick AI company docs and other ASOs live in the parent repository.
