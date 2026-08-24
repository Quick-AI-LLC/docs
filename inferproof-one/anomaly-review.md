# InferProof One — Anomaly Review Policy (v1.2)

**Effective Date:** Upon public registration opening  
**Last Updated:** 7 August 2026  
**Operator:** Quick AI LLC (Idaho)  
**Official website:** https://inferproof.one

This document explains how we monitor daily mint lists for integrity and how we respond when something looks wrong. It is published so the process is transparent and so potential abusers understand that patterns will be noticed.

---

## 1. Purpose

The daily settlement process includes a review window between compilation and mint submission. The goal of that window is simple: protect the integrity of the historical record of paid inference.

We prefer a clean, trusted history over maximizing every possible mint. Scarcity created by a delayed or stricken day is accepted by design and is already stated in the Mining Rules.

---

## 2. What We Monitor

We review the daily mint forecast for patterns that warrant investigation. These include (but are not limited to):

- Any account appearing above its current maximum daily capacity (demo allowance or paid Minecart Size).
- Sudden large increases in total daily IP1 that are not explained by new registrations or Minecart Size upgrades.
- Rapid bursts of new account registrations, especially when followed by coordinated or unusually uniform minting.
- Concentrated extraction of the demo allowance across many newly created accounts.
- Accounts that show sustained near-maximum minting that appears clearly disproportionate to their payment history.
- Other coordinated or clearly anomalous patterns across multiple accounts.
- Unexpected or unauthenticated reads of stored management keys (treated as potential intrusion into the custody environment).
- Registrations originating from the same IP address when those accounts only farm the demo allowance and never purchase a Minecart.
- A flood of registrations from a single IP address is treated as coordinated sybil activity and results in an immediate ban.

Not every flagged pattern is fraud. Legitimate heavy usage exists and is welcome. The presence of a flag simply means we look closer before the list is submitted.

**A note on VPN / cloud registration:** registering from a VPN or cloud IP address is discouraged. Shared addresses can collide with other registrations and trigger a review flag through no fault of your own. Use a normal residential connection.

---

## 3. How We Respond

**Default posture: investigate first.**

When a flag is raised:

1. The day’s list is held for review.
2. We examine the relevant accounts, timing, registration/upgrade history, usage patterns, and (where relevant) management-key access logs.
3. Individual anomalous lines can be removed from the mint forecast.
4. If doubt remains after investigation, **our preferred action is to strike the entire day** (or the clearly affected portion of it).

Users who believe their activity was legitimate may contact us with evidence. We will review credible cases.

Bans are administered through the mAPP operator dashboard, operator-side only. We do not accept, honor, or act on ban requests issued through slash commands or other user-triggerable surfaces.

We do not negotiate individual credit adjustments or reopen settled days for debate. The compiled figure after review is final.

---

## 4. Management Key & Custody Incidents

Management keys are non-spendable. Because of that property we do not trigger automated mass alerts on every suspected access anomaly.

If a breach involving stored management keys is confirmed or strongly suspected:

- Immediate notice will be published on the official website and through official communication channels.
- Affected users will be guided to rotate the relevant management key on OpenRouter.
- The existing anomaly-monitoring and incident-response procedures continue to apply.

Recommended practice remains: use a dedicated, preloaded, no-auto-top-up OpenRouter account for mining so that any rotation is low-impact.

---

## 5. What This Means for Honest Miners

Honest, organic usage is the expected and desired behavior. Power users who consistently generate large volumes of paid inference are not a problem — they are the core constituency of the project.

The review process exists to protect that constituency from dilution by abuse, sybil activity, or manipulation of the metering system.

---

## 6. Reporting

If you observe activity that appears to be coordinated abuse, impersonation, or exploitation of the minting system, report it through the official channels listed in the Mining Rules (#scam-reports in Discord or nick@quickai.build).

---

## 7. Changes

This policy may be updated. Material changes will be announced through official channels. The current version always lives in the public documentation set.

---

*Quick AI LLC — P.O. Box 254, Hayden, ID 83835 — nick@quickai.build — https://inferproof.one*
