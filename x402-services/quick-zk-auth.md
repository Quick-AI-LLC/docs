# Quick ZK Auth

**Version:** 1.0.1  
**Production:** https://zk.quickai.build  
**Purpose:** **Wallet qualification** for agentic commerce and ERC-8004-style workflows. Combines **Semaphore zero-knowledge membership proof** (ownership) with **USDC balance attestation** at a snapshot block. Returns an **attested bundle** linkable to x402 payment and an optional ERC-8004 registry envelope.

## What v1.0.1 attests

| Claim | Field | Mechanism |
|-------|-------|-----------|
| Wallet ownership | `ownership_proof_type: semaphore_zk` | Semaphore proof under scope `quickzkauth-v1` |
| Balance ≥ threshold | `balance_proof_type: rpc_attestation` | USDC `balanceOf` on Base at `snapshot_block` |
| Payment linkage | `x402_payment_ref` | Updated after CDP settlement (`tx_hash`, `payer`) |

**Important:** Balance is **attested on-chain via RPC** in v1.0.1; it is **not** hidden inside a ZK circuit yet. Future versions will add `balance_proof_type: zk_range`.

## Use cases

- Agent onboarding: prove funded Base wallet before higher limits  
- Marketplace gating: minimum USDC bar for buyers or sellers  
- ERC-8004 Validation Registry evidence (`?format=erc8004`)  
- Compose with Eagle Eye for sanctions screening on counterparties  

## Endpoints

| Method | Path | Price (USDC) | Auth |
|--------|------|--------------|------|
| `GET` | `/health` | Free | None |
| `POST` | `/prove` | $0.05 | x402 |
| `POST` | `/prove?format=erc8004` | $0.05 | x402 |
| `GET` | `/proof/:job_id` | Free | None (poll) |

## Request — `POST /prove`

```json
{
  "wallet": "0x0000000000000000000000000000000000000001",
  "min_balance_usdc": 1000,
  "tier": "basic"
}
```

| Field | Required | Default | Description |
|-------|----------|---------|-------------|
| `wallet` | Yes | — | Base wallet to qualify (`0x` + 40 hex) |
| `min_balance_usdc` | Yes | — | Integer ≥ 1 |
| `tier` | No | `basic` | `basic` or `standard` (registry tag hint) |

## Response — attested bundle

| Field | Description |
|-------|-------------|
| `job_id` | UUID; use with `GET /proof/:job_id` |
| `valid` | `true` if `observed_balance_usdc >= min_balance_usdc` |
| `claims` | Ownership, balance, proof types, `snapshot_block`, `nullifier_scope` |
| `proof_hash` | Hash of Semaphore proof material |
| `block_number` | Base block at attestation time |
| `expiry` | Bundle expiration (default ~30 minutes) |
| `verifier_contract` | On-chain verifier address |
| `calldata` | Encoded verify call (placeholder `0x` until v1.1) |
| `on_chain_verifiable` | `true` when `VERIFIER_CONTRACT` is deployed |
| `x402_payment_ref` | Payment binding (see below) |
| `validation_registry_hint` | Suggested ERC-8004 evidence type and tag |
| `erc8004_compatible` | `true` |

### `x402_payment_ref`

| Status | Meaning |
|--------|---------|
| `pending_settlement` | Returned immediately from `POST /prove`; includes `poll_uri` |
| `settled` | Available after CDP settlement; includes `tx_hash`, `payer`, `amount_usdc` |

Poll:

```bash
curl -s https://zk.quickai.build/proof/{job_id}
```

## ERC-8004 envelope — `POST /prove?format=erc8004`

Returns:

```json
{
  "type": "zk_wallet_qualification",
  "version": "1.0.1",
  "subject_wallet_commitment": "<Semaphore commitment — not the raw wallet>",
  "evidence": { },
  "registry_hint": { }
}
```

The `evidence` object is the full attested bundle. Use this format when submitting to an ERC-8004 Validation Registry.

## Replay and privacy

- **Replay control:** One proof per Semaphore identity commitment per `nullifier_scope` within the expiry window. Duplicate requests return **429**.
- **Semaphore group:** Persisted in Redis; `GET /health` reports `semaphore_group_size`. Anonymity is weak when group size is below 3.
- **Retention:** Proof records in Redis expire with `MAX_PROOF_AGE_MINUTES`. Wallets are not stored long-term.

## Architecture

- **ZK:** `@semaphore-protocol` (identity, group, proof) via dynamic ESM import  
- **Balance:** Base RPC `eth_call` to USDC `balanceOf`  
- **Redis:** Group export, nullifiers, proof store, payment attachment after settle  
- **x402:** `resourceServer.onAfterSettle` parses `job_id` from the response body and updates the stored proof  
- **Contract:** `contracts/QuickZKVerifier.sol` for future on-chain verification (Semaphore + planned Circom range proof)  

## Environment variables

### Required

| Variable | Description |
|----------|-------------|
| `CDP_API_KEY_ID` | CDP API key |
| `CDP_API_KEY_SECRET` | CDP API secret |
| `PAY_TO_ADDRESS` | Base settlement wallet |
| `REDIS_URL` | Required for proofs, replay, group persistence, payment binding |

### Optional

| Variable | Description |
|----------|-------------|
| `VERIFIER_CONTRACT` | Deployed `QuickZKVerifier` address (non-zero enables `on_chain_verifiable`) |
| `SEMAPHORE_SCOPE` | Nullifier scope (default `quickzkauth-v1`) |
| `BASE_RPC_URL` | Base RPC endpoint |
| `USDC_ADDRESS` | USDC on Base |
| `MAX_PROOF_AGE_MINUTES` | Bundle TTL (default `30`) |
| `PER_WALLET_COOLDOWN_SECONDS` | Rate limit per wallet (default `5`) |
| `PORT` | HTTP port (default `8000`) |

## Deploy

```bash
cd quick-zk-auth
cp .env.example .env
docker compose up -d --build
```

Stack includes Redis and Caddy for `zk.quickai.build`.

## Examples

### Health

```bash
curl -s https://zk.quickai.build/health
```

### Paid prove

```bash
npx awal x402 pay https://zk.quickai.build/prove \
  -X POST \
  -d '{"wallet":"0xYourWallet","min_balance_usdc":1000,"tier":"basic"}'
```

### Poll payment binding

```bash
curl -s https://zk.quickai.build/proof/550e8400-e29b-41d4-a716-446655440000
```

Replace `job_id` with the UUID from the prove response.

## Roadmap

| Version | Feature |
|---------|---------|
| v1.1 | Deploy `QuickZKVerifier`; real `calldata`; `on_chain_verifiable: true` |
| v1.2 | Circom range proof; `balance_proof_type: zk_range` |