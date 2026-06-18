# Dashboard Page Design
## Complete Page-by-Page Implementation Specification

**Canvas:** 1920 × 1080 pixels (Full HD)  
**Font (Text):** Segoe UI  
**Font (Numbers):** Calibri  
**Theme:** Navy (#1B365D) + Gold (#C4A265)

---

## Page 1: Executive Summary

### Business Objective
Provide senior leadership with a single-glance view of portfolio-wide KPIs, revenue trend, hotel comparison, and distribution by category and channel.

### Business Questions Answered
- What is total revenue and profit across the portfolio?
- How does revenue trend year over year?
- Which hotels generate the most revenue?
- What is the category-wise revenue split?
- Which channels drive the most bookings?

### Layout Wireframe (1920×1080)
```
┌─────────────────────────── NAVIGATION BAR (40px) ──────────────────────────┐
│ [●Summary] [Revenue] [Manager] [Channels] [Satisfaction] [Membership]      │
├────────────────────────────────────────────────────────────────────────────┤
│ Row 1 (y:50, h:120) — KPI CARDS                          │ SLICERS        │
│ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐  │ Year ▼         │
│ │Revenue │ │Bookings│ │  ADR   │ │ Profit │ │  GSS   │  │ Hotel ▼        │
│ │₹5.7B   │ │90,091  │ │₹23,456 │ │₹2.6B   │ │ 3.42  │  │ Category ▼     │
│ │▲ 12.3% │ │▲ 8.1%  │ │▲ 5.2%  │ │▲ 15.1% │ │● 0.0  │  │ City ▼         │
│ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘  │ Quarter [][][]  │
├────────────────────────────────────────┬───────────────────┴────────────────┤
│ Row 2 (y:180, h:380)                  │                                    │
│                                        │                                    │
│ REVENUE BY YEAR (Line Chart)           │ REVENUE BY HOTEL (Bar Chart)       │
│ X: dim_Date[Year]                      │ Y-Axis: dim_Hotel[Hotel_Name]      │
│ Y: [Total Revenue]                     │ X-Axis: [Total Revenue]            │
│ Secondary Y: [Revenue PY] (dashed)     │ Sort: Descending by value          │
│                                        │ Data labels: ON                    │
│ w:900, h:380                           │ w:900, h:380                       │
│                                        │                                    │
├────────────────────────────────────────┼────────────────────────────────────┤
│ Row 3 (y:570, h:380)                  │                                    │
│                                        │                                    │
│ REVENUE BY CATEGORY (Donut Chart)      │ CHANNEL DISTRIBUTION (Donut Chart) │
│ Legend: fct_Bookings[Category]          │ Legend: fct_Bookings[Channel]       │
│ Values: [Total Revenue]                │ Values: [Total Bookings]           │
│ Colors: Luxury=#C4A265,               │ Colors: Website=#1B365D,           │
│   Heritage=#8B0000,                    │   App=#17A2B8, Agent=#C4A265,      │
│   Business=#1B365D, Resort=#17A2B8     │   ThirdParty=#6C757D               │
│ Detail labels: Category + %            │ Detail labels: Channel + %          │
│ w:900, h:380                           │ w:900, h:380                       │
│                                        │                                    │
└────────────────────────────────────────┴────────────────────────────────────┘
│ FOOTER: [Last Refresh] measure                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Visual Specifications

| # | Visual | Type | Width | Height | X | Y |
|---|--------|------|-------|--------|---|---|
| V1 | Total Revenue | Card | 180 | 110 | 20 | 50 |
| V2 | Successful Bookings | Card | 180 | 110 | 210 | 50 |
| V3 | ADR | Card | 180 | 110 | 400 | 50 |
| V4 | Net Profit | Card | 180 | 110 | 590 | 50 |
| V5 | GSS | Card | 180 | 110 | 780 | 50 |
| V6 | Revenue by Year | Line Chart | 900 | 380 | 20 | 180 |
| V7 | Revenue by Hotel | Bar Chart | 900 | 380 | 940 | 180 |
| V8 | Revenue by Category | Donut | 900 | 380 | 20 | 570 |
| V9 | Channel Distribution | Donut | 900 | 380 | 940 | 570 |
| V10 | Year Slicer | Dropdown | 160 | 40 | 1000 | 55 |
| V11 | Hotel Slicer | Dropdown | 160 | 40 | 1000 | 100 |
| V12 | Category Slicer | Buttons | 400 | 35 | 1180 | 55 |
| V13 | Quarter Slicer | Buttons | 250 | 35 | 1180 | 100 |

### KPI Card Details

| Card | Measure | Subtitle | Format | Conditional Color |
|------|---------|----------|--------|-------------------|
| Revenue | [Total Revenue] | [Revenue Trend Icon] | ₹#,##0,,"B" | Green if YoY>0, Red if <0 |
| Bookings | [Successful Bookings] | "Paid bookings" | #,##0 | Navy (always) |
| ADR | [ADR] | "Per room-night" | ₹#,##0 | Navy (always) |
| Profit | [Net Profit] | [Profit Margin %] & "% margin" | ₹#,##0,,"B" | Green if margin>30%, else amber |
| GSS | [GSS] | [GSS Band] | 0.00 | Green ≥4.0, Amber 3.0–3.9, Red <3.0 |

### Cross-Filter Interactions (Page 1)
| Source Visual | Target | Interaction |
|--------------|--------|-------------|
| Revenue by Year (Line) | All others | Highlight |
| Revenue by Hotel (Bar) | All others | Filter |
| Revenue by Category (Donut) | All others | Highlight |
| Channel Distribution (Donut) | All others | Highlight |
| All Slicers | All visuals | Filter |

### Conditional Formatting
| Visual | Rule | Color |
|--------|------|-------|
| Revenue Card subtitle | YoY > 0 | Green (#2E8B57) |
| Revenue Card subtitle | YoY < 0 | Red (#DC3545) |
| GSS Card value | ≥ 4.0 | Green (#2E8B57) |
| GSS Card value | 3.0–3.99 | Amber (#FFC107) |
| GSS Card value | < 3.0 | Red (#DC3545) |
| Hotel Bar chart | Data bars | Gradient Navy → Gold |

### Titles
- Page Title: "Hospitality Analytics — Executive Summary"
- Subtitle: "Portfolio Performance Overview"
- Chart titles: "Revenue Trend by Year", "Revenue by Hotel", "Revenue by Category", "Booking Channel Mix"

---

## Page 2: Revenue & Trends (2021–2025)

### Business Objective
Provide deep revenue analysis with time-series trends, drill-down capabilities, cost comparison, and profitability tracking across room types and time periods.

### Business Questions Answered
- How has revenue grown year-over-year?
- What is the monthly/quarterly revenue pattern?
- How do costs compare to revenue over time?
- Which room types contribute most to revenue?
- What is the profit margin trend?
- What are the YTD, QTD, MTD figures?

### Layout Wireframe
```
┌─────────────────────────── NAVIGATION BAR ─────────────────────────────────┐
├────────────────────────────────────────────────────────────────────────────┤
│ Row 1 — KPI Row                                          │ SLICERS        │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐    │ Year ▼         │
│ │YoY Growth│ │Revenue   │ │ Revenue  │ │ Profit   │    │ Room Type [][] │
│ │  +12.3%  │ │  YTD     │ │  QTD     │ │ Margin   │    │ Category [][][][│
│ │  ▲       │ │ ₹1.2B    │ │ ₹450M    │ │  42.3%   │    │ Quarter [][][][] │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘    │                │
├────────────────────────────────────────────────────────────┴────────────────┤
│ Row 2 — MONTHLY REVENUE TREND (Area Chart)                                  │
│ X-Axis: dim_Date[MonthYear]  (hierarchy: Year→Quarter→Month)               │
│ Y-Axis: [Total Revenue]                                                     │
│ Legend: dim_Date[Year] (multi-line comparison)                              │
│ Drill-down enabled: Year → Quarter → Month                                  │
│ Tooltips: [Total Revenue], [Revenue PY], [YoY Revenue Growth %]           │
│ h: 320                                                                      │
├──────────────────────────────────────────┬──────────────────────────────────┤
│ Row 3                                    │                                  │
│ COST vs REVENUE (Combo Chart)            │ REVENUE BY ROOM TYPE            │
│ X-Axis: dim_Date[YearQuarter]            │ (Stacked Bar Chart)             │
│ Column Y: [Total Revenue]                │ X-Axis: dim_Date[Year]          │
│ Line Y: [Total Cost]                     │ Y-Axis: [Total Revenue]         │
│ Secondary Line: [Net Profit]             │ Legend: fct_Bookings[RoomType]   │
│ h: 300                                   │ Colors: Standard=#1B365D,       │
│                                          │   Deluxe=#C4A265, Suite=#17A2B8 │
│                                          │ h: 300                          │
├──────────────────────────────────────────┴──────────────────────────────────┤
│ Row 4 — PROFIT MARGIN TREND (Line Chart)                                    │
│ X-Axis: dim_Date[YearMonth]                                                │
│ Y-Axis: [Profit Margin %]                                                  │
│ Constant line at 30% (target)                                              │
│ h: 200                                                                      │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Visual Specifications

| # | Visual | Fields - Axis | Fields - Values | Fields - Legend | Tooltips |
|---|--------|---------------|-----------------|----------------|----------|
| V1 | YoY Growth Card | — | [YoY Revenue Growth %] | — | — |
| V2 | Revenue YTD Card | — | [Revenue YTD] | — | — |
| V3 | Revenue QTD Card | — | [Revenue QTD] | — | — |
| V4 | Profit Margin Card | — | [Profit Margin %] | — | — |
| V5 | Monthly Revenue Area | dim_Date[MonthYear] | [Total Revenue] | dim_Date[Year] | [Revenue PY], [YoY Revenue Growth %] |
| V6 | Cost vs Revenue Combo | dim_Date[YearQuarter] | Column: [Total Revenue], Line: [Total Cost], Line2: [Net Profit] | — | [Profit Margin %] |
| V7 | Revenue by Room Type | dim_Date[Year] | [Total Revenue] | fct_Bookings[RoomType] | [Revenue % of Total] |
| V8 | Profit Margin Trend | dim_Date[YearMonth] | [Profit Margin %] | — | [Net Profit], [Total Revenue] |

### Conditional Formatting
| Visual | Rule |
|--------|------|
| YoY Growth Card | Green if positive, Red if negative |
| Profit Margin Trend | Area below line colored Red when < 30% |
| Revenue Area Chart | Revenue color intensity by magnitude |

---

## Page 3: Hotel Manager View (RLS-Secured)

### Business Objective
Provide each hotel manager with a personalized view of their property's performance. This page is fully controlled by RLS — managers see only their own hotel's data without any manual filtering needed.

### Business Questions Answered
- How is my hotel performing in revenue and bookings?
- What is my guest satisfaction score?
- What room types are most popular at my hotel?
- What payment methods do my guests prefer?
- What are the top feedback issues at my property?
- What is my monthly revenue trend?

### Layout Wireframe
```
┌─────────────────────────── NAVIGATION BAR ─────────────────────────────────┐
├────────────────────────────────────────────────────────────────────────────┤
│ HEADER: [Manager Title] measure — dynamic (e.g., "Aman Mehta — Taj Mahal") │
│ Subtitle: City + Category (e.g., "Mumbai | Luxury")                        │
├────────────────────────────────────────────────────────────────────────────┤
│ Row 1 — KPI CARDS                                                          │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│ │My Revenue│ │My Bookings│ │  My GSS  │ │My Profit │ │My Profit%│        │
│ │ ₹815M    │ │ 12,892   │ │  3.45    │ │ ₹370M    │ │  45.4%   │        │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘        │
├────────────────────────────────────────┬───────────────────────────────────┤
│ Row 2                                  │                                   │
│ MONTHLY REVENUE TREND (Line Chart)     │ ROOM TYPE MIX (Donut Chart)       │
│ X: dim_Date[MonthYear]                 │ Legend: fct_Bookings[RoomType]     │
│ Y: [Total Revenue]                     │ Values: [Successful Bookings]     │
│ Forecast line (optional)               │ Detail labels: Type + %           │
│ h: 320                                 │ h: 320                            │
├────────────────────────────────────────┼───────────────────────────────────┤
│ Row 3                                  │                                   │
│ TOP FEEDBACK ISSUES (Table)            │ PAYMENT METHOD (Clustered Bar)    │
│ Columns:                               │ Y-Axis: PaymentMethod             │
│   Feedback, Count, Sentiment           │ X-Axis: [Successful Bookings]     │
│ Sort: Count descending                 │ Sort: Descending                  │
│ Conditional format: Red for Negative   │ Data labels: ON                   │
│ h: 280                                 │ h: 280                            │
└────────────────────────────────────────┴───────────────────────────────────┘
```

### Visual Specifications

| # | Visual | Fields - Axis | Fields - Values | Fields - Legend | Tooltips |
|---|--------|---------------|-----------------|----------------|----------|
| V1 | Header | — | [Manager Title] | — | — |
| V2 | Revenue Card | — | [Total Revenue] | — | [YoY Revenue Growth %] |
| V3 | Bookings Card | — | [Successful Bookings] | — | — |
| V4 | GSS Card | — | [GSS] | — | [GSS Band] |
| V5 | Profit Card | — | [Net Profit] | — | — |
| V6 | Margin Card | — | [Profit Margin %] | — | — |
| V7 | Revenue Trend | dim_Date[MonthYear] | [Total Revenue] | — | [Revenue PY], [YoY Revenue Growth %] |
| V8 | Room Type Donut | — | [Successful Bookings] | fct_Bookings[RoomType] | [Total Revenue] |
| V9 | Feedback Table | dim_Feedback[Feedback] | Count of BookingID, [Positive Feedback %] | — | — |
| V10 | Payment Bar | fct_Bookings[PaymentMethod] | [Successful Bookings] | — | [Total Revenue] |

### Notes
- NO slicers on this page — RLS handles all filtering automatically
- Dynamic title uses [Manager Title] measure
- Year slicer ONLY (synced from navigation) if manager wants to filter by year
- All visuals automatically scoped to the logged-in manager's hotel

---

## Page 4: Booking Source & Channel Analysis

### Business Objective
Analyze booking channel effectiveness, device preferences, and revenue contribution by channel to inform marketing strategy and digital investment.

### Business Questions Answered
- What percentage of bookings come from each channel?
- Is mobile or desktop dominant?
- How have channel preferences changed over time?
- Which channels generate the most revenue?
- How does channel mix vary by hotel?

### Layout Wireframe
```
┌─────────────────────────── NAVIGATION BAR ─────────────────────────────────┐
├────────────────────────────────────────────────────────────────────────────┤
│ Row 1 — DISTRIBUTION                                     │ SLICERS        │
│ ┌───────────────────────┐ ┌───────────────────────┐      │ Year ▼         │
│ │ CHANNEL DISTRIBUTION  │ │ DEVICE TYPE SPLIT      │      │ Hotel ▼        │
│ │ (Donut Chart)         │ │ (Donut Chart)          │      │ Channel [][][][│
│ │ Website: 45%          │ │ Mobile: 62%            │      │ Device [][]    │
│ │ App: 25%              │ │ Desktop: 38%           │      │                │
│ │ Agent: 17%            │ │                        │      │                │
│ │ ThirdParty: 13%       │ │                        │      │                │
│ └───────────────────────┘ └───────────────────────┘      │                │
├────────────────────────────────────────────────────────────┴────────────────┤
│ Row 2 — CHANNEL TREND OVER TIME (Stacked Area Chart)                        │
│ X-Axis: dim_Date[YearQuarter]                                              │
│ Y-Axis: [Total Bookings]                                                   │
│ Legend: fct_Bookings[Channel]                                              │
│ h: 300                                                                      │
├──────────────────────────────────────────┬──────────────────────────────────┤
│ Row 3                                    │                                  │
│ CHANNEL REVENUE (Clustered Bar)          │ HOTEL × CHANNEL (Matrix)         │
│ Y-Axis: fct_Bookings[Channel]            │ Rows: dim_Hotel[Hotel_Name]      │
│ X-Axis: [Total Revenue]                  │ Columns: fct_Bookings[Channel]   │
│ Data labels: ON                          │ Values: [Total Bookings]         │
│ Sort: Revenue descending                 │ Conditional formatting: Heatmap  │
│ h: 280                                   │ h: 280                           │
└──────────────────────────────────────────┴──────────────────────────────────┘
```

### Visual Specifications

| # | Visual | Fields - Axis | Fields - Values | Fields - Legend | Tooltips |
|---|--------|---------------|-----------------|----------------|----------|
| V1 | Channel Donut | — | [Total Bookings] | fct_Bookings[Channel] | [Total Revenue], [Revenue % of Total] |
| V2 | Device Donut | — | [Total Bookings] | fct_Bookings[DeviceType] | [Total Revenue] |
| V3 | Channel Trend | dim_Date[YearQuarter] | [Total Bookings] | fct_Bookings[Channel] | — |
| V4 | Channel Revenue Bar | fct_Bookings[Channel] | [Total Revenue] | — | [Revenue Per Booking], [Successful Bookings] |
| V5 | Hotel×Channel Matrix | Rows: Hotel_Name | [Total Bookings] | Cols: Channel | [Total Revenue] |

---

## Page 5: Feedback & Guest Satisfaction

### Business Objective
Monitor guest experience quality through ratings, feedback analysis, and satisfaction trends to identify service improvement opportunities.

### Business Questions Answered
- What is the overall Guest Satisfaction Score?
- How do staff and cleanliness ratings compare?
- What are the most common feedback categories?
- Which hotels have the highest/lowest satisfaction?
- Is satisfaction improving or declining over time?
- What percentage of feedback is positive vs negative?

### Layout Wireframe
```
┌─────────────────────────── NAVIGATION BAR ─────────────────────────────────┐
├────────────────────────────────────────────────────────────────────────────┤
│ Row 1 — SCORE OVERVIEW                                   │ SLICERS        │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐        │ Year ▼         │
│ │  GSS    │ │ Overall │ │ Staff   │ │Cleanli- │        │ Hotel ▼        │
│ │ GAUGE   │ │ Rating  │ │ Rating  │ │ness     │        │ Category [][][][│
│ │ 3.42    │ │  3.38   │ │  3.41   │ │  3.47   │        │ Sentiment [][]  │
│ │target:4 │ │         │ │         │ │         │        │                │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘        │                │
├────────────────────────────────────────┬─────────────────┴──────────────────┤
│ Row 2                                  │                                    │
│ FEEDBACK CATEGORIES                    │ GSS BY HOTEL                       │
│ (Horizontal Bar Chart)                 │ (Clustered Bar Chart)              │
│ Y-Axis: dim_Feedback[Feedback]         │ Y-Axis: dim_Hotel[Hotel_Name]      │
│ X-Axis: Count of BookingID             │ X-Axis: [GSS]                      │
│ Color by: FeedbackSentiment            │ Constant line at 4.0 (target)      │
│  Positive = Green, Negative = Red      │ Sort: GSS descending               │
│ Sort: Count descending                 │ Data labels: ON                    │
│ h: 320                                 │ h: 320                             │
├────────────────────────────────────────┼────────────────────────────────────┤
│ Row 3                                  │                                    │
│ RATING TREND (Multi-Line Chart)        │ SATISFACTION BY CATEGORY           │
│ X-Axis: dim_Date[YearQuarter]          │ (Clustered Bar)                    │
│ Lines: [Avg Rating],                   │ Y-Axis: dim_Hotel[Category]        │
│   [Avg Staff Rating],                  │ X-Axis: [GSS]                      │
│   [Avg Cleanliness Rating]             │ Data labels: ON                    │
│ Legend: "Overall", "Staff", "Clean"     │ Conditional: Color by score band   │
│ h: 280                                 │ h: 280                             │
└────────────────────────────────────────┴────────────────────────────────────┘
```

### Visual Specifications

| # | Visual | Fields - Axis | Fields - Values | Fields - Legend | Tooltips |
|---|--------|---------------|-----------------|----------------|----------|
| V1 | GSS Gauge | — | [GSS], Target=4.0, Max=5.0 | — | [GSS Band] |
| V2 | Overall Rating Card | — | [Avg Rating] | — | — |
| V3 | Staff Rating Card | — | [Avg Staff Rating] | — | — |
| V4 | Cleanliness Card | — | [Avg Cleanliness Rating] | — | — |
| V5 | Feedback Categories | dim_Feedback[Feedback] | Count of BookingID | dim_Feedback[FeedbackSentiment] | [Positive Feedback %] |
| V6 | GSS by Hotel | dim_Hotel[Hotel_Name] | [GSS] | — | [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating] |
| V7 | Rating Trend | dim_Date[YearQuarter] | [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating] | — | [GSS] |
| V8 | Satisfaction by Category | dim_Hotel[Category] | [GSS] | — | [Positive Feedback %] |

---

## Page 6: Member vs. Non-Member Financial Summary

### Business Objective
Compare the financial performance and behavior between loyalty program members and non-members to evaluate membership program effectiveness and inform retention strategy.

### Business Questions Answered
- How does member revenue compare to non-member?
- Are members more profitable?
- Do members get more discounts?
- Which room types do members prefer?
- How has the member/non-member split changed over time?
- What is the ADR difference between segments?

### Layout Wireframe
```
┌─────────────────────────── NAVIGATION BAR ─────────────────────────────────┐
├────────────────────────────────────────────────────────────────────────────┤
│ Row 1 — KPI COMPARISON                                   │ SLICERS        │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐    │ Year ▼         │
│ │Member    │ │NonMember │ │Member %  │ │Revenue   │    │ Hotel ▼        │
│ │Revenue   │ │Revenue   │ │of Total  │ │Split Pct │    │ MemberType [][] │
│ │₹3.1B     │ │₹2.6B     │ │  55.1%   │ │  M:55/NM:45│  │ RoomType [][][]│
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘    │                │
├────────────────────────────────────────────────────────────┴────────────────┤
│ Row 2 — REVENUE BY YEAR × MEMBER TYPE (100% Stacked Bar)                    │
│ X-Axis: dim_Date[Year]                                                     │
│ Y-Axis: [Total Revenue]                                                    │
│ Legend: fct_Bookings[MemberType]                                           │
│ Colors: Member=#1B365D, Non-Member=#C4A265                                 │
│ h: 280                                                                      │
├──────────────────────────────────────────┬──────────────────────────────────┤
│ Row 3                                    │                                  │
│ PROFIT COMPARISON (Clustered Bar)        │ DISCOUNT ANALYSIS                │
│ Y-Axis: fct_Bookings[MemberType]         │ (Clustered Column)              │
│ X-Axis: [Net Profit]                     │ X-Axis: fct_Bookings[MemberType] │
│ Data labels: ON                          │ Y-Axis: [Avg Discount %]         │
│                                          │         [Total Discounts]        │
│ h: 250                                   │ h: 250                           │
├──────────────────────────────────────────┼──────────────────────────────────┤
│ Row 4                                    │                                  │
│ ADR COMPARISON (Cards side by side)      │ ROOM TYPE × MEMBER (Matrix)      │
│ ┌──────────┐ ┌──────────┐               │ Rows: fct_Bookings[MemberType]   │
│ │Member ADR│ │NM ADR    │               │ Cols: fct_Bookings[RoomType]     │
│ │ ₹24,100  │ │ ₹22,800  │               │ Values: [Successful Bookings]    │
│ └──────────┘ └──────────┘               │ Conditional: Heatmap             │
│ h: 120                                   │ h: 250                           │
└──────────────────────────────────────────┴──────────────────────────────────┘
```

### Visual Specifications

| # | Visual | Fields - Axis | Fields - Values | Fields - Legend | Tooltips |
|---|--------|---------------|-----------------|----------------|----------|
| V1 | Member Revenue Card | — | [Member Revenue] | — | [Member Booking %] |
| V2 | Non-Member Revenue Card | — | [Non-Member Revenue] | — | — |
| V3 | Member % Card | — | [Member Booking %] | — | — |
| V4 | Revenue Split Card | — | Text: "M:55 / NM:45" | — | — |
| V5 | Revenue by Year Stacked | dim_Date[Year] | [Total Revenue] | fct_Bookings[MemberType] | [YoY Revenue Growth %] |
| V6 | Profit Comparison | fct_Bookings[MemberType] | [Net Profit] | — | [Profit Margin %] |
| V7 | Discount Analysis | fct_Bookings[MemberType] | [Avg Discount %], [Total Discounts] | — | — |
| V8 | Member ADR Card | — | [Member ADR] | — | — |
| V9 | Non-Member ADR Card | — | [Non-Member ADR] | — | — |
| V10 | Room × Member Matrix | Rows: MemberType, Cols: RoomType | [Successful Bookings] | — | [Total Revenue] |
