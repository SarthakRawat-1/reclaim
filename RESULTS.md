# Batch Execution Results: 135 Multi-Rail Recovery Cases

This document compiles the empirical performance, financial reconciliation, and compliance audit metrics from executing ReClaim across a benchmark batch of 135 recovery cases spanning Subscriptions & Mandates (Module A), B2B Trade Receivables (Module B), and Checkout Drop-Offs (Module C).

The evaluation demonstrates bounded autonomy in practice: maximizing revenue recovery on viable transactions while enforcing deterministic stopping rules that halt or gate recovery when legally or operationally required.

---

## 1. Executive Performance Summary

| Metric | Measured Value | Unit / Format | Operational Rationale |
| :--- | :--- | :--- | :--- |
| **Total Batch Scope** | 135 | Cases | 60 Module A + 50 Module B + 25 Module C |
| **Total Revenue at Risk** | 63,64,350.42 | INR (636,435,042 paise) | Aggregate addressable unpaid capital |
| **Gross Revenue Recovered** | 38,25,236.56 | INR (382,523,656 paise) | Total funds captured via payment links & retries |
| **Value Recovery Yield** | 60.10% | Percentage | Gross recovered value / Total at-risk value |
| **Cases with Funds Captured** | 61 | Cases (45.19%) | 56 fully settled cases + 5 partial collections |
| **Compliant Exception Halts** | 43 | Cases (31.85%) | Deliberately blocked by deterministic stopping rules |
| **In-Flight / Active Cases** | 31 | Cases (22.96%) | In promise grace periods, off-peak queues, or showcase |
| **Razorpay Platform MDR (2%)** | 76,504.70 | INR (7,650,470 paise) | Standard domestic payment gateway processing fee |
| **GST on Gateway MDR (18%)** | 13,770.85 | INR (1,377,085 paise) | Statutory Goods and Services Tax on platform fees |
| **Total Gateway Deductions** | 90,275.55 | INR (9,027,555 paise) | Effective 2.36% deduction on gross recovered volume |
| **Net Merchant Settlement Payout**| 37,34,961.01 | INR (373,496,101 paise) | Actual net funds deposited into merchant account |
| **MSMED Compound Interest Accrued**| 19,015.89 | INR (1,901,589 paise) | Statutory penal interest computed at 16.50% p.a. |

> [!NOTE]
> **Theoretical Upper Bound (Ideal Pipeline Resolution):**  
> In a theoretical best-case scenario where 100% of the 31 in-flight cases convert and settle successfully:
> * **Settled Cases:** **92 cases** out of 135 (**68.15% Case Recovery Rate**, combining 61 current collections + 31 in-flight conversions).
> * **Gross Recovered Volume:** **47,30,314.36 INR** (38,25,236.56 INR current + 9,05,077.80 INR in-flight).
> * **Theoretical Value Yield:** **74.33%** of total at-risk capital. The remaining 25.67% (16,34,036.06 INR) comprises 11,89,883.66 INR across the 43 permanent compliance halts (hard declines, active commercial disputes, sub-200 INR margin filters, anti-spam expired nudges, and uncollectible bad debts) plus 4,44,152.40 INR uncollected balance from the 5 partial installment arrangements.
> 
> *Why 100% in-flight conversion is an ideal upper bound and unlikely in real-world operations:*
> 1. **Broken Promise Attrition (Module B):** Debtors who have already defaulted on an initial payment commitment face ongoing liquidity distress, meaning a portion of accounts continue to default across subsequent escalation rungs until formal arbitration.
> 2. **Manual Checkout Friction (Module A):** While recurring AutoPay debits execute passively in the background, alternative payment links (issued for lapsed wallet KYC or ineligible EMI) require active customer intervention (opening the message and authenticating via UPI PIN or OTP), where drop-off naturally occurs.
> 3. **Salary Window Depletion (Module A):** Delayed retries scheduled for salary cycles may still fail if competing automated debits (such as home loan EMIs or insurance debits) clear earlier and deplete the available account balance.
> 4. **Cart Abandonment Intent Decay (Module C):** E-commerce checkout drop-offs frequently reflect deliberate purchasing decisions (such as comparison shopping or sudden price sensitivity) rather than transient technical friction, so reminder nudges will not convert every abandoned session.

---

## 2. Module-by-Module Breakdown

| Module | Business Domain | Cases | Revenue at Risk (INR) | Gross Recovered (INR) | Net Yield | Exceptions (Halts) | Key Operational Interventions |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Module A** | Subscriptions & Mandates | 60 | 1,76,738.85 | 49,301.12 | 27.89% | 18 | Rescheduled retries to off-peak hours (Rule 2); alternate links for halted subscriptions (Rule 4); hard decline halts (Rule 1 & 3). |
| **Module B** | B2B Trade Receivables | 50 | 61,38,939.70 | 37,54,132.58 | 61.15% | 11 | MSMED Act statutory escalation (Rungs 1 to 4); interest computation at 3x Bank Rate; dispute halts (Rule 6); MSEFC tribunal sign-off gates (Rule 10). |
| **Module C** | Checkout Drop-Offs | 25 | 48,671.87 | 21,802.86 | 44.80% | 14 | Low-value floor filtering (Rule 12); single omnichannel nudge cap (Rule 11); personalized audio voice notes. |
| **Total** | **All 3 Rails** | **135** | **63,64,350.42** | **38,25,236.56** | **60.10%** | **43** | **100% verified via signed Razorpay webhook payloads.** |

*(Note: Module B recovered volume includes 33,09,980.17 INR from 26 fully paid invoices plus 4,44,152.41 INR collected across 5 partial installment payments).*

## 3. Revenue Captured: Audit of the 61 Recovered Cases

Revenue in ReClaim is recorded strictly upon receiving an HMAC-SHA256 verified Razorpay webhook. Across the 61 cases with captured funds (56 fully settled and 5 partial collections), recovery was achieved through rail-specific interventions:

| Cohort / Archetype | Module | Cases | Recovered (INR) | Operational Failure Mode | Recovery Mechanism Executed |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **NPCI Window Rescheduled Debits** | Module A | 9 | 20,800.02 | UPI AutoPay debit attempted during 10:00-13:00 IST peak congestion | Rule 2 intercepted debit, rescheduled to 13:00:01 IST off-peak window; cleared without customer bounce fee. |
| **Transient Switch Glitch Retries**| Module A | 8 | 21,491.60 | Issuer bank session timeout / switch temporarily inoperative | Silent retry executed during off-peak hours after switch availability confirmed. |
| **Halted Subscription Alternative Links** | Module A | 4 | 7,009.50 | Recurring card debit failed repeatedly; gateway halted mandate | Rule 4 triggered dynamic Razorpay payment link for unpaid invoice directly to subscriber; recovered without churn. |
| **First-Notice Statutory Settlements**| Module B | 13 | 16,62,137.59 | Overdue commercial invoice past Section 15 statutory timeline | Rung 1 formal notice dispatched citing credit period expiration; settled in full via B2B payment link. |
| **Fulfilled Debtor Promises** | Module B | 7 | 8,56,545.07 | Debtor provided explicit payment commitment date | Groq parsed promised settlement date; automated reminders paused; debtor settled within committed window. |
| **Partial Installment Collections** | Module B | 5 | 4,44,152.41 | Debtor facing cashflow strain negotiated installment clearance | Dynamic split payment link issued; partial funds captured and remaining balance re-anchored on escalation ladder. |
| **Broken Promise Escalation Recoveries**| Module B | 2 | 3,02,198.65 | Debtor defaulted on initial commitment | Escalated to Rung 2 with formal statement reflecting Section 16 compound penal interest; prompted immediate settlement. |
| **Pre-Tribunal Escalation Recoveries**| Module B | 2 | 3,42,685.58 | Debtor unresponsive to informal reminders | Advanced to Rung 3 formal legal notice warning of MSEFC tribunal filing; debtor settled before formal submission. |
| **Prompt Baseline Payers** | Module B | 2 | 1,46,413.28 | Trade invoices settled during standard billing cycle | Baseline control group paid via standard invoice link. |
| **High-Intent Cart Conversions** | Module C | 9 | 21,802.86 | Abandoned high-margin e-commerce shopping cart | Single omnichannel recovery nudge dispatched with dynamic payment link and personalized Hinglish voice note. |
| **Total Captured Volume** | **All 3 Rails** | **61** | **38,25,236.56** | **Captured revenue** | **100% verified by signed Razorpay webhook payloads.** |

---

## 4. Bounded Autonomy: Audit of the 43 Intentional Exception Halts

In high-stakes revenue recovery, measuring what the agent **deliberately stops or refuses to do** is just as critical as measuring what it collects. ReClaim's deterministic policy gate enforced 43 hard stops across the batch:

| Stopping Rule | Category | Cases Halted | Trigger Condition | System Action & Compliance Rationale |
| :--- | :--- | :--- | :--- | :--- |
| **Rules 1 & 3** | Hard Declines | 15 | Expired cards (code 54), stolen cards, persistent zero balances | Permanent stop. Refused to retry to prevent customer bank bounce fees and gateway penalties. |
| **Rule 6** | Commercial Dispute | 5 | Buyer replied contesting product delivery, damaged items, or GST mismatch | Outreach frozen immediately. Case tagged for commercial arbitration to protect customer goodwill. |
| **Rule 11** | Anti-Spam Frequency Cap | 9 | Customer received initial checkout nudge but did not complete checkout | Order marked expired without further contact. Prevents aggressive customer spam. |
| **Rule 12** | Unit Margin Floor | 5 | Abandoned cart total was below the 200 INR threshold | Nudge skipped. Outbound recovery costs (SMS/WhatsApp/Voice) would erode cart margin. |
| **Rule 10** | Legal Filing Gate | 2 | Invoice reached Rung 4 after debtor defaulted on multiple commitments | System drafted MSME Samadhaan legal tribunal filing packet, then locked action for mandatory human sign-off. |
| **Rule 5** | Bank Sync Discrepancy | 3 | Netbanking debit completed at issuer switch but gateway reconciliation pending | Automated retries blocked. Escalated to human operations to prevent double-debiting customer. |
| **Statutory Limit** | Bad Debt Write-Off | 4 | Debtor ghosted through all rungs and statutory limitation expired | Transferred to legal accounting as unrecovered bad debt. |
| **Total** | **Compliance Halts** | **43** | | **Every veto logged with SHA-256 hash in PostgreSQL audit ledger.** |

---

## 5. In-Flight Pipeline: Active Cases Pending Resolution 

Revenue recovery does not always resolve instantaneously. A production recovery engine operates as a state machine managing transactions across their natural lifecycle: waiting for customer payroll cycles, debtor promised settlement dates, or customer checkout via alternative payment links. 

The remaining 31 cases represent active pipeline capital currently progressing through bounded workflows:

| Cohort | Module | Cases | Value at Risk (INR) | Operational Stage | Expected Recovery Mechanism |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Delayed Salary Retries** | Module A | 9 | 30,319.55 | Low balance on healthy tenure account; scheduled post-salary credit | AutoPay debit will execute automatically after customer's verified payroll date to avoid bank ECS bounce fees. |
| **Alternative Link Queues** | Module A | 11 | 40,486.37 | Wallet KYC lapsed (6 cases) or card EMI ineligible (5 cases) | Naive auto-debit is blocked; customer received dynamic Razorpay UPI/Card checkout link to complete payment manually. |
| **Showcase Glitch Retry** | Module A | 1 | 3,499.00 | Transient network glitch retried during live showcase execution | Awaiting asynchronous webhook confirmation from issuer switch. |
| **Debtor Promise Grace Period**| Module B | 1 | 1,25,000.00 | Groq extracted verified payment commitment date from buyer reply | Statutory escalation paused under promise tracking; awaiting debtor settlement by promised date. |
| **Broken Promise Escalation** | Module B | 6 | 6,17,073.88 | Debtor broke initial payment promise; actively advancing rungs | Progressing through Rung 2 and Rung 3 (formal notice with compound statutory interest) before Rule 7 hard cap. |
| **Pre-Due Baseline Control** | Module B | 1 | 85,000.00 | Control case before statutory due date under Section 15 | Scheduled for normal commercial payment cycle without penalty interest. |
| **High-Intent Cart Windows** | Module C | 2 | 3,699.00 | High-value abandoned carts with single nudge dispatched under Rule 11 | Within active 24-hour checkout window; personalized payment link and Hinglish audio note live. |
| **Total In-Flight Pipeline** | **All 3 Rails** | **31** | **9,05,077.80** | **Active lifecycle pipeline** | **Represents addressable upside beyond the initial 60.10% settled volume.** |

---

## 6. Financial Settlement Reconciliation (MDR + GST)

Recovery in ReClaim is strictly measured on true merchant cashflow, reflecting domestic platform fees and statutory tax deductions:

```
Gross Recovered Amount:          38,25,236.56 INR  (100.00%)
- Razorpay Platform Fee (2%):       76,504.70 INR  (  2.00%)
- GST on Platform Fee (18%):        13,770.85 INR  (  0.36%)
------------------------------------------------------------
Total Payment Infrastructure Cost:   90,275.55 INR  (  2.36%)
Net Merchant Settlement Payout:   37,34,961.01 INR  ( 97.64%)
```

---

## 7. Case Resolution Distribution

```
┌─────────────────────────────────────────────────────────────┐
│                 135 Total Recovery Cases                     │
├──────────────────────────────┬──────────────────────────────┤
│ Money Captured: 61 Cases     │ Policy Exceptions: 43 Cases  │
│ - Fully Settled: 56 cases    │ - Hard Declines: 15 cases    │
│ - Partially Paid: 5 cases    │ - Expired Anti-Spam: 9 cases │
│                              │ - Low-Value Floor: 5 cases   │
│ In-Flight / Queued: 31 Cases │ - Commercial Disputes: 5 cs  │
│ - Delayed Salary Retries: 9  │ - Written Off: 4 cases       │
│ - Alternative Link Queues: 11│ - Bank Sync Escalated: 3 cs  │
│ - Broken Promise Rungs: 6    │ - Samadhaan Gated: 2 cases   │
│ - Promise Grace Period: 1    │                              │
│ - Showcase Retries / Pre: 4  │                              │
└──────────────────────────────┴──────────────────────────────┘
```

---

## 8. Real-Time Telemetry and Verification

Every single recovery event recorded above:
1. Was initiated following root-cause classification into one of the 27 closed taxonomy categories.
2. Passed the 13 deterministic compliance stopping rules without human intervention (or halted safely at Rule 10).
3. Generated a cryptographically signed Razorpay webhook payload verified with HMAC-SHA256 over raw request bytes.
4. Was written to the append-only `AuditLogEntry` ledger in PostgreSQL and broadcast to connected frontend dashboards over WebSockets (`/ws/audit`).
