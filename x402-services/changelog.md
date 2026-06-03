# Changelog

All notable changes to Quick AI x402 services are documented here. This changelog covers the shared documentation; individual service version bumps are noted per page.

## 2026-06-02

- Initial Gitbook documentation published
- Services documented: Eagle Eye v1.0.0, Quick Signal TA v3.1.0, Quick ZK Auth v1.0.1

## Service roadmaps

### Quick ZK Auth

| Version | Feature | Status |
|---------|---------|--------|
| v1.0.0 | Initial release: Semaphore ZK proof + USDC balance attestation | ✅ Live |
| v1.0.1 | ERC-8004 envelope, payment binding via `onAfterSettle` | ✅ Live |
| v1.1 | Deploy `QuickZKVerifier` contract; real on-chain calldata | 🚧 Planned |
| v1.2 | Circom range proof for private balance attestation | 🔮 Future |

### Eagle Eye

| Version | Feature | Status |
|---------|---------|--------|
| v1.0.0 | OFAC SDN + USDC blacklist screening, batch, profile, signed receipts | ✅ Live |

### Quick Signal TA

| Version | Feature | Status |
|---------|---------|--------|
| v3.1.0 | 10 indicators, CoinGecko/CMC fallback chain, GET + POST | ✅ Live |
