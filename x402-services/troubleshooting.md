# Troubleshooting

Common issues when calling Quick AI x402 services and how to resolve them.

## 402 Payment Required

### I got a 402 but my wallet has USDC

**Likely causes:**

| Cause | Check |
|-------|-------|
| **Wrong network** | Ensure you're on **Base mainnet** (`eip155:8453`), not Base Sepolia or another chain |
| **Wrong asset** | Services expect **USDC** (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`), not ETH or bridged USDC |
| **Insufficient balance** | Cost + gas. A $0.05 call needs at least $0.07–$0.08 in the wallet to cover gas |
| **CDP API key not funded** | Coinbase Developer Platform keys need a funded API wallet. Check your CDP wallet balance |
| **Facilitator not configured** | Verify `CDP_API_KEY_ID` and `CDP_API_KEY_SECRET` in your x402 client setup |

### awal returns 402 but curl shows nothing useful

```bash
# Check the full response including headers
curl -v -X POST https://eagle.quickai.build/check \
  -H "Content-Type: application/json" \
  -d '{"address":"0x0330070FD38Ec3bB94F58FA55D40368271E9e54A"}'
```

Look for the `Payment-Required` header in the response. It contains JSON with the `accepts[]` array describing what payment schemes the service supports.

### I'm getting 402 in production after it worked in testing

x402 payment proofs are **single-use**. If you're reusing a cached payment header, the facilitator will reject it. Ensure your client builds a fresh proof for each request.

## 400 Bad Request

| Symptom | Fix |
|---------|-----|
| `GET /signal?timeframe=1d` with no `token` | `token` is required. Add `&token=BTC` |
| `POST /prove` with missing `wallet` or `min_balance_usdc` | Both fields are required for ZK Auth |
| Invalid Ethereum address | Verify the address is `0x` + 40 hex characters. Checksummed addresses (mixed case) are preferred but lowercase works |
| `addresses` array empty for `/check/batch` | Must contain at least 1 address, max 100 |

## 429 Too Many Requests

**Quick ZK Auth only.** This means a duplicate proof was detected for the same Semaphore identity commitment within the `nullifier_scope` and expiry window.

```bash
# Wait for the cooldown to expire (default: 30 minutes)
# Or use a different wallet
```

Per-wallet cooldown is 5 seconds by default for ZK Auth's `POST /prove`. Back off and retry.

## 503 Service Unavailable

**Eagle Eye only.** The service has started but hasn't completed its first OFAC snapshot sync yet. This is normal on first deploy or after a restart.

```bash
# Retry. Sync time depends on the OFAC list size (~5–15 seconds typically on restart)
curl -s https://eagle.quickai.build/health
```

Returns `200 OK` once ready.

## Client setup issues

### awal: "x402 pay" command not found

```bash
# Ensure you're using a recent version
npx awal@latest x402 pay --help
```

### I can't find the service on agentic.market

Service registration depends on Bazaar discovery extensions being published. After deploy:

1. Check the service logs for `Bazaar extension valid`
2. Verify an unpaid request returns HTTP 402 with `Payment-Required` header
3. Run a paid call with `npx awal x402 pay`
4. Wait up to 1 hour for marketplace indexing

If the service still doesn't appear, contact the Quick AI operator.

### My agent can't parse the response

All paid endpoints return strict JSON. If your client library has parsing issues:

- **Quick Signal TA** — the stochastic field uses `percent_k` (not `%K`) for JSON Schema compatibility
- **Eagle Eye batch** — check for `batch_summary.blocked_at_index` when `route_verdict` is `BLOCKED`
- **Quick ZK Auth** — poll `GET /proof/{job_id}` until `x402_payment_ref.status` is `settled` (typically < 5 seconds)

## Still stuck?

For operational issues (deploy, configuration, service downtime), contact **Quick AI LLC** through the [docs repo](https://github.com/Quick-AI-LLC/docs) or reach the operator directly.
