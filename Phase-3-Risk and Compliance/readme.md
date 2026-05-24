# 🛡️ Phase 3 — Risk & Compliance
### Trade Life Cycle Series | Business Knowledge for Engineers

---

## 🗺️ What You'll Learn in Phase 3

```
┌──────────────────────────────────────────────────────────────────┐
│                     RISK & COMPLIANCE                            │
│                                                                  │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────────────┐  │
│  │    RISK     │   │  COLLATERAL  │   │    REGULATIONS       │  │
│  │ Management  │   │  & MARGIN    │   │  US (Dodd-Frank)     │  │
│  │             │   │              │   │  UK (MiFID II/FCA)   │  │
│  │ • Market    │   │ • Initial    │   │  • Trade Reporting   │  │
│  │ • Credit    │   │ • Variation  │   │  • EMIR              │  │
│  │ • Liquidity │   │ • Margin Call│   │  • Best Execution    │  │
│  │ • Ops Risk  │   │              │   │                      │  │
│  └─────────────┘   └──────────────┘   └──────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

> 💡 **Why does this matter for engineers?**
> Risk & compliance rules directly drive system design — what checks run before a trade,
> what data must be stored, what reports must be sent and when.
> Every rule = a system requirement somewhere.

---

## ⚠️ Section 1 — Risk Management

**Risk** in finance = possibility of losing money or something going wrong.

There are several **types of risk** that banks and funds must manage every day.

---

### 1.1 Market Risk

> **The risk that the price of your investment moves against you.**

```
You bought Apple stock at $180.
Apple announces bad earnings.
Stock drops to $140.
Loss = $40 per share × 10,000 shares = $400,000

That is Market Risk.
```

#### Types of Market Risk:

| Type | What It Is | Example |
|------|-----------|---------|
| **Equity Risk** | Stock prices fall | Apple drops 20% |
| **Interest Rate Risk** | Bond prices fall when rates rise | Fed raises rates → your bonds lose value |
| **FX Risk** | Currency moves against you | USD weakens, your US profits worth less in GBP |
| **Commodity Risk** | Oil/gold price moves | Airline's fuel cost doubles |

#### Key Measure — VaR (Value at Risk):

**VaR** answers: *"How much could I lose in a bad day?"*

```
VaR Example:
  "1-day 99% VaR = $1 million"

Means:
  On 99 out of 100 days, loss will NOT exceed $1 million
  On 1 out of 100 days, loss COULD exceed $1 million

Used by:
  • Banks to set trading limits
  • Regulators to set capital requirements
```

#### Greeks (for Options/Derivatives):

When trading options, risk is measured using "Greeks":

| Greek | Measures | Simple Meaning |
|-------|---------|---------------|
| **Delta** | Sensitivity to price change | If stock moves $1, how much does option move? |
| **Gamma** | Rate of change of Delta | How fast does Delta change? |
| **Theta** | Time decay | Option loses value every day |
| **Vega** | Sensitivity to volatility | Option value changes with market volatility |

---

### 1.2 Credit Risk

> **The risk that the counterparty you traded with CANNOT pay you.**

```
Bank A lends $10 million to Company B.
Company B goes bankrupt.
Company B cannot repay.
Bank A loses $10 million.

That is Credit Risk.
```

#### Types of Credit Risk:

| Type | What It Is | Example |
|------|-----------|---------|
| **Default Risk** | Counterparty can't pay at all | Lehman Brothers 2008 collapse |
| **Counterparty Risk** | Specific to derivatives trades | Your swap partner defaults mid-contract |
| **Settlement Risk** | Other side doesn't deliver on settlement day | Herstatt Bank collapse 1974 |
| **Concentration Risk** | Too much exposure to one party | All loans to one company/country |

#### Credit Risk Mitigation:

```
Tools used to reduce credit risk:

1. Credit Limits
   "We will never lend more than $50M to any single counterparty"

2. Collateral (see Section 2)
   "You must post assets as security before we trade"

3. Netting Agreements (ISDA Master Agreement)
   "If you owe us $10M on trade A and we owe you $7M on trade B
    → net amount = $3M only"

4. CDS (Credit Default Swap)
   "We buy insurance — if counterparty defaults, someone else pays us"
```

#### ISDA Master Agreement:

```
ISDA = International Swaps and Derivatives Association

Master Agreement = Legal contract between two parties
that governs ALL their OTC derivative trades.

Key features:
  • Netting: all trades netted to one number
  • Close-out: if one party defaults, all trades closed immediately
  • Collateral rules (via CSA - Credit Support Annex)

Why it matters for engineers:
  Systems must track ISDA agreements per counterparty
  Netting calculations must be per agreement, not per trade
```

---

### 1.3 Liquidity Risk

> **The risk of not being able to buy or sell when you need to — or only at a very bad price.**

```
Two types:

1. FUNDING LIQUIDITY RISK
   "I need to pay $5M to settle a trade tomorrow
    but I can't raise the cash in time"

2. MARKET LIQUIDITY RISK
   "I want to sell 1 million shares of a small company
    but there are no buyers → price crashes as I sell"
```

#### Bid-Ask Spread as Liquidity Indicator:

```
Very Liquid (Apple):          Illiquid (Small company):
  Bid: $180.00                  Bid: $5.00
  Ask: $180.01                  Ask: $5.50
  Spread: $0.01 (tiny ✅)       Spread: $0.50 (large ⚠️)

Wider spread = less liquid = harder to trade without moving price
```

---

### 1.4 Operational Risk

> **The risk of loss from failed internal processes, people, systems, or external events.**

```
Examples:
  • Wrong trade entered (fat finger error — typed $1M instead of $1K)
  • System outage during market hours
  • Rogue trader bypasses risk controls (Nick Leeson / Barings Bank 1995)
  • Cyber attack on trading systems
  • Settlement instructions sent to wrong account
  • Power failure at data center
```

#### Famous Operational Risk Events:

| Event | Year | What Happened | Loss |
|-------|------|--------------|------|
| Barings Bank | 1995 | Rogue trader Nick Leeson hid losses | $1.3B — bank collapsed |
| Knight Capital | 2012 | Software bug sent millions of wrong orders | $440M in 45 minutes |
| Flash Crash | 2010 | Algorithm triggered market crash | Dow dropped 1000 pts in minutes |

---

### 1.5 Risk Controls in Trading Systems

```
REAL-TIME RISK CHECKS (happen in milliseconds):

Pre-Trade Checks:
  ✅ Order size limit (max qty per order)
  ✅ Position limit (max total exposure)
  ✅ Price reasonability check (is price too far from market?)
  ✅ Fat finger check (order too large compared to daily volume?)
  ✅ Credit limit check

Post-Trade Checks:
  ✅ P&L monitoring (breach = alert)
  ✅ Position reconciliation (do our books match broker's?)
  ✅ VaR calculation (updated every EOD)

Kill Switch:
  ✅ Ability to CANCEL ALL OPEN ORDERS immediately
     Used when a system malfunction is detected
     Required by regulators in US (SEC) and UK (FCA)
```

---

## 💰 Section 2 — Margin & Collateral

### 2.1 What is Margin?

**Margin** = money/assets you must deposit as **security** before trading.

> 💡 **Analogy — Renting an Apartment:**
> Landlord asks for 2 months deposit before you move in.
> If you damage the apartment, landlord uses the deposit.
> That deposit = **Margin**

```
Without Margin:
  You buy £1M of futures contracts.
  Market moves against you by £200K.
  You can't pay → counterparty loses money.

With Margin:
  You must deposit £100K upfront (Initial Margin).
  If you lose £80K → deducted from your margin.
  If margin drops below minimum → Margin Call! ⚠️
```

---

### 2.2 Types of Margin

#### Initial Margin (IM)
Deposit required **before** you can open a position.

```
You want to trade S&P 500 futures.
Exchange requires 10% Initial Margin.
Contract value = $500,000
Initial Margin = $50,000 (must deposit before trading)
```

#### Variation Margin (VM)
Daily **profit/loss** settlement — your margin account is adjusted every day.

```
Day 1: You open futures position. Deposit $50,000 IM.
Day 2: Market moves against you. Loss = $5,000.
       → $5,000 deducted from margin account.
       → Margin balance = $45,000

Day 3: Market moves in your favor. Gain = $8,000.
       → $8,000 added to margin account.
       → Margin balance = $53,000
```

#### Maintenance Margin
Minimum balance you must maintain.

```
Initial Margin:     $50,000
Maintenance Margin: $40,000

If balance drops below $40,000 → MARGIN CALL ⚠️
```

---

### 2.3 Margin Call

> A **margin call** is when the broker/exchange demands you deposit more money because your margin has fallen too low.

```
MARGIN CALL FLOW:

Day 4: Market crashes. Your loss = $12,000.
       Margin balance = $45,000 - $12,000 = $33,000

$33,000 < $40,000 (Maintenance Margin) → MARGIN CALL! ⚠️

Broker calls/emails:
  "Please deposit $17,000 by 12:00 PM tomorrow
   to bring balance back to $50,000"

Your options:
  1. Deposit $17,000 cash ✅
  2. Liquidate (close) some positions ✅
  3. Do nothing → broker FORCE-CLOSES your position ❌
```

---

### 2.4 Collateral Management

**Collateral** = assets pledged to reduce counterparty risk in OTC derivatives.

```
Bank A and Bank B do an Interest Rate Swap.
Exposure = $10M (Bank B owes Bank A)

Bank B must post $10M worth of collateral:
  • Cash (most common)
  • Government Bonds (Treasuries, Gilts)
  • High-quality corporate bonds

If Bank B defaults → Bank A keeps the collateral ✅
```

#### Collateral Eligibility:

| Collateral Type | Accepted? | Haircut |
|----------------|-----------|---------|
| Cash (USD/GBP) | ✅ Always | 0% |
| US Treasuries | ✅ Always | 2% |
| UK Gilts | ✅ Always | 2% |
| Investment Grade Bonds | ✅ Usually | 5–15% |
| Equities | ⚠️ Sometimes | 15–25% |
| Junk Bonds | ❌ Rarely | — |

> 💡 **Haircut** = discount applied to collateral value.
> Bond worth $100M posted as collateral with 5% haircut = only $95M credit given.
> Protects against price drops in the collateral itself.

#### Collateral Flow:

```
Daily Collateral Management Cycle:

Morning:
  1. Calculate all OTC derivative exposures
  2. Compare to collateral already posted
  3. Calculate net shortfall or excess

Midday:
  4. Issue margin calls (if counterparty owes more collateral)
  5. Receive margin calls (if you owe more)

Afternoon:
  6. Transfer collateral (cash or securities via SWIFT/CREST/DTC)
  7. Confirm receipt

This happens EVERY BUSINESS DAY for all OTC positions.
```

---

## 📜 Section 3 — Regulatory Framework

### 3.1 Why Regulation Exists

The 2008 Financial Crisis was the turning point.

```
PRE-2008:
  OTC derivatives = completely unregulated
  Banks traded massive volumes, nobody knew total exposure
  AIG sold trillions of CDS with no collateral
  Lehman Brothers collapsed → nobody knew who owed what
  Global financial system nearly collapsed

POST-2008:
  US → Dodd-Frank Act (2010)
  EU/UK → EMIR + MiFID II
  
  Key new rules:
    • Mandatory clearing through CCPs
    • Mandatory trade reporting
    • Mandatory collateral for OTC derivatives
    • More capital requirements for banks
```

---

### 3.2 🇺🇸 US Regulations

#### SEC — Securities and Exchange Commission

```
Regulates: Stocks, bonds, ETFs, investment advisers
Powers:
  • Approve new securities
  • Enforce securities laws
  • Require disclosures (10-K, 10-Q filings)
  • Investigate insider trading
  • Fine/ban firms and individuals

Key Rules:
  • Regulation NMS — best execution in equity markets
  • Rule 10b-5 — anti-fraud, anti-insider trading
  • Reg SHO — short selling rules
```

#### FINRA — Financial Industry Regulatory Authority

```
Regulates: Broker-dealers (sell-side firms)
Self-regulatory organization (SRO) — industry polices itself, SEC oversees

Key Functions:
  • License brokers (Series 7 exam, etc.)
  • Monitor trading activity for manipulation
  • TRACE system — trade reporting for bonds
  • BrokerCheck — public database of broker history
```

#### CFTC — Commodity Futures Trading Commission

```
Regulates: Futures, options on futures, swaps
Think: SEC for derivatives markets

Key Rules:
  • Mandatory swap clearing (post Dodd-Frank)
  • Swap Dealer registration
  • Position limits in commodity markets
```

#### Dodd-Frank Act (2010) — Key Provisions:

| Provision | What It Requires |
|-----------|-----------------|
| **Mandatory Clearing** | Standard OTC swaps must be cleared through CCP |
| **Trade Reporting** | All swaps must be reported to Swap Data Repository (SDR) |
| **Margin Rules** | Collateral required for uncleared swaps |
| **Volcker Rule** | Banks cannot trade for own profit (proprietary trading ban) |
| **Stress Testing** | Large banks must prove they can survive a crisis |

---

### 3.3 🇬🇧 UK Regulations

#### FCA — Financial Conduct Authority

```
Main regulator for financial services in UK.
Post-Brexit: UK has its own rules (UK MiFID II, UK EMIR)

Regulates:
  • Banks, brokers, asset managers
  • Insurance, mortgages
  • Crypto assets (partially)

Key Powers:
  • Authorize firms (no FCA authorization = can't operate)
  • Conduct supervision and audits
  • Fine firms (largest fine: £1.1B to Standard Chartered)
  • Ban individuals from working in finance
  • Market abuse enforcement

FCA Principles:
  Firms must be honest, treat customers fairly,
  manage risk properly, and maintain market integrity
```

#### MiFID II — Markets in Financial Instruments Directive II

```
Originally EU law. UK adopted its own version post-Brexit (UK MiFID II).

Most important regulation for trading systems.
```

**Key MiFID II Requirements:**

##### 1. Best Execution
```
Firms must prove they got the BEST possible deal for clients.

Best = not just price, but also:
  • Speed of execution
  • Likelihood of execution
  • Total cost (including commissions)

Firms must:
  • Have Best Execution Policy (documented)
  • Publish annual execution quality reports (RTS 27, RTS 28)
  • Review and prove best execution per order
```

##### 2. Transaction Reporting (RTS 22)
```
Every trade must be reported to FCA within T+1.

What must be reported:
  • Trade date & time (to millisecond)
  • Instrument (ISIN)
  • Buyer and Seller (LEI codes)
  • Price, Quantity, Currency
  • Trading venue
  • Trader ID

Who reports:
  • Investment firms (banks, brokers, asset managers)
  • To: National Competent Authority (NCA) in UK = FCA

Purpose:
  FCA uses this data to detect:
  • Market manipulation
  • Insider trading
  • Unusual trading patterns
```

##### 3. Market Transparency (Pre & Post Trade)
```
Pre-Trade Transparency:
  Exchanges must publish bid/ask prices BEFORE trade
  (so all participants see fair prices)

Post-Trade Transparency:
  All executed trades must be published immediately after
  Price, volume, time — publicly available

Exceptions allowed for:
  • Large trades (to avoid market impact)
  • Illiquid instruments
```

##### 4. Clock Synchronization (RTS 25)
```
All trading systems must synchronize clocks to UTC.

Accuracy requirements:
  • Algorithmic trading systems: within 100 microseconds
  • Other electronic systems: within 1 millisecond
  • Voice trading: within 1 second

Why? So regulators can reconstruct exact order of events.
Engineers: This is a real system requirement!
```

##### 5. Algorithmic Trading Controls
```
Firms using algorithms must:
  • Test algorithms before deployment (kill switch required)
  • Mark all algorithmic orders with Algo ID
  • Have DEA (Direct Electronic Access) controls
  • Submit annual self-assessment to FCA
  • Maintain order records for 5 years
```

---

### 3.4 EMIR — European Market Infrastructure Regulation

```
UK adopted own version: UK EMIR (post-Brexit)
EU version: EMIR (still applies to EU entities)

Target: OTC Derivatives (swaps, forwards, options)

3 Main Pillars:
```

#### Pillar 1 — Mandatory Clearing

```
Standard OTC derivatives (interest rate swaps, CDS)
MUST be cleared through a CCP (LCH, EUREX Clearing)

Who must clear?
  • Financial Counterparties (FC): banks, funds, brokers
  • Non-Financial Counterparties above threshold (NFC+)

Threshold example:
  If your gross notional OTC position > €1 billion → must clear
```

#### Pillar 2 — Trade Reporting

```
All OTC derivative trades must be reported to a
Trade Repository (TR).

UK TRs: DTCC, ICE TR, UnaVista (LSEG)

What must be reported:
  • Both counterparties (using LEI codes)
  • Trade economics (notional, currency, maturity)
  • Collateral posted
  • Daily valuations (mark-to-market)

Deadline: By end of next business day (T+1)

Who reports:
  Both sides must report (or agree one side reports for both)
  Under EMIR Refit: financial counterparty reports for smaller clients
```

#### Pillar 3 — Risk Mitigation for Uncleared Trades

```
For trades NOT going through CCP:

1. Timely Confirmation
   Must confirm trade details within 1-2 business days

2. Portfolio Reconciliation
   Regularly compare open positions with counterparty
   (daily for large portfolios, weekly/monthly for smaller)

3. Dispute Resolution
   Process for resolving valuation differences

4. Portfolio Compression
   Periodically terminate redundant trades to reduce notional

5. Margin Exchange
   Post and collect Initial & Variation Margin
```

---

### 3.5 LEI — Legal Entity Identifier

> A **LEI** is like a **PAN/passport number for companies** in financial markets.

```
LEI = 20-character alphanumeric code

Example: 5493006MHB84DD0ZWV18 (JP Morgan Chase)

Required for:
  • MiFID II transaction reporting
  • EMIR trade reporting
  • Dodd-Frank swap reporting

You cannot report a trade without a valid LEI.
LEI must be renewed annually.

Who assigns LEI: GLEIF (Global LEI Foundation)
```

---

### 3.6 Trade Reporting — End to End

```
Trade Executed
      │
      ▼
T+1 Deadline approaches

Firm's Reporting System:
  1. Extract trade data from OMS/booking system
  2. Enrich with reference data (LEI, ISIN, venue MIC)
  3. Validate against reporting rules
  4. Format message (ISO 20022 XML / CSV)
  5. Submit to Trade Repository or FCA

If submission fails:
  • Fix and resubmit within deadline
  • Late reporting = regulatory breach → potential fine

FCA/Regulator:
  6. Validates received reports
  7. Flags anomalies for investigation
  8. Uses data for market surveillance
```

#### Reporting Comparison:

| Feature | MiFID II (UK) | EMIR (UK) | Dodd-Frank (US) |
|---------|--------------|-----------|-----------------|
| Covers | All financial instruments | OTC derivatives only | Swaps only |
| Reports to | FCA | Trade Repository | SDR (Swap Data Repository) |
| Deadline | T+1 | T+1 | T+1 |
| Who reports | Executing firm | Both counterparties | Both sides |
| Uses LEI | ✅ Yes | ✅ Yes | ✅ Yes |
| Format | ISO 20022 | ISO 20022 | CFTC format |

---

## 🔍 Section 4 — Market Abuse & Surveillance

Regulators require firms to **detect and prevent** market abuse.

### Types of Market Abuse:

| Type | What It Is | Example |
|------|-----------|---------|
| **Insider Trading** | Trading on non-public information | CEO buys stock before announcing merger |
| **Market Manipulation** | Artificially moving prices | "Spoofing" — placing fake orders to move price |
| **Front Running** | Trading ahead of client orders | Broker buys before executing large client order |
| **Wash Trading** | Buying and selling to yourself to fake volume | Creating illusion of market activity |
| **Spoofing** | Placing large orders then cancelling | Trick others into thinking there's real demand |

### Surveillance Systems:

```
All firms must have Trade Surveillance systems.

These systems:
  • Monitor ALL orders and trades in real-time
  • Compare against known abuse patterns
  • Generate alerts for compliance team to review
  • Maintain audit trail for 5-7 years
  • Report suspicious activity to FCA/SEC (SAR)

Examples of surveillance alerts:
  • Trader buys stock, then large news announcement → investigate
  • Pattern of orders cancelled just before execution → spoofing alert
  • Trades clustered around earnings dates → insider trading check
```

---

## 🏗️ Section 5 — Compliance Impact on Systems

Every regulation translates into **system requirements**. Here's how:

| Regulation | System Requirement |
|-----------|-------------------|
| MiFID II clock sync | NTP/PTP time servers, microsecond timestamps on all orders |
| Transaction reporting | Reporting module, LEI database, ISO 20022 message builder |
| Best execution | Execution quality analytics, venue comparison, audit logs |
| EMIR trade reporting | Daily report generation, Trade Repository connectivity |
| Algo trading controls | Kill switch API, algo ID tagging, pre/post deployment testing |
| Record keeping (5 yrs) | Immutable audit log, data archival system |
| Margin calls | Real-time P&L engine, collateral management system |
| VaR limits | Risk calculation engine, limit breach alerts |
| Pre-trade risk checks | Low-latency validation engine (must not slow trading!) |

---

## 🧠 Quick Revision — Phase 3 Key Terms

| Term | One-Line Explanation |
|------|---------------------|
| **Market Risk** | Risk of prices moving against your position |
| **Credit Risk** | Risk that counterparty won't pay you |
| **Operational Risk** | Risk from failed systems, people, or processes |
| **VaR** | Max expected loss with a given confidence level |
| **Initial Margin** | Deposit required before opening a trade |
| **Variation Margin** | Daily P&L settlement on margin account |
| **Margin Call** | Demand to deposit more margin when balance falls |
| **Collateral** | Assets pledged to reduce counterparty risk |
| **Haircut** | Discount applied to collateral value |
| **ISDA** | Legal framework governing OTC derivative trades |
| **SEC** | US regulator for securities markets |
| **CFTC** | US regulator for futures and swaps |
| **FCA** | UK financial markets regulator |
| **MiFID II** | UK/EU law governing trading — best execution, reporting |
| **EMIR** | UK/EU law governing OTC derivatives — clearing, reporting |
| **Dodd-Frank** | US post-2008 law — mandatory clearing and reporting |
| **LEI** | Unique company ID required for trade reporting |
| **SDR** | US Swap Data Repository — where Dodd-Frank reports go |
| **Trade Repository** | Where EMIR reports go (DTCC, ICE TR, UnaVista) |
| **Spoofing** | Placing fake orders to manipulate price — illegal |

---

## ✅ Phase 3 — Completion Checklist

- [ ] I can name and explain the 4 types of risk
- [ ] I understand VaR and how it is used
- [ ] I know what Initial Margin, Variation Margin, and Margin Call mean
- [ ] I understand what collateral is and what a haircut is
- [ ] I know what the ISDA Master Agreement does
- [ ] I can explain what Dodd-Frank changed after 2008
- [ ] I understand what MiFID II requires (best execution, reporting, clock sync)
- [ ] I understand EMIR's 3 pillars (clearing, reporting, risk mitigation)
- [ ] I know what an LEI is and why it is mandatory
- [ ] I can explain how regulations create system requirements for engineers

---
