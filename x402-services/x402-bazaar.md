# x402 & Bazaar integration

All Quick AI agent services use the same payment and discovery patterns. This page describes how clients and marketplaces interact with them.

## Payment rail

| Property | Value |
|----------|-------|
| Network | `eip155:8453` (Base mainnet) |
| Scheme | `exact` |
| Asset | USDC `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| Facilitator | Coinbase CDP (`@coinbase/x402`) |
| Protocol version | x402 v2 (`X402-Version: 2` response header via Caddy) |

## Request lifecycle

```text
1. Client calls paid endpoint without payment
   → 402 Payment Required
   → Payment-Required header (base64 JSON with accepts[], Bazaar extensions)

2. Client builds payment proof (awal, x402 client SDK, or agent wallet)

3. Client retries same request with payment header
   → Facilitator verifies
   → Route handler executes
   → Facilitator settles USDC to PAY_TO_ADDRESS
   → Payment-Response header on success (where applicable)
```

## Pricing summary

| Service | Route | Price |
|---------|-------|-------|
| Eagle Eye | `POST /check` | $0.005 |
| Eagle Eye | `POST /check/profile` | $0.006 |
| Eagle Eye | `POST /check/batch` | $0.005 × addresses (max $0.50) |
| Quick Signal TA | `POST /signal`, `GET /signal` | $0.05 |
| Quick ZK Auth | `POST /prove` | $0.05 |

`GET /health` and Quick ZK Auth `GET /proof/:job_id` are free.

## Bazaar discovery

Each paid route registers a **Bazaar discovery extension** at startup with:

- `method`: `POST` or `GET` (must be set at declaration time for validator acceptance)
- `bodyType`: `json` for POST body routes
- `input` / `inputSchema`: what the agent must send
- `output.example` / `output.schema`: what the agent receives

Services run **`validateBazaarAtStartup()`** before listening. Invalid schemas cause the process to exit.

### Indexing checklist

After deploy:

1. Confirm startup log: `Bazaar extension valid`  
2. Unpaid curl returns **402** with `Payment-Required`  
3. Run one paid call per route (Hermes / awal)  
4. Validate on [agentic.market](https://agentic.market) or CDP discovery tools  

## Client tools

### awal (CLI)

```bash
npx awal x402 pay https://{host}/{path} -X POST -d '{...json...}'
```

### Unpaid probe

```bash
curl -i -X POST https://{host}/{path} \
  -H "Content-Type: application/json" \
  -d '{...}'
```

Expect `HTTP/2 402` and a `Payment-Required` header.

## Required operator configuration

Every service requires:

| Variable | Purpose |
|----------|---------|
| `CDP_API_KEY_ID` | Facilitator authentication |
| `CDP_API_KEY_SECRET` | Facilitator authentication |
| `PAY_TO_ADDRESS` | USDC payee on Base |

Eagle Eye additionally requires `SERVICE_PRIVATE_KEY` for audit signatures.

Quick ZK Auth additionally requires `REDIS_URL` for v1.0.1 features (proof store, payment binding, replay control).

## Error codes (common)

| HTTP | Meaning |
|------|---------|
| 402 | Payment required or settlement failed |
| 400 | Invalid request body or query |
| 429 | Quick ZK Auth replay (duplicate proof in scope) |
| 503 | Eagle Eye health before first OFAC snapshot |
| 404 | Unknown path |