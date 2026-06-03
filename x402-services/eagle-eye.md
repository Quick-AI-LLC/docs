# Eagle Eye

**Version:** 1.0.0  
**Production:** https://eagle.quickai.build  
**Purpose:** Enterprise-oriented **wallet compliance screening** for U.S.-regulated flows. Screens destination addresses against the **OFAC SDN digital currency list** and **Circle USDC `isBlacklisted`** on Base. Returns a structured verdict, risk score, human-readable memo, and **off-chain signed audit receipt** (signing does not require on-chain gas).

## Use cases

- Pre-transfer screening for agents, treasuries, and payment routers
- Batch route checks (up to 100 addresses) with a single route verdict
- Optional wallet activity profile for additional risk context

## Endpoints

| Method | Path | Price (USDC) | Auth |
|--------|------|--------------|------|
| `GET` | `/health` | Free | None |
| `POST` | `/check` | $0.005 | x402 |
| `POST` | `/check/profile` | $0.006 | x402 |
| `POST` | `/check/batch` | $0.005 × N (max $0.50) | x402 |

`/health` returns **503** until the first OFAC snapshot sync completes.

## Request bodies

### `POST /check` and `POST /check/profile`

```json
{
  "address": "0x0330070FD38Ec3bB94F58FA55D40368271E9e54A"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `address` | string | EVM address (`0x` + 40 hex characters) |

### `POST /check/batch`

```json
{
  "addresses": [
    "0xabc000000000000000000000000000000000001",
    "0x0330070FD38Ec3bB94F58FA55D40368271E9e54A"
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
|| `addresses` | string[] | 1–100 addresses; ordered route; first FAIL determines route verdict |

## Response (single check)

| Field | Type | Description |
|-------|------|-------------|
| `address` | string | Screened address |
| `verdict` | `PASS` \| `FAIL` | Aggregate result |
| `is_sanctioned` | boolean | OFAC SDN match |
| `is_blacklisted` | boolean | USDC blacklist on Base |
| `risk_score` | integer | 0–100 |
| `reasons` | string[] | Machine-readable hits |
| `memo` | string | Audit narrative for humans and logs |
| `audit_timestamp` | string | ISO time of check |
| `snapshot_timestamp` | string | OFAC snapshot used |
| `audit_signature` | string | Off-chain signature over audit payload |
| `signer_address` | string | Service signer address |
| `profile` | object | Present only on `/check/profile` |

### Batch response

Includes `results[]` per address and `batch_summary` with `route_verdict` (`CLEAR` or `BLOCKED`), `blocked_at_index`, and counts.

## Architecture

- **Hot path:** In-memory OFAC + blacklist sets; sub-10ms lookups
- **Background:** OFAC JSON sync (default ~10h) + USDC `isBlacklisted` via Base RPC; double-buffered snapshot
- **Signing:** `SERVICE_PRIVATE_KEY` in environment (dedicated wallet; sign-only)
- **x402:** `ExactEvmScheme` on `eip155:8453`
- **Bazaar:** Per-route POST declarations with `method: POST`, `bodyType: json`, and JSON Schema for input/output

## Environment variables

### Required

| Variable | Description |
|----------|-------------|
| `CDP_API_KEY_ID` | Coinbase Developer Platform API key |
| `CDP_API_KEY_SECRET` | CDP API secret |
| `PAY_TO_ADDRESS` | Base wallet receiving USDC settlement |
| `SERVICE_PRIVATE_KEY` | Key used to sign audit receipts |

### Optional

| Variable | Description |
|----------|-------------|
| `REDIS_URL` | Profile cache (1h TTL on `/check/profile`) |
| `OFAC_LIST_URL` | Custom OFAC JSON source |
| `BASE_RPC_URL` | Base RPC (default: public mainnet) |
| `USDC_ADDRESS` | USDC contract on Base |
| `SYNC_INTERVAL_HOURS` | OFAC refresh interval |
| `PORT` | HTTP port (default `8000`) |

## Deploy

```bash
cd eagle-eye
cp .env.example .env
docker compose up -d --build
```

Point DNS `eagle.quickai.build` at the VPS. Caddy obtains TLS from `Caddyfile`.

## Examples

### Health

```bash
curl -s https://eagle.quickai.build/health
```

### Unpaid (402)

```bash
curl -i -X POST https://eagle.quickai.build/check \
  -H "Content-Type: application/json" \
  -d '{"address":"0x0330070FD38Ec3bB94F58FA55D40368271E9e54A"}'
```

### Paid (awal)

```bash
npx awal x402 pay https://eagle.quickai.build/check \
  -X POST \
  -d '{"address":"0x0330070FD38Ec3bB94F58FA55D40368271E9e54A"}'
```