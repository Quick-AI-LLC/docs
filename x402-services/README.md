# Quick AI — x402 Agent Services

Quick AI operates three paid HTTP APIs on **Base mainnet** (`eip155:8453`), settled in **USDC** via the Coinbase **CDP x402** facilitator. Agents discover and pay through standard **x402 v2** flows (for example `awal` or the CDP SDK) and **Bazaar** discovery metadata for marketplaces such as [agentic.market](https://agentic.market).

|| Service | URL | Role | Price range |
||---------|-----|------|-------------|
|| **Eagle Eye** | https://eagle.quickai.build | U.S. sanctions + USDC blacklist screening | $0.005–$0.50 |
|| **Quick Signal TA** | https://signal.quickai.build | Crypto technical analysis (10 indicators) | $0.05 |
|| **Quick ZK Auth** | https://zk.quickai.build | Wallet ownership (ZK) + USDC qualification | $0.05 |

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

## Repository layout

| Path | Service |
|------|---------|
| `eagle-eye/` | Eagle Eye v1.0.0 |
| `quick-signal-ta/` | Quick Signal TA v3.1.0 |
| `quick-zk-auth/` | Quick ZK Auth v1.0.1 |