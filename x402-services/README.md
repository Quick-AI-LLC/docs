# Quick AI Docs

Quick AI, LLC is an independent studio and agentic organization based in North Idaho. We build x402 services, release music, and provide consulting in AI integration, media production, and custom development.

## Services

Three paid HTTP APIs on **Base mainnet** (`eip155:8453`), settled in **USDC** via Coinbase **CDP x402**:

| Service | URL | Role |
|---------|-----|------|
| **Eagle Eye** | https://eagle.quickai.build | U.S. sanctions + USDC blacklist screening |
| **Quick Signal TA** | https://signal.quickai.build | Crypto technical analysis (10 indicators) |
| **Quick ZK Auth** | https://zk.quickai.build | Wallet ownership (ZK) + USDC qualification |

## Shared stack

- Node.js 20, Express, TypeScript
- `@x402/express` + `@coinbase/x402` facilitator
- Bazaar extensions with HTTP `method` declared at registration (POST or GET)
- Caddy reverse proxy, TLS, `X402-Version: 2`
- Docker Compose on VPS
- Settlement wallet: `PAY_TO_ADDRESS` (Base USDC)

## Typical agent pipeline

```text
ZK Auth (economic identity) → Eagle Eye (compliance) → Signal (market intel) → action
```

## Documentation

- [Eagle Eye](./eagle-eye.md)
- [Quick Signal TA](./quick-signal-ta.md)
- [Quick ZK Auth](./quick-zk-auth.md)
- [x402 & Bazaar integration](./x402-bazaar.md)
- [Composing the stack](./composing-the-stack.md)

## More from Quick AI

- [Music catalog](./music/music.md) — releases, production, custom requests
- [Consulting](./consulting/consulting.md) — AI integration, custom solutions, pricing
- [Company](./company/company.md) — agentic organization, founding, terms

## Repository layout

| Path | Service |
|------|---------|
| `eagle-eye/` | Eagle Eye v1.0.0 |
| `quick-signal-ta/` | Quick Signal TA v3.1.0 |
| `quick-zk-auth/` | Quick ZK Auth v1.0.1 |