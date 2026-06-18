# Architecture Diagrams
## Hospitality Analytics Power BI Project

---

## 1. Overall Solution Architecture

### Explanation
This diagram shows the end-to-end solution from data sources through transformation, modeling, visualization, and consumption. It represents the complete technology stack and data pipeline.

### Mermaid Code
```mermaid
graph TB
    subgraph Sources["DATA SOURCES"]
        S1[("Hospitality_Data_Global_fct.csv<br/>100K rows")]
        S2[("Finance_dim.csv<br/>100K rows")]
        S3[("Feedback_dim.csv<br/>100K rows")]
        S4[("OnlineBooking_dim.csv<br/>100K rows")]
        S5[("Hotel_List_dim.xlsx<br/>7 rows")]
        S6[("Manager_Details_dim.xlsx<br/>7 rows")]
    end

    subgraph ETL["POWER QUERY ETL"]
        E1[Date Parsing<br/>DD-MM-YYYY → Date]
        E2[Encoding Fix<br/>Latin-1 → UTF-8]
        E3[PII Masking<br/>Email Obfuscation]
        E4[Calculated Columns<br/>NetProfit, GSS, IsPaid]
        E5[Date Table Generation<br/>2021-2025 Calendar]
    end

    subgraph Model["DATA MODEL (Star Schema)"]
        F[fct_Bookings<br/>100K rows]
        D1[dim_Date<br/>1,826 rows]
        D2[dim_Hotel<br/>7 rows]
        D3[dim_Manager<br/>7 rows]
        D4[dim_Finance<br/>100K rows]
        D5[dim_Feedback<br/>100K rows]
        D6[dim_OnlineBooking<br/>100K rows]
    end

    subgraph DAX["DAX ENGINE"]
        M1[51 Measures]
        M2[Time Intelligence]
        M3[KPI Calculations]
        M4[Conditional Logic]
    end

    subgraph Report["POWER BI REPORT"]
        R1[Home Page]
        R2[Executive Summary]
        R3[Revenue & Trends]
        R4[Hotel Manager View]
        R5[Channel Analysis]
        R6[Guest Satisfaction]
        R7[Member vs Non-Member]
        R8[Hotel Drillthrough]
        R9[Booking Detail]
    end

    subgraph Security["SECURITY LAYER"]
        RLS[Row-Level Security<br/>USERPRINCIPALNAME]
    end

    subgraph Service["POWER BI SERVICE"]
        WS[Workspace]
        DS[Dataset + Refresh]
        APP[Published App]
    end

    subgraph Users["CONSUMERS"]
        U1[Executives<br/>Full Access]
        U2[Hotel Managers<br/>RLS Filtered]
        U3[Analysts<br/>Full Access]
    end

    Sources --> ETL
    ETL --> Model
    Model --> DAX
    DAX --> Report
    Report --> Security
    Security --> Service
    Service --> Users
```

### PlantUML Code
```plantuml
@startuml
!theme cerulean
title Overall Solution Architecture - Hospitality Analytics

package "Data Sources" {
    database "CSV Files\n(4 × 100K rows)" as CSV
    database "Excel Files\n(2 × 7 rows)" as XLS
}

package "Power Query ETL" {
    component "Date Parsing" as DP
    component "Encoding Fix" as EF
    component "PII Masking" as PM
    component "Calc Columns" as CC
    component "Date Table Gen" as DT
}

package "Data Model" {
    component "Star Schema\n7 Tables" as SM
    component "Relationships\n6 Defined" as REL
}

package "DAX Engine" {
    component "51 Measures" as DAX
    component "Time Intelligence" as TI
}

package "Report Layer" {
    component "9 Pages\n(7 visible + 2 hidden)" as RPT
    component "Navigation + Bookmarks" as NAV
}

package "Security" {
    component "RLS\nUSERPRINCIPALNAME()" as RLS
}

package "Power BI Service" {
    component "Workspace" as WS
    component "Scheduled Refresh" as SR
    component "Published App" as APP
}

actor "Executives" as EX
actor "Managers (7)" as MGR
actor "Analysts" as AN

CSV --> DP
XLS --> DP
DP --> EF
EF --> PM
PM --> CC
CC --> DT
DT --> SM
SM --> REL
REL --> DAX
DAX --> TI
TI --> RPT
RPT --> NAV
NAV --> RLS
RLS --> WS
WS --> SR
SR --> APP
APP --> EX
APP --> MGR
APP --> AN

note right of RLS
  Dynamic single role
  Filters by manager email
  7 managers see own hotel
  Executives see all
end note
@enduml
```

### ASCII Representation
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    HOSPITALITY ANALYTICS - SOLUTION ARCHITECTURE              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ DATA SOURCES                                                     │        │
│  │  [CSV×4: 100K rows each]  [Excel×2: 7 rows each]               │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ POWER QUERY ETL                                                  │        │
│  │  Date Parsing → Encoding Fix → PII Mask → Calc Cols → Date Gen  │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ STAR SCHEMA DATA MODEL                                           │        │
│  │  1 Fact (fct_Bookings) + 6 Dimensions + 6 Relationships         │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ DAX ENGINE: 51 Measures                                          │        │
│  │  Revenue(11) + Booking(9) + GSS(6) + Time(9) + Compare(12) + 4  │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ REPORT LAYER: 9 Pages                                            │        │
│  │  [Home][Exec][Revenue][Manager][Channel][Satisfaction][Member]   │        │
│  │  [Hotel Drillthrough (hidden)][Booking Detail (hidden)]          │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ ROW-LEVEL SECURITY                                               │        │
│  │  Role: HotelManager | Filter: ManagerEmail = USERPRINCIPALNAME() │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │ POWER BI SERVICE                                                 │        │
│  │  Workspace → Dataset (Daily Refresh 6AM) → Published App         │        │
│  └──────────────┬───────────────────────────────────┬──────────────┘        │
│                 │                                   │                        │
│                 ▼                                   ▼                        │
│         ┌──────────────┐                  ┌──────────────────┐              │
│         │ EXECUTIVES   │                  │ HOTEL MANAGERS    │              │
│         │ Full Access  │                  │ RLS Filtered      │              │
│         │ All 7 Hotels │                  │ 1 Hotel Each      │              │
│         └──────────────┘                  └──────────────────┘              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Star Schema Diagram

### Explanation
The data model follows a star schema with fct_Bookings as the central fact table connected to 6 dimension tables. Three dimensions (Finance, Feedback, OnlineBooking) are fact extensions at the same grain (1:1 on BookingID).

### Mermaid Code
```mermaid
erDiagram
    fct_Bookings ||--o{ dim_Date : "CheckInDate = Date"
    fct_Bookings }o--|| dim_Hotel : "HotelName = Hotel_Name"
    dim_Hotel ||--|| dim_Manager : "Hotel_Name = HotelName"
    fct_Bookings ||--|| dim_Finance : "BookingID = BookingID"
    fct_Bookings ||--|| dim_Feedback : "BookingID = BookingID"
    fct_Bookings ||--|| dim_OnlineBooking : "BookingID = BookingID"

    fct_Bookings {
        int BookingID PK
        string CustomerID
        string GuestName
        string Gender
        int Age
        string Country
        string RoomType
        date CheckInDate FK
        date CheckOutDate
        int NightsStayed
        decimal RatePerNight
        string MemberType
        string HotelName FK
        string City
        string Category
        string PaymentMethod
        string Channel
        string DeviceType
        string PaymentStatus
        decimal RoomRevenue
        int IsPaid
    }

    dim_Date {
        date Date PK
        int Year
        int Quarter
        string QuarterLabel
        int Month
        string MonthName
        int FiscalYear
        int FiscalQuarter
        string MonthYear
    }

    dim_Hotel {
        string Hotel_Name PK
        string City
        string Category
    }

    dim_Manager {
        string HotelName PK
        string ManagerID
        string ManagerName
        string ManagerEmail
    }

    dim_Finance {
        int BookingID PK
        int DiscountPercent
        decimal TaxAmount
        decimal TotalRevenue
        decimal Cost
        decimal AdditionalServiceCost
        decimal AdditionalServiceRevenue
        decimal DiscountsGiven
        decimal NetProfit
        decimal ProfitMarginPct
    }

    dim_Feedback {
        int BookingID PK
        decimal Rating
        string Feedback
        date SubmittedDate
        decimal StaffRating
        decimal CleanlinessRating
        decimal GuestSatisfactionScore
        string FeedbackSentiment
    }

    dim_OnlineBooking {
        int BookingID PK
        string Channel
        string GuestEmail
        date BookingDate
        string Location
        string DeviceType
    }
```

### ASCII Representation
```
                         ┌─────────────────────┐
                         │     dim_Date         │
                         │─────────────────────│
                         │ PK Date             │
                         │    Year, Quarter    │
                         │    Month, MonthYear │
                         │    FiscalYear       │
                         │    1,826 rows       │
                         └──────────┬──────────┘
                                    │ Many:1
                                    │ Single →
                                    │
┌──────────────────┐               │               ┌──────────────────┐
│   dim_Hotel      │               │               │   dim_Manager    │
│──────────────────│               │               │──────────────────│
│PK Hotel_Name     │◄──── 1:1 BOTH ┼──────────────►│PK HotelName     │
│   City           │               │               │   ManagerID      │
│   Category       │               │               │   ManagerName    │
│   7 rows         │               │               │   ManagerEmail   │
└────────┬─────────┘               │               │   7 rows         │
         │ Many:1                  │               └──────────────────┘
         │ Single →                │
         │                         │
┌────────┴─────────────────────────┴──────────────────────────────────────┐
│                         fct_Bookings                                      │
│──────────────────────────────────────────────────────────────────────────│
│ PK BookingID    | FK HotelName → dim_Hotel                               │
│    CustomerID   | FK CheckInDate → dim_Date                              │
│    GuestName    |    PaymentMethod, Channel, DeviceType                   │
│    Gender, Age  |    PaymentStatus, MemberType                           │
│    Country      |    RoomRevenue (calc), IsPaid (calc)                   │
│    RoomType     |                                                        │
│    NightsStayed |    100,000 rows                                        │
│    RatePerNight |                                                        │
└───────┬──────────────────┬──────────────────────┬────────────────────────┘
        │ 1:1 BOTH         │ 1:1 BOTH             │ 1:1 BOTH
        ▼                  ▼                      ▼
┌───────────────┐  ┌────────────────┐  ┌─────────────────────┐
│ dim_Finance   │  │ dim_Feedback   │  │ dim_OnlineBooking   │
│───────────────│  │────────────────│  │─────────────────────│
│PK BookingID   │  │PK BookingID    │  │PK BookingID         │
│  TotalRevenue │  │  Rating        │  │  Channel            │
│  Cost         │  │  Feedback      │  │  BookingDate        │
│  TaxAmount    │  │  StaffRating   │  │  Location           │
│  DiscountsGvn │  │  Cleanliness   │  │  DeviceType         │
│  NetProfit    │  │  GSS (calc)    │  │  GuestEmail(masked) │
│  ProfitMrg%   │  │  Sentiment     │  │                     │
│  100K rows    │  │  100K rows     │  │  100K rows          │
└───────────────┘  └────────────────┘  └─────────────────────┘
```

---

## 3. Data Flow Diagram

### Explanation
Shows data movement from raw source files through transformation stages to the final Power BI report consumption.

### Mermaid Code
```mermaid
flowchart LR
    subgraph Source["SOURCE FILES"]
        direction TB
        F1["Hospitality_Data_Global_fct.csv"]
        F2["Finance_dim.csv"]
        F3["Feedback_dim.csv"]
        F4["OnlineBooking_dim.csv"]
        F5["Hotel_List_dim.xlsx"]
        F6["Manager_Details_dim.xlsx"]
    end

    subgraph PQ["POWER QUERY TRANSFORMS"]
        direction TB
        T1["Parse Dates<br/>DD-MM-YYYY → Date"]
        T2["Fix Encoding<br/>Latin-1 for OnlineBooking"]
        T3["Mask PII<br/>Email Obfuscation"]
        T4["Add Columns<br/>NetProfit, GSS, IsPaid<br/>RoomRevenue, Sentiment"]
        T5["Generate<br/>dim_Date Table<br/>2021–2025"]
        T6["Dedup & Validate<br/>Remove duplicates<br/>Null handling"]
    end

    subgraph Model["IN-MEMORY MODEL"]
        direction TB
        M1["fct_Bookings<br/>23 columns"]
        M2["dim_Finance<br/>11 columns"]
        M3["dim_Feedback<br/>9 columns"]
        M4["dim_OnlineBooking<br/>7 columns"]
        M5["dim_Hotel + dim_Manager<br/>3+4 columns"]
        M6["dim_Date<br/>17 columns"]
    end

    subgraph DAX["DAX MEASURES"]
        direction TB
        D1["Revenue: 11 measures"]
        D2["Bookings: 9 measures"]
        D3["Satisfaction: 6 measures"]
        D4["Time Intel: 9 measures"]
        D5["Compare: 12 measures"]
        D6["Display: 4 measures"]
    end

    subgraph Output["REPORT OUTPUT"]
        direction TB
        O1["9 Report Pages"]
        O2["Interactive Visuals"]
        O3["Filtered by RLS"]
    end

    F1 --> T1
    F2 --> T1
    F3 --> T1
    F4 --> T2
    F5 --> T6
    F6 --> T6
    T1 --> T4
    T2 --> T3
    T3 --> T4
    T4 --> T6
    T6 --> M1
    T6 --> M2
    T6 --> M3
    T6 --> M4
    T6 --> M5
    T5 --> M6
    M1 --> D1
    M2 --> D1
    M3 --> D3
    M4 --> D2
    M5 --> D5
    M6 --> D4
    D1 --> O1
    D2 --> O1
    D3 --> O1
    D4 --> O1
    D5 --> O1
    D6 --> O2
    O1 --> O3
```

### ASCII Representation
```
SOURCE FILES          POWER QUERY           DATA MODEL          DAX              OUTPUT
─────────────         ───────────           ──────────          ───              ──────

┌──────────┐    ┌──────────────────┐    ┌──────────────┐   ┌──────────┐   ┌───────────┐
│Fact CSV  │───►│ Parse Dates      │───►│ fct_Bookings │──►│Revenue   │──►│ 9 Report  │
│(100K)    │    │ DD-MM-YYYY→Date  │    │ (23 cols)    │   │Measures  │   │ Pages     │
└──────────┘    └──────────────────┘    └──────────────┘   │(11)      │   │           │
                                                            └──────────┘   │ Filtered  │
┌──────────┐    ┌──────────────────┐    ┌──────────────┐   ┌──────────┐   │ by RLS    │
│Finance   │───►│ Null→0           │───►│ dim_Finance  │──►│Booking   │   │           │
│CSV(100K) │    │ Add NetProfit    │    │ (11 cols)    │   │Measures  │   │ Published │
└──────────┘    └──────────────────┘    └──────────────┘   │(9)       │   │ as App    │
                                                            └──────────┘   │           │
┌──────────┐    ┌──────────────────┐    ┌──────────────┐   ┌──────────┐   │ Daily     │
│Feedback  │───►│ Add GSS + Sent.  │───►│ dim_Feedback │──►│Satisf.   │   │ Refresh   │
│CSV(100K) │    │                  │    │ (9 cols)     │   │Measures  │   │ 6AM IST   │
└──────────┘    └──────────────────┘    └──────────────┘   │(6)       │   └───────────┘
                                                            └──────────┘
┌──────────┐    ┌──────────────────┐    ┌──────────────┐   ┌──────────┐
│Online    │───►│ Latin-1 Encoding │───►│dim_OnlineBkg │──►│Time Intl │
│CSV(100K) │    │ Mask Email       │    │ (7 cols)     │   │Measures  │
└──────────┘    └──────────────────┘    └──────────────┘   │(9)       │
                                                            └──────────┘
┌──────────┐    ┌──────────────────┐    ┌──────────────┐   ┌──────────┐
│Hotel.xlsx│───►│ Trim + Type      │───►│ dim_Hotel    │──►│Compare   │
│(7 rows)  │    │                  │    │ (3 cols)     │   │Measures  │
└──────────┘    └──────────────────┘    └──────────────┘   │(12)      │
                                                            └──────────┘
┌──────────┐    ┌──────────────────┐    ┌──────────────┐
│Manager   │───►│ Lowercase Email  │───►│ dim_Manager  │
│.xlsx(7)  │    │ Trim             │    │ (4 cols)     │
└──────────┘    └──────────────────┘    └──────────────┘

                ┌──────────────────┐    ┌──────────────┐
                │ GENERATE         │───►│ dim_Date     │
                │ Calendar 2021-25 │    │ (17 cols)    │
                └──────────────────┘    └──────────────┘
```

---

## 4. ETL Architecture

### Mermaid Code
```mermaid
flowchart TD
    subgraph Extract["EXTRACT"]
        E1["Csv.Document()<br/>UTF-8 Encoding"]
        E2["Csv.Document()<br/>Latin-1 (1252)"]
        E3["Excel.Workbook()"]
    end

    subgraph Transform["TRANSFORM"]
        T1["PromoteHeaders"]
        T2["TransformColumnTypes<br/>Int64, Decimal, Text"]
        T3["Date Parsing<br/>Text.Split → #date()"]
        T4["Null Handling<br/>ReplaceValue null→0/Unknown"]
        T5["Text Trimming<br/>Text.Trim all text cols"]
        T6["PII Masking<br/>Email → abc***@domain"]
        T7["Calculated Columns<br/>NetProfit, GSS, Sentiment<br/>RoomRevenue, IsPaid, ProfitMargin"]
        T8["Deduplication<br/>Table.Distinct on PK"]
        T9["Calendar Generation<br/>List.Dates + AddColumn×15"]
    end

    subgraph Load["LOAD"]
        L1["Close & Apply"]
        L2["Import Mode<br/>In-Memory Storage"]
        L3["~20MB Dataset"]
    end

    E1 --> T1
    E2 --> T1
    E3 --> T1
    T1 --> T2
    T2 --> T3
    T3 --> T4
    T4 --> T5
    T5 --> T6
    T6 --> T7
    T7 --> T8
    T8 --> L1
    T9 --> L1
    L1 --> L2
    L2 --> L3
```

---

## 5. Row-Level Security Architecture

### Explanation
Shows how the single dynamic RLS role propagates through the data model to restrict access per hotel manager.

### Mermaid Code
```mermaid
flowchart TD
    A["User Logs In<br/>e.g., aman.mehta@tajhotels.com"] --> B

    B["USERPRINCIPALNAME()<br/>Returns: aman.mehta@tajhotels.com"] --> C

    C["RLS Filter Applied<br/>dim_Manager[ManagerEmail] =<br/>aman.mehta@tajhotels.com"] --> D

    D["dim_Manager FILTERED<br/>1 row: MGR001, Aman Mehta,<br/>Taj Mahal Palace"] --> E

    E["R3: Bidirectional Filter<br/>dim_Hotel ↔ dim_Manager<br/>Propagates to dim_Hotel"] --> F

    F["dim_Hotel FILTERED<br/>Hotel_Name = Taj Mahal Palace"] --> G

    G["R2: fct_Bookings filtered<br/>HotelName = Taj Mahal Palace<br/>~14,270 rows visible"] --> H

    H["R4,R5,R6: Extensions filtered<br/>via BookingID relationship"]

    H --> I["dim_Finance<br/>~14,270 rows"]
    H --> J["dim_Feedback<br/>~14,270 rows"]
    H --> K["dim_OnlineBooking<br/>~14,270 rows"]

    L["Executive (No Role)<br/>Sees ALL 100K rows"] -.-> G

    style A fill:#C4A265
    style C fill:#DC3545,color:#fff
    style F fill:#2E8B57,color:#fff
    style L fill:#1B365D,color:#fff
```

### ASCII Representation
```
┌──────────────────────────────────────────────────────────────────┐
│                    RLS FILTER PROPAGATION                          │
├──────────────────────────────────────────────────────────────────┤
│                                                                    │
│  USER LOGIN: aman.mehta@tajhotels.com                             │
│       │                                                            │
│       ▼                                                            │
│  ┌────────────────────────────────────────────┐                   │
│  │ USERPRINCIPALNAME() = aman.mehta@tajhotels │                   │
│  └────────────────────┬───────────────────────┘                   │
│                       │                                            │
│                       ▼                                            │
│  ┌────────────────────────────────────────────┐                   │
│  │ dim_Manager FILTERED → 1 row (MGR001)      │                   │
│  │ Aman Mehta | Taj Mahal Palace              │                   │
│  └────────────────────┬───────────────────────┘                   │
│                       │ R3 (Bidirectional)                         │
│                       ▼                                            │
│  ┌────────────────────────────────────────────┐                   │
│  │ dim_Hotel FILTERED → 1 row                 │                   │
│  │ Taj Mahal Palace | Mumbai | Luxury          │                   │
│  └────────────────────┬───────────────────────┘                   │
│                       │ R2 (Many:1 propagation)                    │
│                       ▼                                            │
│  ┌────────────────────────────────────────────┐                   │
│  │ fct_Bookings FILTERED → ~14,270 rows       │                   │
│  │ (Only Taj Mahal Palace bookings)            │                   │
│  └────┬───────────────┬───────────────┬───────┘                   │
│       │ R4            │ R5            │ R6                         │
│       ▼               ▼               ▼                            │
│  ┌──────────┐   ┌──────────┐   ┌──────────────┐                  │
│  │dim_Finance│   │dim_Feedbk│   │dim_OnlineBkg │                  │
│  │~14,270   │   │~14,270   │   │~14,270       │                  │
│  └──────────┘   └──────────┘   └──────────────┘                  │
│                                                                    │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│  EXECUTIVE (No role assigned) → Sees ALL 100,000 rows             │
│                                                                    │
└──────────────────────────────────────────────────────────────────┘
```

---

## 6. Dashboard Navigation Architecture

### Mermaid Code
```mermaid
flowchart TD
    HOME["🏠 HOME<br/>(Landing Page)"] --> ES
    HOME --> RT
    HOME --> HM
    HOME --> CA
    HOME --> GS
    HOME --> MM

    ES["📊 EXECUTIVE SUMMARY"] --> |"Drill-through<br/>Hotel_Name"| HD
    RT["📈 REVENUE & TRENDS"] --> |"Drill-through<br/>Hotel_Name"| HD
    CA["🌐 CHANNEL ANALYSIS"] --> |"Drill-through<br/>Hotel_Name"| HD
    GS["⭐ GUEST SATISFACTION"] --> |"Drill-through<br/>Hotel_Name"| HD

    HM["🏨 HOTEL MANAGER<br/>(RLS Auto-filtered)"]
    MM["👥 MEMBER vs NON-MEMBER"]

    HD["🔍 HOTEL DRILLTHROUGH<br/>(Hidden Page)"]
    BD["📋 BOOKING DETAIL<br/>(Hidden Page)"]

    HD --> |"← Back"| ES
    HD --> |"← Back"| RT
    HD --> |"← Back"| CA
    HD --> |"← Back"| GS
    BD --> |"← Back"| HM

    ES -.-> |"Navigation Bar"| RT
    RT -.-> |"Navigation Bar"| HM
    HM -.-> |"Navigation Bar"| CA
    CA -.-> |"Navigation Bar"| GS
    GS -.-> |"Navigation Bar"| MM

    style HOME fill:#1B365D,color:#fff
    style HD fill:#C4A265,color:#000
    style BD fill:#C4A265,color:#000
    style HM fill:#2E8B57,color:#fff
```

### ASCII Representation
```
                    ┌─────────────────────┐
                    │     🏠 HOME          │
                    │   (Landing Page)     │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│📊 Executive  │    │📈 Revenue &  │    │🏨 Hotel      │
│   Summary    │    │   Trends     │    │   Manager    │
│              │    │              │    │  (RLS Auto)  │
└──────┬───────┘    └──────┬───────┘    └──────────────┘
       │                   │
       │ drill             │ drill         ┌──────────────┐
       │                   │               │🌐 Channel    │
       ▼                   ▼               │   Analysis   │
┌──────────────────────────────────┐       └──────┬───────┘
│ 🔍 HOTEL DRILLTHROUGH (Hidden)   │              │ drill
│ Full hotel detail view           │◄─────────────┘
│ [← Back returns to source]       │
└──────────────────────────────────┘       ┌──────────────┐
                                           │⭐ Guest      │
                                           │  Satisfaction│
       ┌───────────────────────────┐       └──────┬───────┘
       │ 📋 BOOKING DETAIL (Hidden)│              │ drill
       │ Single booking view       │◄─────────────┘
       │ [← Back]                  │
       └───────────────────────────┘       ┌──────────────┐
                                           │👥 Member vs  │
    ════════════════════════════════        │  Non-Member  │
    NAVIGATION BAR on every page           └──────────────┘
    connects all 7 visible pages
    ════════════════════════════════
```

---

## 7. AI-DLC Lifecycle Architecture

### Explanation
The AI-Driven Development Lifecycle methodology used for this project, showing all 5 phases with their deliverables and gate approvals.

### Mermaid Code
```mermaid
flowchart LR
    P1["PHASE 1<br/>INCEPTION<br/>Requirements<br/>Analysis"] --> G1{Gate 1<br/>Approval}
    G1 --> P2["PHASE 2<br/>ELABORATION<br/>Solution<br/>Design"]
    P2 --> G2{Gate 2<br/>Approval}
    G2 --> P3["PHASE 3<br/>CONSTRUCTION<br/>Build &<br/>Implement"]
    P3 --> G3{Gate 3<br/>Approval}
    G3 --> P4["PHASE 4<br/>TESTING<br/>Validate &<br/>Verify"]
    P4 --> G4{Gate 4<br/>Approval}
    G4 --> P5["PHASE 5<br/>DEPLOYMENT<br/>Release &<br/>Document"]

    P1 -.- D1["BRD<br/>Stakeholders<br/>Data Profiling<br/>Clarifying Qs"]
    P2 -.- D2["Star Schema<br/>KPI Defs<br/>RLS Design<br/>Page Specs"]
    P3 -.- D3["Power Query<br/>51 DAX Measures<br/>9 Pages<br/>Build Guide"]
    P4 -.- D4["Test Cases<br/>KPI Validation<br/>RLS Testing<br/>UAT Checklist"]
    P5 -.- D5["Deploy Guide<br/>User Manual<br/>Admin Guide<br/>Runbook"]

    style P1 fill:#1B365D,color:#fff
    style P2 fill:#17A2B8,color:#fff
    style P3 fill:#C4A265,color:#000
    style P4 fill:#2E8B57,color:#fff
    style P5 fill:#8B0000,color:#fff
```

### ASCII Representation
```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ PHASE 1 │───►│ PHASE 2 │───►│ PHASE 3 │───►│ PHASE 4 │───►│ PHASE 5 │
│INCEPTION│ ✓  │ELABORA- │ ✓  │CONSTRUC-│ ✓  │ TESTING │ ✓  │DEPLOY-  │
│         │    │TION     │    │TION     │    │         │    │MENT     │
└────┬────┘    └────┬────┘    └────┬────┘    └────┬────┘    └────┬────┘
     │              │              │              │              │
     ▼              ▼              ▼              ▼              ▼
┌─────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│• BRD    │  │• Schema  │  │• PQ Code │  │• 35 Test │  │• Deploy  │
│• Stakeh.│  │• 40 KPIs │  │• 51 DAX  │  │  Cases   │  │  Guide   │
│• Data   │  │• RLS     │  │• 9 Pages │  │• RLS     │  │• User    │
│  Profile│  │  Design  │  │• Build   │  │  Matrix  │  │  Manual  │
│• Assump.│  │• Slicer  │  │  Guide   │  │• UAT     │  │• Admin   │
│• Risks  │  │  Strategy│  │• Theme   │  │  27 items│  │  Guide   │
│• Qs     │  │• Nav     │  │          │  │• Perf.   │  │• Runbook │
└─────────┘  └──────────┘  └──────────┘  └──────────┘  └──────────┘

Gate:  ✓ Approved    ✓ Approved    ✓ Approved    ✓ Approved    COMPLETE
```

---

## 8. Power BI Service Deployment Architecture

### Mermaid Code
```mermaid
flowchart TD
    subgraph Desktop["DEVELOPMENT"]
        DEV["Power BI Desktop<br/>.pbix file<br/>~20MB"]
    end

    subgraph Service["POWER BI SERVICE (app.powerbi.com)"]
        subgraph Workspace["Workspace: Hospitality Analytics - Production"]
            DS["Dataset<br/>Hospitality_Analytics_Dashboard"]
            RPT["Report<br/>Hospitality_Analytics_Dashboard"]
        end

        subgraph Refresh["Data Refresh"]
            GW["On-Premises Gateway<br/>(if files on network)"]
            SCH["Scheduled Refresh<br/>Daily 6:00 AM IST"]
        end

        subgraph Security["Security Configuration"]
            RLS_ROLE["Role: HotelManager<br/>7 members assigned"]
            PERMS["Workspace Permissions<br/>Admin/Member/Viewer"]
        end

        subgraph Distribution["Distribution"]
            APP_PUB["Published App<br/>Hospitality Analytics"]
        end
    end

    subgraph Users["END USERS"]
        EXEC["Executives<br/>(No RLS role)<br/>Full access"]
        MGR["Hotel Managers ×7<br/>(HotelManager role)<br/>Filtered access"]
        TEAM["Finance/Marketing<br/>(No RLS role)<br/>Full access"]
    end

    DEV -->|"Publish"| Workspace
    GW -->|"Credentials"| DS
    SCH -->|"Triggers"| DS
    DS --> RPT
    RPT --> RLS_ROLE
    RLS_ROLE --> APP_PUB
    PERMS --> APP_PUB
    APP_PUB --> EXEC
    APP_PUB --> MGR
    APP_PUB --> TEAM
```

### ASCII Representation
```
┌───────────────────┐         ┌─────────────────────────────────────────────┐
│ POWER BI DESKTOP  │ Publish │        POWER BI SERVICE                      │
│                   │────────►│                                              │
│ .pbix (~20MB)     │         │  ┌─────────────────────────────────────┐    │
│ Development &     │         │  │ WORKSPACE: Hospitality Analytics    │    │
│ Testing           │         │  │                                     │    │
└───────────────────┘         │  │  ┌──────────┐    ┌──────────────┐  │    │
                              │  │  │ DATASET  │◄───│ GATEWAY      │  │    │
┌───────────────────┐         │  │  │          │    │ (if on-prem) │  │    │
│ SOURCE FILES      │         │  │  └────┬─────┘    └──────────────┘  │    │
│ CSV/Excel         │─ ─ ─ ─ ─│─ │─ ─ ─ ─│─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ │    │
│ (Shared drive /   │         │  │       │ Scheduled Refresh         │    │
│  SharePoint)      │         │  │       │ Daily 6:00 AM IST         │    │
└───────────────────┘         │  │       ▼                           │    │
                              │  │  ┌──────────┐                     │    │
                              │  │  │ REPORT   │                     │    │
                              │  │  │ 9 pages  │                     │    │
                              │  │  └────┬─────┘                     │    │
                              │  │       │                           │    │
                              │  │       ▼                           │    │
                              │  │  ┌──────────────────┐             │    │
                              │  │  │ RLS SECURITY     │             │    │
                              │  │  │ Role: HotelMgr   │             │    │
                              │  │  │ 7 members        │             │    │
                              │  │  └────┬─────────────┘             │    │
                              │  └───────┼─────────────────────────────┘    │
                              │          │                                   │
                              │          ▼                                   │
                              │  ┌──────────────────┐                       │
                              │  │ PUBLISHED APP    │                       │
                              │  │ "Hospitality     │                       │
                              │  │  Analytics"      │                       │
                              │  └────┬────────┬────┘                       │
                              └───────┼────────┼────────────────────────────┘
                                      │        │
                              ┌───────┘        └───────┐
                              ▼                        ▼
                     ┌──────────────┐        ┌──────────────────┐
                     │ EXECUTIVES   │        │ HOTEL MANAGERS    │
                     │ Full data    │        │ Own hotel only    │
                     │ (no role)    │        │ (RLS filtered)    │
                     └──────────────┘        └──────────────────┘
```

---

## 9. User Access Architecture

### Mermaid Code
```mermaid
flowchart TD
    subgraph AAD["AZURE ACTIVE DIRECTORY"]
        U1["aman.mehta@tajhotels.com"]
        U2["kavya.shah@tajhotels.com"]
        U3["rohan.das@tajhotels.com"]
        U4["sneha.roy@tajhotels.com"]
        U5["arjun.rao@tajhotels.com"]
        U6["priya.nair@tajhotels.com"]
        U7["sanjay.iyer@tajhotels.com"]
        U8["ceo@tajhotels.com"]
        U9["cfo@tajhotels.com"]
        U10["bi.lead@tajhotels.com"]
    end

    subgraph Roles["ACCESS CONTROL"]
        R1["Workspace Role: Admin<br/>BI Lead"]
        R2["Workspace Role: Viewer<br/>All others"]
        R3["RLS Role: HotelManager<br/>7 managers"]
        R4["No RLS Role<br/>Executives (full access)"]
    end

    subgraph Data["DATA VISIBILITY"]
        ALL["ALL DATA<br/>100,000 bookings<br/>7 hotels"]
        H1["Taj Mahal Palace<br/>~14,270 rows"]
        H2["Taj Lake Palace<br/>~14,148 rows"]
        H3["Taj Falaknuma<br/>~14,448 rows"]
        H4["Taj Bengal<br/>~14,283 rows"]
        H5["Taj West End<br/>~14,209 rows"]
        H6["Taj Exotica<br/>~14,378 rows"]
        H7["Taj Coromandel<br/>~14,264 rows"]
    end

    U1 --> R3 --> H1
    U2 --> R3 --> H2
    U3 --> R3 --> H3
    U4 --> R3 --> H4
    U5 --> R3 --> H5
    U6 --> R3 --> H6
    U7 --> R3 --> H7
    U8 --> R4 --> ALL
    U9 --> R4 --> ALL
    U10 --> R1 --> ALL
```

---

## 10. End-to-End System Architecture

### Explanation
Comprehensive view combining all architectural layers from infrastructure through data, application, and consumption tiers.

### Mermaid Code
```mermaid
graph TB
    subgraph Infra["INFRASTRUCTURE TIER"]
        FS["File Server / SharePoint<br/>Source CSV & Excel files"]
        GW["Data Gateway<br/>(On-premises)"]
        AAD["Azure Active Directory<br/>User Authentication"]
    end

    subgraph Data["DATA TIER"]
        PQ["Power Query Engine<br/>ETL Processing"]
        IM["In-Memory Model<br/>VertiPaq Compression<br/>~20MB Dataset"]
    end

    subgraph App["APPLICATION TIER"]
        DAX_ENG["DAX Formula Engine<br/>51 Measures"]
        VIS["Visualization Engine<br/>9 Pages, 60+ Visuals"]
        RLS_ENG["Security Engine<br/>RLS Filter Evaluation"]
    end

    subgraph Service_Tier["SERVICE TIER"]
        PBI["Power BI Service<br/>app.powerbi.com"]
        REFRESH["Refresh Service<br/>Daily 6AM IST"]
        EMBED["App Distribution<br/>Published App"]
    end

    subgraph Consumer["CONSUMPTION TIER"]
        BROWSER["Web Browser<br/>app.powerbi.com"]
        MOBILE["Power BI Mobile<br/>iOS / Android"]
        EXPORT["Excel Export<br/>PDF / PowerPoint"]
    end

    FS --> GW
    GW --> PQ
    PQ --> IM
    IM --> DAX_ENG
    DAX_ENG --> VIS
    VIS --> RLS_ENG
    RLS_ENG --> PBI
    AAD --> RLS_ENG
    PBI --> REFRESH
    REFRESH --> FS
    PBI --> EMBED
    EMBED --> BROWSER
    EMBED --> MOBILE
    EMBED --> EXPORT
```

### ASCII Representation
```
╔══════════════════════════════════════════════════════════════════════════════╗
║                    END-TO-END SYSTEM ARCHITECTURE                           ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║  ┌─────────────────────────────────────────────────────────────────────┐   ║
║  │ INFRASTRUCTURE TIER                                                  │   ║
║  │  [File Server/SharePoint] ──── [Data Gateway] ──── [Azure AD]       │   ║
║  └─────────────────────────────────┬───────────────────────────────────┘   ║
║                                    │                                        ║
║  ┌─────────────────────────────────▼───────────────────────────────────┐   ║
║  │ DATA TIER                                                            │   ║
║  │  [Power Query ETL] ──── [VertiPaq In-Memory Model ~20MB]            │   ║
║  │   7 queries, 64 steps      7 tables, 6 relationships                │   ║
║  └─────────────────────────────────┬───────────────────────────────────┘   ║
║                                    │                                        ║
║  ┌─────────────────────────────────▼───────────────────────────────────┐   ║
║  │ APPLICATION TIER                                                     │   ║
║  │  [DAX Engine] ─── [Visualization Engine] ─── [RLS Security Engine]  │   ║
║  │   51 measures       9 pages, 60+ visuals      1 role, 7 members     │   ║
║  └─────────────────────────────────┬───────────────────────────────────┘   ║
║                                    │                                        ║
║  ┌─────────────────────────────────▼───────────────────────────────────┐   ║
║  │ SERVICE TIER (Power BI Service)                                      │   ║
║  │  [Workspace] ─── [Scheduled Refresh] ─── [Published App]            │   ║
║  │   Pro License      Daily 6AM IST          Hospitality Analytics     │   ║
║  └─────────────────────────────────┬───────────────────────────────────┘   ║
║                                    │                                        ║
║  ┌─────────────────────────────────▼───────────────────────────────────┐   ║
║  │ CONSUMPTION TIER                                                     │   ║
║  │                                                                      │   ║
║  │  ┌────────────┐   ┌─────────────┐   ┌────────────┐                 │   ║
║  │  │ Web Browser│   │ Mobile App  │   │ Export     │                 │   ║
║  │  │ (Desktop)  │   │ (iOS/And)   │   │ (Excel/PDF)│                 │   ║
║  │  └──────┬─────┘   └──────┬──────┘   └──────┬─────┘                 │   ║
║  │         │                │                 │                         │   ║
║  │         ▼                ▼                 ▼                         │   ║
║  │  ┌────────────────────────────────────────────────────┐             │   ║
║  │  │ Executives (Full) │ Managers (RLS) │ Teams (Full)  │             │   ║
║  │  │    3 users         │    7 users      │   5+ users   │             │   ║
║  │  └────────────────────────────────────────────────────┘             │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                             ║
╚══════════════════════════════════════════════════════════════════════════════╝

METRICS:
  Data Volume: 100,000 bookings | 7 tables | 500K+ data points
  Performance: <5 sec page load | <60 sec refresh | ~20MB model
  Security: 1 dynamic RLS role | 7 filtered users | Executives unfiltered
  Availability: Daily refresh 6AM IST | 99.9% SLA (Power BI Service)
```

---

## Summary of All Diagrams

| # | Diagram | Purpose | Format Provided |
|---|---------|---------|-----------------|
| 1 | Overall Solution Architecture | Full stack overview | Mermaid + PlantUML + ASCII |
| 2 | Star Schema | Data model relationships | Mermaid ER + ASCII |
| 3 | Data Flow | Source to consumption pipeline | Mermaid + ASCII |
| 4 | ETL Architecture | Power Query processing stages | Mermaid |
| 5 | RLS Architecture | Security filter propagation | Mermaid + ASCII |
| 6 | Dashboard Navigation | Page flow and drill-through | Mermaid + ASCII |
| 7 | AI-DLC Lifecycle | Methodology phases | Mermaid + ASCII |
| 8 | Deployment Architecture | Power BI Service components | Mermaid + ASCII |
| 9 | User Access | Authentication and authorization | Mermaid |
| 10 | End-to-End System | Complete 5-tier architecture | Mermaid + ASCII |

---

## Usage Notes

- **Mermaid diagrams** can be rendered in GitHub, Azure DevOps, Notion, or any Mermaid-compatible viewer
- **PlantUML diagrams** require a PlantUML renderer (VS Code extension, online at plantuml.com)
- **ASCII diagrams** are universally viewable in any text editor or documentation system
- All diagrams are suitable for client presentations, technical documentation, and architecture review boards

*End of Architecture Diagrams*
