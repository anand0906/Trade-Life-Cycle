# 🏢 Phase 4 — Back Office & Operations
### Trade Life Cycle Series | Business Knowledge for Engineers

---

## 🗺️ What You'll Learn in Phase 4

```
┌─────────────────────────────────────────────────────────────────────┐
│                     BACK OFFICE & OPERATIONS                        │
│                                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────────┐  │
│  │   CORPORATE  │  │RECONCILIATION│  │     P&L & ACCOUNTING     │  │
│  │   ACTIONS    │  │   & BREAKS   │  │                          │  │
│  │              │  │              │  │  • Mark-to-Market        │  │
│  │ • Dividends  │  │ • Position   │  │  • Trading Book          │  │
│  │ • Stock Split│  │ • Cash       │  │  • Banking Book          │  │
│  │ • Rights     │  │ • Break      │  │  • Realized vs           │  │
│  │ • Mergers    │  │   Resolution │  │    Unrealized P&L        │  │
│  └──────────────┘  └──────────────┘  └──────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
```

> 💡 **Front, Middle, Back Office — what's the difference?**
>
> ```
> FRONT OFFICE          MIDDLE OFFICE         BACK OFFICE
> (Revenue generators)  (Risk & Control)      (Operations)
>
> Traders               Risk Managers         Settlement teams
> Sales                 Compliance            Reconciliation
> Portfolio Managers    Finance Control       Corporate Actions
>
> "Make the money"      "Check the risk"      "Process everything"
> ```

---

## 📢 Section 1 — Corporate Actions

A **corporate action** is any event announced by a company that **affects its shares or bondholders**.

> 💡 **Why it matters:**
> When a corporate action happens, every firm holding that security must
> update their positions, cash, and records — automatically and correctly.
> Getting this wrong = major financial and regulatory consequences.

---

### 1.1 Types of Corporate Actions

```
CORPORATE ACTIONS
├── Mandatory (you have no choice — happens automatically)
│   ├── Cash Dividend
│   ├── Stock Split / Reverse Split
│   ├── Merger / Acquisition (cash deal)
│   └── Redemption (bond matured)
│
├── Voluntary (you must choose — elect one option)
│   ├── Rights Issue (buy more shares or sell rights)
│   ├── Tender Offer (sell back to company or not)
│   └── Conversion (convert bonds to shares or not)
│
└── Mandatory with Choice
    ├── Scrip Dividend (cash or shares — your choice)
    └── Merger with election (cash or shares)
```

---

### 1.2 Cash Dividend

> Company distributes **cash profits** to shareholders.

```
Example: Apple declares dividend

  Declaration Date:  15 Jan — "We will pay $0.24 per share"
  Ex-Dividend Date:  22 Jan — Must OWN shares BEFORE this date
  Record Date:       23 Jan — Company records who owns shares
  Payment Date:      15 Feb — Cash hits your account

Key rule:
  If you buy shares ON or AFTER Ex-Dividend Date
  → you do NOT get the dividend
  → share price drops by approx dividend amount on Ex-Date
```

#### Timeline:

```
Jan 15          Jan 22          Jan 23          Feb 15
   │               │               │               │
   ▼               ▼               ▼               ▼
Declaration     Ex-Dividend     Record          Payment
Date            Date            Date            Date
"$0.24/share    "Buy before     "Who owns       "$0.24 per
 announced"      this date       shares today    share paid
                 to get div"     gets dividend"  to accounts"
```

#### Impact on Systems:

```
When dividend is announced:
  1. Corporate Actions team receives SWIFT MT564 (announcement)
  2. System identifies all positions holding Apple stock
  3. On Payment Date:
     • Cash credited to all holders (qty × $0.24)
     • Transaction booked in accounting system
  4. Custodian confirms cash received

If you lent shares (stock lending) → dividend goes to borrower
→ "Manufactured dividend" paid back to you instead
```

---

### 1.3 Stock Split

> Company **divides existing shares** into more shares at a lower price.
> Total value stays the same — just more pieces of the same pie.

```
Apple does 4-for-1 Stock Split

BEFORE split:           AFTER split:
  Price:  $400            Price:  $100
  Shares: 1,000           Shares: 4,000
  Value:  $400,000        Value:  $400,000 (same!)

You held 1,000 shares at $400
After split: 4,000 shares at $100
Total value unchanged ✅
```

#### Why companies do splits:
- Lower price = more accessible to small investors
- More shares = more liquidity

#### Reverse Stock Split (opposite):

```
Company struggling with low stock price.
Does 1-for-10 reverse split.

BEFORE:                 AFTER:
  Price:  $1              Price:  $10
  Shares: 10,000          Shares: 1,000
  Value:  $10,000         Value:  $10,000 (same)

Purpose: Avoid being delisted (exchanges require min price)
Signal: Usually negative — company in trouble ⚠️
```

#### System Impact:

```
On Split Date:
  All positions in Apple automatically multiplied by split ratio
  All open orders adjusted (limit price ÷ 4, qty × 4)
  Historical prices adjusted for comparability
  Reference data updated (new share count)

This must happen overnight — before markets open.
Getting it wrong = all positions/P&L wrong for the day ❌
```

---

### 1.4 Rights Issue

> Company wants to raise more money. It gives **existing shareholders the right** to buy new shares — usually at a discount.

```
Example: Barclays Rights Issue

  1 rights share for every 4 shares held
  New share price: £2.00 (vs current market price £3.00)
  → 25% discount

You hold: 1,000 Barclays shares
You receive: 250 rights (1 per 4 shares)

Your choices:
  1. EXERCISE → Pay £2.00 × 250 = £500 → Get 250 new shares ✅
  2. SELL RIGHTS → Sell the 250 rights in market for cash ✅
  3. DO NOTHING → Rights expire worthless ❌ (bad — you lose value)
```

#### Timeline:

```
Announcement → Ex-Rights Date → Record Date → Subscription Period → New Shares Listed
    │               │               │               │                      │
  Rights          Rights          Who gets        Pay and               New shares
  announced       attached to     rights          subscribe             in your
                  shares          determined                            account
```

#### System Impact:

```
  • New instrument created (rights share — separate ISIN)
  • All eligible positions receive rights entitlement
  • Elections must be tracked per account
  • Cash collected for subscriptions
  • New shares delivered on settlement date
  • Unused rights lapsed and removed
```

---

### 1.5 Mergers & Acquisitions (M&A)

> One company buys another — shareholders of acquired company receive cash or shares.

```
Example: Microsoft acquires Activision for $95/share (cash deal)

You hold 500 Activision shares
On completion date:
  → 500 shares cancelled
  → $47,500 cash (500 × $95) deposited in your account

Share-for-share deal example:
  Company A acquires Company B: "1.2 A shares per B share"
  You hold 1,000 B shares
  → 1,000 B shares cancelled
  → 1,200 A shares received
```

---

### 1.6 Bond Corporate Actions

Bonds have their own corporate actions:

| Event | What Happens |
|-------|-------------|
| **Coupon Payment** | Interest paid to bondholders (e.g., every 6 months) |
| **Maturity / Redemption** | Face value ($1,000) returned to holder at end |
| **Early Redemption (Call)** | Issuer pays back bond before maturity date |
| **Conversion** | Bondholder converts bond to shares (convertible bonds) |
| **Default** | Issuer cannot pay → bond value drops sharply |

---

### 1.7 Corporate Actions — Key SWIFT Messages

| SWIFT Message | Purpose |
|--------------|---------|
| **MT564** | Corporate Action Notification (announcement) |
| **MT565** | Corporate Action Instruction (your election) |
| **MT566** | Corporate Action Confirmation (election confirmed) |
| **MT567** | Corporate Action Status (processing update) |
| **MT568** | Corporate Action Narrative (additional details) |

---

## 🔄 Section 2 — Reconciliation

> **Reconciliation** = comparing your internal records with external records to make sure they match.

> 💡 **Analogy — Bank Statement:**
> Every month you compare your spending with your bank statement.
> If your records say £500 spent but bank says £520 → there's a discrepancy.
> You investigate and fix it.
> That's exactly what financial firms do — every single day.

---

### 2.1 Why Reconciliation Matters

```
Without reconciliation:
  ❌ Firm thinks it holds 10,000 Apple shares
  ❌ Custodian actually holds 9,500 shares
  ❌ Firm reports wrong position to regulator
  ❌ Wrong P&L calculated
  ❌ Dividend calculated on wrong quantity
  ❌ Risk limits calculated on wrong exposure

With reconciliation:
  ✅ Differences caught early
  ✅ Errors investigated and fixed
  ✅ Accurate books = accurate risk and P&L
  ✅ Regulatory reporting is correct
```

---

### 2.2 Types of Reconciliation

#### 📌 Position Reconciliation (Securities)

Compare **what your system says you hold** vs **what custodian says you hold**.

```
Internal System:                Custodian Statement:
  AAPL: 10,000 shares             AAPL: 10,000 shares  ✅ Match
  MSFT:  5,000 shares             MSFT:  4,800 shares  ❌ BREAK!
  TSLA:  2,000 shares             TSLA:  2,000 shares  ✅ Match

Action: Investigate MSFT break of 200 shares
```

#### 📌 Cash Reconciliation

Compare **your internal cash ledger** vs **bank/custodian cash statement**.

```
Internal cash ledger:  $1,250,000
Custodian cash:        $1,237,500
Difference:            $12,500  ❌ BREAK!

Possible reasons:
  • Settlement not yet reflected
  • Fee charged but not recorded
  • Dividend received but not booked
  • Erroneous entry in internal system
```

#### 📌 Trade Reconciliation

Compare **your trade records** vs **broker's trade records**.

```
Your OMS says:
  Trade 001: Buy 1000 AAPL @ $180.05 on 24-May ✅
  Trade 002: Sell 500 MSFT @ $420.00 on 24-May ❌ NOT IN BROKER RECORDS!

Action: Call broker — did trade execute? Was it cancelled?
```

#### 📌 P&L Reconciliation

Compare **trading desk P&L** vs **finance/accounting P&L**.

```
Trading desk says: +$250,000 today
Finance says:      +$232,000 today
Difference:        $18,000 ❌

Possible reasons:
  • Different pricing source used
  • Trade booked in wrong account
  • FX rate difference
  • Accrued interest calculated differently
```

---

### 2.3 Reconciliation Process (Daily Cycle)

```
END OF DAY:

Step 1 — Extract
  Pull position/cash data from internal systems
  Pull statements from custodians, brokers, CCPs

Step 2 — Load & Normalize
  Convert all data to common format
  (Different custodians send in different formats!)

Step 3 — Match
  Compare line by line
  Auto-match where records agree ✅
  Flag mismatches as BREAKS ❌

Step 4 — Investigate Breaks
  Categorize each break:
    • Timing difference (will resolve tomorrow)
    • Missing trade (investigate)
    • Price difference (wrong price used)
    • Quantity difference (check corporate actions)

Step 5 — Resolve
  Fix the error in internal system, OR
  Chase custodian/broker for correction

Step 6 — Sign-Off
  Operations manager signs off recon is complete
  Unresolved breaks escalated

Step 7 — Reporting
  Daily breaks report sent to management
  Aged breaks (unresolved > X days) escalated to senior ops
```

---

### 2.4 Break Categories

| Break Type | Cause | Resolution |
|-----------|-------|-----------|
| **Timing Break** | Trade settled but not yet in statement | Wait — resolves next day |
| **Price Break** | Different price used by each side | Agree on correct price source |
| **Quantity Break** | Different share count | Check for corporate action impact |
| **Missing Trade** | Trade in our system, not in broker's | Confirm trade with broker |
| **Phantom Trade** | In broker's records, not ours | Check if we captured the trade |
| **Currency Break** | FX rate difference | Agree on rate to use |

---

### 2.5 Nostro Reconciliation (Banks)

Specific to banks — reconciling **correspondent bank accounts**.

```
Nostro Account = "Our money held at another bank"
  (e.g., Barclays holds USD at JP Morgan New York)
  
Vostro Account = "Your money held at our bank"
  (opposite perspective)

Nostro Recon:
  Barclays internal ledger for USD account ↔ JP Morgan statement
  
  Must reconcile every day — any break = risk of paying wrong amount
  or missing a receipt.
```

---

## 💹 Section 3 — P&L & Accounting

**P&L = Profit and Loss** — how much money the firm made or lost.

---

### 3.1 Realized vs Unrealized P&L

```
UNREALIZED P&L (Paper profit/loss — position still open):
  Bought Apple at $150. Current price = $180.
  Unrealized P&L = +$30 per share (haven't sold yet)
  
  If price drops to $140 tomorrow:
  Unrealized P&L = -$10 per share (still haven't sold)

REALIZED P&L (Actual profit/loss — position closed):
  Bought Apple at $150. Sold at $180.
  Realized P&L = +$30 per share (locked in, real money)

Total P&L = Realized P&L + Unrealized P&L
```

---

### 3.2 Mark-to-Market (MtM)

> **Mark-to-Market** = valuing your positions at **today's current market price** (not what you paid).

```
Day 1:  Buy 1,000 Apple shares at $150.  Book value = $150,000
Day 2:  Apple closes at $165.            MtM value  = $165,000
        Unrealized P&L = +$15,000

Day 3:  Apple closes at $155.            MtM value  = $155,000
        Unrealized P&L = +$5,000
        Day's P&L = -$10,000 (moved against you today)

Day 4:  Sell all shares at $160.         Realized P&L = +$10,000
```

#### Why MtM matters:

```
• Gives true picture of portfolio value at any moment
• Used to calculate margin requirements (variation margin)
• Required for regulatory capital calculations
• P&L reported daily using MtM
```

#### Mark-to-Model (MtM alternative):

```
When there's NO market price (illiquid instruments):
  → Use a mathematical model to estimate fair value

Examples:
  • Exotic OTC derivatives
  • Distressed bonds with no recent trades
  • Private equity holdings

Risk: Model could be wrong → "Model Risk"
Post-2008: Regulators now require more scrutiny of models
```

---

### 3.3 Trading Book vs Banking Book

Banks hold securities in two different "books" with different accounting rules:

```
┌──────────────────────────────────────────────────────────┐
│  TRADING BOOK                  BANKING BOOK              │
│                                                          │
│  Intent: Short-term trading    Intent: Hold to maturity  │
│  or market-making              or long-term relationship  │
│                                                          │
│  Accounting: Mark-to-Market    Accounting: Amortized Cost│
│  (daily price changes          (no daily price changes   │
│   hit P&L immediately)          unless impaired)         │
│                                                          │
│  Capital: Market Risk charge   Capital: Credit Risk      │
│  (Basel framework)             charge                    │
│                                                          │
│  Examples:                     Examples:                 │
│  • Equities for trading        • Loans to customers      │
│  • Bonds for market-making     • Bonds held to maturity  │
│  • Derivatives                 • Mortgages               │
└──────────────────────────────────────────────────────────┘
```

> ⚠️ **2008 Crisis issue:** Some banks moved risky assets from trading book to banking book to avoid MtM losses. Post-crisis regulators tightened rules on this — called "FRTB" (Fundamental Review of Trading Book).

---

### 3.4 Accruals & Accounting Entries

#### Bond Accrued Interest:

```
Bond: 5% coupon, $1,000 face value, pays coupon every 6 months

Daily accrual = $1,000 × 5% ÷ 365 = $0.137/day

If you buy the bond 90 days after last coupon:
  You must pay 90 days of accrued interest to seller
  Accrued Interest = $0.137 × 90 = $12.33

Why? Seller held bond for 90 days and earned those 90 days
of interest — you compensate them at purchase.
```

#### Key Accounting Entries for a Trade:

```
BUY 1000 Apple shares at $180 (Trade Date):

  DR  Securities (Asset)         $180,000
  CR  Cash / Payable             $180,000

SELL those shares at $200 (Realized gain):

  DR  Cash / Receivable          $200,000
  CR  Securities (Asset)         $180,000
  CR  Realized P&L Gain           $20,000
```

---

### 3.5 End of Day (EOD) Process

Every business day, back office runs a structured EOD process:

```
MARKET CLOSE (4:00 PM ET / 4:30 PM GMT)
      │
      ▼
1. PRICE CAPTURE
   Pull closing prices from Bloomberg/Reuters
   For all instruments held

      │
      ▼
2. MARK-TO-MARKET
   Reprice all positions at closing prices
   Calculate daily P&L per book/portfolio

      │
      ▼
3. RISK CALCULATION
   Recalculate VaR, Greeks, exposure
   Check against limits

      │
      ▼
4. RECONCILIATION
   Compare positions and cash with custodians
   Flag and investigate breaks

      │
      ▼
5. SETTLEMENT PROCESSING
   Process all T+1 / T+2 settlements due tomorrow
   Send settlement instructions to custodians (SWIFT)

      │
      ▼
6. CORPORATE ACTIONS PROCESSING
   Apply any dividends, splits, etc. due today
   Update positions accordingly

      │
      ▼
7. REPORTING
   Generate P&L reports for management
   Generate regulatory reports (MiFID II, EMIR)
   Send confirmation statements to clients

      │
      ▼
8. SIGN-OFF
   Operations manager reviews and approves
   Next day opens with clean books ✅
```

---

## 🔗 Section 4 — How It All Fits Together

```
┌─────────────────────────────────────────────────────────────────────┐
│                    BACK OFFICE ECOSYSTEM                            │
│                                                                     │
│  TRADE                                                              │
│  EXECUTED ──►  OMS/Booking  ──►  Position System                   │
│                    │                    │                           │
│                    │                    ├──► Reconciliation         │
│                    │                    │    (vs custodian)         │
│                    │                    │                           │
│                    │                    ├──► P&L Engine             │
│                    │                    │    (MtM daily)            │
│                    │                    │                           │
│                    │                    └──► Risk System            │
│                    │                         (VaR, limits)          │
│                    │                                                │
│                    ▼                                                │
│             Settlement System ──► Custodian ──► Exchange/CSD       │
│                    │                                                │
│                    ▼                                                │
│             Corporate Actions ──► Update positions                  │
│                    │                                                │
│                    ▼                                                │
│             Accounting System ──► Finance Reports                   │
│                    │                                                │
│                    ▼                                                │
│             Regulatory Reporting ──► FCA / DTCC / SDR              │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Quick Revision — Phase 4 Key Terms

| Term | One-Line Explanation |
|------|---------------------|
| **Corporate Action** | Company event affecting shares/bonds (dividend, split, merger) |
| **Ex-Dividend Date** | Must own shares before this date to receive dividend |
| **Record Date** | Company records who is eligible for corporate action |
| **Stock Split** | Shares divided into more at lower price — value unchanged |
| **Rights Issue** | Existing shareholders offered new shares at a discount |
| **Reconciliation** | Comparing internal records vs external records daily |
| **Break** | Discrepancy found during reconciliation |
| **Nostro** | Bank's own money held at another bank |
| **Realized P&L** | Profit/loss locked in when position is closed |
| **Unrealized P&L** | Paper profit/loss on open positions |
| **Mark-to-Market** | Valuing positions at today's market price |
| **Mark-to-Model** | Valuing illiquid positions using a mathematical model |
| **Trading Book** | Securities held for short-term trading — MtM daily |
| **Banking Book** | Loans/bonds held long-term — amortized cost accounting |
| **Accrued Interest** | Interest earned but not yet paid on a bond |
| **EOD Process** | End-of-day batch — pricing, recon, settlement, reporting |
| **MT564** | SWIFT message for corporate action announcement |
| **FRTB** | Post-2008 rule tightening trading vs banking book boundary |

---

## ✅ Phase 4 — Completion Checklist

- [ ] I can explain what a corporate action is and name 5 types
- [ ] I understand dividend timeline — ex-date, record date, payment date
- [ ] I can explain a stock split and its system impact
- [ ] I know what a rights issue is and what choices shareholders have
- [ ] I understand what reconciliation is and why it runs daily
- [ ] I can name 4 types of reconciliation (position, cash, trade, P&L)
- [ ] I know what a reconciliation "break" is and how it's resolved
- [ ] I understand realized vs unrealized P&L with an example
- [ ] I can explain mark-to-market and mark-to-model
- [ ] I know the difference between trading book and banking book
- [ ] I understand what happens in an end-of-day process

---

## ➡️ What's Next? — Phase 5 Preview

```
Phase 5 — Systems & Tech Context (Engineer Angle)

You'll learn:
  • Full system landscape of a bank/brokerage
    (OMS → EMS → Risk → Settlement → Accounting)
  • Data flows between systems
  • FIX, SWIFT, ISO 20022 deep dive
  • Reference data (ISIN, CUSIP, SEDOL, LEI)
  • Static data management
  • How to read and design trading system architecture
```

---

*📌 Series: Trade Life Cycle — Business Knowledge for Software Engineers*
*📌 Scope: US & UK Capital Markets*
