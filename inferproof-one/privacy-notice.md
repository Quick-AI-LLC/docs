# InferProof One — Privacy Notice (v1.2)

**Effective Date:** Upon public registration opening  
**Last Updated:** 7 August 2026  
**Operator:** Quick AI LLC (Idaho)  
**Official website:** https://inferproof.one

This Privacy Notice applies specifically to the InferProof One (IP1) mining system. It sits alongside the general Quick AI LLC Privacy Policy (v2.0) and covers the data practices unique to this product.

---

## 1. What We Collect

When you use InferProof One we collect and process:

- **Management key** — the non-spendable OpenRouter management key you generate and paste into the app. It is stored encrypted at rest under a hardened custody environment and is used solely to read paid usage for metering.
- **OAuth identities** — the Discord, GitHub, and any other providers you link. These resolve to a single internal publicId and are the sole means of authenticating into the mining app.
- **Bound wallet address** — the single Ethereum / Base address linked to your publicId.
- **Cumulative payment balance** — the total USDC or ETH you have sent from the bound wallet for Minecart Size unlocks.
- **Usage snapshots** — daily aggregates of paid tokens reported by OpenRouter for the account that owns the pasted management key (after free-model filtering).
- **Public ID** — a sequential number (#00001, #00002, …) assigned at registration.
- **Registration IP address** — the source IP address of the connection at the moment you register. This is captured solely as a security and fraud-reduction measure. It is used to detect duplicate and flooded registrations, is never sold or shared, and does not grant us access to anything on your device.

We do **not** collect, store, or have access to:

- Your OpenRouter billing information, payment methods, or invoice history.
- Private keys, seed phrases, or any wallet credentials.
- The content of any prompts or completions you send through OpenRouter.
- Spendable API keys — the only key we ever hold is your non-spendable management key.

---

## 2. How We Use the Data

Data is used solely to:

- Meter eligible paid usage via the management key,
- Enforce the demo allowance and Minecart Size daily caps,
- Compile the daily mint lists,
- Operate the review window,
- Detect and review duplicate or flooded registrations for fraud prevention,
- Display your dashboard (totals, remaining demo, current Minecart, public ID),
- Publish anonymized daily summaries (totals, top amounts + public IDs, earner count).

We do not sell, rent, or share this data with third parties for marketing or any other commercial purpose.

---

## 3. Management Key Custody

Management keys are non-spendable. Nevertheless, Quick AI treats them with the same security standard applied to spendable credentials:

- Keys live in a dedicated, hardened environment.
- Reads are performed through a secrets service that issues short-TTL HMAC-tagged access events; unexpected reads are treated as potential intrusion.
- Separation of duties is maintained between the read-monitor, the oracle process, and operational deploy access.

You generate the key on OpenRouter and paste it yourself. Official documentation:  
https://openrouter.ai/docs/guides/overview/auth/management-api-keys

**Recommended practice:** create a dedicated OpenRouter account that is preloaded and has no auto-top-up.

If a breach involving stored management keys is discovered, Quick AI will issue immediate notice through the official website and official communication channels.

---

## 4. Public by Design

- Minecart Size payments occur on the Base blockchain. Transaction hashes, amounts, and the receiving address are public record.
- Your sequential public ID and the anonymized daily summaries that reference it are intentionally public so the community can verify activity without exposing wallet addresses.

---

## 5. Account Access

Access to the mining app is tied exclusively to the OAuth methods you have linked to your publicId. If you lose access to those providers, Quick AI cannot recover the account for you.

---

## 6. Retention

- Active metering data and encrypted management keys are retained for the duration of the one-year mining window plus a short administrative period afterward.
- Registration IP addresses are retained only for the duration of the registration window and the one-year mining period and are deleted when registration closes.
- After the window ends and final settlement is complete, we will delete or anonymize operational records that are no longer required.
- On-chain records (IP1 balances, payment transactions) remain on the blockchain permanently by design.

---

## 7. Security

Credentials used for OpenRouter metering are encrypted at rest. Access is limited to the systems that perform daily compilation and review. No system is perfect; you remain responsible for the security of your own OpenRouter account, the decision to hand over a management key, and your wallet.

---

## 8. Your Choices

- You may remove a management key or disconnect OAuth methods at any time (metering will stop for that identity).
- You may change your bound wallet subject to the rules in the Mining Rules.
- You may request deletion of off-chain account data after the mining window has fully closed by contacting us. On-chain data cannot be deleted.

---

## 9. Contact

Questions about this Privacy Notice or your InferProof data:

- **Website:** https://inferproof.one  
- **Email:** nick@quickai.build  
- **Mail:** Quick AI LLC, P.O. Box 254, Hayden, ID 83835

---

*This Notice is specific to InferProof One. For general Quick AI LLC practices see the Privacy Policy v2.0 in this repository.*
