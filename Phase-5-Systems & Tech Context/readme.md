# ⚙️ Phase 5 — Systems & Tech Context (Engineer Angle)
### Trade Life Cycle Series | Business Knowledge for Engineers

---

## 🗺️ What You'll Learn in Phase 5

```
┌────────────────────────────────────────────────────────────────────────┐
│                  SYSTEMS & TECH — ENGINEER'S VIEW                      │
│                                                                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │
│  │   SYSTEM    │  │  MESSAGING  │  │  REFERENCE  │  │    DATA     │  │
│  │  LANDSCAPE  │  │ PROTOCOLS   │  │    DATA     │  │   FLOWS     │  │
│  │             │  │             │  │             │  │             │  │
│  │ OMS EMS     │  │ FIX  SWIFT  │  │ ISIN CUSIP  │  │ End-to-End  │  │
│  │ Risk Recon  │  │ ISO 20022   │  │ SEDOL  LEI  │  │ Architecture│  │
│  │ Settlement  │  │ MQ Kafka    │  │ MIC   FIGI  │  │ Diagrams    │  │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
```

> 💡 **This phase ties everything together.**
> Every system below exists because of business requirements you
> learned in Phases 1–4. As you read, connect each system back
> to the business process it supports.

---

## 🏗️ Section 1 — Full System Landscape of a Bank / Brokerage

### 1.1 The Big Picture

```
╔══════════════════════════════════════════════════════════════════════════╗
║                    TRADING FIRM — SYSTEM LANDSCAPE                       ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                          ║
║  FRONT OFFICE SYSTEMS                                                    ║
║  ┌──────────┐  ┌──────────┐  ┌────────────┐  ┌──────────────────────┐  ║
║  │   OMS    │  │   EMS    │  │  Pricing   │  │  Analytics /         │  ║
║  │ (Order   │  │(Execution│  │  Engine    │  │  Research Tools      │  ║
║  │  Mgmt)   │  │  Mgmt)   │  │            │  │  (Bloomberg, etc.)   │  ║
║  └────┬─────┘  └────┬─────┘  └─────┬──────┘  └──────────────────────┘  ║
║       │             │              │                                     ║
║  MIDDLE OFFICE SYSTEMS             │                                     ║
║  ┌────┴─────────────┴──────────────┴──────────┐                         ║
║  │              RISK ENGINE                    │                         ║
║  │  (Real-time P&L, VaR, Greeks, Limits)       │                         ║
║  └─────────────────────────────────────────────┘                         ║
║       │                                                                  ║
║  BACK OFFICE SYSTEMS                                                     ║
║  ┌────┴────────┐  ┌────────────┐  ┌──────────┐  ┌──────────────────┐   ║
║  │  Trade      │  │Reconcilia- │  │Corporate │  │   Regulatory     │   ║
║  │  Capture /  │  │tion System │  │ Actions  │  │   Reporting      │   ║
║  │  Booking    │  │            │  │ System   │  │   (MiFID/EMIR)   │   ║
║  └─────┬───────┘  └────────────┘  └──────────┘  └──────────────────┘   ║
║        │                                                                 ║
║  SETTLEMENT & ACCOUNTING                                                 ║
║  ┌─────┴───────┐  ┌────────────┐  ┌──────────────────────────────────┐  ║
║  │  Settlement │  │  Custody / │  │       Accounting / GL            │  ║
║  │  System     │  │  Position  │  │       (General Ledger)           │  ║
║  │             │  │  System    │  │                                  │  ║
║  └─────────────┘  └────────────┘  └──────────────────────────────────┘  ║
║                                                                          ║
║  INFRASTRUCTURE                                                          ║
║  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────────────────┐   ║
║  │Reference │  │  Market  │  │Messaging │  │  Static Data /        │   ║
║  │  Data    │  │  Data    │  │  Layer   │  │  Instrument Master    │   ║
║  │  (ISIN,  │  │(Bloomberg│  │(MQ/Kafka)│  │                       │   ║
║  │  LEI..)  │  │Reuters)  │  │          │  │                       │   ║
║  └──────────┘  └──────────┘  └──────────┘  └───────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════════╝
```

---

### 1.2 OMS — Order Management System

> **Purpose:** Manage the full lifecycle of orders from creation to execution.

```
WHO uses it:    Portfolio Managers, Traders (Buy-side)
WHERE it sits:  Front Office

WHAT it does:
  ✅ Accept orders from portfolio managers
  ✅ Run pre-trade compliance checks
  ✅ Route orders to brokers / exchanges via FIX
  ✅ Track order status (New → Partial → Filled)
  ✅ Maintain order history and audit trail
  ✅ Allocate filled orders to accounts (one order, many funds)

KEY DATA:
  • Order (instrument, side, qty, type, price)
  • Execution (filled qty, fill price, venue, timestamp)
  • Allocation (which fund gets which portion of fill)

POPULAR PRODUCTS:
  Buy-side:  Charles River Development (CRD), BlackRock Aladdin
  Sell-side: Fidessa, FlexTrade, Ion Trading
```

#### Order Allocation Example:

```
Portfolio Manager places ONE order:
  "Buy 10,000 Apple shares"

Gets filled. Now allocate across 3 funds:
  Fund A (Growth)       → 5,000 shares (50%)
  Fund B (Balanced)     → 3,000 shares (30%)
  Fund C (Income)       → 2,000 shares (20%)

Each fund gets separate trade confirmation.
Average price used for all = fair allocation.
```

---

### 1.3 EMS — Execution Management System

> **Purpose:** Smart, fast execution — routing orders to best venues.

```
WHO uses it:    Traders (Buy-side), often combined with OMS
WHERE it sits:  Front Office

WHAT it does:
  ✅ Smart Order Routing (SOR) — find best price across venues
  ✅ Algorithmic execution (TWAP, VWAP, POV)
  ✅ Direct Market Access (DMA) — connect directly to exchanges
  ✅ Real-time market data integration
  ✅ Transaction Cost Analysis (TCA)
  ✅ Dark pool access

OMS vs EMS:
  OMS = "What to trade and manage orders"
  EMS = "How to execute the trade best"
  Often integrated: OEMS (Order & Execution Management System)
```

---

### 1.4 Risk Engine

> **Purpose:** Real-time monitoring of all positions, P&L, and risk limits.

```
WHO uses it:    Risk Managers, Middle Office
WHERE it sits:  Middle Office

WHAT it does:
  ✅ Real-time position aggregation across all books
  ✅ Real-time P&L (Mark-to-Market)
  ✅ VaR calculation (end of day, intraday)
  ✅ Greeks calculation for options/derivatives
  ✅ Limit monitoring and breach alerts
  ✅ Scenario analysis ("what if market drops 10%?")
  ✅ Stress testing

PERFORMANCE REQUIREMENT:
  Must process thousands of trades per second
  Risk checks must complete in < 1 millisecond (for HFT firms)
  Position must always be current — no stale data

POPULAR PRODUCTS:
  Murex, Calypso, Numerix, OpenGamma, in-house builds
```

---

### 1.5 Trade Capture / Booking System

> **Purpose:** Record every executed trade — the "book of record."

```
WHO uses it:    Back Office, Finance
WHERE it sits:  Back Office

WHAT it does:
  ✅ Receive trade confirmations from OMS/EMS
  ✅ Validate trade details
  ✅ Book trade to correct portfolio / legal entity
  ✅ Generate internal trade ID
  ✅ Feed downstream systems (risk, settlement, accounting)
  ✅ Store immutable audit trail

KEY CONCEPT — Straight Through Processing (STP):
  Trade flows automatically from execution to booking
  to settlement — with NO manual intervention.
  
  High STP rate = efficient operations = lower cost
  Low STP rate  = manual work = errors = higher cost

  Target: 95%+ STP rate
```

---

### 1.6 Settlement System

> **Purpose:** Manage the movement of cash and securities on settlement date.

```
WHO uses it:    Settlement / Operations team
WHERE it sits:  Back Office

WHAT it does:
  ✅ Receive booked trades
  ✅ Generate settlement instructions
  ✅ Send instructions to custodians (via SWIFT)
  ✅ Monitor settlement status
  ✅ Handle settlement fails (and penalties)
  ✅ Track cash movements

SETTLEMENT FAIL:
  If one side doesn't deliver by settlement date:
  → Trade "fails"
  → Seller may be fined (CSDR in UK/EU mandates cash penalties)
  → Buy-in process may start (force-buy shares from market)

SETTLEMENT FAIL PENALTY RATES (CSDR):
  Liquid shares:   1 basis point (0.01%) per day
  Illiquid shares: 0.5 basis point per day
  Bonds:           0.1 basis point per day
```

---

### 1.7 Reconciliation System

> **Purpose:** Daily comparison of internal records vs external statements.

```
WHO uses it:    Operations / Reconciliation team
WHERE it sits:  Back Office

WHAT it does:
  ✅ Ingest internal position/cash data
  ✅ Ingest external statements (custodian, broker, CCP)
  ✅ Auto-match records
  ✅ Flag breaks with details
  ✅ Track break resolution
  ✅ Produce daily break reports

POPULAR PRODUCTS:
  SmartStream TLM, Duco, Hazeltree, SS&C Algorithmics

KEY METRIC: Break rate and aged breaks
  Aged break = unresolved > 3 days → escalate
```

---

### 1.8 Accounting System / General Ledger (GL)

> **Purpose:** Official financial books of the firm.

```
WHO uses it:    Finance / Accounting team
WHERE it sits:  Back Office / Finance

WHAT it does:
  ✅ Record all financial transactions (double-entry bookkeeping)
  ✅ Produce P&L statements
  ✅ Produce Balance Sheet
  ✅ Support regulatory capital calculations
  ✅ Tax accounting

FEEDS FROM:
  Trade Capture → accounting entries generated per trade
  Corporate Actions → dividends, coupons posted
  MtM Engine → daily unrealized P&L entries
  Settlement → cash movements posted

POPULAR PRODUCTS:
  SunGard, Oracle Financials, SAP, in-house GL systems
```

---

## 📡 Section 2 — Messaging Protocols

### 2.1 FIX Protocol — Deep Dive

> **FIX = Financial Information eXchange**
> Industry-standard protocol for order and execution messages.

```
VERSION HISTORY:
  FIX 4.0  (1992) — Basic orders
  FIX 4.1  (1995) — Added allocations
  FIX 4.2  (1998) — Most widely used today
  FIX 4.4  (2003) — Added derivatives support
  FIX 5.0  (2006) — Separation of session and application layer
  FIXT 1.1 (2006) — Transport layer for FIX 5.0
```

#### FIX Message Anatomy:

```
Every FIX message has 3 sections:

┌──────────────────────────────────────────────────────┐
│  HEADER          │  BODY              │  TRAILER      │
│                  │                   │               │
│  8 = FIX version │  Message-specific │  10 = Checksum│
│  9 = Body length │  fields           │               │
│  35 = Msg type   │                   │               │
│  49 = Sender     │                   │               │
│  56 = Target     │                   │               │
│  34 = Seq No     │                   │               │
│  52 = Timestamp  │                   │               │
└──────────────────────────────────────────────────────┘
```

#### Complete FIX Tag Reference (most important tags):

| Tag | Field Name | Values / Example |
|-----|-----------|-----------------|
| 8 | BeginString | FIX.4.2 |
| 9 | BodyLength | 150 |
| 35 | MsgType | D=NewOrder, 8=ExecReport, F=Cancel |
| 49 | SenderCompID | FUND_ABC |
| 56 | TargetCompID | BROKER_XYZ |
| 34 | MsgSeqNum | 1001 |
| 52 | SendingTime | 20240524-09:30:00.123 |
| 11 | ClOrdID | Client order ID: ORD-20240524-001 |
| 37 | OrderID | Broker order ID: BRK-88271 |
| 55 | Symbol | AAPL |
| 48 | SecurityID | ISIN: US0378331005 |
| 22 | SecurityIDSource | 4=ISIN, 1=CUSIP |
| 54 | Side | 1=Buy, 2=Sell, 5=SellShort |
| 38 | OrderQty | 1000 |
| 40 | OrdType | 1=Market, 2=Limit, 3=Stop |
| 44 | Price | 180.05 |
| 59 | TimeInForce | 0=Day, 1=GTC, 3=IOC, 4=FOK |
| 39 | OrdStatus | 0=New, 1=PartFill, 2=Filled, 4=Cancelled |
| 32 | LastQty | 500 (qty filled in this report) |
| 31 | LastPx | 180.05 (price of this fill) |
| 14 | CumQty | 1000 (total filled so far) |
| 6 | AvgPx | 180.03 (average fill price) |
| 58 | Text | "Order rejected: price out of range" |
| 10 | CheckSum | 073 |

#### TimeInForce Values (Tag 59):

| Value | Name | Meaning |
|-------|------|---------|
| 0 | Day | Cancel if not filled by end of day |
| 1 | GTC | Good Till Cancelled — stays until filled or cancelled |
| 3 | IOC | Immediate Or Cancel — fill what you can, cancel rest |
| 4 | FOK | Fill Or Kill — fill entire qty immediately or cancel all |
| 6 | GTD | Good Till Date — specify expiry date |

#### FIX Session Management:

```
FIX connection has TWO layers:

1. SESSION LAYER (keeps connection alive):
   Logon    (35=A) → connect and authenticate
   Heartbeat (35=0) → "still alive" ping every 30 seconds
   TestRequest (35=1) → "are you there?" probe
   Resend Request (35=2) → "I missed messages, resend from X"
   Logout   (35=5) → disconnect gracefully

2. APPLICATION LAYER (actual orders/trades):
   New Order Single (35=D)
   Execution Report (35=8)
   etc.

Sequence Numbers:
  Every message has a SeqNum (Tag 34)
  Increments by 1 each message
  If SeqNum gap detected → Resend Request issued
  Reset to 1 at start of each trading day
```

---

### 2.2 SWIFT — Deep Dive

> **SWIFT = Society for Worldwide Interbank Financial Telecommunication**
> Secure messaging network for banks, used for settlements and payments.

```
NOT a payment system — just a MESSAGING system.
Banks use SWIFT to send instructions and confirmations.
Actual money moves via central bank systems (Fedwire, CHAPS, TARGET2).

Network: 11,000+ financial institutions in 200+ countries
Message volume: ~40 million messages per day

SWIFT ADDRESS = BIC (Bank Identifier Code):
  BARCGB22XXX
  │   │  │ │
  │   │  │ └── Branch code (XXX = head office)
  │   │  └──── Location code (22 = London)
  │   └──────── Country code (GB = UK)
  └──────────── Bank code (BARC = Barclays)
```

#### Key SWIFT Message Categories for Trading:

```
MT5xx — Securities Messages (most relevant for trade life cycle)

MT502  → Order to buy/sell (old, rarely used now)
MT509  → Trade status message
MT515  → Client confirmation of purchase/sale
MT518  → Market-side securities trade confirmation
MT540  → Receive free (receive securities, no payment)
MT541  → Receive against payment (DVP — receive shares, pay cash)
MT542  → Deliver free (send securities, no payment)
MT543  → Deliver against payment (DVP — deliver shares, receive cash)
MT544  → Receive free confirmation
MT545  → Receive against payment confirmation
MT546  → Deliver free confirmation
MT547  → Deliver against payment confirmation
MT548  → Settlement status and processing advice
MT564  → Corporate action notification
MT565  → Corporate action instruction (your election)
MT566  → Corporate action confirmation
MT900  → Confirmation of debit (cash)
MT910  → Confirmation of credit (cash)
MT950  → Statement message (account statement)
```

#### MT541 Example — Receive Against Payment:

```
Buyer instructs custodian to receive 1000 Apple shares
and pay $180,050 on settlement date.

:20:  SETT20240525001        (Reference number)
:23G: NEWM                   (New message)
:98A: :SETT//20240525        (Settlement date: 25 May 2024)
:35B: ISIN US0378331005      (Instrument: Apple ISIN)
      //AAPL                  (Ticker)
:36B: :SETT//UNIT/1000,      (Quantity: 1000 shares)
:97A: :SAFE//ACC12345        (Your safekeeping account)
:19A: :SETT//USD180050,      (Settlement amount: $180,050)
:95P: :DEAG//GSUSUS33XXX    (Delivering agent: Goldman SWIFT BIC)
```

---

### 2.3 ISO 20022 — The Modern Standard

> **ISO 20022** = Next-generation financial messaging standard.
> Replacing SWIFT MT messages and FIX in many areas.

```
OLD (SWIFT MT / FIX):          NEW (ISO 20022):
  Text-based, tag-value           XML-based, structured
  Limited data fields             Rich data model
  Ambiguous in places             Unambiguous, standardized
  Hard to extend                  Extensible by design

ADOPTION:
  SWIFT CBPR+ (cross-border payments) → 2025 mandatory
  UK CHAPS payments → migrated 2023
  Eurosystem TARGET2 → migrated 2023
  US FedNow → built on ISO 20022
  MiFID II transaction reporting → ISO 20022 format
  EMIR trade reporting → ISO 20022 format

KEY MESSAGE FAMILIES:
  pain.xxx → Payment Initiation
  pacs.xxx → Payments Clearing & Settlement
  camt.xxx → Cash Management
  sese.xxx → Securities Settlement
  setr.xxx → Securities Trade
  acmt.xxx → Account Management
  auth.xxx → Regulatory Reporting (MiFID II / EMIR)
```

#### ISO 20022 vs SWIFT MT Comparison:

| Feature | SWIFT MT | ISO 20022 |
|---------|---------|-----------|
| Format | Text / Tag-Value | XML / JSON |
| Data richness | Limited | Very rich |
| LEI support | No | Yes (native) |
| Extensible | Hard | Easy |
| Machine readable | Partial | Fully |
| Used for | Existing settlements | New systems, regulatory |

---

### 2.4 Message Queuing — MQ & Kafka

> Financial systems communicate asynchronously using **message queues**.

```
WHY MESSAGE QUEUES?

Trading systems cannot afford to wait.
If System A sends data to System B directly:
  → If B is down, A is blocked ❌
  → If B is slow, A slows down ❌

With a message queue:
  A sends message to queue → immediately continues ✅
  B reads from queue when ready ✅
  No direct coupling between A and B ✅
```

#### IBM MQ (Traditional):

```
Used by: Most large banks and financial institutions
Type: Point-to-point messaging
Use case: Critical, guaranteed delivery (settlements, trade booking)

Features:
  • Guaranteed delivery (persistent messages)
  • Exactly-once delivery
  • Transaction support
  • Used for SWIFT connectivity

Common in:
  OMS → Trade Capture
  Settlement → Custodian interface
  Corporate Actions distribution
```

#### Apache Kafka (Modern):

```
Used by: Modern trading platforms, market data
Type: Publish-subscribe, event streaming
Use case: High-volume, real-time streaming

Features:
  • Millions of messages/second
  • Replay messages (replay trade events)
  • Multiple consumers per topic
  • Distributed and fault-tolerant

Common in:
  Market data feeds (real-time prices)
  Real-time risk calculation
  Audit log streaming
  Trade event streaming
```

#### MQ vs Kafka:

| Feature | IBM MQ | Apache Kafka |
|---------|--------|-------------|
| Pattern | Point-to-point / Pub-sub | Pub-sub / Event stream |
| Throughput | Moderate | Very high |
| Message retention | Until consumed | Configurable (days/forever) |
| Replay | No | Yes |
| Ordering | Per queue | Per partition |
| Best for | Critical guaranteed delivery | High-volume streaming |
| In finance | Trade booking, settlement | Market data, risk streaming |

---

## 🔖 Section 3 — Reference Data

> **Reference data** = static data that describes financial instruments,
> counterparties, and market infrastructure.
> Without correct reference data — nothing works.

---

### 3.1 Instrument Identifiers

Every financial instrument has **unique identifiers** — like a passport for a security.

#### ISIN — International Securities Identification Number

```
Format: 2-letter country code + 10 alphanumeric + 1 check digit
Length: 12 characters

Examples:
  US0378331005  → Apple Inc (US listed)
  GB0002634946  → Barclays (UK listed)
  US912828ZT01  → US Treasury Bond

Country codes:
  US = United States
  GB = United Kingdom
  DE = Germany
  XS = International (Eurobonds)

Used in:
  SWIFT messages (field 35B)
  FIX messages (Tag 48, SecurityIDSource=4)
  MiFID II reporting
  EMIR reporting
```

#### CUSIP — Committee on Uniform Securities Identification Procedures

```
Format: 9 characters (alphanumeric)
Used:   US and Canada ONLY

Example:
  037833100 → Apple Inc

Structure:
  Characters 1-6: Issuer code
  Characters 7-8: Issue code
  Character 9:    Check digit

Note: CUSIP is embedded in ISIN
  US037833100 5 ← ISIN for Apple
     │└──┘
     │ CUSIP (037833100)
     US + CUSIP + check digit = ISIN
```

#### SEDOL — Stock Exchange Daily Official List

```
Format: 7 characters
Used:   UK and international securities

Example:
  0263494 → Barclays (UK)
  B3KWKK9 → International instrument

Assigned by: London Stock Exchange
Used in: UK and European trade reporting
```

#### FIGI — Financial Instrument Global Identifier

```
Format: 12 characters (alphanumeric)
Free and open standard (unlike CUSIP which requires license)

Example:
  BBG000B9XRY4 → Apple Inc (NASDAQ)
  BBG000C05GD5 → Apple Inc (Frankfurt listing)

Note: Same company, different listings = different FIGI
      (Because it's exchange-specific)

Assigned by: Bloomberg
```

#### Identifier Comparison:

| ID | Scope | Exchange-specific? | Free? | Used in |
|----|-------|--------------------|-------|---------|
| **ISIN** | Global | No (issuer level) | Yes | SWIFT, MiFID II, EMIR |
| **CUSIP** | US/Canada | No | No (license fee) | US trade confirmation |
| **SEDOL** | UK/Global | No | No | UK reporting |
| **FIGI** | Global | Yes (per listing) | Yes | Bloomberg systems |
| **RIC** | Global | Yes | No | Reuters/Refinitiv |
| **Ticker** | Exchange | Yes | Yes | OMS, EMS display |

---

### 3.2 Counterparty Identifiers

#### LEI — Legal Entity Identifier

```
Format: 20 alphanumeric characters
Purpose: Unique global ID for ANY legal entity trading in markets

Example:
  5493006MHB84DD0ZWV18 → JP Morgan Chase
  VWMYAEQSTOPNV0SUGU82 → Goldman Sachs

Structure:
  Characters 1-4:   LOU (Local Operating Unit) prefix
  Characters 5-18:  Entity-specific ID
  Characters 19-20: Check digits

Mandatory for:
  MiFID II transaction reporting
  EMIR trade reporting
  Dodd-Frank swap reporting

Renewal: Must be renewed ANNUALLY
          Expired LEI = cannot trade / report ⚠️

Issued by: GLEIF accredited LOUs
           In UK: London Stock Exchange (as LOU)
```

#### BIC — Bank Identifier Code (SWIFT address)

```
Format: 8 or 11 characters
Used for: SWIFT messaging routing

Example:
  BARCGB22    → Barclays UK (8-char = head office)
  BARCGB22LON → Barclays London branch (11-char)

Parts:
  BARC  → Institution code (4 chars)
  GB    → Country code (2 chars)
  22    → Location code (2 chars)
  LON   → Branch code (3 chars, optional)
```

---

### 3.3 Venue Identifiers

#### MIC — Market Identifier Code

```
Format: 4 characters (ISO 10383 standard)
Purpose: Identifies trading venues (exchanges, dark pools, MTFs)

Examples:
  XNYS → New York Stock Exchange
  XNAS → NASDAQ
  XLON → London Stock Exchange
  XCME → Chicago Mercantile Exchange
  XEUR → Eurex (derivatives)
  BATE → BATS Europe (MTF)

Used in:
  MiFID II transaction reporting (execution venue)
  FIX messages (Tag 207: SecurityExchange)
  Best execution reporting
```

---

### 3.4 Static Data Management

> **Static data** = reference data that changes infrequently but must always be accurate.

```
WHAT IS STATIC DATA?

Instrument Static Data:
  • ISIN, CUSIP, SEDOL, FIGI
  • Instrument name, type (equity, bond, future)
  • Currency
  • Exchange / listing venue (MIC)
  • Lot size, tick size
  • Settlement convention (T+1 or T+2)
  • Country of issue
  • Issuer details

Counterparty Static Data:
  • LEI
  • BIC (SWIFT address)
  • Legal name, registered address
  • ISDA agreement reference
  • Credit limits
  • Regulatory classification (FC, NFC under EMIR)

Market / Calendar Data:
  • Trading calendars (holidays per market)
  • Settlement calendars
  • Daylight saving time schedules

WHY IT MATTERS:
  Trade booked with wrong settlement convention → settlement fail
  Wrong LEI on report → regulatory breach
  Wrong holiday calendar → wrong T+2 date calculated
```

#### Static Data Lifecycle:

```
NEW INSTRUMENT (e.g., company IPOs):

1. IPO announced → ISIN assigned by national body
2. CUSIP/SEDOL/FIGI assigned
3. Instrument created in static data system
4. Data validated (pricing source linked, MIC confirmed)
5. Distributed to all downstream systems
   (OMS, Risk, Settlement, Recon, Reporting)
6. Available for trading

CORPORATE ACTION CHANGES instrument data:
  Stock split → share count, price multiplier updated
  Name change → instrument name updated
  ISIN change → new ISIN linked to old

DATA VENDORS:
  Bloomberg, Refinitiv (LSEG), FactSet, SIX Financial
  They aggregate and normalize static data from global sources
```

---

## 🔄 Section 4 — End-to-End Data Flow

### 4.1 Full Trade Data Flow (Systems View)

```
STEP 1: INVESTMENT DECISION
  Portfolio Manager → OMS
  "Buy 10,000 Apple (ISIN: US0378331005)"

  Data: Order object created
  Fields: ISIN, side=BUY, qty=10000, type=LIMIT, price=180.00

         │
         ▼

STEP 2: PRE-TRADE CHECK
  OMS → Risk Engine (sync, milliseconds)
  "Does this order breach any limits?"

  Risk Engine checks:
    Position limits (static data: limit per instrument)
    Exposure limits (current position from Position System)
    Credit check (counterparty credit from static data)

  Response: APPROVED / REJECTED

         │ (if approved)
         ▼

STEP 3: ORDER ROUTING
  OMS → FIX Engine → Broker EMS

  FIX Message (35=D New Order Single):
    Tag 55: AAPL
    Tag 48: US0378331005 (ISIN)
    Tag 54: 1 (Buy)
    Tag 38: 10000
    Tag 40: 2 (Limit)
    Tag 44: 180.00

         │
         ▼

STEP 4: EXECUTION
  Broker routes to NASDAQ (MIC: XNAS)
  Matching Engine fills order

  FIX Message back (35=8 Execution Report):
    Tag 39: 2 (Filled)
    Tag 32: 10000 (qty filled)
    Tag 31: 180.02 (fill price)
    Tag 75: 20240524 (trade date)

         │
         ▼

STEP 5: TRADE CAPTURE
  OMS → Trade Capture System (via MQ)

  Trade object:
    TradeID:     TRD-20240524-00442
    ISIN:        US0378331005
    Side:        BUY
    Qty:         10,000
    Price:       180.02
    TradeDate:   2024-05-24
    SettleDate:  2024-05-25 (T+1)
    Counterparty: Goldman Sachs (LEI: PJLZZVSITHA6CUZVS197)
    Venue:       XNAS

         │
         ▼ (fan out to multiple systems via MQ/Kafka)

STEP 6: PARALLEL PROCESSING

  → Risk Engine
     Position updated: +10,000 AAPL
     Unrealized P&L calculated
     VaR recalculated

  → Settlement System
     Settlement instruction prepared
     SWIFT MT541 generated (receive against payment)
     Sent to custodian for settlement on T+1

  → Accounting System
     Accounting entry generated:
       DR Securities (AAPL)  $1,800,200
       CR Cash Payable        $1,800,200

  → Regulatory Reporting
     MiFID II transaction report prepared
     Must be filed with FCA by T+1

         │
         ▼

STEP 7: SETTLEMENT (T+1 = Next Day)
  Custodian sends SWIFT MT545 (receipt confirmation)
  10,000 AAPL shares moved from Goldman's DTC account
  to Fund's DTC account (DVP — simultaneous cash)

  Settlement System updates: SETTLED ✅

         │
         ▼

STEP 8: RECONCILIATION (EOD)
  Reconciliation System:
    Internal position: 10,000 AAPL ↔ Custodian: 10,000 AAPL ✅ Match

STEP 9: EOD PROCESSES
  Closing price pulled (Bloomberg): $181.50
  MtM P&L: (181.50 - 180.02) × 10,000 = +$14,800 unrealized ✅
```

---

### 4.2 Market Data Flow

```
EXCHANGES / VENUES
  NYSE, NASDAQ, LSE publish prices in real-time

         │
         ▼

DATA VENDORS
  Bloomberg, Refinitiv aggregate from all venues
  Normalize into standard format

         │
         ▼ (via APIs, direct feeds)

INTERNAL MARKET DATA PLATFORM
  Distributes internally via Kafka topics or MQ
  Ultra-low latency for algo trading

         │
         ▼ (fan out)

  OMS / EMS    → display prices to traders
  Risk Engine  → real-time P&L, margin
  Pricing Engine → derivatives pricing (Black-Scholes, etc.)
  Recon System → closing prices for EOD recon
  Regulatory   → trade price validation
```

---

## 🏛️ Section 5 — Architecture Patterns in Trading Systems

### 5.1 Low-Latency Architecture (HFT / Algo Trading)

```
Requirements:
  Order to exchange in < 10 microseconds (HFT)
  Risk check in < 1 microsecond

Techniques:
  • Co-location: servers physically inside exchange data center
  • Kernel bypass networking (DPDK, RDMA)
  • Lock-free data structures
  • FPGAs for order processing
  • Avoid Java GC pauses (use C++, or Java with careful GC tuning)
  • Pre-allocated memory — no runtime allocation
  • CPU pinning and NUMA awareness
```

### 5.2 Event-Driven Architecture

```
Trading systems are naturally event-driven:

Events:
  OrderPlaced → OrderRouted → OrderFilled
  TradeBooked → PositionUpdated → RiskChecked
  PriceUpdated → MtMRecalculated → LimitChecked
  DividendAnnounced → PositionAdjusted → CashPosted

Pattern:
  Each event published to Kafka / MQ
  Multiple consumers react independently
  Loose coupling between systems
  Full audit trail (every event stored)

Benefits:
  • Systems can be added/changed without rewiring
  • Full replay of history
  • Easy to debug ("what happened at 10:32:05?")
```

### 5.3 Straight Through Processing (STP)

```
GOAL: Trade flows from execution to settlement
      with ZERO manual intervention.

HIGH STP (good):
  Trade executed
      → Auto-booked ✅
      → Auto-allocated ✅
      → SWIFT auto-generated ✅
      → Auto-matched in recon ✅
      → Settled ✅

LOW STP (bad):
  Trade executed
      → Manual booking needed ❌
      → Manual allocation ❌
      → SWIFT manually generated ❌
      → Manual recon ❌

STP failures are caused by:
  • Missing static data (no ISIN set up)
  • Enrichment failures (wrong counterparty LEI)
  • Validation failures (price out of tolerance)
  • System connectivity issues
```

### 5.4 Disaster Recovery in Trading Systems

```
RTO (Recovery Time Objective): How fast must we recover?
  Trading systems: RTO = minutes (some: seconds)

RPO (Recovery Point Objective): How much data can we lose?
  Trading systems: RPO = 0 (zero data loss acceptable)

Approach:
  Active-Active: Two data centers, both running simultaneously
  Primary fails → secondary takes over in < 30 seconds
  
  Regulatory requirement:
  FCA and SEC require documented DR plans
  Must be tested regularly (usually annually)
```

---

## 🧠 Quick Revision — Phase 5 Key Terms

| Term | One-Line Explanation |
|------|---------------------|
| **OMS** | Manages order lifecycle from creation to fill |
| **EMS** | Executes orders — smart routing, algos, DMA |
| **STP** | Trade flows automatically with no manual steps |
| **FIX Protocol** | Standard messaging language for order routing |
| **SWIFT** | Secure bank messaging network (not a payment system) |
| **ISO 20022** | Modern XML-based messaging standard replacing SWIFT MT |
| **IBM MQ** | Guaranteed delivery queue — used for critical trade messages |
| **Apache Kafka** | High-volume event streaming — used for market data, risk |
| **ISIN** | 12-char global instrument ID |
| **CUSIP** | 9-char US/Canada instrument ID |
| **SEDOL** | 7-char UK instrument ID |
| **FIGI** | 12-char open global instrument ID (Bloomberg) |
| **LEI** | 20-char global legal entity ID — mandatory for reporting |
| **BIC** | 8/11-char SWIFT bank address |
| **MIC** | 4-char market/venue identifier |
| **Static Data** | Reference data: instruments, counterparties, calendars |
| **Market Data** | Real-time prices from exchanges via Bloomberg/Refinitiv |
| **RTO** | Max time allowed to recover after system failure |
| **RPO** | Max data loss allowed in a disaster |
| **Co-location** | Servers placed inside exchange for ultra-low latency |

---

## ✅ Phase 5 — Completion Checklist

- [ ] I can name all major systems in a trading firm and their purpose
- [ ] I understand what OMS does vs what EMS does
- [ ] I know what STP is and why it matters
- [ ] I can read a FIX message and identify key tags
- [ ] I know the difference between FIX, SWIFT, and ISO 20022
- [ ] I understand when to use IBM MQ vs Apache Kafka
- [ ] I know what ISIN, CUSIP, SEDOL, FIGI, LEI, BIC, MIC are
- [ ] I can explain static data and why it's critical
- [ ] I can trace a trade from decision to settlement across systems
- [ ] I understand event-driven architecture in trading systems
- [ ] I know what co-location means and why HFT firms use it

---

## 🎓 Series Complete — What You've Mastered

```
✅ Phase 1 — Financial Markets Foundations
   Markets, participants, asset classes, instruments

✅ Phase 2 — Core Trade Life Cycle
   Pre-trade, execution (FIX), clearing (DTCC/LCH), settlement (T+1/T+2)

✅ Phase 3 — Risk & Compliance
   Market/credit/ops risk, margin, Dodd-Frank, MiFID II, EMIR

✅ Phase 4 — Back Office & Operations
   Corporate actions, reconciliation, P&L, MtM, EOD process

✅ Phase 5 — Systems & Tech Context
   OMS/EMS/Risk/Settlement, FIX/SWIFT/ISO20022, ISIN/LEI/MIC
```

### You Are Now Ready For:
```
  • Capital markets software engineering roles
  • Trade lifecycle business analyst interviews
  • System design discussions in finance interviews
  • Understanding requirements in any trading system project
```

---

*📌 Series: Trade Life Cycle — Business Knowledge for Software Engineers*
*📌 Scope: US & UK Capital Markets*
