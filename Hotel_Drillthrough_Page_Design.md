# Hotel Drillthrough Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Hotel Drillthrough |
| Page Visibility | **Hidden** (not in navigation bar or tab strip) |
| Canvas Size | 1920 × 1080 px |
| Background Color | #F5F6FA |
| Drill-Through Filter Field | dim_Hotel[Hotel_Name] |
| Trigger | Right-click any Hotel_Name value on Pages 1,2,4,5 → "Drill through → Hotel Drillthrough" |
| Purpose | Full 360° view of a single hotel's performance when users want to deep-dive |

---

## 2. Drill-Through Configuration

### 2.1 Filter Field Setup

In Power BI Desktop:
1. Navigate to the "Hotel Drillthrough" page
2. In the Visualizations pane, find **"Add drill-through fields here"** (below Filters pane)
3. Drag **dim_Hotel[Hotel_Name]** into that well
4. Set "Keep all filters" = **ON** (preserves Year slicer from source page)

### 2.2 Source Pages That Can Drill Here

| Source Page | Triggering Visual | Right-Click Field |
|-------------|-------------------|-------------------|
| Executive Summary | Revenue by Hotel (bar chart) | Hotel_Name on Y-axis |
| Revenue & Trends | Category bar (filtered to hotel) | Hotel_Name in tooltip/context |
| Channel Analysis | Hotel×Channel Matrix (row) | Hotel_Name in rows |
| Guest Satisfaction | GSS by Hotel (bar chart) | Hotel_Name on Y-axis |

### 2.3 Cross-Filter Preservation

When a user drills through from Page 1 with Year=2023 selected:
- The drill-through page shows data for BOTH the selected hotel AND Year=2023
- All active slicer selections from the source page are preserved

---

## 3. Wireframe (1920 × 1080)

```
┌══════════════════════════════════════════════════════════════════════════════════════┐
║ y:0  HEADER BAR (h:80) Background: #1B365D                                          ║
║                                                                                      ║
║  [← Back]   "Taj Mahal Palace"              "Mumbai | Luxury"                       ║
║  (button)    Segoe UI 24pt Bold White        Segoe UI 14pt #B0BEC5                  ║
║              Manager: Aman Mehta (MGR001)                                            ║
║                                                                                      ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:85  KPI ROW (h:110)                                                                ║
║                                                                                      ║
║  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐    ║
║  │₹734M   │ │₹336M   │ │12,860  │ │₹28,100 │ │ 4.13   │ │ 45.8%  │ │ 72%    │    ║
║  │Revenue │ │Net     │ │Book-   │ │ADR     │ │GSS     │ │Margin  │ │Positive│    ║
║  │        │ │Profit  │ │ings    │ │        │ │        │ │   %    │ │Feedback│    ║
║  └────────┘ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘    ║
║  x:20      x:280      x:540      x:800      x:1060     x:1320     x:1580          ║
║  w:240     w:240      w:240      w:240      w:240      w:240      w:240            ║
╠════════════════════════════════════════════╦═════════════════════════════════════════╣
║ y:205  ROW 1 (h:300)                       ║                                         ║
║                                            ║  ROOM TYPE DISTRIBUTION                  ║
║  MONTHLY REVENUE TREND (Line)              ║  (Donut Chart)                           ║
║  x:20, y:205, w:1100, h:300               ║  x:1140, y:205, w:760, h:300             ║
║                                            ║                                         ║
║  X: dim_Date[MonthYear]                    ║  Legend: RoomType                        ║
║  Y: [Total Revenue]                        ║  Values: [Successful Bookings]           ║
║  Secondary: [Revenue PY]                   ║                                         ║
║                                            ║                                         ║
╠════════════════════════════════════════════╬═════════════════════════════════════════╣
║ y:515  ROW 2 (h:240)                       ║                                         ║
║                                            ║  PAYMENT METHOD                          ║
║  CHANNEL BREAKDOWN (Clustered Bar)         ║  (Clustered Bar)                         ║
║  x:20, y:515, w:620, h:240                ║  x:660, y:515, w:620, h:240              ║
║                                            ║                                         ║
║  Y: Channel                                ║  Y: PaymentMethod                        ║
║  X: [Total Revenue]                        ║  X: [Successful Bookings]                ║
║                                            ║                                         ║
║  ──────────────────────────────────────────║──────────────────────────────────────── ║
║                                            ║  MEMBER SPLIT (Donut)                    ║
║                                            ║  x:1300, y:515, w:600, h:240             ║
║                                            ║  Legend: MemberType                      ║
║                                            ║  Values: [Total Revenue]                 ║
╠════════════════════════════════════════════╩═════════════════════════════════════════╣
║ y:765  ROW 3 (h:235)                                                                 ║
║                                                                                      ║
║  FEEDBACK TABLE                             RATING BREAKDOWN CARDS                   ║
║  x:20, y:765, w:1100, h:235                x:1140, y:765, w:760, h:235              ║
║                                                                                      ║
║  Columns: Feedback | Count | Sentiment     ┌────────┐ ┌────────┐ ┌────────┐        ║
║  Sort: Count desc                           │Overall │ │ Staff  │ │ Clean  │        ║
║  Conditional: Red for Negative              │  4.13  │ │  4.10  │ │  4.15  │        ║
║                                             └────────┘ └────────┘ └────────┘        ║
║                                                                                      ║
║                                             GUEST DEMOGRAPHICS (Mini bar)            ║
║                                             Top 5 Countries by booking count         ║
║                                                                                      ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:1010  FOOTER (h:30)                                                                ║
║ [Last Refresh]   [← Back to previous page]    Internal Use Only                      ║
╚══════════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element Specifications

### 4.1 Header Bar (Dark)

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Back Button | Button (Back) | 20 | 20 | 100 | 40 | "← Back" | White text, transparent bg, rounded |
| Hotel Name | Card (drill field) | 140 | 12 | 700 | 36 | dim_Hotel[Hotel_Name] from context | Segoe UI 24pt Bold White, no bg |
| Location | Card (measure) | 140 | 50 | 400 | 22 | [Manager Subtitle] (M53) → "Mumbai \| Luxury" | Segoe UI 14pt #B0BEC5, no bg |
| Manager Info | Card | 900 | 20 | 500 | 22 | "Manager: " & dim_Manager[ManagerName] | Segoe UI 12pt #B0BEC5, no bg |
| Background | Shape (Rectangle) | 0 | 0 | 1920 | 80 | — | Fill: #1B365D |

**Back Button Configuration:**
1. Insert → Buttons → **Back** (Power BI native back button)
2. Format → Style: Outline, color White, text "← Back"
3. Format → Fill: Transparent
4. Format → Border: 1px White, radius 4px
5. Position: x:20, y:20, w:100, h:40
6. Action: Back (automatically returns to drill-through source page)

### 4.2 KPI Cards Row (7 cards)

| Card# | Measure | x | y | w | h | Format | Label |
|-------|---------|---|---|---|---|--------|-------|
| 1 | [Total Revenue] (M01) | 20 | 90 | 240 | 105 | ₹#,##0,,"M" | "Revenue" |
| 2 | [Net Profit] (M03) | 280 | 90 | 240 | 105 | ₹#,##0,,"M" | "Net Profit" |
| 3 | [Successful Bookings] (M13) | 540 | 90 | 240 | 105 | #,##0 | "Bookings" |
| 4 | [ADR] (M05) | 800 | 90 | 240 | 105 | ₹#,##0 | "ADR" |
| 5 | [GSS] (M21) | 1060 | 90 | 240 | 105 | 0.00 | "GSS Score" |
| 6 | [Profit Margin %] (M04) | 1320 | 90 | 240 | 105 | 0.0"%" | "Profit Margin" |
| 7 | [Positive Feedback %] (M25) | 1580 | 90 | 240 | 105 | 0.0"%" | "Positive Feedback" |

**Card Styling:** White bg, border #E8E8E8, radius 8px, shadow, 4px Gold top accent. Callout: Calibri 20pt Bold #1B365D.

**Conditional formatting:**
- Card 5 (GSS): ≥4.0=#2E8B57, 3.0–3.99=#FFC107, <3.0=#DC3545
- Card 6 (Margin): >40%=#2E8B57, 20-40%=#FFC107, <20%=#DC3545

### 4.3 Monthly Revenue Trend (Line Chart)

| Property | Value |
|----------|-------|
| Type | Line Chart |
| Position | x:20, y:205, w:1100, h:300 |
| Background | White, border, radius 8px, shadow |
| Title | "Monthly Revenue Trend" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field |
|------|-------|
| X-Axis | dim_Date[MonthYear] |
| Y-Axis | [Total Revenue] (M01) |
| Secondary Y | [Revenue PY] (M27) |
| Tooltips | [YoY Revenue Growth %] (M28), [Successful Bookings] (M13) |

**Format:** Line=#1B365D 3px, markers ON. PY line=#B0BEC5 dashed. Y-Axis format ₹0M. Legend top-right.

### 4.4 Room Type Donut

| Property | Value |
|----------|-------|
| Type | Donut Chart |
| Position | x:1140, y:205, w:760, h:300 |
| Title | "Room Type Mix" |

**Field Wells:** Legend: RoomType, Values: [Successful Bookings] (M13). Tooltips: [Total Revenue] (M01), [ADR] (M05).

**Colors:** Standard=#1B365D, Deluxe=#C4A265, Suite=#17A2B8. Inner radius 55%. Detail labels ON.

### 4.5 Channel Breakdown (Bar)

| Property | Value |
|----------|-------|
| Type | Clustered Bar |
| Position | x:20, y:515, w:620, h:240 |
| Title | "Revenue by Channel" |

**Field Wells:** Y: Channel, X: [Total Revenue] (M01). Tooltips: [Successful Bookings] (M13), [Revenue Per Booking] (M06).

**Colors:** Website=#1B365D, App=#17A2B8, Agent=#C4A265, ThirdParty=#6C757D. Sort desc. Data labels ON.

### 4.6 Payment Method (Bar)

| Property | Value |
|----------|-------|
| Type | Clustered Bar |
| Position | x:660, y:515, w:620, h:240 |
| Title | "Payment Methods" |

**Field Wells:** Y: PaymentMethod, X: [Successful Bookings] (M13). Tooltips: [Total Revenue] (M01).

**Format:** Bar color #1B365D. Sort desc. Data labels ON.

### 4.7 Member Split (Donut)

| Property | Value |
|----------|-------|
| Type | Donut Chart |
| Position | x:1300, y:515, w:600, h:240 |
| Title | "Member vs Non-Member Revenue" |

**Field Wells:** Legend: MemberType, Values: [Total Revenue] (M01). Tooltips: [Successful Bookings] (M13), [Member Booking %] (M38).

**Colors:** Member=#1B365D, Non-Member=#C4A265. Detail labels: Category + Percent.

### 4.8 Feedback Table

| Property | Value |
|----------|-------|
| Type | Table |
| Position | x:20, y:765, w:1100, h:235 |
| Title | "Guest Feedback Summary" |

**Columns:**
| Column | Field | Width | Format |
|--------|-------|-------|--------|
| 1 | dim_Feedback[Feedback] | 350px | Left-aligned |
| 2 | Count of BookingID | 100px | #,##0 |
| 3 | dim_Feedback[FeedbackSentiment] | 120px | Center |

Sort: Column 2 descending. Conditional: Sentiment="Negative" → Red font (#DC3545) + light red bg (#FFF0F0).

### 4.9 Rating Cards (3 mini-cards)

| Card | Measure | x | y | w | h |
|------|---------|---|---|---|---|
| Overall | [Avg Rating] (M22) | 1140 | 770 | 240 | 75 |
| Staff | [Avg Staff Rating] (M23) | 1400 | 770 | 240 | 75 |
| Cleanliness | [Avg Cleanliness Rating] (M24) | 1660 | 770 | 240 | 75 |

Styling: White, radius 6px, Calibri 18pt Bold. Conditional color (≥4.0 Green).

### 4.10 Guest Demographics (Bar — Top 5 Countries)

| Property | Value |
|----------|-------|
| Type | Clustered Bar |
| Position | x:1140, y:855, w:760, h:145 |
| Title | "Top Guest Countries" |

**Field Wells:** Y: fct_Bookings[Country], X: [Successful Bookings] (M13). TopN filter: Top 5 by [Successful Bookings].

**Format:** Bar color #1B365D. Data labels ON. Compact (9pt fonts).

---

## 5. DAX Measures Used

M01, M03, M04, M05, M06, M13, M21, M22, M23, M24, M25, M27, M28, M38, M51, M53

---

## 6. Build Steps

1. Add page → Rename "Hotel Drillthrough" → Canvas 1920×1080 → BG: #F5F6FA
2. **Right-click page tab → Hide page** (must be hidden)
3. Drag **dim_Hotel[Hotel_Name]** to "Add drill-through fields here" in Visualizations pane
4. Set "Keep all filters" = ON
5. Add rectangle shape (1920×80) at top, fill #1B365D
6. Insert → Buttons → **Back** → Position x:20, y:20, format white outline
7. Add Card for Hotel_Name (from drill context) → 24pt White, transparent bg
8. Add Card for [Manager Subtitle] → 14pt #B0BEC5
9. Build 7 KPI cards per Section 4.2
10. Build Monthly Revenue Line per Section 4.3
11. Build Room Type Donut per Section 4.4
12. Build Channel Bar per Section 4.5
13. Build Payment Bar per Section 4.6
14. Build Member Donut per Section 4.7
15. Build Feedback Table per Section 4.8
16. Build 3 Rating mini-cards per Section 4.9
17. Build Country bar per Section 4.10
18. Add footer ([Last Refresh] + Back button duplicate at bottom)
19. **Test:** Go to Executive Summary → right-click a hotel bar → verify "Drill through → Hotel Drillthrough" appears → click it → verify all data shows for that hotel only
20. **Verify Back button** returns to source page
