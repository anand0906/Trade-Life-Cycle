# ⚡ Phase 2 — Core Trade Life Cycle
### Trade Life Cycle Series | Business Knowledge for Engineers

---

## 🗺️ The Full Trade Journey

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        TRADE LIFE CYCLE                                  │
│                                                                          │
│  ┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐       │
│  │ PRE-TRADE │───►│ EXECUTION │───►│POST-TRADE │───►│SETTLEMENT │       │
│  └───────────┘    └───────────┘    └───────────┘    └───────────┘       │
│                                                                          │
│  "I want        "Order sent       "Trade          "Money &               │
│   to buy"        & matched"        confirmed"      shares                │
│                                                    exchanged"            │
└──────────────────────────────────────────────────────────────────────────┘
```

> 💡 **Simple Analogy — Online Shopping:**
> | Shopping | Trade Life Cycle |
> |----------|-----------------|
> | Browse & add to cart | Pre-Trade (decide what to buy) |
> | Click "Place Order" | Execution (order goes to exchange) |
> | Order confirmation email | Post-Trade (trade confirmation) |
> | Package delivered | Settlement (shares & money exchanged) |

---

## 📋 Section 1 — PRE-TRADE

Everything that happens **before** an order is actually sent to the market.

---

### 1.1 Investment Decision

A fund manager (buy-side) decides:
- **What** to buy/sell (Apple stock, UK Gilt bond, etc.)
- **How much** (1000 shares, £500,000 worth)
- **Why** (based on research, market signal, rebalancing)

```
Portfolio Manager
       │
       │ "Apple looks undervalued.
       │  Buy 5,000 shares."
       │
       ▼
  Trader / Dealer Desk
  (Now finds best way to execute)
```

---

### 1.2 Order Types

An **order** is an instruction to buy or sell. There are several types:

#### 📌 Market Order
Buy/Sell **immediately at whatever the current price is**.

```
Current Apple price = $180
You place: "Buy 100 shares, Market Order"
Result: Filled immediately at ~$180 (could be $180.01 or $179.98)

✅ Guaranteed to execute
⚠️  Price not guaranteed (slippage risk)
```

#### 📌 Limit Order
Buy/Sell **only at a specific price or better**.

```
Current Apple price = $180
You place: "Buy 100 shares, Limit $175"
Result: Order waits until price drops to $175, then executes

✅ Price guaranteed
⚠️  May never execute if price doesn't reach $175
```

#### 📌 Stop Order (Stop-Loss)
Triggers a market order **when price hits a certain level** — used to limit losses.

```
You bought Apple at $180.
You place: "Stop Order at $160" (to limit loss)
Result: If Apple drops to $160 → automatically sells to stop further loss

✅ Protects against big losses
⚠️  In fast-moving market, may execute worse than $160
```

#### 📌 Stop-Limit Order
Combination — triggers a **limit order** when stop price is hit.

```
Stop price = $160, Limit price = $158
When Apple hits $160 → places a Limit Order at $158
Result: Will only sell if it can get $158 or better
```

#### Order Type Comparison:

| Order Type | Execution Guaranteed? | Price Guaranteed? | Best For |
|------------|----------------------|-------------------|----------|
| Market | ✅ Yes | ❌ No | Urgent trades, liquid stocks |
| Limit | ❌ No | ✅ Yes | Patient investors, less liquid |
| Stop | ✅ Yes (when triggered) | ❌ No | Risk management |
| Stop-Limit | ❌ No | ✅ Yes | Risk management with price control |

---

### 1.3 Order Management System (OMS)

The **OMS is the core software system** on the buy-side that manages all orders from creation to execution.

```
┌─────────────────────────────────────────────────────┐
│              ORDER MANAGEMENT SYSTEM (OMS)           │
│                                                      │
│  Receives order from Portfolio Manager               │
│          │                                           │
│          ▼                                           │
│  Logs the order (timestamp, instrument, qty, type)   │
│          │                                           │
│          ▼                                           │
│  Runs Pre-Trade Checks (compliance, risk)            │
│          │                                           │
│          ▼                                           │
│  Routes to Execution (broker or exchange)            │
│          │                                           │
│          ▼                                           │
│  Tracks order status (pending → partial → filled)    │
│          │                                           │
│          ▼                                           │
│  Sends execution report back                         │
└─────────────────────────────────────────────────────┘

Popular OMS Platforms:
  Buy-side:  Charles River, Aladdin (BlackRock), Thinkfolio
  Sell-side: Fidessa, Flextrade, Ion Trading
```

#### OMS Order States (important for engineers!):

```
NEW ──► PENDING ──► PARTIALLY_FILLED ──► FILLED
                          │
                          └──► CANCELLED
                          └──► REJECTED
```

| State | Meaning |
|-------|---------|
| **New** | Order just created |
| **Pending** | Sent to market, waiting |
| **Partially Filled** | Some shares bought, rest pending |
| **Filled** | All shares bought/sold |
| **Cancelled** | Trader cancelled before fill |
| **Rejected** | Failed pre-trade checks |

---

### 1.4 Pre-Trade Compliance & Risk Checks

Before an order is sent to market, **automated checks** run to ensure:

```
Order submitted
      │
      ▼
┌─────────────────────────────────────┐
│        PRE-TRADE CHECKS             │
│                                     │
│  1. Compliance Check                │
│     • Is this instrument allowed?   │
│     • Investment mandate check      │
│     • Restricted list check         │
│                                     │
│  2. Risk Check                      │
│     • Position limit check          │
│     • Concentration limit           │
│     • Exposure limit                │
│                                     │
│  3. Credit Check (sell-side)        │
│     • Does client have enough cash? │
│     • Credit limit with broker?     │
└──────────────┬──────────────────────┘
               │
       ┌───────┴───────┐
       ▼               ▼
   APPROVED         REJECTED
   (goes to          (sent back
    market)          with reason)
```

| Check Type | What It Checks | Example |
|------------|---------------|---------|
| **Mandate Check** | Is fund allowed to buy this? | Pension fund restricted from junk bonds |
| **Restricted List** | Is the stock on a banned list? | Insider trading restrictions |
| **Position Limit** | Are we buying too much of one stock? | Can't hold more than 5% of company |
| **Cash Check** | Do we have enough money? | Must have funds before buying |

---

## ⚡ Section 2 — EXECUTION

Order is now sent to the market to be matched with a buyer/seller.

---

### 2.1 Order Routing

After OMS approves the order, it needs to be sent to the right place to execute.

```
OMS (Buy-side)
      │
      │  (via FIX Protocol)
      ▼
Broker / Dealer Desk (Sell-side)
      │
      ├──► Direct to Exchange (NYSE, LSE)
      │
      ├──► Dark Pool (private exchange)
      │
      └──► Market Maker (for OTC trades)
```

#### Smart Order Routing (SOR):
Banks/brokers use **algorithms** to find the best execution venue automatically.

```
"Buy 10,000 Apple shares"
         │
         ▼
Smart Order Router checks:
  • NASDAQ price: $180.00
  • NYSE price:   $180.02
  • Dark Pool:    $179.98 ← best!

Routes to Dark Pool first, rest to NASDAQ
```

---

### 2.2 FIX Protocol — Critical for Engineers!

**FIX = Financial Information eXchange**

FIX is the **universal language** that all financial systems use to communicate orders.

> 💡 Think of it like **HTTP for trading** — just as web browsers and servers talk via HTTP, trading systems talk via FIX.

```
Buy-side OMS                           Sell-side / Exchange
     │                                         │
     │  ──── FIX Message (New Order) ─────────►│
     │                                         │
     │  ◄─── FIX Message (Execution Report) ───│
     │                                         │
     │  ──── FIX Message (Cancel Request) ────►│
     │                                         │
     │  ◄─── FIX Message (Cancel Confirm) ─────│
```

#### FIX Message Structure:

A FIX message is a string of **Tag=Value** pairs separated by `|`

```
Example: New Order Single (placing a buy order)

8=FIX.4.2 | 35=D | 49=CLIENT1 | 56=BROKER1 | 11=ORD001 |
55=AAPL   | 54=1 | 38=1000    | 40=2       | 44=175.00 | 10=123

Decoded:
  8=FIX.4.2   → FIX version
  35=D         → Message type = New Order Single
  49=CLIENT1   → Sender (buy-side firm)
  56=BROKER1   → Target (broker)
  11=ORD001    → Client Order ID
  55=AAPL      → Symbol (Apple)
  54=1         → Side: 1=Buy, 2=Sell
  38=1000      → Quantity: 1000 shares
  40=2         → Order Type: 2=Limit
  44=175.00    → Price: $175
  10=123       → Checksum
```

#### Key FIX Message Types:

| MsgType (Tag 35) | Name | Direction | Purpose |
|-----------------|------|-----------|---------|
| D | New Order Single | Buy-side → Broker | Place a new order |
| F | Order Cancel Request | Buy-side → Broker | Cancel an order |
| G | Order Cancel/Replace | Buy-side → Broker | Modify an order |
| 8 | Execution Report | Broker → Buy-side | Order status update |
| 9 | Order Cancel Reject | Broker → Buy-side | Cancel was rejected |

#### FIX Execution Report — Order States:

```
Tag 39 (OrdStatus) in Execution Report:

0 = New           (order received)
1 = Partially Filled
2 = Filled        (fully executed ✅)
4 = Cancelled
8 = Rejected
```

---

### 2.3 Matching Engine (How Trades Actually Happen)

The exchange has a **Matching Engine** — software that matches buy orders with sell orders.

#### Order Book:

An **order book** is a real-time list of all pending buy and sell orders.

```
APPLE (AAPL) ORDER BOOK
─────────────────────────────────────────
BID (Buy Orders)       ASK (Sell Orders)
─────────────────────────────────────────
Price   Qty            Price   Qty
$180.00  500           $180.05  300
$179.98  1000          $180.10  700
$179.95  2000          $180.15  1500
$179.90  500           $180.20  1000
─────────────────────────────────────────
SPREAD = $180.05 - $180.00 = $0.05
```

- **Bid** = highest price buyers are willing to pay
- **Ask** = lowest price sellers are willing to accept
- **Spread** = difference between bid and ask (broker earns this)

#### Matching Logic:

```
New BUY order arrives: "Buy 300 shares at $180.05"

Matching Engine checks ASK side:
  → Lowest ask = $180.05 for 300 shares ← MATCH!

Result:
  Trade executed: 300 shares at $180.05 ✅
  Both orders removed from book
```

#### Matching Algorithms:

| Algorithm | Rule | Used By |
|-----------|------|---------|
| **Price-Time Priority** | Best price first, then earliest order | Most exchanges (NYSE, LSE) |
| **Pro-Rata** | Fill proportionally based on order size | Some futures exchanges |

---

### 2.4 Trade Confirmation

Once a match happens, both sides get a **confirmation**:

```
Exchange/Broker sends back:
  ✅ Trade Date: 2024-05-24
  ✅ Instrument: AAPL
  ✅ Side: BUY
  ✅ Quantity: 1000 shares
  ✅ Price: $180.05
  ✅ Counterparty: [Anonymous on exchange]
  ✅ Trade ID: TRD-2024-88271
```

This is sent via **FIX Execution Report** (Tag 35=8) back to the OMS.

---

### 2.5 Algorithmic Trading (Algos)

Large orders (e.g., buy 500,000 shares) can't be placed all at once — it would **move the market** and increase cost. So traders use **algorithms** to split and execute smartly.

```
"Buy 500,000 Apple shares"
            │
            ▼
   Algorithm splits it:
   ├── 9:30 AM — Buy 50,000
   ├── 10:00 AM — Buy 45,000
   ├── 11:00 AM — Buy 52,000
   ├── ...
   └── 4:00 PM — Buy 48,000

Goal: Blend in with market, minimize price impact
```

#### Common Execution Algorithms:

| Algorithm | Full Name | Strategy |
|-----------|-----------|----------|
| **TWAP** | Time-Weighted Average Price | Spread order evenly over time |
| **VWAP** | Volume-Weighted Average Price | Match market's natural volume pattern |
| **POV** | Percentage of Volume | Trade as X% of whatever volume occurs |
| **IS** | Implementation Shortfall | Minimize deviation from decision price |

---

## 📄 Section 3 — POST-TRADE PROCESSING

Trade is executed. Now what? A lot of important things happen behind the scenes.

---

### 3.1 Trade Capture & Booking

The executed trade must be **recorded in the firm's systems**:

```
Trade Executed on Exchange
          │
          ▼
Trade Capture System
  • Records all trade details
  • Assigns internal Trade ID
  • Books to correct portfolio/account
          │
          ▼
Risk System updated
  • Portfolio position updated
  • P&L calculated (real-time)
          │
          ▼
Compliance notified
  • Post-trade checks
  • Regulatory reporting triggered
```

---

### 3.2 Clearing

**Clearing** is the process of **verifying and finalizing trade details** between buyer and seller before actual settlement.

> 💡 **Analogy:**
> You and a friend bet $100 on a cricket match. Before paying, you both confirm:
> - Who bet what
> - How much
> - Who won
> That confirmation process = **Clearing**

```
Trade Executed
      │
      ▼
Central Counterparty (CCP) steps in
  US Equities: DTCC (Depository Trust & Clearing Corporation)
  UK Equities: LCH (London Clearing House)
      │
      ▼
CCP becomes the buyer to every seller
       and the seller to every buyer
      │
      ├── Calculates net obligations
      │   (if you bought and sold same stock,
      │    only net position moves)
      │
      └── Guarantees settlement
          (even if one party defaults, CCP covers it)
```

#### Why CCP Matters:

```
WITHOUT CCP (bilateral):               WITH CCP:
                                       
Firm A ←──────────────► Firm B        Firm A ◄──► CCP ◄──► Firm B
                                       
If Firm B defaults →                  If Firm B defaults →
Firm A loses money ❌                  CCP covers Firm A ✅
```

#### Netting (Efficiency):

```
Without Netting:
  Trade 1: Buy 1000 AAPL
  Trade 2: Sell 600 AAPL
  Trade 3: Buy 200 AAPL
  → 3 separate settlements needed

With Netting:
  Net position = +600 AAPL (buy 1000, sell 600, buy 200)
  → Only 1 settlement needed ✅ (saves cost and risk)
```

---

### 3.3 Settlement

**Settlement** is when the actual exchange of **money ↔ securities** happens.

```
BUYER                    CLEARING HOUSE                  SELLER
  │                            │                            │
  │── Sends Cash ─────────────►│◄──── Sends Shares ─────────│
  │                            │                            │
  │◄── Receives Shares ────────│────── Receives Cash ───────►│
```

#### DVP — Delivery vs Payment

**DVP** is the golden rule of settlement: **securities are delivered only when payment is made simultaneously**. This eliminates risk of one side not delivering.

```
DVP Settlement:
  Securities transfer ──┐
                         ├──► Happen SIMULTANEOUSLY ✅
  Cash transfer     ─────┘

Without DVP (risky):
  Buyer sends cash first → Seller could disappear! ❌
```

#### Settlement Timeline:

| Market | Settlement | Meaning |
|--------|-----------|---------|
| US Equities | **T+1** | Trade settles next business day |
| UK Equities | **T+2** | Trade settles 2 business days later |
| US Treasuries | T+1 | Government bonds |
| FX (Spot) | T+2 | Currency trades |
| FX (Same day) | T+0 | Immediate settlement |

```
Example: Trade on Monday (T)
  T+1 settlement → Tuesday
  T+2 settlement → Wednesday

Note: Weekends & holidays don't count as business days
  Trade on Friday (T+2) → Settlement on Tuesday
```

#### What Happens During Settlement:

```
T (Trade Day):
  Trade executed on exchange

T+1 or T+2 (Settlement Day):
  1. Custodians confirm holdings
  2. CCP sends settlement instructions
  3. Central Securities Depository (CSD) moves shares
     US: DTC (Depository Trust Company)
     UK: CREST
  4. Cash moves via banking system (Fedwire in US, CHAPS in UK)
  5. Both sides receive confirmations
```

---

### 3.4 Trade Confirmation & Affirmation

**Confirmation** = Seller sends trade details to buyer
**Affirmation** = Buyer agrees the details are correct

```
Broker (Sell-side)
      │
      │  Sends Confirmation:
      │  "We sold you 1000 AAPL at $180.05 on 24-May"
      ▼
Fund Manager (Buy-side)
      │
      │  Checks and Affirms:
      │  "Confirmed — details match our records ✅"
      ▼
Sent to CCP for clearing

If not affirmed in time → potential settlement FAIL ⚠️
```

---

### 3.5 SWIFT Messaging

For cross-border trades and bond settlements, **SWIFT** is used for secure messaging.

> 💡 **SWIFT** = Society for Worldwide Interbank Financial Telecommunication
> It's a secure messaging network (not a payment system itself) used by banks globally.

```
Common SWIFT Messages in Trade Settlement:
  MT540 → Deliver securities (seller instructs custodian)
  MT541 → Receive securities (buyer instructs custodian)
  MT542 → Deliver securities free of payment
  MT543 → Receive securities free of payment
  MT544/545/546/547 → Settlement confirmations
  MT950 → Statement of account
```

---

## 🔁 Section 4 — Complete Trade Flow (End to End)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    END-TO-END TRADE FLOW                                 │
│                                                                          │
│  1. DECISION                                                             │
│     Portfolio Manager: "Buy 10,000 Apple shares"                         │
│                │                                                         │
│  2. OMS        ▼                                                         │
│     Order created → Pre-trade checks → Approved                         │
│                │                                                         │
│  3. ROUTING    ▼                                                         │
│     FIX message sent to broker (New Order Single)                        │
│                │                                                         │
│  4. EXECUTION  ▼                                                         │
│     Broker routes to NASDAQ → Matching Engine finds seller               │
│     → Trade executed at $180.05                                          │
│                │                                                         │
│  5. CONFIRMATION ▼                                                       │
│     FIX Execution Report sent back to OMS                               │
│     OMS updates order status → FILLED                                   │
│                │                                                         │
│  6. TRADE CAPTURE ▼                                                      │
│     Trade booked in system → Position updated → P&L updated             │
│                │                                                         │
│  7. CLEARING   ▼                                                         │
│     Trade sent to DTCC → CCP validates → Netting done                   │
│                │                                                         │
│  8. SETTLEMENT (T+1) ▼                                                   │
│     DTC moves 10,000 AAPL shares to buyer's custodian                   │
│     Fedwire moves $1,800,500 to seller's bank                           │
│     DVP — simultaneous! ✅                                               │
│                │                                                         │
│  9. RECONCILIATION ▼                                                     │
│     Custodian confirms holdings match expected                           │
│     Trade life cycle COMPLETE ✅                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 🏦 Section 5 — Key Systems & Their Roles

| System | Who Uses It | Purpose |
|--------|------------|---------|
| **OMS** | Buy-side | Manage orders from creation to execution |
| **EMS** | Buy-side | Execution Management — advanced routing/algo |
| **FIX Engine** | Both | Send/receive FIX messages |
| **Risk System** | Both | Real-time position & risk monitoring |
| **Clearing System** | CCP (DTCC/LCH) | Clear and net trades |
| **Custody System** | Custodian banks | Hold and track securities |
| **Settlement System** | CSD (DTC/CREST) | Final movement of cash & securities |

---

## 🌐 Section 6 — US vs UK: Key Infrastructure Differences

| Role | 🇺🇸 US | 🇬🇧 UK |
|------|--------|--------|
| Exchange (Equities) | NYSE, NASDAQ | London Stock Exchange |
| Clearing House | DTCC (equities), OCC (options) | LCH |
| Settlement (Securities) | DTC | CREST (now Euroclear UK) |
| Cash Settlement | Fedwire | CHAPS |
| Settlement Cycle | T+1 | T+2 |
| Regulator | SEC, FINRA | FCA |
| Trade Reporting | FINRA TRACE | MiFID II / FCA |
| SWIFT member | Yes | Yes |

---

## 🧠 Quick Revision — Phase 2 Key Terms

| Term | One-Line Explanation |
|------|---------------------|
| **OMS** | Buy-side system to manage all orders |
| **FIX Protocol** | Standard messaging language for trading |
| **Order Book** | Real-time list of buy/sell orders on exchange |
| **Matching Engine** | Exchange software that matches buyers with sellers |
| **Bid/Ask** | Bid = buyer price, Ask = seller price |
| **Spread** | Difference between bid and ask |
| **CCP** | Central party that guarantees settlement (DTCC, LCH) |
| **Netting** | Combining multiple trades to minimize settlement |
| **DVP** | Securities & cash exchanged simultaneously |
| **T+1 / T+2** | Settlement happens 1 or 2 days after trade |
| **DTC / CREST** | Central depositories holding securities (US / UK) |
| **SWIFT MT540** | Instruction message to deliver securities |
| **TWAP / VWAP** | Algorithms to execute large orders efficiently |
| **Affirmation** | Buy-side confirms trade details are correct |
| **SOR** | Smart Order Router — finds best execution venue |

---

## ✅ Phase 2 — Completion Checklist

- [ ] I understand the 4 stages: Pre-Trade → Execution → Post-Trade → Settlement
- [ ] I know the 4 main order types and when to use each
- [ ] I can explain what an OMS does and its order states
- [ ] I understand FIX Protocol and can read a basic FIX message
- [ ] I understand how an order book and matching engine work
- [ ] I can explain what a CCP does and why it matters
- [ ] I understand DVP and why T+1/T+2 settlement matters
- [ ] I know the difference between DTCC vs LCH, DTC vs CREST
- [ ] I understand what SWIFT messages are used for

---
