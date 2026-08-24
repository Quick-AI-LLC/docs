# InferProof One — Blue Paper v1.1

**17 August 2026 · Quick AI LLC (Idaho)**  
https://inferproof.one · https://x.com/inferproofone · https://github.com/Quick-AI-LLC/docs/tree/main/inferproof-one

A time-bounded on-chain certificate of paid AI inference.

This document describes the motivation, design, and operational posture of InferProof One (IP1). It is the formal reference for the apparatus. Public-facing rules, privacy practices, and anomaly policy are maintained in this documentation set and are affirmed by this paper.

---

## 0. Terminology

- **iT** — Inference tokens (prompt + completion).
- **MiT** — One million inference tokens.
- **IP1t** — The IP1 token (the on-chain unit).
- **IP1** — The project as a whole.
- **1-YW** — The one-year window.
- **mAPP** — InferProof One Mining Application.
- **publicId** — Sequential identifier assigned at registration (#00001, #00002, …).
- **Management key** — A non-spendable OpenRouter key capable of reading account-level paid usage. Stored encrypted under custody operated by Quick AI LLC.
- **Minecart Size** — A one-time payment that unlocks a fixed daily mining capacity.
- **Day N / N+1 / N+2** — Usage day (UTC), compilation + review day, and mining-submission day respectively.

Mining accruals are whole IP1t only (1 MiT = 1.0 IP1t). Token decimals = 1 exist for integration compatibility.

---

## 1. Origin

In recent years a number of projects have attempted to incorporate artificial-intelligence inference into Proof-of-Work style security models (Pearl, Nockchain, and related efforts). These systems treat inference as a component of chain security. In practice the construction collapses under a more fundamental property of Proof-of-Work itself: who controls the most compute. Whether that compute is applied to hash functions or matrix multiplications is secondary. Legal and illegal sources of compute are treated identically. AI-augmented PoW therefore inherits the same centralization pressures and the same incentive to acquire compute by any means available.

From the standpoint of a heavy user of paid AI inference, such constructions offered little direct benefit. The inference is executed to secure someone else’s chain. The person who already paid for the compute receives no durable, portable record of that expenditure. The economic fact of the expenditure disappears into the provider’s internal ledger.

InferProof One begins from the opposite premise. It discards the security function entirely. The sole purpose of the apparatus is to take energy that has already been expended in paid inference calls and store a verifiable, time-bounded historical receipt of that expenditure on-chain. The token is not an emission schedule, not a consensus mechanism, and not a claim on future work. It is a certificate that real money was spent on real compute under identical rules for every participant, for exactly one year.

This thesis — certification of paid compute rather than contribution to chain security — defines every subsequent design choice.

### Who Mines on IP1

The apparatus is built for people who already pay for inference or who are prepared to do so in the ordinary course of their work:

- Long-time crypto participants who understand the physical cost of compute and want a transparent mapping of paid energy to a permanent public record.
- Agentic users and builders running agents, pipelines, or production systems against OpenRouter and similar providers.
- AI practitioners who value a time-bounded historical certificate of early API-scale inference.
- Crypto participants who have not yet engaged deeply with AI tooling and want a low-friction path (OpenRouter account + management key + Base wallet).

Speculators are strongly advised to look elsewhere. There is no emission schedule designed for trading velocity, no protocol-level liquidity, and no official market-centric integrations during the 1-YW.

---

## 2. Conceptual Design

### Identity and Access

Accounts are anchored by a sequential publicId. External OAuth providers (Discord primary; GitHub and others additive) resolve to this publicId. Each publicId is bound one-to-one with a Base wallet. All payments must originate from the currently bound wallet. The first wallet change is free; subsequent changes are subject to a thirty-day timelock.

### Metering Source

Metering relies on account-level non-spendable OpenRouter management keys. Users generate and paste the key; it is stored encrypted under hardened custody. Eligibility is determined by the presence of a USD value in the OpenRouter activity roll. Rows carrying no USD value are treated as free and are excluded. The distinction is the provider’s own billing signal and cannot be gamed by renaming models.

### Conversion and Demo

1,000,000 paid inference tokens = 1.0 IP1t (floor). Every newly registered account receives a one-time cumulative demo allowance of 25 IP1t under the same rules. When total minted reaches 25, further mining is blocked until a Minecart Size is purchased.

### Minecart Size and Payment

| USDC | Daily Cap | Label |
|------|-----------|-------|
| $9 | 1 | Minecart 30 |
| $17 | 5 | Minecart 150 |
| $25 | 10 | Minecart 300 |
| $54 | 25 | Minecart 750 |
| $85 | 50 | Minecart 1,500 |
| $139 | 100 | Minecart 3,000 |

Payments in USDC (exact) or native ETH (amounts frozen at launch). Must come from the bound wallet. No refunds. Cumulative paid balance unlocks higher tiers.

### Daily Settlement Cycle

Day N = 00:00–23:59:59.999 UTC. Compiled ~00:05 UTC on Day N+1. Review window ~23h 55m. Mint submitted 00:00 UTC on Day N+2. The compiled figure after review is final. Days may be delayed or stricken for integrity reasons. Unbroken 365-day mining is not guaranteed.

### Integrity Posture

The operator’s primary commitment is to the integrity of the historical record. Preferred action in the face of anomalies is to pause, investigate, remove suspect lines, or strike a day rather than force a compromised submission.

### Governance Handoff and Sunset

The 1-YW is finite. At its conclusion the operator’s privileged role ends. Governance passes to a restricted DAO formed from the holder set. Quick AI LLC transfers operational ownership rights and does not participate in DAO governance. The mAPP is planned for sunset within ~6 months after mining ends. The on-chain record remains permanent.

---

## 3. Conclusion

InferProof One is a time-bounded certificate of paid AI inference. It does not secure a chain, does not emit tokens according to an inflation schedule, and does not intermediate billing or inference itself. It records that a given identity expended real money on real compute during a fixed one-year interval, under rules that apply identically to every participant.

The design choices — non-spendable management keys under custody, daily settlement with an explicit review window, hard capacity limits calibrated to observed max-day volume, acceptance of downtime in preference to a corrupted record, a clean contractual sunset, and a hard refusal to advance speculative infrastructure during the 1-YW — follow directly from the original premise: store the energy already spent, nothing more.

The apparatus is developed and hosted by Quick AI LLC (Idaho). At the conclusion of the 1-YW the operator’s role ends. The historical certificate remains.

End of Blue Paper v1.1
