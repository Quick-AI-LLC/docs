# InferProof One — Mining Rules (v1.1)

**Effective Date:** Upon public registration opening  
**Last Updated:** 5 August 2026  
**Operator:** Quick AI LLC (Idaho)  
**Official website:** https://inferproof.one

These Mining Rules govern participation in InferProof One (IP1). By registering, pasting a management key, binding a wallet, or making any payment, you agree to these Rules in full.

---

## 1. Nature of IP1

IP1 is a pure historical receipt of paid AI inference.  
It records that a specific OpenRouter identity paid for a quantity of tokens during the one-year mining window.  

IP1 is **not** an investment product, security, or speculative instrument. There is no expectation of profit from the efforts of Quick AI LLC or any other party. Quick AI designs, secures, and operates the mining system for a fixed period. After that period ends, Quick AI’s operational role concludes.

**Mining accrual vs token decimals:** While mining, accruals are in **whole IP1 only** (see §3). The on-chain token uses **decimals = 1** so common wallet and integration paths that expect a non-zero decimal place work cleanly after mint. Fractional (0.1) units may appear in post-mint transfers or later promotions; they do **not** change the whole-IP1 mining floor.

---

## 2. Demo Allowance

We want everyone who already pays for inference to be able to experience the mining system. Hosting the infrastructure that meters usage, compiles daily lists, runs the review window, and submits mints costs real money.

Therefore every newly registered account receives a **one-time cumulative demo allowance of 25 IP1**.

- This is a lifetime total, not a daily cap. It is distinct from the daily capacity of a paid Minecart Size.
- Demo mints follow the exact same daily settlement and review process as paid mints. IP1 is earned only as you log qualifying paid inference — not as a free up-front grant of 25 tokens.
- When an account’s total minted IP1 reaches 25, further minting is blocked until a paid Minecart Size is purchased.
- There are no partial IP1 **accruals**. 1,000,000 paid tokens = 1.0 IP1 (floor).
- **Purchasing a Minecart Size ends demo.** Remaining demo headroom is discarded; the account uses the cart’s daily cap thereafter. If you want to finish a few more demo tokens first, delay the purchase yourself.

---

## 3. Conversion & Eligibility

- **Conversion rate:** 1,000,000 paid OpenRouter tokens = 1.0 IP1 (floor).
- **Paid usage only.** Eligibility is determined by the presence of a USD value in the OpenRouter activity roll. Rows carrying no USD value are treated as free and are excluded. The distinction is the provider’s own billing signal.
- Metering is performed using the **management key** you generate on OpenRouter and paste into InferProof. Only paid usage attributed to the account that owns that management key counts.
- Recommended practice: create a **dedicated OpenRouter account** that is preloaded and has **no auto-top-up**. This isolates mining activity from any primary billing account.

Management keys are non-spendable (they cannot run inference). Nevertheless, InferProof treats them with the same custody standard as if they were spendable. You must weigh the pros and cons of generating a management key and handing it to the operator. Official documentation on management keys is available at:  
https://openrouter.ai/docs/guides/overview/auth/management-api-keys

---

## 4. Daily Settlement (N / N+1 / N+2)

- **Day N** = 00:00:00.000 UTC to 23:59:59.999 UTC.
- Day N usage is compiled on **Day N+1 at approximately 00:05 UTC**.
- A review period of roughly 23 hours and 55 minutes then follows.
- The Day N mint (batchMint) is submitted at **00:00 UTC on Day N+2**.
- Usage occurring at or after **23:59:01 UTC** belongs to the next day.
- The compiled figure after review is **final**. There are no credit negotiations, retroactive adjustments, or disputes over individual totals.
- Daily mints may be delayed or an entire day’s list may be held or stricken for integrity reasons. Unbroken 365-day minting is not guaranteed. Scarcity created by missing or delayed days is accepted by design.

---

## 5. Minecart Sizes, Payment & Upgrades

Ongoing minting after the demo requires a one-time payment that unlocks a daily capacity (Minecart Size).

Current sizes (USDC):

| USDC | Daily Cap | Hero Name       |
|------|-----------|-----------------|
| $9   | 1         | Minecart 30     |
| $17  | 5         | Minecart 150    |
| $25  | 10        | Minecart 300    |
| $54  | 25        | Minecart 750    |
| $85  | 50        | Minecart 1,500  |
| $139 | 100       | Minecart 3,000  |

- Payments are accepted in **USDC** (exact amounts above) or **native ETH**. ETH amounts are set at mainnet deployment so that ETH value ≈ the USDC price of each size, then **frozen**.
- Payment must originate from the wallet currently bound to the account.
- **No refunds.** All payments are final.
- **Upgrades:** You may increase your Minecart Size at any time by paying from your bound wallet. Cumulative paid balance unlocks higher tiers. Overpayment credits toward the next size. Amount beyond the highest size is a tip.

---

## 6. Accounts, Login, Wallet Binding & Public ID

- At most **two accounts per registration IP** (household / F&F).
- The internal **publicId** (sequential #00001, #00002, …) is the single source of truth for identity.
- External OAuth providers (Discord primary; GitHub and others additive) resolve to the same publicId.
- **Access to the mining app is tied exclusively to the OAuth methods you have linked.** If you lose access to those providers, we cannot recover the account.
- One publicId ↔ one wallet (1:1). The first wallet change is free. Subsequent changes are subject to a 30-day timelock.
- **Verified Miner role:** Auto-assigned after registration + Discord OAuth. Community label only.

---

## 7. Operator Rights

During the mining window the operator (Quick AI LLC) retains the ability to:

- Pause minting.
- Permanently revoke future minting rights on specific addresses in cases of severe abuse, manipulation, or integrity violations.
- Hold or strike an entire day’s mint list when anomalies are detected.

---

## 8. Scope of Quick AI’s Role

Quick AI’s business is limited to designing a fair mining system, securing it, facilitating the daily metering → review → mint process, and reducing shared costs through batching.

Anything related to secondary markets, liquidity, bridges, price, speculative trading, or ongoing token utilities is **completely out of scope**.

After the one-year window ends, Quick AI’s operational role concludes. Governance passes to a restricted DAO formed from the holder set. Quick AI LLC will transfer relevant ownership rights and will not participate in DAO governance.

---

## 9. Community Safety & Scam Reporting

Quick AI LLC and the official InferProof channels will **never**:

- Proactively direct-message (DM) you on X or in Discord.
- Ask you for money, private keys, seed phrases, OpenRouter credentials, OAuth tokens, logins, or any other sensitive information.
- Solicit funds for exchange listings, liquidity, marketing, or any other purpose.

Any message that claims to be from InferProof / Quick AI and asks for the above is a **scam**.

Report via the dedicated **#scam-reports** channel in the official Discord or by emailing nick@quickai.build.

---

## 10. Start Date & Window Length

The mining window runs for one year from the official **start date** (announced when the system is ready for public registration). The end is determined by an immutable on-chain timestamp.

---

## 11. Acceptance

By registering or continuing to use the InferProof One system you acknowledge that you have read, understood, and agree to these Mining Rules.

These Rules may be updated. Material changes will be announced through official channels. Continued participation after an update constitutes acceptance of the revised Rules.

---

*Quick AI LLC — P.O. Box 254, Hayden, ID 83835 — nick@quickai.build — https://inferproof.one*
