# Functional Specification Document
## Hospitality Analytics Dashboard — Solution Design

| Field | Value |
|-------|-------|
| Phase | Phase 2 — Elaboration / Solution Design |
| Version | 2.0 |
| Date | June 2026 |
| Methodology | AI-DLC |

---

## 1. Star Schema Design

### 1.1 Schema Overview

The data model follows a Star Schema pattern with one central fact table
(`fct_Bookings`) connected to six dimension tables. Three dimensions share
the same grain as the fact table (1:1 on BookingID) and act as fact extensions
holding specialized metrics. Two dimensions are true lookup tables (Hotel, Manager),
and one is a generated calendar table (Date).

### 1.2 Schema Diagram

```
                         ┌─────────────────────────────────┐
                         │          dim_Date                │
                         │─────────────────────────────────│
                         │ PK  Date           (date)       │
                         │     Year           (int)        │
                         │     Quarter        (int)        │
                         │     QuarterLabel   (text)       │
                         │     Month          (int)        │
                         │     MonthName      (text)       │
                         │     MonthShort     (text)       │
                         │     DayOfMonth     (int)        │
                         │     WeekDay        (int)        │
                         │     WeekDayName    (text)       │
                         │     IsWeekend      (bool)       │
                         │     YearMonth      (text)       │
                         │     YearQuarter    (text)       │
                         │     FiscalYear     (int)        │
                         │     FiscalQuarter  (int)        │
                         │     FiscalYearLabel(text)       │
                         │     MonthYear      (text)       │
                         └────────────────┬────────────────┘
                                          │
                                          │ Many : 1
                                          │ Single direction (→)
                                          │
┌───────────────────────┐                 │                 ┌───────────────────────┐
│     dim_Hotel         │                 │                 │     dim_Manager       │
│───────────────────────│                 │                 │───────────────────────│
│ PK Hotel_Name (text)  │◄─── 1:1 ───────┼────────────────►│ PK HotelName  (text)  │
│    City       (text)  │  Bidirectional  │                 │    ManagerID  (text)  │
│    Category   (text)  │                 │                 │    ManagerName(text)  │
│                       │                 │                 │    ManagerEmail(text) │
└───────────┬───────────┘                 │                 └───────────────────────┘
            │                             │
            │ Many : 1                    │
            │ Single direction (→)        │
            │                             │
┌───────────┴─────────────────────────────┴─────────────────────────────────────────┐
│                              fct_Bookings                                          │
│───────────────────────────────────────────────────────────────────────────────────│
│ PK  BookingID       (int)          │  FK  HotelName      (text) → dim_Hotel      │
│     CustomerID      (text)         │  FK  CheckInDate    (date) → dim_Date       │
│     FirstName       (text)         │                                              │
│     LastName        (text)         │      PaymentMethod  (text)                   │
│     GuestName       (text)         │      Channel        (text)                   │
│     Gender          (text)         │      DeviceType     (text)                   │
│     Age             (int)          │      PaymentStatus  (text)                   │
│     Country         (text)         │      IsPaid         (int) [calculated 0/1]   │
│     RoomType        (text)         │      RoomRevenue    (decimal) [calculated]   │
│     CheckInDate     (date)         │                                              │
│     CheckOutDate    (date)         │                                              │
│     NightsStayed    (int)          │                                              │
│     RatePerNight    (decimal)      │                                              │
│     MemberType      (text)         │                                              │
│     City            (text)         │                                              │
│     Category        (text)         │                                              │
└────────┬─────────────────┬─────────────────┬──────────────────────────────────────┘
         │                 │                 │
         │ 1:1             │ 1:1             │ 1:1
         │ Bidirectional   │ Bidirectional   │ Bidirectional
         │                 │                 │
┌────────┴──────────┐ ┌───┴───────────────┐ ┌┴──────────────────────┐
│   dim_Finance     │ │   dim_Feedback    │ │  dim_OnlineBooking    │
│───────────────────│ │───────────────────│ │───────────────────────│
│PK BookingID (int) │ │PK BookingID (int) │ │PK BookingID    (int)  │
│   CustomerID(text)│ │   CustomerID(text)│ │   CustomerID   (text) │
│   DiscountPercent │ │   Rating   (dec)  │ │   Channel      (text) │
│            (int)  │ │   Feedback (text) │ │   GuestEmail   (text) │
│   TaxAmount(dec)  │ │   SubmittedDate   │ │   BookingDate  (date) │
│   TotalRevenue    │ │            (date) │ │   Location     (text) │
│           (dec)   │ │   StaffRating     │ │   DeviceType   (text) │
│   Cost     (dec)  │ │           (dec)   │ │                       │
│   AdditionalSvc   │ │   CleanlinessRtg  │ │                       │
│     Cost   (dec)  │ │           (dec)   │ │                       │
│   AdditionalSvc   │ │   GuestSatisf.    │ │                       │
│     Revenue(dec)  │ │     Score  (dec)  │ │                       │
│   DiscountsGiven  │ │   FeedbackSenti-  │ │                       │
│           (dec)   │ │     ment   (text) │ │                       │
│   NetProfit(dec)  │ │                   │ │                       │
│   ProfitMargin    │ │                   │ │                       │
│     Pct    (dec)  │ │                   │ │                       │
└───────────────────┘ └───────────────────┘ └───────────────────────┘
```

---

## 2. Relationships — Complete Specification

### 2.1 Relationship Table

| ID | From Table | From Column | To Table | To Column | Cardinality | Cross-Filter Direction | Active | Purpose |
|----|-----------|-------------|----------|-----------|-------------|----------------------|--------|---------|
| R1 | fct_Bookings | CheckInDate | dim_Date | Date | Many : 1 | Single (fct → dim) | Yes | Time intelligence on check-in date |
| R2 | fct_Bookings | HotelName | dim_Hotel | Hotel_Name | Many : 1 | Single (fct → dim) | Yes | Hotel filtering and grouping |
| R3 | dim_Hotel | Hotel_Name | dim_Manager | HotelName | 1 : 1 | Both (Bidirectional) | Yes | RLS propagation from Manager to Hotel |
| R4 | fct_Bookings | BookingID | dim_Finance | BookingID | 1 : 1 | Both (Bidirectional) | Yes | Financial metrics per booking |
| R5 | fct_Bookings | BookingID | dim_Feedback | BookingID | 1 : 1 | Both (Bidirectional) | Yes | Ratings and feedback per booking |
| R6 | fct_Bookings | BookingID | dim_OnlineBooking | BookingID | 1 : 1 | Both (Bidirectional) | Yes | Channel/booking details per booking |

### 2.2 Relationship Design Decisions

**Why Bidirectional on R3 (Hotel ↔ Manager)?**
- Required for RLS propagation. When dim_Manager is filtered by USERPRINCIPALNAME(), that filter must flow backward to dim_Hotel and then forward to fct_Bookings.
- Without bidirectional, the RLS filter on dim_Manager would not reach the fact table.

**Why Bidirectional on R4, R5, R6 (Fact ↔ Extensions)?**
- These are 1:1 relationships at the same grain. Bidirectional allows measures defined on either table to filter correctly regardless of which side hosts the visual field.
- Since they are 1:1, there is no ambiguity risk.

**Why Single direction on R1, R2 (Fact → Dim)?**
- Standard star schema pattern. The dimension filters the fact, not the reverse.
- Prevents unintended filter propagation that could create ambiguity.

### 2.3 Inactive Relationships (Future Use)

| From | To | Purpose |
|------|-----|---------|
| fct_Bookings[CheckOutDate] | dim_Date[Date] | Analysis by checkout date (activate with USERELATIONSHIP) |
| dim_OnlineBooking[BookingDate] | dim_Date[Date] | Analysis by booking date (activate with USERELATIONSHIP) |

These are not created by default but documented for future enhancement.

---

## 3. Complete Data Dictionary

### 3.1 fct_Bookings (Fact Table)

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | BookingID | Integer | Source CSV | No | Unique booking transaction identifier | Primary Key; range 10001–109999 |
| 2 | CustomerID | Text | Source CSV | No | Customer identifier | Format: CUST00001–CUST00200; 200 unique guests |
| 3 | FirstName | Text | Source CSV | No | Guest first name | 147 unique values |
| 4 | LastName | Text | Source CSV | No | Guest last name | 78 unique values |
| 5 | GuestName | Text | Source CSV | No | Full guest name | Concatenation of First + Last |
| 6 | Gender | Text | Source CSV | No | Guest gender | Values: Male, Female |
| 7 | Age | Integer | Source CSV | No | Guest age at time of booking | Range: 18–85 |
| 8 | Country | Text | Source CSV | No | Guest nationality | 10 countries; India dominant (40%) |
| 9 | RoomType | Text | Source CSV | No | Room category booked | Values: Standard (50%), Deluxe (35%), Suite (15%) |
| 10 | CheckInDate | Date | Transformed | No | Check-in date (parsed from DD-MM-YYYY) | Primary date for time intelligence; FK to dim_Date |
| 11 | CheckOutDate | Date | Transformed | No | Check-out date (parsed from DD-MM-YYYY) | Always ≥ CheckInDate |
| 12 | NightsStayed | Integer | Source CSV | No | Number of nights | Values: 1, 2, 3, 4, 5 |
| 13 | RatePerNight | Decimal | Source CSV | No | Room rate charged per night (₹) | Range: ₹1,140–₹102,881 |
| 14 | MemberType | Text | Source CSV | No | Loyalty membership status | Values: Member (55%), Non-Member (45%) |
| 15 | HotelName | Text | Source CSV | No | Hotel property name | FK to dim_Hotel; 7 values |
| 16 | City | Text | Source CSV | No | Hotel city location | 7 cities |
| 17 | Category | Text | Source CSV | No | Hotel category | Values: Luxury, Heritage, Business, Resort |
| 18 | PaymentMethod | Text | Source CSV | No | Payment mode used | Values: Credit Card, Cash, UPI, Debit Card, Net Banking |
| 19 | Channel | Text | Source CSV | No | Booking source channel | Values: Website (45%), App (25%), Agent (17%), ThirdParty (13%) |
| 20 | DeviceType | Text | Source CSV | No | Device used for booking | Values: Mobile, Desktop |
| 21 | PaymentStatus | Text | Source CSV | No | Payment outcome | Values: Paid (90%), Pending (8%), Failed (2%) |
| 22 | RoomRevenue | Decimal | Calculated | No | NightsStayed × RatePerNight | Power Query calculated column |
| 23 | IsPaid | Integer | Calculated | No | 1 if PaymentStatus="Paid", else 0 | Helper column for efficient filtering |

### 3.2 dim_Finance

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | BookingID | Integer | Source CSV | No | PK/FK to fct_Bookings | 1:1 match with fact table |
| 2 | CustomerID | Text | Source CSV | No | Customer identifier | Matches fact table |
| 3 | DiscountPercent | Integer | Source CSV | No | Discount tier applied | Values: 0, 5, 10, 15, 20 (evenly distributed) |
| 4 | TaxAmount | Decimal | Source CSV | No | Tax charged on booking (₹) | Average: ₹5,107 |
| 5 | TotalRevenue | Decimal | Source CSV | No | Total revenue including services (₹) | Range: ₹4,720–₹474,397; Avg: ₹63,434 |
| 6 | Cost | Decimal | Source CSV | No | Operational cost incurred (₹) | Range: ₹1,923–₹204,276; Avg: ₹28,529 |
| 7 | AdditionalServiceCost | Decimal | Source CSV | No | Cost of additional services (₹) | Average: ₹675 |
| 8 | AdditionalServiceRevenue | Decimal | Source CSV | No | Revenue from additional services (₹) | Average: ₹1,248 |
| 9 | DiscountsGiven | Decimal | Source CSV | No | Monetary discount amount (₹) | Average: ₹6,331 |
| 10 | NetProfit | Decimal | Calculated | No | Revenue − Cost − AddSvcCost + AddSvcRev − Discounts | Power Query calculated column |
| 11 | ProfitMarginPct | Decimal | Calculated | No | (NetProfit / TotalRevenue) × 100 | Power Query calculated column |

### 3.3 dim_Feedback

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | BookingID | Integer | Source CSV | No | PK/FK to fct_Bookings | 1:1 match |
| 2 | CustomerID | Text | Source CSV | No | Customer identifier | Matches fact |
| 3 | Rating | Decimal | Source CSV | No | Overall rating | Scale: 1.5–5.0 (36 distinct values) |
| 4 | Feedback | Text | Source CSV | No | Feedback category text | 10 distinct categories |
| 5 | SubmittedDate | Date | Transformed | No | Feedback submission date | Parsed from DD-MM-YYYY |
| 6 | StaffRating | Decimal | Source CSV | No | Staff service rating | Scale: 1.0–5.0 (40 distinct values) |
| 7 | CleanlinessRating | Decimal | Source CSV | No | Cleanliness/hygiene rating | Scale: 1.0–5.0 (41 distinct values) |
| 8 | GuestSatisfactionScore | Decimal | Calculated | No | (Rating + StaffRating + CleanlinessRating) / 3 | Simple average |
| 9 | FeedbackSentiment | Text | Calculated | No | Positive or Negative classification | Negative = Average, Could be better, Check-in was slow |

### 3.4 dim_OnlineBooking

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | BookingID | Integer | Source CSV | No | PK/FK to fct_Bookings | 1:1 match |
| 2 | CustomerID | Text | Source CSV | No | Customer identifier | Matches fact |
| 3 | Channel | Text | Source CSV | No | Booking channel | Website, App, Agent, ThirdParty |
| 4 | GuestEmail | Text | Source CSV | Yes | Masked guest email | Masked to "abc***@domain.com" or removed |
| 5 | BookingDate | Date | Transformed | No | Date booking was made | Parsed from DD-MM-YYYY |
| 6 | Location | Text | Source CSV | No | Hotel property selected | 7 Taj properties |
| 7 | DeviceType | Text | Source CSV | No | Device used | Mobile, Desktop |

### 3.5 dim_Hotel

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | Hotel_Name | Text | Excel | No | Official hotel property name | PK; 7 values |
| 2 | City | Text | Excel | No | City location | 7 cities |
| 3 | Category | Text | Excel | No | Hotel classification | Luxury, Heritage, Business, Resort |

### 3.6 dim_Manager

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | HotelName | Text | Excel | No | Hotel name (PK/FK to dim_Hotel) | 1:1 with Hotel_Name |
| 2 | ManagerID | Text | Excel | No | Manager identifier | MGR001–MGR007 |
| 3 | ManagerName | Text | Excel | No | Manager full name | Used in dynamic titles |
| 4 | ManagerEmail | Text | Excel | No | Official email | Used for RLS via USERPRINCIPALNAME() |

### 3.7 dim_Date

| # | Column Name | Data Type | Source | Nullable | Description | Business Rule |
|---|-------------|-----------|--------|----------|-------------|---------------|
| 1 | Date | Date | Generated | No | Calendar date (PK) | 01-Jan-2021 to 31-Dec-2025 (1,826 rows) |
| 2 | Year | Integer | Calculated | No | Calendar year | 2021, 2022, 2023, 2024, 2025 |
| 3 | Quarter | Integer | Calculated | No | Quarter number | 1, 2, 3, 4 |
| 4 | QuarterLabel | Text | Calculated | No | Quarter display | "Q1", "Q2", "Q3", "Q4" |
| 5 | Month | Integer | Calculated | No | Month number | 1–12 |
| 6 | MonthName | Text | Calculated | No | Full month name | January–December |
| 7 | MonthShort | Text | Calculated | No | 3-letter abbreviation | Jan–Dec |
| 8 | DayOfMonth | Integer | Calculated | No | Day of month | 1–31 |
| 9 | WeekDay | Integer | Calculated | No | Day of week (Mon=1) | 1–7 |
| 10 | WeekDayName | Text | Calculated | No | Full weekday name | Monday–Sunday |
| 11 | IsWeekend | Boolean | Calculated | No | Weekend flag | TRUE if Sat/Sun |
| 12 | YearMonth | Text | Calculated | No | Sortable year-month | "2021-01" format |
| 13 | YearQuarter | Text | Calculated | No | Year-Quarter display | "2021-Q1" format |
| 14 | FiscalYear | Integer | Calculated | No | Indian FY (Apr–Mar) | FY2022 = Apr2021–Mar2022 |
| 15 | FiscalQuarter | Integer | Calculated | No | Fiscal quarter | Q1=Apr-Jun, Q2=Jul-Sep, Q3=Oct-Dec, Q4=Jan-Mar |
| 16 | FiscalYearLabel | Text | Calculated | No | FY display | "FY22", "FY23", etc. |
| 17 | MonthYear | Text | Calculated | No | Display format | "Jan 2021" |

---

## 4. Business Rules

### 4.1 Revenue Calculation Rules

| Rule ID | Rule Name | Definition | Applies To |
|---------|-----------|------------|------------|
| BR-01 | Paid Bookings Filter | All revenue, cost, and profit metrics include ONLY records where fct_Bookings[PaymentStatus] = "Paid" | All financial measures |
| BR-02 | Net Profit Formula | NetProfit = TotalRevenue − Cost − AdditionalServiceCost + AdditionalServiceRevenue − DiscountsGiven | M03, dim_Finance[NetProfit] |
| BR-03 | Profit Margin | ProfitMarginPct = (NetProfit / TotalRevenue) × 100 | M04 |
| BR-04 | ADR Formula | ADR = Total Revenue / Total Room Nights Sold (SUM of NightsStayed for Paid bookings) | M05 |
| BR-05 | Currency | All monetary values are Indian Rupees (₹) | Display formatting |

### 4.2 Guest Satisfaction Rules

| Rule ID | Rule Name | Definition | Applies To |
|---------|-----------|------------|------------|
| BR-06 | GSS Formula | GSS = (Rating + StaffRating + CleanlinessRating) / 3 — simple unweighted average | M19 |
| BR-07 | Sentiment Classification | Positive = Good service, Excellent stay, Very good, Room was clean and comfortable, Food quality was great, Exceptional hospitality, Will visit again | FeedbackSentiment column |
| BR-08 | Sentiment Classification | Negative = Average, Could be better, Check-in was slow | FeedbackSentiment column |

### 4.3 Time Intelligence Rules

| Rule ID | Rule Name | Definition | Applies To |
|---------|-----------|------------|------------|
| BR-09 | Primary Date | CheckInDate is the primary date axis for all time-series analysis | All time-based visuals |
| BR-10 | Fiscal Year | Indian fiscal year: April 1 – March 31. FY2022 = Apr 2021 – Mar 2022 | dim_Date, fiscal measures |
| BR-11 | YoY Comparison | Year-over-Year compares same calendar period in consecutive years | M26 |
| BR-12 | YTD Fiscal | Year-to-Date uses fiscal year end of March 31 | Fiscal YTD measure |

### 4.4 Security Rules

| Rule ID | Rule Name | Definition | Applies To |
|---------|-----------|------------|------------|
| BR-13 | RLS Filter | dim_Manager[ManagerEmail] = USERPRINCIPALNAME() filters to single hotel | RLS role |
| BR-14 | Executive Access | Users NOT assigned to any RLS role see all data (full access) | Workspace admins/members |
| BR-15 | PII Masking | GuestEmail is masked or removed before report consumption | Power Query transformation |

### 4.5 Booking Rules

| Rule ID | Rule Name | Definition | Applies To |
|---------|-----------|------------|------------|
| BR-16 | Total Bookings | Count of ALL BookingIDs regardless of PaymentStatus | Volume metrics |
| BR-17 | Successful Bookings | Count where PaymentStatus = "Paid" only | Financial booking count |
| BR-18 | Repeat Guest | A customer with > 1 booking across the dataset | Repeat rate calculation |

---

## 5. Complete KPI Definitions

### 5.1 Revenue & Financial KPIs

| KPI ID | KPI Name | DAX Measure Name | Formula | Unit | Format | Target/Benchmark |
|--------|----------|------------------|---------|------|--------|------------------|
| KPI-01 | Total Revenue | [Total Revenue] | CALCULATE(SUM(dim_Finance[TotalRevenue]), fct_Bookings[PaymentStatus]="Paid") | ₹ | ₹#,##0,,"M" | Track trend |
| KPI-02 | Net Profit | [Net Profit] | CALCULATE(SUMX(dim_Finance, [TotalRevenue]-[Cost]-[AdditionalServiceCost]+[AdditionalServiceRevenue]-[DiscountsGiven]), Paid filter) | ₹ | ₹#,##0,,"M" | Positive |
| KPI-03 | Profit Margin % | [Profit Margin %] | DIVIDE([Net Profit], [Total Revenue], 0) × 100 | % | 0.0% | > 30% |
| KPI-04 | ADR | [ADR] | DIVIDE([Total Revenue], SUM(NightsStayed) for Paid) | ₹ | ₹#,##0 | Track trend |
| KPI-05 | Revenue Per Booking | [Revenue Per Booking] | DIVIDE([Total Revenue], [Successful Bookings]) | ₹ | ₹#,##0 | Track trend |
| KPI-06 | Total Tax | [Total Tax] | CALCULATE(SUM(dim_Finance[TaxAmount]), Paid) | ₹ | ₹#,##0,,"M" | Informational |
| KPI-07 | Total Discounts | [Total Discounts] | CALCULATE(SUM(dim_Finance[DiscountsGiven]), Paid) | ₹ | ₹#,##0,,"M" | Monitor |
| KPI-08 | Add. Service Revenue | [Additional Service Revenue] | CALCULATE(SUM(dim_Finance[AdditionalServiceRevenue]), Paid) | ₹ | ₹#,##0 | Grow |
| KPI-09 | Total Cost | [Total Cost] | CALCULATE(SUM(dim_Finance[Cost]), Paid) | ₹ | ₹#,##0,,"M" | Control |
| KPI-10 | Avg Discount % | [Avg Discount %] | CALCULATE(AVERAGE(dim_Finance[DiscountPercent]), Paid) | % | 0.0% | < 15% |

### 5.2 Booking & Operational KPIs

| KPI ID | KPI Name | DAX Measure Name | Formula | Unit | Format |
|--------|----------|------------------|---------|------|--------|
| KPI-11 | Total Bookings | [Total Bookings] | COUNTROWS(fct_Bookings) | Count | #,##0 |
| KPI-12 | Successful Bookings | [Successful Bookings] | CALCULATE(COUNTROWS, Paid) | Count | #,##0 |
| KPI-13 | Failed Rate % | [Failed Booking Rate %] | DIVIDE(Failed count, Total) × 100 | % | 0.0% |
| KPI-14 | Pending Rate % | [Pending Booking Rate %] | DIVIDE(Pending count, Total) × 100 | % | 0.0% |
| KPI-15 | Avg Nights Stayed | [Avg Nights Stayed] | AVERAGE(NightsStayed) for Paid | Nights | 0.0 |
| KPI-16 | Total Room Nights | [Total Room Nights] | SUM(NightsStayed) for Paid | Nights | #,##0 |
| KPI-17 | Unique Guests | [Unique Guests] | DISTINCTCOUNT(CustomerID) | Count | #,##0 |
| KPI-18 | Repeat Guest Rate % | [Repeat Guest Rate %] | Guests with >1 booking / Total guests × 100 | % | 0.0% |

### 5.3 Guest Satisfaction KPIs

| KPI ID | KPI Name | DAX Measure Name | Formula | Unit | Format | Target |
|--------|----------|------------------|---------|------|--------|--------|
| KPI-19 | GSS | [GSS] | AVERAGE(GuestSatisfactionScore) | Score | 0.00 | ≥ 4.0 |
| KPI-20 | Avg Rating | [Avg Rating] | AVERAGE(Rating) | Score | 0.00 | ≥ 4.0 |
| KPI-21 | Avg Staff Rating | [Avg Staff Rating] | AVERAGE(StaffRating) | Score | 0.00 | ≥ 4.0 |
| KPI-22 | Avg Cleanliness | [Avg Cleanliness Rating] | AVERAGE(CleanlinessRating) | Score | 0.00 | ≥ 4.0 |
| KPI-23 | Positive Feedback % | [Positive Feedback %] | Positive count / Total × 100 | % | 0.0% | > 70% |
| KPI-24 | Negative Feedback % | [Negative Feedback %] | Negative count / Total × 100 | % | 0.0% | < 30% |

### 5.4 Time Intelligence KPIs

| KPI ID | KPI Name | DAX Measure Name | Formula | Unit | Format |
|--------|----------|------------------|---------|------|--------|
| KPI-25 | Revenue PY | [Revenue PY] | CALCULATE([Total Revenue], SAMEPERIODLASTYEAR) | ₹ | ₹#,##0,,"M" |
| KPI-26 | YoY Growth % | [YoY Revenue Growth %] | (Current − PY) / PY × 100 | % | +0.0%;-0.0% |
| KPI-27 | Revenue MTD | [Revenue MTD] | CALCULATE([Total Revenue], DATESMTD) | ₹ | ₹#,##0,,"M" |
| KPI-28 | Revenue QTD | [Revenue QTD] | CALCULATE([Total Revenue], DATESQTD) | ₹ | ₹#,##0,,"M" |
| KPI-29 | Revenue YTD | [Revenue YTD] | CALCULATE([Total Revenue], DATESYTD) | ₹ | ₹#,##0,,"M" |
| KPI-30 | Running Total | [Running Total Revenue] | CALCULATE with cumulative filter | ₹ | ₹#,##0,,"M" |

### 5.5 Comparative & Segmentation KPIs

| KPI ID | KPI Name | DAX Measure Name | Formula | Unit | Format |
|--------|----------|------------------|---------|------|--------|
| KPI-31 | Member Revenue | [Member Revenue] | [Total Revenue] WHERE Member | ₹ | ₹#,##0,,"M" |
| KPI-32 | Non-Member Revenue | [Non-Member Revenue] | [Total Revenue] WHERE Non-Member | ₹ | ₹#,##0,,"M" |
| KPI-33 | Member Booking % | [Member Booking %] | Member count / Total × 100 | % | 0.0% |
| KPI-34 | Hotel Revenue Rank | [Hotel Revenue Rank] | RANKX(ALL(HotelName), [Total Revenue]) | Rank | #0 |
| KPI-35 | Revenue % of Total | [Revenue % of Total] | DIVIDE([Total Revenue], ALL total) × 100 | % | 0.0% |
| KPI-36 | Avg Booking Lead Time | [Avg Booking Lead Time] | AVERAGEX(DATEDIFF(BookingDate, CheckInDate, DAY)) | Days | 0.0 |

### 5.6 Display & Formatting KPIs

| KPI ID | KPI Name | DAX Measure Name | Formula | Purpose |
|--------|----------|------------------|---------|---------|
| KPI-37 | Revenue Trend Icon | [Revenue Trend Icon] | IF YoY > 0 then "▲" else "▼" | Visual indicator |
| KPI-38 | GSS Band | [GSS Band] | SWITCH(TRUE, ≥4.5 Excellent, ≥4.0 Very Good, etc.) | Rating label |
| KPI-39 | Manager Title | [Manager Title] | SELECTEDVALUE(ManagerName) & " — " & HotelName | Dynamic heading |
| KPI-40 | Last Refresh | [Last Refresh] | NOW() formatted | Footer display |

---

## 6. Slicer Strategy

### 6.1 Global Slicers (Present on ALL pages)

| Slicer | Source Field | Visual Style | Default Value | Position |
|--------|-------------|--------------|---------------|----------|
| Year | dim_Date[Year] | Dropdown | All | Top-right |
| Hotel | dim_Hotel[Hotel_Name] | Dropdown | All | Top-right |

### 6.2 Page-Specific Slicers

| Page | Slicer | Source Field | Style | Default |
|------|--------|-------------|-------|---------|
| 1 - Executive Summary | Quarter | dim_Date[QuarterLabel] | Horizontal buttons | All |
| 1 - Executive Summary | Category | dim_Hotel[Category] | Horizontal buttons | All |
| 1 - Executive Summary | City | dim_Hotel[City] | Dropdown | All |
| 2 - Revenue & Trends | Room Type | fct_Bookings[RoomType] | Horizontal buttons | All |
| 2 - Revenue & Trends | Category | dim_Hotel[Category] | Horizontal buttons | All |
| 2 - Revenue & Trends | Quarter | dim_Date[QuarterLabel] | Horizontal buttons | All |
| 3 - Manager View | (No slicers — RLS filters automatically) | — | — | — |
| 4 - Channel Analysis | Channel | fct_Bookings[Channel] | Horizontal buttons | All |
| 4 - Channel Analysis | Device Type | fct_Bookings[DeviceType] | Horizontal buttons | All |
| 5 - Satisfaction | Category | dim_Hotel[Category] | Horizontal buttons | All |
| 5 - Satisfaction | Sentiment | dim_Feedback[FeedbackSentiment] | Horizontal buttons | All |
| 6 - Member vs Non-Member | Member Type | fct_Bookings[MemberType] | Horizontal buttons | All |
| 6 - Member vs Non-Member | Room Type | fct_Bookings[RoomType] | Horizontal buttons | All |

### 6.3 Slicer Synchronization

| Slicer Group | Synced Across Pages | Visible On |
|--------------|--------------------:|------------|
| Year | All 6 pages | All 6 pages |
| Hotel | Pages 1, 2, 4, 5, 6 | Pages 1, 2, 4, 5, 6 |
| Category | Pages 1, 2, 5 | Pages 1, 2, 5 |
| Quarter | Pages 1, 2 | Pages 1, 2 |

### 6.4 Slicer Formatting Standards

| Property | Value |
|----------|-------|
| Font | Segoe UI, 10pt |
| Background | White (#FFFFFF) |
| Border | Light gray (#E0E0E0), 1px |
| Selection color | Navy (#1B365D) |
| Header | ON, bold, 11pt |
| Single select vs Multi | Multi-select (default) |
| "Select All" option | Enabled |
| Search box | Enabled for Hotel dropdown |

---

## 7. Drill-Through Strategy

### 7.1 Drill-Through Pages

| # | Page Name | Trigger Field | Source Pages | Hidden | Purpose |
|---|-----------|---------------|--------------|--------|---------|
| DT-1 | Hotel Drillthrough | dim_Hotel[Hotel_Name] | Pages 1, 2, 4, 5 | Yes | Deep dive into a specific hotel's full metrics |
| DT-2 | Booking Detail | fct_Bookings[BookingID] | Pages 3, 4 | Yes | Individual booking inspection |

### 7.2 Hotel Drillthrough Page — Specification

**Trigger:** Right-click any hotel name on Pages 1, 2, 4, or 5 → "Drill through → Hotel Drillthrough"

**Drill-through filter field:** dim_Hotel[Hotel_Name]

**Visuals on this page:**

| Visual | Type | Fields | Position |
|--------|------|--------|----------|
| Hotel Name | Card (large) | Hotel_Name | Top-left |
| City & Category | Card | City, Category | Top-left (below) |
| Manager Name | Card | dim_Manager[ManagerName] | Top-left (below) |
| Total Revenue | KPI Card | [Total Revenue] vs [Revenue PY] | Top row |
| Net Profit | KPI Card | [Net Profit] | Top row |
| GSS Score | KPI Card | [GSS] | Top row |
| Bookings | KPI Card | [Successful Bookings] | Top row |
| Monthly Revenue Trend | Line Chart | dim_Date[MonthYear] vs [Total Revenue] | Middle-left |
| Revenue by Room Type | Donut | RoomType vs [Total Revenue] | Middle-right |
| Channel Breakdown | Clustered Bar | Channel vs [Total Bookings] | Bottom-left |
| Feedback Top Issues | Table | Feedback, Count, Sentiment | Bottom-center |
| Payment Method Split | Pie | PaymentMethod vs Count | Bottom-right |
| Back Button | Button | ← Back (navigates to previous page) | Top-right corner |

### 7.3 Booking Detail Page — Specification

**Trigger:** Right-click BookingID on matrix/table visuals

**Drill-through filter field:** fct_Bookings[BookingID]

**Visuals on this page:**

| Visual | Type | Fields |
|--------|------|--------|
| Booking ID | Card | BookingID |
| Guest Info | Multi-row card | GuestName, Gender, Age, Country, MemberType |
| Stay Details | Multi-row card | Hotel, Room, CheckIn, CheckOut, NightsStayed, Rate |
| Financial | Multi-row card | TotalRevenue, Cost, NetProfit, DiscountPercent, Tax |
| Feedback | Multi-row card | Rating, StaffRating, CleanlinessRating, Feedback, GSS |
| Booking Info | Multi-row card | Channel, DeviceType, PaymentMethod, PaymentStatus |
| Back Button | Button | ← Back |

---

## 8. Bookmark & Navigation Strategy

### 8.1 Page Navigation

| Method | Implementation |
|--------|---------------|
| Tab-style navigation | Horizontal buttons at top of every page |
| Active page indicator | Active button highlighted with Navy fill (#1B365D), white text |
| Inactive buttons | White fill, Navy border, Navy text |
| Button labels | Page 1 icon + "Summary", Page 2 "Revenue", Page 3 "Manager", etc. |

**Navigation bar position:** Top of canvas, spanning full width, height 40px

### 8.2 Bookmarks

| # | Bookmark Name | Purpose | Configuration |
|---|---------------|---------|---------------|
| BM-01 | Reset All Filters | Clears all slicers to defaults | All slicers reset; Data: ON, Display: OFF |
| BM-02 | Current Year | Sets Year = 2024 (latest complete year) | Year slicer = 2024 |
| BM-03 | Luxury Hotels | Filters to Luxury category only | Category = Luxury |
| BM-04 | Heritage Hotels | Filters to Heritage category only | Category = Heritage |
| BM-05 | Members Only | Shows Member data only | MemberType = Member |
| BM-06 | High Revenue Hotels | Top 3 hotels by revenue | Hotel slicer = top 3 |

### 8.3 Bookmark Buttons

Add a "Quick Filters" dropdown or button group on Page 1 (Executive Summary):
- "All Data" → BM-01
- "2024" → BM-02
- "Luxury" → BM-03
- "Heritage" → BM-04

### 8.4 Tooltip Pages

| # | Tooltip Page | Trigger Visual | Content |
|---|-------------|----------------|---------|
| TT-1 | Revenue Tooltip | Revenue bar/line charts | Mini card: Revenue, Profit, ADR, YoY% |
| TT-2 | Hotel Tooltip | Hotel-related visuals | Hotel name, City, Category, Manager, GSS |
| TT-3 | Feedback Tooltip | Satisfaction visuals | Rating breakdown: Overall, Staff, Cleanliness |

**Tooltip page setup:**
- Page size: Custom 320 × 240 pixels
- Background: White with light shadow
- Mark as tooltip page (Page information → Tooltip = ON)

---

## 9. Row-Level Security Design

### 9.1 RLS Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    USER LOGS IN                               │
│              (e.g., aman.mehta@tajhotels.com)                │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  RLS ROLE: "HotelManager"                                    │
│  FILTER: dim_Manager[ManagerEmail] = USERPRINCIPALNAME()     │
│                                                              │
│  Result: dim_Manager filtered to 1 row (MGR001)             │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼ (Bidirectional filter on R3)
┌─────────────────────────────────────────────────────────────┐
│  dim_Hotel filtered to: "Taj Mahal Palace"                   │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼ (Single direction filter from R2)
┌─────────────────────────────────────────────────────────────┐
│  fct_Bookings filtered to: HotelName = "Taj Mahal Palace"   │
│  (only ~14,270 rows visible)                                 │
└────────┬────────────────┬───────────────────┬───────────────┘
         │                │                   │
         ▼                ▼                   ▼
   dim_Finance       dim_Feedback      dim_OnlineBooking
   (filtered via     (filtered via     (filtered via
    BookingID)        BookingID)        BookingID)
```

### 9.2 Role Definition

| Property | Value |
|----------|-------|
| Role Name | HotelManager |
| Table | dim_Manager |
| DAX Expression | `[ManagerEmail] = USERPRINCIPALNAME()` |

### 9.3 Role Assignment (Power BI Service)

| User Email | Role | Sees Data For |
|------------|------|---------------|
| aman.mehta@tajhotels.com | HotelManager | Taj Mahal Palace (Mumbai) |
| kavya.shah@tajhotels.com | HotelManager | Taj Lake Palace (Udaipur) |
| rohan.das@tajhotels.com | HotelManager | Taj Falaknuma Palace (Hyderabad) |
| sneha.roy@tajhotels.com | HotelManager | Taj Bengal (Kolkata) |
| arjun.rao@tajhotels.com | HotelManager | Taj West End (Bengaluru) |
| priya.nair@tajhotels.com | HotelManager | Taj Exotica (Goa) |
| sanjay.iyer@tajhotels.com | HotelManager | Taj Coromandel (Chennai) |
| [executives — NOT assigned] | None | ALL hotels (full portfolio) |

### 9.4 RLS Testing Matrix

| Test # | Login As | Expected Result | Validate |
|--------|----------|-----------------|----------|
| T-01 | aman.mehta@tajhotels.com | Only Taj Mahal Palace data | Revenue, bookings, feedback all Mumbai only |
| T-02 | kavya.shah@tajhotels.com | Only Taj Lake Palace data | Udaipur only |
| T-03 | rohan.das@tajhotels.com | Only Taj Falaknuma Palace | Hyderabad only |
| T-04 | sneha.roy@tajhotels.com | Only Taj Bengal data | Kolkata only |
| T-05 | arjun.rao@tajhotels.com | Only Taj West End data | Bengaluru only |
| T-06 | priya.nair@tajhotels.com | Only Taj Exotica data | Goa only |
| T-07 | sanjay.iyer@tajhotels.com | Only Taj Coromandel data | Chennai only |
| T-08 | No role (executive) | All 7 hotels visible | Full 100K records |
| T-09 | Unknown email | No data visible | Blank dashboard (security) |

### 9.5 Critical RLS Configuration Notes

1. **Bidirectional filter on R3 is MANDATORY** — without it, the RLS filter stops at dim_Manager and never reaches dim_Hotel or fct_Bookings.
2. **Do NOT add bidirectional on R2** (fct_Bookings → dim_Hotel) — this is unnecessary and could create ambiguity.
3. **dim_Manager must be set to "Filter dim_Hotel"** in the relationship properties (edit relationship → Cross filter direction = Both).
4. After publishing, test with "View As" in Power BI Service (not just Desktop).

---

## 10. Date Table Design

### 10.1 Generation Specification

| Property | Value |
|----------|-------|
| Start Date | 01 January 2021 |
| End Date | 31 December 2025 |
| Total Rows | 1,826 |
| Granularity | Day |
| Marked as Date Table | Yes (Column: Date) |
| Auto date/time | Disabled (Options → Data Load) |

### 10.2 Column Specifications

| Column | Type | Formula Logic | Sort By Column | Example Value |
|--------|------|---------------|----------------|---------------|
| Date | Date | List.Dates start to end | — | 2023-06-15 |
| Year | Int | Date.Year([Date]) | — | 2023 |
| Quarter | Int | Date.QuarterOfYear([Date]) | — | 2 |
| QuarterLabel | Text | "Q" & Quarter | Quarter | "Q2" |
| Month | Int | Date.Month([Date]) | — | 6 |
| MonthName | Text | Date.MonthName([Date]) | Month | "June" |
| MonthShort | Text | Text.Start(MonthName, 3) | Month | "Jun" |
| DayOfMonth | Int | Date.Day([Date]) | — | 15 |
| WeekDay | Int | Date.DayOfWeek(Mon=1) + 1 | — | 4 (Thursday) |
| WeekDayName | Text | Date.DayOfWeekName([Date]) | WeekDay | "Thursday" |
| IsWeekend | Bool | WeekDay >= 6 | — | FALSE |
| YearMonth | Text | Year & "-" & PadStart(Month, 2, "0") | — | "2023-06" |
| YearQuarter | Text | Year & "-Q" & Quarter | — | "2023-Q2" |
| FiscalYear | Int | If Month >= 4 then Year+1 else Year | — | 2024 (for Jun 2023) |
| FiscalQuarter | Int | Apr-Jun=1, Jul-Sep=2, Oct-Dec=3, Jan-Mar=4 | — | 1 (for Jun 2023) |
| FiscalYearLabel | Text | "FY" & Right(FiscalYear, 2) | FiscalYear | "FY24" |
| MonthYear | Text | MonthShort & " " & Year | Date | "Jun 2023" |

### 10.3 Date Hierarchy for Drill-Down

```
Level 1: Year (2021, 2022, 2023, 2024, 2025)
  └── Level 2: Quarter (Q1, Q2, Q3, Q4)
       └── Level 3: Month (Jan, Feb, ..., Dec)
            └── Level 4: Date (individual days)
```

### 10.4 Sort-By Column Mapping

These must be configured in Power BI to ensure correct display order:

| Column to Sort | Sort By |
|----------------|---------|
| MonthName | Month |
| MonthShort | Month |
| QuarterLabel | Quarter |
| WeekDayName | WeekDay |
| MonthYear | Date |
| YearQuarter | Date (first date of quarter) |
| FiscalYearLabel | FiscalYear |

### 10.5 Time Intelligence Support

The Date table enables these DAX functions:
- SAMEPERIODLASTYEAR — YoY comparisons
- DATESMTD / DATESQTD / DATESYTD — Period-to-date
- DATEADD — Period shifting
- TOTALYTD / TOTALQTD / TOTALMTD — Alternative syntax
- PREVIOUSMONTH / PREVIOUSQUARTER / PREVIOUSYEAR — Prior period
- DATESYTD with custom year-end ("3/31") — Fiscal YTD

---

## 11. Dashboard Information Architecture

### 11.1 Navigation Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        NAVIGATION BAR (all pages)                        │
│  [Summary] [Revenue] [Manager] [Channels] [Satisfaction] [Membership]  │
└─────────────────────────────────────────────────────────────────────────┘

┌───────────────┐     ┌───────────────┐     ┌────────────────────────┐
│ Page 1:       │────►│ Page 2:       │     │ Page 3:                │
│ Executive     │     │ Revenue &     │     │ Hotel Manager View     │
│ Summary       │     │ Trends        │     │ (RLS Secured)          │
│               │     │               │     │                        │
│ Entry point   │     │ Deep revenue  │     │ Property-specific      │
│ All KPIs      │     │ analysis      │     │ Managers only see      │
│               │     │               │     │ their hotel            │
└───────┬───────┘     └───────┬───────┘     └────────────────────────┘
        │                     │
        │ Drill-through       │ Drill-through
        ▼                     ▼
┌─────────────────────────────────────────┐
│ Hidden: Hotel Drillthrough              │
│ Full detail for selected hotel          │
│ ← Back button returns to source page   │
└─────────────────────────────────────────┘

┌───────────────┐     ┌───────────────┐     ┌────────────────────────┐
│ Page 4:       │     │ Page 5:       │     │ Page 6:                │
│ Booking       │     │ Guest         │     │ Member vs              │
│ Channels      │     │ Satisfaction  │     │ Non-Member             │
│               │     │               │     │                        │
│ Marketing     │     │ Quality team  │     │ Strategy & Finance     │
│ insights      │     │ insights      │     │ insights               │
└───────────────┘     └───────────────┘     └────────────────────────┘
```

### 11.2 Information Hierarchy Per Page

**Page 1 — Executive Summary (Landing Page)**
```
Priority: Overview → Trend → Composition → Distribution
├── TOP: KPI Cards (Revenue, Bookings, ADR, Profit, GSS)
├── MIDDLE-LEFT: Revenue Trend by Year (line chart)
├── MIDDLE-RIGHT: Revenue by Hotel (bar chart)
├── BOTTOM-LEFT: Revenue by Category (donut)
└── BOTTOM-RIGHT: Channel Distribution (donut)
```

**Page 2 — Revenue & Trends**
```
Priority: Growth → Trend → Breakdown → Margin
├── TOP: Growth KPI (YoY%), Revenue YTD, Profit Margin%
├── MIDDLE: Monthly Revenue Area Chart (with drill-down)
├── BOTTOM-LEFT: Cost vs Revenue Combo Chart
├── BOTTOM-CENTER: Revenue by Room Type
└── BOTTOM-RIGHT: Profit Margin Trend Line
```

**Page 3 — Hotel Manager View**
```
Priority: My Hotel → My Metrics → My Trends → My Issues
├── TOP: Dynamic Title (Manager + Hotel Name)
├── ROW 2: KPI Cards (Revenue, Bookings, GSS, Profit)
├── MIDDLE-LEFT: Monthly Revenue Trend (my hotel)
├── MIDDLE-RIGHT: Room Type Distribution (donut)
├── BOTTOM-LEFT: Top Feedback Issues (table)
└── BOTTOM-RIGHT: Payment Method Breakdown
```

**Page 4 — Channel Analysis**
```
Priority: Distribution → Trend → Revenue → Detail
├── TOP-LEFT: Channel Distribution (donut with %)
├── TOP-RIGHT: Device Type Split (donut)
├── MIDDLE: Channel Trend Over Time (stacked area)
├── BOTTOM-LEFT: Channel Revenue (clustered bar)
└── BOTTOM-RIGHT: Hotel × Channel Matrix
```

**Page 5 — Guest Satisfaction**
```
Priority: Score → Breakdown → Distribution → Trend
├── TOP-LEFT: GSS Gauge (with target line at 4.0)
├── TOP-RIGHT: Rating Cards (Overall, Staff, Cleanliness)
├── MIDDLE-LEFT: Feedback Category Bar Chart
├── MIDDLE-RIGHT: GSS by Hotel (horizontal bar)
├── BOTTOM-LEFT: Rating Trend (3 lines over time)
└── BOTTOM-RIGHT: Satisfaction by Category
```

**Page 6 — Member vs. Non-Member**
```
Priority: Comparison → Revenue → Profit → Behavior
├── TOP: KPI Cards (Member Rev, Non-Member Rev, Member %, Revenue Split)
├── MIDDLE-LEFT: Revenue by Year × MemberType (100% stacked)
├── MIDDLE-RIGHT: Profit Comparison (clustered bar)
├── BOTTOM-LEFT: Discount by MemberType
├── BOTTOM-CENTER: ADR Comparison Cards
└── BOTTOM-RIGHT: Room Type × MemberType Matrix
```

### 11.3 Visual Interaction Rules

| Source Visual | Target Visual | Interaction Type |
|--------------|---------------|-----------------|
| Donut chart (any) | All other visuals on page | Highlight (not filter) |
| Bar chart (any) | All other visuals on page | Filter |
| Line/Area chart | KPI cards | No interaction |
| Line/Area chart | Other charts | Highlight |
| Slicers | All visuals | Filter (always) |
| Matrix | All visuals | Filter |
| Table | All visuals | No interaction |

**Rationale:** Donut charts use "highlight" to avoid removing context from other visuals. Bar charts "filter" to enable focused analysis. Line charts don't filter KPI cards to keep top-level numbers stable.

### 11.4 Color Palette

| Use | Color | Hex | Application |
|-----|-------|-----|-------------|
| Primary | Navy Blue | #1B365D | Headers, active buttons, primary series |
| Secondary | Gold | #C4A265 | Accent, secondary series, highlights |
| Success | Green | #2E8B57 | Positive growth, targets met |
| Danger | Red | #DC3545 | Negative growth, alerts |
| Neutral | Gray | #6C757D | Tertiary data, borders |
| Info | Teal | #17A2B8 | Informational elements |
| Warning | Amber | #FFC107 | Caution indicators |
| Background | White | #FFFFFF | Canvas background |
| Light BG | Light Gray | #F8F9FA | Card backgrounds |
| Text | Dark | #212529 | Body text |

**Category-specific colors:**
| Category | Color | Hex |
|----------|-------|-----|
| Luxury | Gold | #C4A265 |
| Heritage | Deep Red | #8B0000 |
| Business | Navy | #1B365D |
| Resort | Teal | #17A2B8 |

**Channel-specific colors:**
| Channel | Color | Hex |
|---------|-------|-----|
| Website | Navy | #1B365D |
| App | Teal | #17A2B8 |
| Agent | Gold | #C4A265 |
| ThirdParty | Gray | #6C757D |

---

## 12. Summary of Deliverables Produced in Phase 2

| # | Deliverable | Status |
|---|-------------|--------|
| 1 | Star Schema with fact and dimension tables | ✓ Complete (Section 1) |
| 2 | Relationship diagram with cardinality and filter directions | ✓ Complete (Section 2) |
| 3 | Data dictionary for every table and column | ✓ Complete (Section 3) |
| 4 | Business rules for every KPI | ✓ Complete (Section 4) |
| 5 | Complete KPI definitions (40 measures) | ✓ Complete (Section 5) |
| 6 | Slicer strategy across all pages | ✓ Complete (Section 6) |
| 7 | Drill-through strategy | ✓ Complete (Section 7) |
| 8 | Bookmark and navigation strategy | ✓ Complete (Section 8) |
| 9 | Row-Level Security design | ✓ Complete (Section 9) |
| 10 | Date table design | ✓ Complete (Section 10) |
| 11 | Dashboard information architecture | ✓ Complete (Section 11) |

---

*End of Phase 2: Solution Design*
*Awaiting approval to proceed to Phase 3: Construction.*
