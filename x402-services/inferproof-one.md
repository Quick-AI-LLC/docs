# InferProof One (IP1)

**Version:** 0.4.0 (mainnet)  
**Production:** https://inferproof.one  
**Purpose:** **Time-bounded on-chain usage certificate for paid AI inference.** Users register with a Discord/GitHub/Google OAuth identity, bind a wallet, and "mine" IP1 tokens proportional to their paid inference usage — up to a Minecart-tier daily cap. An operator oracle compiles daily usage, reviews, and `batchMint`s the token certificate on Base. The IP1 treasury is the **same x402 settlement destination** as the Quick AI service stack.

## How it works

1. **Register** with Discord, GitHub, or Google OAuth (Discord OAuth also grants the **Verified Miner** role on the IP1 Mines server)
2. **Bind a wallet** — payments must come from the bound wallet; re-binding is timelocked (fraud protection)
3. **Pick a Minecart tier** — pay USDC or ETH to the treasury from the bound wallet
4. **Mine** — daily paid inference usage converts to IP1 up to your tier cap
5. **Settle** — the oracle compiles the day, reviews the artifact, and `batchMint`s IP1 to your wallet on Base

## Payment model

| Minecart | Daily Cap | USDC | ETH (frozen) |
|----------|-----------|------|--------------|
| 30 | 1 IP1/day | $9 | 0.0048 ETH |
| 150 | 5 IP1/day | $17 | 0.0090 ETH |
| 300 | 10 IP1/day | $25 | 0.0132 ETH |
| 750 | 25 IP1/day | $54 | 0.0286 ETH |
| 1,500 | 50 IP1/day | $85 | 0.0450 ETH |
| 3,000 | 100 IP1/day | $139 | 0.0735 ETH |

- **Cumulative balance** — tiers unlock when total paid crosses the threshold; overpay credits toward the next tier; anything beyond the top tier is a tip
- **No refunds** — cumulative accounting, upgrade anytime
- **Demo allowance** — 25 IP1/day before a wallet is bound or paid
- Unlock is **verified on-chain** (cumulative transfers from the bound wallet to the treasury) before the cap is raised — no free upgrades

## Addresses (Base mainnet, `eip155:8453`)

| Item | Address |
|------|---------|
| **Treasury (x402 destination)** | `0x85557C90027354eAeFFe9Ee4B4f4014818d8D3e1` |
| **USDC** | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| **IP1 token** | `IP1.sol` — `batchMint(recipients[], amounts[])`, no supply cap; guards: `mintEndTime`, `whenNotPaused`, `revoked[]` |

## Identity & security

- **OAuth identity** (Discord / GitHub / Google) with PKCE; encrypted API keys stored server-side, hint only (last 4 chars) — key never shown again after save
- **Wallet bind** — first bind free; subsequent binds timelocked (fraud protection)
- **Verified Miner role** — applied on Discord OAuth login/registration to the IP1 Mines server; role assignment is soft-fail (login still succeeds if Discord API is unreachable)

## Architecture

| Layer | Stack |
|-------|-------|
| Contract | Foundry + OpenZeppelin v5 (`src/IP1.sol`) |
| Backend / Oracle | Node.js 22 + TypeScript (`apps/api`) — M2 mint list, M3 registration, M4 compile/review/batchMint |
| Frontend | Next.js 15 (`apps/web`) — registration, dashboard, daily process view, gated admin |
| Chain | Base mainnet (8453), hardcoded in oracle broadcast |
| Oracle | `MAX_BATCH_RECIPIENTS = 250`; splits heavy days into gas-safe `batchMint` txs |

## Daily settlement cycle

```text
~00:05 UTC (N+1)  compile Day N usage (OpenRouter activity → IP1 amounts)
~23:55 UTC (N+1)  review artifact (operator)
00:00 UTC (N+2)   batchMint → recipients' wallets on Base
```

## Deployment

- **VPS:** Ionos `[REDACTED IP]` — PM2 processes `[REDACTED]` (oracle [REDACTED PORT]), `[REDACTED]` (identity [REDACTED PORT]), `[REDACTED]` (Discord InferNode), `[REDACTED]` (Next.js :3000)
- **TLS:** Caddy fronts `https://inferproof.one` → :3000
- **Build:** web is Next.js standalone; `NEXT_PUBLIC_APP_URL=https://inferproof.one` must be baked at build time
- **Env:** `[REDACTED PATH]/api/.env` (loaded at runtime via custom loader, no dotenv dep)

## Docs & source

- Product spec: `InferProof_Consolidated_Overview_v9.docx`
- Repository: `Quick-AI-LLC/InferProof-One` (private)
- Token contract: `src/IP1.sol` — `batchMint`, no per-address/call cap, no totalSupply cap

## Status

Mainnet live (Aug 2026). Real contract deployment targeted Sept 1; the live burner contract (`TEST5`) proves the apparatus on mainnet until then.
