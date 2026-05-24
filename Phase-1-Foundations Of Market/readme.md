# 📈 Phase 1 — Financial Markets Foundations
### Trade Life Cycle Series | Business Knowledge for Engineers

---

## 🗺️ What You'll Learn in Phase 1

```
1. Financial Markets Overview
   ├── What is a Financial Market?
   ├── Who participates? (Buy-side vs Sell-side)
   ├── Asset Classes
   └── Exchange vs OTC Markets

2. Instruments & Products
   ├── Equities (Stocks)
   ├── Fixed Income (Bonds)
   ├── ETFs
   └── Derivatives (Futures, Options, Swaps)
```

---

## 🏦 Section 1 — Financial Markets Overview

### 1.1 What is a Financial Market?

Think of a financial market like a **giant marketplace** — just like Amazon or a vegetable market — but instead of buying vegetables or electronics, people buy and sell **financial instruments** (stocks, bonds, currencies, etc.).

> 💡 **Simple Analogy:**
> - Vegetable market → seller has tomatoes → buyer pays money → tomatoes are exchanged
> - Stock market → company issues shares → investor pays money → shares are exchanged

**Why do markets exist?**

| Need | Who Has It | Solution |
|------|-----------|----------|
| Need money to grow business | Companies | Issue stocks/bonds |
| Want to grow savings | Investors | Buy stocks/bonds |
| Want to buy foreign goods | Importers | Trade currencies (FX) |
| Want to hedge risk | Farmers, Airlines | Use derivatives |

---

### 1.2 Market Participants — Buy-Side vs Sell-Side

This is **one of the most important concepts** in capital markets. Every person/firm in finance sits on one of these two sides.

```
╔══════════════════════════════════════════════════════════════╗
║                    FINANCIAL MARKET                          ║
║                                                              ║
║   BUY-SIDE                        SELL-SIDE                  ║
║   (Investors / Money Managers)    (Banks / Brokers)          ║
║                                                              ║
║   • Mutual Funds                  • Goldman Sachs            ║
║   • Hedge Funds                   • JP Morgan                ║
║   • Pension Funds                 • Barclays                 ║
║   • Insurance Companies           • Morgan Stanley           ║
║   • Retail Investors (YOU!)       • Stockbrokers             ║
║                                                              ║
║   They BUY/SELL securities        They FACILITATE trades     ║
║   to GROW money                   and EARN commissions       ║
╚══════════════════════════════════════════════════════════════╝
```

#### 🔵 Buy-Side — "The Investors"

These are entities that **use money to invest** and grow it.

| Type | Example | What They Do |
|------|---------|-------------|
| **Mutual Fund** | Vanguard, Fidelity | Pool money from public, invest in stocks/bonds |
| **Hedge Fund** | Bridgewater, Citadel | Aggressive investing strategies, high returns |
| **Pension Fund** | UK NHS Pension, US CalPERS | Invest retirement money of employees |
| **Insurance Co.** | AIG, Aviva | Invest premium money to pay future claims |
| **Retail Investor** | You and me | Individual investing via Zerodha, Robinhood, etc. |

> 💡 **Simple Way to Remember:**
> Buy-side = **"I have money, I want to grow it"**

#### 🔴 Sell-Side — "The Facilitators"

These are banks and brokers that **help buy-side execute trades** and earn fees/commissions.

| Type | Example | What They Do |
|------|---------|-------------|
| **Investment Bank** | Goldman Sachs, JP Morgan | Underwrite IPOs, facilitate large trades |
| **Broker-Dealer** | Charles Schwab, IG Group | Take orders from clients, execute on exchanges |
| **Market Maker** | Citadel Securities, Virtu | Always ready to buy/sell, provide liquidity |
| **Research Firm** | Morningstar | Provide analysis & recommendations |

> 💡 **Simple Way to Remember:**
> Sell-side = **"I help you trade, you pay me fees"**

#### Real-World Flow Example:
```
Pension Fund (Buy-side)           Goldman Sachs (Sell-side)         NYSE (Exchange)
       │                                  │                               │
       │  "I want to buy                  │                               │
       │   10,000 Apple shares"           │                               │
       │ ──────────────────────────────► │                               │
       │                                  │  Routes order to exchange     │
       │                                  │ ─────────────────────────────►│
       │                                  │                               │ Matches with
       │                                  │                               │ a seller
       │                                  │ ◄─────────────────────────────│
       │  Trade confirmed!                │                               │
       │ ◄────────────────────────────── │                               │
```

---

### 1.3 Key Market Participants (Full Picture)

```
┌─────────────────────────────────────────────────────────┐
│                  WHO IS IN THE MARKET?                  │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  REGULATORS (Watchdogs)                                 │
│  US: SEC, FINRA, CFTC                                   │
│  UK: FCA (Financial Conduct Authority)                  │
│  They make the rules everyone must follow               │
│                                                         │
│  EXCHANGES (Marketplace)                                │
│  US: NYSE, NASDAQ, CBOE                                 │
│  UK: London Stock Exchange (LSE)                        │
│  They provide the platform to trade                     │
│                                                         │
│  CLEARING HOUSES (Safety Net)                           │
│  US: DTCC/OCC    UK: LCH                                │
│  They ensure trades actually settle properly            │
│                                                         │
│  CUSTODIANS (Safekeepers)                               │
│  BNY Mellon, State Street, HSBC                         │
│  They hold your securities safely                       │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

### 1.4 Exchange vs OTC Markets

Trades happen in two types of environments:

#### 🏛️ Exchange-Traded Markets

Like a **physical marketplace with strict rules**.

```
Exchange Market (e.g., NYSE)
─────────────────────────────
✅ Centralized — all trades go through one place
✅ Transparent — prices visible to everyone
✅ Regulated — strict rules
✅ Standardized — fixed contract sizes
✅ Lower risk — exchange guarantees settlement

Examples:
  • Buying Apple stock on NASDAQ
  • Trading S&P 500 futures on CME
  • UK: BP shares on London Stock Exchange
```

#### 🤝 OTC (Over-The-Counter) Markets

Like a **private deal between two parties**, no exchange involved.

```
OTC Market
─────────────────────────────
✅ Flexible — custom terms between two parties
✅ Private — no public price disclosure
⚠️  Counterparty risk — what if other side defaults?
⚠️  Less regulated (historically)

Examples:
  • Interest Rate Swaps between banks
  • FX trading (Forex market is OTC!)
  • Corporate bond trading
  • Credit Default Swaps (CDS)
```

#### Comparison Table:

| Feature | Exchange | OTC |
|---------|----------|-----|
| Where traded | Centralized exchange | Directly between parties |
| Price transparency | High (public) | Low (private) |
| Regulation | Heavy | Lighter (post-2008 reforms) |
| Counterparty risk | Low (exchange guarantees) | Higher |
| Flexibility | Low (standardized) | High (customizable) |
| Examples | Stocks, ETFs, listed futures | FX, swaps, corporate bonds |

> 💡 **Post-2008 change:** After the financial crisis, regulators (Dodd-Frank in US, EMIR in UK/EU) forced many OTC derivatives to go through **Central Counterparties (CCPs)** to reduce risk.

---

## 💰 Section 2 — Asset Classes & Instruments

An **asset class** is a category of financial instruments that share similar characteristics.

```
ASSET CLASSES
├── Equities (Stocks/Shares)
├── Fixed Income (Bonds)
├── FX (Foreign Exchange / Currencies)
├── Commodities (Gold, Oil, Wheat)
└── Derivatives (Futures, Options, Swaps)
```

---

### 2.1 Equities (Stocks / Shares)

#### What is a Stock?

When a company wants to raise money, it **splits itself into small pieces** called **shares** and sells them to the public. If you buy a share, you **own a tiny piece of that company**.

```
Apple Inc. wants to raise $1 Billion
         │
         ▼
Splits into 1,000,000,000 shares
Each share = $1 (simplified)
         │
         ▼
Sells shares to public via IPO (Initial Public Offering)
         │
         ▼
You buy 10 shares → You own 10/1,000,000,000 of Apple
```

#### Why Buy Stocks?

| Reason | Explanation | Example |
|--------|-------------|---------|
| **Capital Gain** | Stock price rises, you profit | Bought Apple at $100, now $180 → $80 profit |
| **Dividends** | Company shares profits with shareholders | Apple pays $0.24/share per quarter |
| **Voting Rights** | You can vote in company decisions | 1 share = 1 vote typically |

#### Key Terms:

| Term | Meaning | Example |
|------|---------|---------|
| **IPO** | First time company sells shares to public | Facebook IPO in 2012 |
| **Market Cap** | Total value of company = Share Price × Total Shares | Apple = ~$3 Trillion |
| **Dividend** | Profit shared with shareholders | $0.25 per share per quarter |
| **Ticker Symbol** | Short code for the stock | AAPL (Apple), BARC (Barclays UK) |
| **Bull Market** | Prices going up | 2020-2021 pandemic rally |
| **Bear Market** | Prices going down | 2008 financial crisis |

#### US vs UK Equity Markets:

| Feature | US | UK |
|---------|----|----|
| Main Exchange | NYSE, NASDAQ | London Stock Exchange (LSE) |
| Currency | USD ($) | GBP (£) |
| Settlement | T+1 (since 2024) | T+2 |
| Regulator | SEC | FCA |
| Index | S&P 500, Dow Jones | FTSE 100, FTSE 250 |

---

### 2.2 Fixed Income (Bonds)

#### What is a Bond?

A bond is a **loan given by an investor to a borrower** (government or company). The borrower promises to:
1. Pay **interest** (called coupon) periodically
2. Return the **original amount** (face value/principal) at the end

```
You (Investor) ──── lends $1,000 ────────────────► US Government
                                                         │
               ◄── pays $50/year (5% coupon) ──────────┘
               ◄── returns $1,000 after 10 years ───────┘
```

#### Bond vs Stock:

| Feature | Bond | Stock |
|---------|------|-------|
| You become | Lender (Creditor) | Owner (Shareholder) |
| Returns | Fixed coupon (predictable) | Dividends + capital gains (variable) |
| Risk | Lower | Higher |
| Priority in bankruptcy | Paid first | Paid last |
| Who issues | Governments, Companies | Companies only |

#### Types of Bonds:

| Type | Issuer | Example | Risk |
|------|--------|---------|------|
| **Government Bond** | Government | US Treasury, UK Gilts | Very Low |
| **Corporate Bond** | Companies | Apple Bond, Tesco Bond | Medium |
| **Municipal Bond** | City/State (US only) | NYC Bond | Low-Medium |
| **Junk Bond** | Risky companies | High Yield bonds | High |

#### Key Bond Terms:

| Term | Meaning | Example |
|------|---------|---------|
| **Face Value / Par** | Original loan amount | $1,000 |
| **Coupon Rate** | Annual interest % | 5% = $50/year on $1,000 |
| **Maturity Date** | When principal is returned | 10 years from issue |
| **Yield** | Actual return based on current price | Changes as price changes |
| **Credit Rating** | Bond's risk rating | AAA (safest) → D (defaulted) |

#### US vs UK Bond Markets:

| Feature | US | UK |
|---------|----|----|
| Government Bond Name | Treasury Bond / T-Bill | Gilt |
| Regulator | SEC, FINRA | FCA |
| Settlement | T+1 | T+2 |
| Short-term gov bonds | T-Bills (< 1 year) | Treasury Bills |

---

### 2.3 ETFs (Exchange-Traded Funds)

#### What is an ETF?

An ETF is like a **basket of many stocks/bonds packaged into one product** that you can buy/sell on an exchange like a regular stock.

```
S&P 500 ETF (e.g., SPY)
│
├── Contains Apple (7%)
├── Contains Microsoft (6%)
├── Contains Amazon (3%)
├── Contains 497 more companies...
│
You buy 1 share of SPY → you're invested in ALL 500 companies
```

#### Why ETFs?

| Benefit | Explanation |
|---------|-------------|
| **Diversification** | Don't put eggs in one basket — instant diversification |
| **Low Cost** | Cheaper than mutual funds |
| **Liquid** | Buy/sell anytime during market hours |
| **Transparent** | Holdings published daily |

#### Examples:

| ETF | Tracks | Exchange |
|-----|--------|----------|
| SPY | S&P 500 (top 500 US companies) | NYSE |
| QQQ | NASDAQ 100 (top 100 tech companies) | NASDAQ |
| ISF | FTSE 100 (top 100 UK companies) | LSE |
| GLD | Gold price | NYSE |

---

### 2.4 Derivatives

#### What is a Derivative?

A derivative is a **contract whose value DEPENDS ON (is "derived from") the price of something else** — called the "underlying asset".

```
Underlying Asset Examples:
  • Stock (Apple shares)
  • Index (S&P 500)
  • Commodity (Oil, Gold, Wheat)
  • Currency (USD/GBP)
  • Interest Rate

Derivative = Contract based on these
```

#### Why Derivatives Exist?

Two main reasons:

1. **Hedging** — Protect against losses
2. **Speculation** — Bet on price movements with leverage

#### 📌 Futures

A **futures contract** is an agreement to **buy or sell something at a fixed price on a future date**.

> 💡 **Real-World Analogy:**
> You're a baker. Wheat price today is $100/ton. You're worried it'll rise to $150 in 3 months.
> So you sign a futures contract: *"I will buy wheat at $110/ton in 3 months"*
> → You lock in $110. Even if it goes to $150, you pay only $110. ✅

```
Today (Jan):   Sign contract → "Buy Oil at $80/barrel in April"
April comes:   
  • Oil price = $100 → You WIN, bought at $80 (saved $20/barrel)
  • Oil price = $60  → You LOSE, bought at $80 (paid $20 extra/barrel)
```

| Feature | Detail |
|---------|--------|
| Obligation | MUST buy/sell on expiry |
| Exchange | Traded on exchanges (CME in US) |
| Use | Hedging, speculation |
| Example | Oil futures, S&P 500 futures |

#### 📌 Options

An **option** gives you the **RIGHT but NOT the obligation** to buy/sell at a fixed price.

> 💡 **Real-World Analogy:**
> You want to buy a house listed at £500,000. You pay £5,000 to the seller for the option to buy it within 3 months.
> - If price rises to £600,000 → you exercise your option, buy at £500,000 ✅
> - If price falls to £400,000 → you walk away, lose only £5,000 ✅

| Type | Right | When to Use |
|------|-------|-------------|
| **Call Option** | Right to BUY at fixed price | Expect price to go UP |
| **Put Option** | Right to SELL at fixed price | Expect price to go DOWN |

```
Key Terms:
  Strike Price = Fixed price in the contract ($150 for Apple)
  Premium      = Cost to buy the option ($5)
  Expiry Date  = When the option expires (3rd Friday of month)
  In-the-Money = Option is profitable to exercise
```

#### 📌 Swaps

A **swap** is an agreement to **exchange cash flows** between two parties.

> 💡 **Real-World Analogy:**
> You have a variable loan (rate changes every month).
> Your friend has a fixed loan (always 5%).
> You both agree to swap payments — you pay his fixed rate, he pays your variable rate.
> This is an **Interest Rate Swap**.

| Type | What is Swapped | Who Uses It |
|------|----------------|-------------|
| **Interest Rate Swap** | Fixed rate ↔ Variable rate | Banks, Corporates |
| **Currency Swap** | USD payments ↔ GBP payments | Multinationals |
| **Credit Default Swap** | Premium ↔ Protection on default | Banks, Hedge funds |

> ⚠️ **Historic Note:** Credit Default Swaps (CDS) played a major role in the 2008 Financial Crisis. This led to heavy regulation of OTC derivatives in both US (Dodd-Frank) and UK/EU (EMIR).

---

## 🌍 Section 3 — US vs UK Markets — Key Differences

| Feature | 🇺🇸 United States | 🇬🇧 United Kingdom |
|---------|-------------------|-------------------|
| Main Stock Exchange | NYSE, NASDAQ | London Stock Exchange (LSE) |
| Main Index | S&P 500, Dow Jones | FTSE 100, FTSE 250 |
| Currency | USD ($) | GBP (£) |
| Stock Regulator | SEC (Securities & Exchange Commission) | FCA (Financial Conduct Authority) |
| Derivatives Regulator | CFTC | FCA |
| Clearing House | DTCC (equities), OCC (options) | LCH |
| Settlement (Equities) | T+1 (effective May 2024) | T+2 |
| Bond Name (Gov) | Treasury / T-Bills / T-Notes | Gilts |
| Trade Reporting | TRACE (FINRA) | MiFID II / FCA |
| Market Hours | 9:30 AM – 4:00 PM ET | 8:00 AM – 4:30 PM GMT |

---

## 🔄 Section 4 — How It All Connects (Big Picture)

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE FINANCIAL ECOSYSTEM                       │
│                                                                  │
│  ISSUERS                    MARKETS                INVESTORS     │
│  (Need Money)               (Marketplace)          (Have Money)  │
│                                                                  │
│  Companies ──── IPO ──────► Exchange ─────────────► Buy-Side    │
│  Governments ── Bond ─────► OTC/Exchange ──────────► Funds      │
│                                                                  │
│                    SELL-SIDE (Facilitators)                      │
│                    Banks & Brokers sit in between                │
│                    They route, execute, and settle trades        │
│                                                                  │
│  INFRASTRUCTURE                                                  │
│  Clearing Houses → ensure trades complete                        │
│  Custodians      → safekeep assets                               │
│  Regulators      → make and enforce rules                        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Quick Revision — Key Concepts Summary

| Concept | One-Line Explanation |
|---------|---------------------|
| **Buy-Side** | Investors who put money to work (funds, pensions) |
| **Sell-Side** | Banks/brokers who facilitate trades for a fee |
| **Exchange** | Centralized regulated marketplace (NYSE, LSE) |
| **OTC** | Private trades directly between two parties |
| **Equity** | Ownership in a company (stock/share) |
| **Bond** | Loan to a company/government with fixed interest |
| **ETF** | Basket of assets traded like a stock |
| **Futures** | Contract to buy/sell at fixed price on future date |
| **Options** | Right (not obligation) to buy/sell at fixed price |
| **Swaps** | Exchange of cash flows between two parties |
| **T+1 / T+2** | Settlement happens 1 or 2 days after trade |
| **DTCC / LCH** | Clearing houses — guarantee trade settlement |
| **SEC / FCA** | US / UK market regulators |
| **ISIN / CUSIP** | Unique ID for a financial instrument |

---

## ✅ Phase 1 — Completion Checklist

- [ ] I understand what a financial market is and why it exists
- [ ] I can explain Buy-side vs Sell-side with examples
- [ ] I know the difference between Exchange and OTC markets
- [ ] I understand Equities — what a stock is, why people buy it
- [ ] I understand Bonds — what they are, coupon, maturity
- [ ] I understand ETFs — why they are useful
- [ ] I can explain Futures, Options, Swaps in simple terms
- [ ] I know key US and UK market differences (exchanges, regulators, settlement)

---
