# 📈 Trade Life Cycle — Master Reference Guide

> **Goal:** Understand every step a financial trade goes through, from the moment a trader decides to buy/sell, to the final settlement and record keeping. Written in plain English with analogies, diagrams, and tables.

---

## 📌 Table of Contents

1. [What is a Trade?](#1-what-is-a-trade)
2. [Who Are the Players?](#2-who-are-the-players)
3. [The Big Picture — Trade Life Cycle Overview](#3-the-big-picture--trade-life-cycle-overview)
4. [Phase 1 — Pre-Trade](#4-phase-1--pre-trade)
5. [Phase 2 — Trade Execution](#5-phase-2--trade-execution)
6. [Phase 3 — Trade Capture & Validation](#6-phase-3--trade-capture--validation)
7. [Phase 4 — Clearing](#7-phase-4--clearing)
8. [Phase 5 — Settlement](#8-phase-5--settlement)
9. [Phase 6 — Post-Settlement (Reconciliation & Reporting)](#9-phase-6--post-settlement-reconciliation--reporting)
10. [Key Dates in a Trade](#10-key-dates-in-a-trade)
11. [Common Asset Classes & Their Differences](#11-common-asset-classes--their-differences)
12. [Failures & Exceptions](#12-failures--exceptions)
13. [Key Terms Glossary](#13-key-terms-glossary)
14. [Interview Cheat Sheet](#14-interview-cheat-sheet)

---

## 1. What is a Trade?

A **trade** is simply an **agreement between two parties to exchange something of value** — usually a financial instrument (stocks, bonds, derivatives) in exchange for money.

> 🧠 **Analogy:** Think of buying a laptop on Amazon.
> - You click "Buy" → that's the **trade execution**
> - Amazon confirms your order → **trade confirmation**
> - The warehouse packs & ships it → **clearing**
> - You receive the laptop & Amazon gets your money → **settlement**

The same steps happen in financial markets — just faster, and with millions of dollars.

---

## 2. Who Are the Players?

```
┌─────────────────────────────────────────────────────────────────────┐
│                        TRADE ECOSYSTEM                              │
│                                                                     │
│  ┌──────────┐      ┌──────────┐      ┌──────────┐                  │
│  │  Buy Side│      │  Broker/ │      │ Sell Side│                  │
│  │(Investor)│◄────►│  Dealer  │◄────►│  (Bank)  │                  │
│  └──────────┘      └──────────┘      └──────────┘                  │
│        │                │                  │                        │
│        └────────────────┼──────────────────┘                       │
│                         ▼                                           │
│                  ┌─────────────┐                                    │
│                  │   Exchange  │  (e.g., NSE, NYSE, LSE)            │
│                  │    /OTC     │                                     │
│                  └─────────────┘                                    │
│                         │                                           │
│                         ▼                                           │
│                  ┌─────────────┐                                    │
│                  │  Clearing   │  (e.g., NSCCL, DTCC, LCH)         │
│                  │   House     │                                     │
│                  └─────────────┘                                    │
│                         │                                           │
│                         ▼                                           │
│                  ┌─────────────┐                                    │
│                  │  Custodian/ │  (holds securities & cash)         │
│                  │  Depository │  (e.g., NSDL, CDSL, DTC)          │
│                  └─────────────┘                                    │
└─────────────────────────────────────────────────────────────────────┘
```

| Player | Role | Example |
|---|---|---|
| **Buy Side** | Investors who buy financial instruments | Mutual funds, hedge funds, pension funds |
| **Sell Side** | Banks/brokers who sell/execute trades | Goldman Sachs, JP Morgan, Zerodha |
| **Broker/Dealer** | Intermediary who connects buyer & seller | ICICI Securities, Fidelity |
| **Exchange** | Marketplace where trades happen | NSE, BSE, NYSE, LSE |
| **Clearing House (CCP)** | Guarantees the trade won't fail | NSCCL, DTCC, LCH |
| **Custodian** | Holds your securities safely | HDFC Custodial, State Street |
| **Depository** | Electronic record of who owns what | NSDL, CDSL, DTC |
| **Regulator** | Ensures rules are followed | SEBI (India), SEC (US), FCA (UK) |

---

## 3. The Big Picture — Trade Life Cycle Overview

```
╔═══════════════════════════════════════════════════════════════════╗
║                    TRADE LIFE CYCLE                               ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                   ║
║  ┌──────────┐                                                     ║
║  │PRE-TRADE │  → Research, compliance checks, order creation      ║
║  └────┬─────┘                                                     ║
║       │                                                           ║
║       ▼                                                           ║
║  ┌──────────┐                                                     ║
║  │EXECUTION │  → Order placed on exchange, matched with counter   ║
║  └────┬─────┘                                                     ║
║       │                                                           ║
║       ▼                                                           ║
║  ┌──────────────────┐                                             ║
║  │TRADE CAPTURE &   │  → Trade details recorded, validated,       ║
║  │CONFIRMATION      │    confirmed by both parties                 ║
║  └────┬─────────────┘                                             ║
║       │                                                           ║
║       ▼                                                           ║
║  ┌──────────┐                                                     ║
║  │CLEARING  │  → CCP steps in, novation, margin calls             ║
║  └────┬─────┘                                                     ║
║       │                                                           ║
║       ▼                                                           ║
║  ┌──────────┐                                                     ║
║  │SETTLEMENT│  → Actual exchange of securities & cash             ║
║  └────┬─────┘                                                     ║
║       │                                                           ║
║       ▼                                                           ║
║  ┌──────────────────┐                                             ║
║  │POST-SETTLEMENT   │  → Reconciliation, reporting, accounting    ║
║  └──────────────────┘                                             ║
╚═══════════════════════════════════════════════════════════════════╝
```

---

## 4. Phase 1 — Pre-Trade

> **"Before you place any order, a lot of work happens behind the scenes."**

### 4.1 What Happens Here?

```
┌─────────────────────────────────────────────────────┐
│                 PRE-TRADE PHASE                     │
│                                                     │
│  1. Market Research & Analysis                      │
│     └─ Analyst says: "Buy Infosys, target ₹2000"   │
│                                                     │
│  2. Investment Decision                             │
│     └─ Fund Manager approves: "Buy 10,000 shares"  │
│                                                     │
│  3. Compliance / Pre-Trade Checks                   │
│     ├─ Sufficient funds/credit?                     │
│     ├─ Is this trade within risk limits?            │
│     ├─ Any regulatory restrictions?                 │
│     └─ Is the security on restricted list?          │
│                                                     │
│  4. Order Creation                                  │
│     └─ "Buy 10,000 INFY @ ₹1,950 Limit Order"      │
└─────────────────────────────────────────────────────┘
```

### 4.2 Types of Orders

| Order Type | Meaning | Example |
|---|---|---|
| **Market Order** | Buy/Sell immediately at current price | "Buy now, whatever the price" |
| **Limit Order** | Buy/Sell only at a specific price or better | "Buy only if price ≤ ₹1,950" |
| **Stop Order** | Trigger market order when price hits a level | "Sell if price drops to ₹1,800" |
| **IOC** (Immediate or Cancel) | Execute immediately or cancel | Used for large block trades |
| **GTC** (Good Till Cancelled) | Stay active until manually cancelled | Long-term investor orders |

### 4.3 Pre-Trade Checks (Compliance)

```
Order Request
     │
     ▼
┌────────────────────────────────┐
│       PRE-TRADE CHECKS         │
│                                │
│  ✅ Fund availability          │
│  ✅ Credit limits              │
│  ✅ Position limits            │
│  ✅ Restricted securities list │
│  ✅ KYC / AML rules            │
│  ✅ Regulatory requirements    │
└────────────────────────────────┘
     │
     ▼
   PASS → Send to Execution
   FAIL → Reject & Alert Trader
```

---

## 5. Phase 2 — Trade Execution

> **"The order goes to the market. A match is found. Trade is done."**

### 5.1 How Execution Works

```
TRADER                    EXCHANGE / OTC MARKET
  │                              │
  │──── Places Order ───────────►│
  │                              │
  │                    ┌─────────────────┐
  │                    │  Order Book     │
  │                    │  BUY   │  SELL  │
  │                    │ 1,950  │ 1,952  │
  │                    │ 1,948  │ 1,955  │
  │                    │ 1,945  │ 1,960  │
  │                    └─────────────────┘
  │                              │
  │                    Matching Engine finds
  │                    a Seller at ₹1,950
  │                              │
  │◄──── Trade Executed ─────────│
  │      (Trade Confirmation)    │
```

### 5.2 Execution Venues

| Venue | Description | Examples |
|---|---|---|
| **Exchange** | Centralized, regulated, transparent | NSE, BSE, NYSE |
| **OTC (Over-the-Counter)** | Direct between two parties, no exchange | Bonds, FX, Derivatives |
| **Dark Pool** | Private exchange, no public order book | Used by large funds to avoid market impact |
| **MTF** | Multilateral Trading Facility (EU) | BATS Europe, Turquoise |

### 5.3 What is a Trade Confirmation?

After execution, both parties receive a **trade confirmation** with:

```
╔══════════════════════════════════════════════╗
║           TRADE CONFIRMATION                 ║
╠══════════════════════════════════════════════╣
║  Trade ID      : TRD-20240523-00142          ║
║  Security      : INFY (Infosys Ltd.)         ║
║  ISIN          : INE009A01021                ║
║  Action        : BUY                         ║
║  Quantity      : 10,000 shares               ║
║  Price         : ₹1,950.00                  ║
║  Trade Value   : ₹1,95,00,000               ║
║  Trade Date    : 23-May-2024 (T)             ║
║  Settlement    : 25-May-2024 (T+2)           ║
║  Counterparty  : Broker XYZ                  ║
╚══════════════════════════════════════════════╝
```

---

## 6. Phase 3 — Trade Capture & Validation

> **"The trade is recorded in the system and both sides agree on the details."**

### 6.1 Trade Capture

Once executed, the trade details flow into:

```
Exchange/OTC
     │
     ▼
┌──────────────────────────────────┐
│    TRADE CAPTURE SYSTEMS         │
│                                  │
│  OMS (Order Management System)   │
│  EMS (Execution Management Sys)  │
│  TMS (Trade Management System)   │
│                                  │
│  Records: Price, Qty, Security,  │
│  Counterparty, Time, Trader ID   │
└──────────────────────────────────┘
     │
     ▼
┌──────────────────────────────────┐
│         VALIDATION               │
│                                  │
│  ✅ Trade details match?         │
│  ✅ ISIN valid?                  │
│  ✅ Counterparty known?          │
│  ✅ Settlement date correct?     │
│  ✅ No duplicates?               │
└──────────────────────────────────┘
```

### 6.2 Trade Affirmation vs Confirmation

| Term | Who Does It | When | Purpose |
|---|---|---|---|
| **Confirmation** | Broker sends to client | Same day (T+0) | Tells the investor what was executed |
| **Affirmation** | Client affirms back | T+0 or T+1 | Client agrees to the trade details |
| **Allocation** | Buy-side splits trade | T+0 or T+1 | Large fund splits trade across accounts |

> 🧠 **Analogy:** You ordered a pizza (execution). The shop calls to confirm your address (confirmation). You say "Yes, correct" (affirmation). The pizza is split into boxes for different people at your table (allocation).

---

## 7. Phase 4 — Clearing

> **"A middleman (CCP) steps in between buyer and seller to guarantee the trade."**

### 7.1 What is Clearing?

**Clearing** is the process of reconciling and guaranteeing a trade **before** actual settlement happens.

```
WITHOUT CLEARING (risky):
  Buyer ◄──────────────────────────────► Seller
  (If seller defaults, buyer loses!)

WITH CLEARING (safe):
  Buyer ◄──────► CCP ◄──────────────► Seller
  (CCP guarantees both sides)
         Clearing House
         becomes the
         "Buyer to every Seller"
         "Seller to every Buyer"
```

This process of CCP replacing the original counterparty is called **NOVATION**.

### 7.2 Netting

Instead of settling every trade individually, clearing houses **net** positions:

```
Example: You traded 5 times today in Infosys

  Trade 1: BUY  1,000 shares
  Trade 2: SELL   500 shares
  Trade 3: BUY    300 shares
  Trade 4: SELL   200 shares
  Trade 5: BUY    100 shares

WITHOUT NETTING: 5 separate settlements
WITH NETTING:    NET = Buy 700 shares (just 1 settlement!)

SAVES: 4 transactions worth of cost & risk
```

### 7.3 Margin

To protect against default risk, the CCP collects **margin** from both parties:

| Margin Type | Purpose | When Collected |
|---|---|---|
| **Initial Margin** | Buffer for potential future losses | When trade is first opened |
| **Variation Margin** | Daily mark-to-market losses | Daily (if position moves against you) |
| **Maintenance Margin** | Minimum level to keep position open | Ongoing |

> 🧠 **Analogy:** When you rent a flat, you pay a **security deposit** (initial margin). If you damage something, the landlord takes from the deposit (variation margin).

### 7.4 Clearing Flow

```
Trade Executed
     │
     ▼
CCP Receives Trade Details
     │
     ▼
┌────────────────────────────┐
│    CLEARING PROCESS        │
│                            │
│  1. Trade Matching         │
│  2. Novation               │
│  3. Netting                │
│  4. Margin Calculation     │
│  5. Margin Collection      │
└────────────────────────────┘
     │
     ▼
Ready for Settlement
```

---

## 8. Phase 5 — Settlement

> **"The actual exchange happens — securities go to buyer, cash goes to seller."**

### 8.1 What is Settlement?

**Settlement** is the FINAL step where:
- 🏦 **Cash moves** from Buyer's bank → Seller's bank
- 📜 **Securities move** from Seller's account → Buyer's account

```
BEFORE SETTLEMENT:
  Buyer  has: Cash ₹1.95Cr  | 0 shares of INFY
  Seller has: 10,000 INFY   | ₹0

AFTER SETTLEMENT:
  Buyer  has: ₹0             | 10,000 shares of INFY ✅
  Seller has: 0 INFY shares  | ₹1.95Cr ✅

  "Delivery vs Payment (DVP)" — both happen simultaneously!
```

### 8.2 DVP — Delivery vs Payment

DVP ensures **neither party loses**:

```
  ┌─────────────────────────────────────────┐
  │              DVP MODEL                  │
  │                                         │
  │  Securities  ─────────────────────────► │
  │  (Seller → Buyer's Depository Account)  │
  │                                         │
  │  Cash  ◄───────────────────────────── │
  │  (Buyer → Seller's Bank Account)        │
  │                                         │
  │  BOTH happen SIMULTANEOUSLY             │
  │  (No risk of one side not delivering)   │
  └─────────────────────────────────────────┘
```

### 8.3 Settlement Cycles

Different countries and asset classes settle at different speeds:

| Asset Class | Settlement Cycle | Example |
|---|---|---|
| **Indian Equities (NSE/BSE)** | T+1 | Trade Monday, settle Tuesday |
| **US Equities** | T+1 (from May 2024) | Trade Monday, settle Tuesday |
| **European Equities** | T+2 | Trade Monday, settle Wednesday |
| **Government Bonds** | T+1 | Next business day |
| **FX Spot** | T+2 | Two business days |
| **Money Market** | T+0 or T+1 | Same/next day |

> 🔑 **T = Trade Date.** T+1 means 1 business day after trade date.

### 8.4 Settlement Flow

```
Settlement Date Arrives (T+1 / T+2)
          │
          ▼
   ┌──────────────┐
   │  CUSTODIAN   │  ← Buyer's custodian confirms cash available
   └──────┬───────┘
          │
          ▼
   ┌──────────────┐
   │  DEPOSITORY  │  ← Seller's account debited, Buyer's account credited
   │  (NSDL/CDSL) │     (Securities move electronically)
   └──────┬───────┘
          │
          ▼
   ┌──────────────┐
   │  CLEARING    │  ← Cash settlement confirmed
   │  BANK        │     (Money moves between banks)
   └──────────────┘
          │
          ▼
   SETTLEMENT COMPLETE ✅
```

---

## 9. Phase 6 — Post-Settlement (Reconciliation & Reporting)

> **"Making sure the books match and everything is reported correctly."**

### 9.1 Reconciliation

After settlement, back-office teams reconcile (compare & match):

```
OUR INTERNAL RECORDS
      vs.
BROKER STATEMENTS
      vs.
CUSTODIAN STATEMENTS
      vs.
DEPOSITORY RECORDS

All 4 must MATCH. Any difference = a "break" that must be investigated.
```

### 9.2 Types of Reconciliation

| Type | What is Compared | Frequency |
|---|---|---|
| **Position Recon** | Our holdings vs Custodian/Depository | Daily |
| **Cash Recon** | Our cash balance vs Bank statement | Daily |
| **Trade Recon** | Our trade records vs Broker confirms | Daily |
| **P&L Recon** | System P&L vs Risk/Finance systems | Daily |

### 9.3 Regulatory Reporting

After trades settle, firms must report to regulators:

| Regulation | Region | What Must Be Reported |
|---|---|---|
| **MiFID II** | Europe | Trade details, execution quality |
| **EMIR** | Europe | Derivatives trades to trade repositories |
| **Dodd-Frank** | USA | OTC derivatives, swaps |
| **SEBI Reporting** | India | Institutional trades, FII data |

### 9.4 Accounting (Books & Records)

```
After Settlement:
  ✅ Update portfolio management system
  ✅ Book P&L (Profit and Loss)
  ✅ Update NAV (Net Asset Value) for funds
  ✅ Record accruals (dividends, interest)
  ✅ Tax reporting
```

---

## 10. Key Dates in a Trade

```
──────────────────────────────────────────────────────────────────►  Time

  T                T+1               T+2              T+3
  │                │                 │                │
  │ Trade Date     │ Confirmation    │ Settlement      │ Custody
  │ Order Placed   │ Deadline        │ Date            │ Reports
  │ Trade Executed │ Allocation      │ Cash & Secs     │ Recon
  │ Trade Captured │ Affirmation     │ Exchange        │ Reporting
```

| Date | Name | Description |
|---|---|---|
| **T (Trade Date)** | Execution Date | Date the trade is agreed and executed |
| **T+1** | Confirmation Date | Both parties confirm trade details |
| **T+1 or T+2** | Settlement Date | Actual cash & securities exchange |
| **Ex-Dividend Date** | Ex-Date | Buyer on/after this date doesn't get dividend |
| **Record Date** | Book Close Date | Snapshot of who owns the security |
| **Value Date** | Same as Settlement | Effective date of cash movement |

---

## 11. Common Asset Classes & Their Differences

### 11.1 Equities (Stocks)

```
Traded on:     Exchange (NSE, NYSE)
Settlement:    T+1
Cleared by:    NSCCL, DTCC
Held at:       NSDL, CDSL, DTC
Corporate Actions: Dividends, Splits, Bonuses
```

### 11.2 Fixed Income (Bonds)

```
Traded on:     OTC mostly (some exchange-listed)
Settlement:    T+1 (Government), T+2 (Corporate)
Cleared by:    CCIL (India), LCH, DTCC
Features:      Coupon payments, Maturity, Yield
Accrued Interest: Must be calculated on settlement
```

### 11.3 Derivatives

| Type | Description | Settlement |
|---|---|---|
| **Futures** | Agreement to buy/sell at future date | Daily MTM + Final Settlement |
| **Options** | Right (not obligation) to buy/sell | Physical or Cash settlement |
| **Swaps** | Exchange of cashflows (Interest Rate, FX) | Periodic cash payments |
| **Forwards** | Like futures but OTC, customized | On maturity date |

### 11.4 Foreign Exchange (FX)

```
FX Spot:    Traded now, settled T+2
FX Forward: Agreed now, settled in future
FX Swap:    Buy spot + Sell forward (or vice versa)
Settlement: Payment netting via CLS Bank (global FX settlement)
```

### 11.5 Asset Class Comparison Table

| Feature | Equities | Bonds | Futures | FX Spot |
|---|---|---|---|---|
| **Exchange Traded?** | Yes | Sometimes | Yes | No (OTC) |
| **Settlement** | T+1 | T+1/T+2 | Daily MTM | T+2 |
| **Central Clearing?** | Yes | Yes | Yes | Via CLS |
| **Margin Required?** | Partial | No | Yes | No |
| **Accrued Interest?** | No | Yes | No | No |

---

## 12. Failures & Exceptions

### 12.1 What is a Settlement Failure?

When a trade **does not settle on time**, it's called a **fail** or **settlement failure**.

**Causes:**
- Seller doesn't have the securities
- Buyer doesn't have enough cash
- Operational error (wrong account, ISIN mismatch)
- Corporate action disruption

### 12.2 Consequences of Fails

```
FAIL SCENARIO
     │
     ├── Penalties charged by exchange/CCP
     │
     ├── Buy-in process (exchange forces purchase on seller's behalf)
     │
     ├── Regulatory fines (EU: CSDR; US: Regulation SHO)
     │
     └── Reputational damage
```

### 12.3 Common Exceptions & Breaks

| Exception | Cause | Resolution |
|---|---|---|
| **Unmatched Trade** | Buyer & Seller records differ | Investigate + correct details |
| **Cash Break** | Cash account doesn't balance | Trace missing transaction |
| **Position Break** | Holdings don't match custodian | Reconcile, check corporate actions |
| **Failed Settlement** | Securities/cash not available | Chase, buy-in, or roll forward |
| **Duplicate Trade** | Same trade booked twice | Cancel duplicate |
| **Incorrect ISIN** | Wrong security code | Amend and re-submit |

---

## 13. Key Terms Glossary

| Term | Simple Meaning |
|---|---|
| **Counterparty** | The other party in your trade |
| **ISIN** | Unique code for a security (like Aadhaar for stocks) |
| **Novation** | CCP replaces original counterparty |
| **Netting** | Combine multiple trades into one net position |
| **DVP** | Delivery vs Payment — simultaneous exchange |
| **MTM** | Mark-to-Market — value at current market price |
| **Margin** | Security deposit held by CCP |
| **Custodian** | Bank that holds your securities on your behalf |
| **Depository** | Electronic record of security ownership |
| **Affirmation** | Client confirms they agree to trade details |
| **Allocation** | Fund splits trade across multiple portfolios |
| **Fail** | Trade that doesn't settle on expected date |
| **STP** | Straight-Through Processing — fully automated, no manual touch |
| **CCP** | Central Counterparty Clearing House |
| **FOP** | Free of Payment — securities move without cash (used in transfers) |
| **Repo** | Repurchase agreement — borrow cash using securities as collateral |
| **Corporate Action** | Event by company affecting securities (dividend, split, rights issue) |

---

## 14. Interview Cheat Sheet

### 🔑 Must-Know Concepts

```
1. Trade Life Cycle Phases:
   Pre-Trade → Execution → Capture/Confirmation → Clearing → Settlement → Post-Settlement

2. Settlement = actual exchange of Cash + Securities
   Clearing   = netting + risk management BEFORE settlement

3. DVP = Delivery vs Payment (buyer gets securities ONLY when seller gets cash)

4. Novation = CCP becomes buyer to every seller, seller to every buyer

5. Netting reduces the number of settlements needed

6. T+1 = India & US equities settlement (as of recent changes)
   T+2 = European equities, FX Spot

7. Initial Margin = upfront deposit
   Variation Margin = daily P&L settlement

8. Reconciliation = matching our books vs external statements

9. STP (Straight Through Processing) = no manual intervention end-to-end

10. CCP examples: NSCCL (India), DTCC (US), LCH (UK/EU)
    Depository examples: NSDL/CDSL (India), DTC (US), Euroclear (EU)
```

### 🧠 Quick Analogy Map

| Finance Term | Real Life Analogy |
|---|---|
| Trade Execution | Clicking "Buy Now" on Amazon |
| Trade Confirmation | Order confirmation email |
| Clearing | Warehouse preparing your package |
| Settlement | Package delivered + payment done |
| CCP | Amazon as guarantor between buyer & 3rd-party seller |
| Margin | Security deposit for renting a flat |
| Netting | Splitting the bill at dinner (net what each person owes) |
| Depository | Land registry (records who owns what property) |
| Custodian | Bank locker (holds valuables safely) |
| Reconciliation | Matching your receipt with your bank statement |

---

> 📘 **Pro Tip for Interviews:** Always think in terms of **risk** at each phase:
> - Pre-Trade: Market risk, compliance risk
> - Execution: Slippage, market impact
> - Clearing: Counterparty risk (solved by CCP)
> - Settlement: Settlement risk (solved by DVP)
> - Post-Settlement: Operational risk (solved by recon)
>
> Understanding **why** each phase exists (what risk it mitigates) shows true mastery.

---

*Last Updated: May 2026 | Reference: Capital Markets Domain*
