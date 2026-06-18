# Star Schema Design
## Hospitality Analytics Dashboard

---

## Schema Diagram

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
                         │                                 │
                         │     Rows: 1,826                 │
                         │     Generated via Power Query   │
                         └────────────────┬────────────────┘
                                          │
                                          │ R1: Many:1
                                          │ Single direction (Fact→Dim)
                                          │
┌───────────────────────┐                 │                 ┌───────────────────────┐
│     dim_Hotel         │                 │                 │     dim_Manager       │
│───────────────────────│                 │                 │───────────────────────│
│ PK Hotel_Name (text)  │◄──── R3: 1:1 ──┼────────────────►│ PK HotelName  (text)  │
│    City       (text)  │  BIDIRECTIONAL  │                 │    ManagerID  (text)  │
│    Category   (text)  │                 │                 │    ManagerName(text)  │
│                       │                 │                 │    ManagerEmail(text) │
│    Rows: 7            │                 │                 │    Rows: 7            │
│    Source: Excel      │                 │                 │    Source: Excel      │
└───────────┬───────────┘                 │                 └───────────────────────┘
            │                             │
            │ R2: Many:1                  │
            │ Single direction            │
            │ (Fact→Dim)                  │
            │                             │
┌───────────┴─────────────────────────────┴─────────────────────────────────────────┐
│                              fct_Bookings                                          │
│───────────────────────────────────────────────────────────────────────────────────│
│ PK  BookingID       (int)      │  FK  HotelName      (text) → dim_Hotel          │
│     CustomerID      (text)     │  FK  CheckInDate    (date) → dim_Date           │
│     GuestName       (text)     │                                                  │
│     Gender          (text)     │      PaymentMethod  (text)                       │
│     Age             (int)      │      Channel        (text)                       │
│     Country         (text)     │      DeviceType     (text)                       │
│     RoomType        (text)     │      PaymentStatus  (text)                       │
│     CheckInDate     (date)     │      IsPaid         (int) [calc]                 │
│     CheckOutDate    (date)     │      RoomRevenue    (decimal) [calc]             │
│     NightsStayed    (int)      │                                                  │
│     RatePerNight    (decimal)  │      Rows: 100,000                               │
│     MemberType      (text)     │      Source: CSV                                 │
│     City            (text)     │                                                  │
│     Category        (text)     │                                                  │
└────────┬─────────────────┬─────────────────┬──────────────────────────────────────┘
         │                 │                 │
         │ R4: 1:1         │ R5: 1:1         │ R6: 1:1
         │ Bidirectional   │ Bidirectional   │ Bidirectional
         │                 │                 │
┌────────┴──────────┐ ┌───┴───────────────┐ ┌┴──────────────────────┐
│   dim_Finance     │ │   dim_Feedback    │ │  dim_OnlineBooking    │
│───────────────────│ │───────────────────│ │───────────────────────│
│PK BookingID (int) │ │PK BookingID (int) │ │PK BookingID    (int)  │
│   CustomerID      │ │   CustomerID      │ │   CustomerID          │
│   DiscountPercent │ │   Rating          │ │   Channel             │
│   TaxAmount       │ │   Feedback        │ │   GuestEmail (masked) │
│   TotalRevenue    │ │   SubmittedDate   │ │   BookingDate         │
│   Cost            │ │   StaffRating     │ │   Location            │
│   AddSvcCost      │ │   CleanlinessRtg  │ │   DeviceType          │
│   AddSvcRevenue   │ │   GSS [calc]      │ │                       │
│   DiscountsGiven  │ │   Sentiment [calc]│ │   Rows: 100,000       │
│   NetProfit [calc]│ │                   │ │   Source: CSV          │
│   ProfitMargin%   │ │   Rows: 100,000   │ │   Encoding: Latin-1   │
│     [calc]        │ │   Source: CSV      │ │                       │
│                   │ │                   │ │                       │
│   Rows: 100,000   │ │                   │ │                       │
│   Source: CSV      │ │                   │ │                       │
└───────────────────┘ └───────────────────┘ └───────────────────────┘
```

---

## Relationship Summary

| ID | From → To | Join Column | Cardinality | Cross-Filter | Purpose |
|----|-----------|-------------|-------------|--------------|---------|
| R1 | fct_Bookings → dim_Date | CheckInDate = Date | Many:1 | Single | Time intelligence |
| R2 | fct_Bookings → dim_Hotel | HotelName = Hotel_Name | Many:1 | Single | Hotel grouping |
| R3 | dim_Hotel ↔ dim_Manager | Hotel_Name = HotelName | 1:1 | Both | RLS propagation |
| R4 | fct_Bookings ↔ dim_Finance | BookingID = BookingID | 1:1 | Both | Financial metrics |
| R5 | fct_Bookings ↔ dim_Feedback | BookingID = BookingID | 1:1 | Both | Satisfaction data |
| R6 | fct_Bookings ↔ dim_OnlineBooking | BookingID = BookingID | 1:1 | Both | Booking details |

---

## RLS Filter Propagation Path

```
USERPRINCIPALNAME() → dim_Manager[ManagerEmail]
    ↓ filters dim_Manager to 1 row
    ↓ R3 (bidirectional) propagates to dim_Hotel
    ↓ dim_Hotel filtered to 1 hotel
    ↓ R2 (fact→dim, but dim now filtered) restricts fct_Bookings
    ↓ fct_Bookings filtered to ~14,200 rows
    ↓ R4, R5, R6 (bidirectional) propagate to extension tables
    ↓ dim_Finance, dim_Feedback, dim_OnlineBooking all filtered
```

---

## Table Classification

| Table | Type | Grain | Role in Model |
|-------|------|-------|---------------|
| fct_Bookings | Fact | 1 row per booking | Central hub of all measures |
| dim_Date | Dimension (Type 0) | 1 row per day | Time intelligence & drill-down |
| dim_Hotel | Dimension (Type 1) | 1 row per hotel | Hotel master & RLS bridge |
| dim_Manager | Dimension (Type 1) | 1 row per manager | RLS anchor table |
| dim_Finance | Fact Extension | 1 row per booking | Financial measures |
| dim_Feedback | Fact Extension | 1 row per booking | Satisfaction measures |
| dim_OnlineBooking | Fact Extension | 1 row per booking | Channel & booking details |
