# Member vs Non-Member Analysis Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Member vs Non-Member |
| Page Order | 7th page (after Guest Satisfaction) |
| Canvas Size | 1920 × 1080 px |
| Background Color | #F5F6FA (Light Cool Gray) |
| Page-Level Filter | fct_Bookings[PaymentStatus] = "Paid" (hidden) |
| Business Objective | Compare financial performance and behavior between loyalty members and non-members to evaluate membership program ROI and inform retention strategy |

---

## 2. Business Questions Answered

1. How does member revenue compare to non-member revenue?
2. Are loyalty members more profitable than non-members?
3. Do members pay higher or lower rates (ADR)?
4. How has the member/non-member revenue split changed year over year?
5. Do members receive more discounts — and is that justified by volume?
6. Which room types do members prefer vs non-members?
7. What is the loyalty penetration rate (member booking %)?
8. Is the membership program growing over time?

---

## 3. Wireframe (1920 × 1080)

```
┌══════════════════════════════════════════════════════════════════════════════════════┐
║ y:0  NAV BAR (h:44) #1B365D                                                        ║
║ [Home][Executive][Revenue][Manager][Channels][Satisfaction][●Membership]             ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:44  PAGE HEADER (h:52) Background: #FFFFFF                                         ║
║                                                                                      ║
║  "Member vs Non-Member Analysis"              "Loyalty Program Intelligence"        ║
║  ────────────────────────────── (1px #E0E0E0) ──────────────────────────────────── ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:100  KPI ROW (h:120)                                                               ║
║                                                                                      ║
║  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ │ SLICERS            ║
║  │₹2.65B   │ │₹3.08B   │ │ 55.0%   │ │₹28,800  │ │₹27,600  │ │                    ║
║  │Member   │ │Non-Membr │ │Member   │ │Member   │ │Non-Membr│ │ Year     [▼]       ║
║  │Revenue  │ │Revenue   │ │Booking %│ │ADR      │ │ADR      │ │ Hotel    [▼]       ║
║  │         │ │          │ │         │ │         │ │         │ │ RoomType            ║
║  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ │ [Std][Dlx][Ste]    ║
║  x:20       x:270       x:520       x:770       x:1020       │ MemberType          ║
║  w:230      w:230       w:230       w:230       w:230         │ [Member][Non-Mbr]   ║
║                                                                │ x:1480, w:200       ║
╠══════════════════════════════════════════════════╦═════════════╩══════════════════════╣
║ y:230  ROW 1 (h:280)                             ║                                    ║
║                                                  ║  PROFIT COMPARISON                  ║
║  REVENUE BY YEAR × MEMBER TYPE                   ║  (Clustered Bar)                    ║
║  (100% Stacked Column Chart)                     ║  x:980, y:230, w:920, h:280         ║
║  x:20, y:230, w:940, h:280                       ║                                     ║
║                                                  ║  Y: fct_Bookings[MemberType]        ║
║  X: dim_Date[Year]                               ║  X: [Net Profit]                    ║
║  Y: [Total Revenue]                              ║                                     ║
║  Legend: fct_Bookings[MemberType]                 ║  + Secondary bars:                  ║
║                                                  ║    [Total Revenue] (lighter)         ║
║  Colors: Member=#1B365D, Non-Member=#C4A265      ║    [Total Cost] (outline)            ║
║  Data labels: Percentage inside segments          ║                                     ║
║                                                  ║  Sort: by Net Profit desc            ║
║                                                  ║                                     ║
╠══════════════════════════════════════════════════╬═════════════════════════════════════╣
║ y:520  ROW 2 (h:240)                             ║                                     ║
║                                                  ║  ROOM TYPE × MEMBER MATRIX           ║
║  DISCOUNT ANALYSIS                               ║  (Matrix Visual)                     ║
║  (Grouped Column Chart)                          ║  x:980, y:520, w:920, h:240          ║
║  x:20, y:520, w:940, h:240                       ║                                     ║
║                                                  ║  Rows: fct_Bookings[MemberType]      ║
║  X: fct_Bookings[MemberType]                     ║  Columns: fct_Bookings[RoomType]     ║
║  Y: [Avg Discount %] + [Total Discounts]         ║  Values: [Successful Bookings],      ║
║  Grouped columns side by side                     ║          [Total Revenue], [ADR]      ║
║                                                  ║                                     ║
║  Colors: AvgDisc=#C4A265, TotalDisc=#1B365D      ║  Conditional: Heatmap on bookings    ║
║                                                  ║                                     ║
╠══════════════════════════════════════════════════╬═════════════════════════════════════╣
║ y:770  ROW 3 (h:230)                             ║                                     ║
║                                                  ║  MEMBER SHARE TREND                  ║
║  BOOKING VOLUME TREND BY MEMBER TYPE             ║  (Line Chart)                        ║
║  (Stacked Column Chart)                          ║  x:980, y:770, w:920, h:230          ║
║  x:20, y:770, w:940, h:230                       ║                                     ║
║                                                  ║  X: dim_Date[Year]                   ║
║  X: dim_Date[Year]                               ║  Y: [Member Booking %]               ║
║  Y: [Successful Bookings]                        ║                                     ║
║  Legend: fct_Bookings[MemberType]                 ║  Constant line at 50% (parity)       ║
║                                                  ║  Shows: Is member share growing?     ║
║  Shows: Volume split over years                  ║                                     ║
║                                                  ║                                     ║
╠══════════════════════════════════════════════════╩═════════════════════════════════════╣
║ y:1010  FOOTER (h:30)                                                                  ║
║ [Last Refresh]           © 2026 Taj Hotels           Internal Use Only                  ║
╚══════════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element Specifications

### 4.1 Navigation Bar
Active button = "Membership" (7th position): Fill #C4A265, Text #0D1B2A.

### 4.2 Page Header

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Title | Text Box | 20 | 50 | 500 | 30 | "Member vs Non-Member Analysis" | Segoe UI 20pt Bold #1B365D |
| Subtitle | Text Box | 20 | 76 | 350 | 18 | "Loyalty Program Intelligence" | Segoe UI 11pt #6C757D |
| Divider | Line | 20 | 96 | 1880 | 1 | — | #E0E0E0 |

### 4.3 KPI Cards (5 cards)

| Card# | Measure | x | y | w | h | Format | Label | Color Logic |
|-------|---------|---|---|---|---|--------|-------|-------------|
| 1 | [Member Revenue] (M36) | 20 | 105 | 230 | 115 | ₹#,##0.00,,"B" | "Member Revenue" | Static #1B365D |
| 2 | [Non-Member Revenue] (M37) | 270 | 105 | 230 | 115 | ₹#,##0.00,,"B" | "Non-Member Revenue" | Static #C4A265 |
| 3 | [Member Booking %] (M38) | 520 | 105 | 230 | 115 | 0.0"%" | "Member Booking %" | >55% Green, <45% Red, else Navy |
| 4 | [Member ADR] (M43) | 770 | 105 | 230 | 115 | ₹#,##0 | "Member ADR" | Static #1B365D |
| 5 | [Non-Member ADR] (M44) | 1020 | 105 | 230 | 115 | ₹#,##0 | "Non-Member ADR" | Static #C4A265 |

**Card Styling (all):**
- Background: #FFFFFF, border 1px #E8E8E8, radius 8px, shadow
- Top accent: 4px — Cards 1,3,4 use #1B365D (Navy); Cards 2,5 use #C4A265 (Gold)
- Callout: Calibri 22pt Bold
- Callout color: Card 1,4=#1B365D; Card 2,5=#C4A265; Card 3=conditional
- Category label: Segoe UI 9pt #6C757D

---

### 4.4 Slicers

| Slicer | Field | x | y | w | h | Style |
|--------|-------|---|---|---|---|-------|
| Year | dim_Date[Year] | 1480 | 105 | 190 | 38 | Dropdown |
| Hotel | dim_Hotel[Hotel_Name] | 1480 | 150 | 190 | 38 | Dropdown (search ON) |
| Room Type | fct_Bookings[RoomType] | 1480 | 198 | 190 | 32 | Horizontal tiles (3) |
| Member Type | fct_Bookings[MemberType] | 1690 | 105 | 160 | 32 | Horizontal tiles (2) |

Slicer format: White bg, border #E0E0E8, 6px radius. Selected tile: #C4A265 fill, white text.

---

### 4.5 Revenue by Year × Member Type (100% Stacked Column)

| Property | Value |
|----------|-------|
| Type | 100% Stacked Column Chart |
| Position | x:20, y:230, w:940, h:280 |
| Background | #FFFFFF, border 1px #E8E8E8, radius 8px, shadow |
| Title | "Revenue Split by Year & Membership" — Segoe UI 14pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[Year] | Yearly comparison |
| Y-Axis (Values) | [Total Revenue] (M01) | Revenue splits to 100% |
| Legend | fct_Bookings[MemberType] | Member / Non-Member |
| Tooltips | [Member Revenue] (M36) | Absolute member amount |
| Tooltips | [Non-Member Revenue] (M37) | Absolute non-member amount |
| Tooltips | [Member Booking %] (M38) | Penetration rate |

**Format:**

| Setting | Value |
|---------|-------|
| Segment: Member | #1B365D (Navy) |
| Segment: Non-Member | #C4A265 (Gold) |
| Column width | 50% |
| X-Axis font | Segoe UI 10pt #333333 |
| X-Axis title | OFF |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis title | OFF |
| Y-Axis format | 0"%" |
| Y-Axis gridlines | ON, #F0F0F0, dashed |
| Data labels | ON, inside center, Calibri 10pt Bold White |
| Data labels format | 0"%" |
| Data labels min height | Show only if segment > 15% |
| Legend | ON, Top-Right, Segoe UI 9pt |
| Padding | 16px |

---

### 4.6 Profit Comparison (Clustered Bar Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Bar Chart |
| Position | x:980, y:230, w:920, h:280 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Revenue, Cost & Profit by Membership" — Segoe UI 14pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | fct_Bookings[MemberType] | Member / Non-Member |
| X-Axis (Values) | [Total Revenue] (M01) | First bar cluster |
| X-Axis (Values) | [Total Cost] (M02) | Second bar cluster |
| X-Axis (Values) | [Net Profit] (M03) | Third bar cluster |
| Tooltips | [Profit Margin %] (M04) | Margin comparison |
| Tooltips | [Successful Bookings] (M13) | Volume context |
| Tooltips | [ADR] (M05) | Rate context |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | By MemberType alphabetical (Member first) |
| Bar colors (by measure): | |
| — Total Revenue | #1B365D (Navy) |
| — Total Cost | #DC3545 (Red, lighter transparency 40%) |
| — Net Profit | #2E8B57 (Green) |
| Bar spacing (between clusters) | 30% |
| Y-Axis font | Segoe UI 11pt #333333 |
| Y-Axis title | OFF |
| X-Axis font | Calibri 9pt #6C757D |
| X-Axis title | OFF |
| X-Axis display units | Billions |
| X-Axis format | ₹0.0B |
| Gridlines | ON, #F0F0F0 |
| Data labels | ON, outside end, Calibri 9pt Bold |
| Data labels format | ₹#,##0.0,,"B" |
| Legend | ON, Top-Right: "Revenue", "Cost", "Profit" |
| Legend font | Segoe UI 9pt |
| Padding | 14px |

---

### 4.7 Discount Analysis (Clustered Column Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Column Chart |
| Position | x:20, y:520, w:940, h:240 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Discount Analysis by Membership" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | fct_Bookings[MemberType] | 2 groups |
| Y-Axis (Values) | [Avg Discount %] (M11) | Average discount tier |
| Y-Axis (Values) | [Total Discounts] (M08) | Absolute discount given |
| Tooltips | [Total Revenue] (M01) | Revenue context |
| Tooltips | [Successful Bookings] (M13) | Volume |
| Tooltips | [Revenue Per Booking] (M06) | Ticket context |

**Format:**

| Setting | Value |
|---------|-------|
| Column colors: | |
| — Avg Discount % | #C4A265 (Gold) |
| — Total Discounts | #1B365D (Navy) |
| Column width | 40% |
| X-Axis font | Segoe UI 11pt #333333 |
| X-Axis title | OFF |
| Y-Axis (primary) font | Calibri 9pt #6C757D |
| Y-Axis (primary) title | "Avg Discount %" — Segoe UI 9pt |
| Y-Axis (primary) format | 0"%" |
| Y-Axis (secondary) title | "Total Discounts" — Segoe UI 9pt |
| Y-Axis (secondary) format | ₹0,,"M" |
| Gridlines | ON, #F0F0F0 |
| Data labels | ON, above, Calibri 10pt Bold |
| Legend | ON, Top-Right |
| Padding | 14px |

**Note:** If Power BI doesn't support dual-axis on clustered column easily, use a Combo Chart (Line + Column) instead: Columns=[Total Discounts], Line=[Avg Discount %].

---

### 4.8 Room Type × Member Matrix

| Property | Value |
|----------|-------|
| Type | Matrix |
| Position | x:980, y:520, w:920, h:240 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Room Type Preference by Membership" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Rows | fct_Bookings[MemberType] | Member / Non-Member |
| Columns | fct_Bookings[RoomType] | Standard / Deluxe / Suite |
| Values | [Successful Bookings] (M13) | Primary value |
| Values | [Total Revenue] (M01) | Secondary value |
| Values | [ADR] (M05) | Rate comparison |

**Format:**

| Setting | Value |
|---------|-------|
| Style preset | Minimal |
| Header font | Segoe UI 9pt SemiBold #1B365D |
| Row font | Segoe UI 10pt #333333 |
| Value font | Calibri 10pt #333333 |
| Values format | Bookings: #,##0 | Revenue: ₹#,##0,,"M" | ADR: ₹#,##0 |
| Row height | 28px |
| Gridlines | ON, both, #E8E8E8 |
| Column subtotals | ON |
| Row subtotals | ON |
| Padding | 10px |

**Conditional Formatting (Heatmap on Bookings):**
1. Format → Cell elements → Background color → ON → fx
2. Apply to: [Successful Bookings]
3. Format by: Color scale
4. Minimum: #FFFFFF (White)
5. Maximum: #1B365D (Navy)
6. Font color on dark cells: Auto → White

---

### 4.9 Booking Volume Trend (Stacked Column)

| Property | Value |
|----------|-------|
| Type | Stacked Column Chart |
| Position | x:20, y:770, w:940, h:230 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Booking Volume by Year & Membership" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[Year] | Yearly |
| Y-Axis (Values) | [Successful Bookings] (M13) | Stacked volume |
| Legend | fct_Bookings[MemberType] | Member / Non-Member segments |
| Tooltips | [Member Booking %] (M38) | Penetration |
| Tooltips | [Total Revenue] (M01) | Revenue context |

**Format:**

| Setting | Value |
|---------|-------|
| Segment: Member | #1B365D (Navy) |
| Segment: Non-Member | #C4A265 (Gold) |
| Column width | 50% |
| X-Axis font | Segoe UI 10pt #333333 |
| X-Axis title | OFF |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis title | OFF |
| Y-Axis format | #,##0 |
| Y-Axis gridlines | ON, #F0F0F0 |
| Data labels | ON, inside center, Calibri 9pt Bold White |
| Data labels format | #,##0 |
| Legend | ON, Top-Right |
| Padding | 14px |

---

### 4.10 Member Share Trend (Line Chart)

| Property | Value |
|----------|-------|
| Type | Line Chart |
| Position | x:980, y:770, w:920, h:230 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Member Booking Share Over Time" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[Year] | Yearly trend |
| Y-Axis (Values) | [Member Booking %] (M38) | Member penetration rate |
| Tooltips | [Successful Bookings] (M13) | Total volume |
| Tooltips | [Member Revenue] (M36) | Member revenue |
| Tooltips | [Non-Member Revenue] (M37) | Non-member revenue |

**Format:**

| Setting | Value |
|---------|-------|
| Line color | #1B365D (Navy) |
| Line width | 3px |
| Markers | ON, circle, 8px, filled #1B365D |
| X-Axis font | Segoe UI 10pt #6C757D |
| X-Axis title | OFF |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis title | OFF |
| Y-Axis format | 0"%" |
| Y-Axis range | Min: 40%, Max: 70% (focused range) |
| Y-Axis gridlines | ON, #F0F0F0, dashed |
| Data labels | ON, above, Calibri 11pt Bold #1B365D |
| Data labels format | 0.0"%" |
| Constant Line | Value=50, Color=#DC3545, Style=Dashed, Width=2px, Label="Parity (50%)" |
| Padding | 14px |

**Constant line at 50%:** Represents equal split. If member share is above this line, loyalty program is dominant.

**Adding constant line:**
1. Analytics pane → Constant Line → Add → Value: 50
2. Color: #DC3545, Style: Dashed
3. Data label: "Parity (50%)", Position: Above

---

### 4.11 Footer

Same as all pages: [Last Refresh] left, copyright center, internal note right, line at y:1010.

---

## 5. DAX Measures Used

| Visual | Measure(s) | ID |
|--------|-----------|-----|
| Card 1 | [Member Revenue] | M36 |
| Card 2 | [Non-Member Revenue] | M37 |
| Card 3 | [Member Booking %] | M38 |
| Card 4 | [Member ADR] | M43 |
| Card 5 | [Non-Member ADR] | M44 |
| Revenue 100% Stacked | [Total Revenue] | M01 |
| Revenue Stacked (tooltips) | [Member Revenue], [Non-Member Revenue], [Member Booking %] | M36, M37, M38 |
| Profit Comparison | [Total Revenue], [Total Cost], [Net Profit] | M01, M02, M03 |
| Profit (tooltips) | [Profit Margin %], [Successful Bookings], [ADR] | M04, M13, M05 |
| Discount Analysis | [Avg Discount %], [Total Discounts] | M11, M08 |
| Discount (tooltips) | [Total Revenue], [Successful Bookings], [Revenue Per Booking] | M01, M13, M06 |
| Room × Member Matrix | [Successful Bookings], [Total Revenue], [ADR] | M13, M01, M05 |
| Booking Volume Stacked | [Successful Bookings] | M13 |
| Booking Volume (tooltips) | [Member Booking %], [Total Revenue] | M38, M01 |
| Member Share Line | [Member Booking %] | M38 |
| Member Share (tooltips) | [Successful Bookings], [Member Revenue], [Non-Member Revenue] | M13, M36, M37 |
| Footer | [Last Refresh] | M51 |

**Unique measures: 15** (M01, M02, M03, M04, M05, M06, M08, M11, M13, M36, M37, M38, M43, M44, M51)

---

## 6. Cross-Filter Interactions

| Source (clicked) | → KPI Cards | → Revenue Stacked | → Profit Bar | → Discount Col | → Matrix | → Volume Stacked | → Share Line |
|-----------------|-------------|--------------------|--------------|--------------------|----------|-----------------|--------------|
| Revenue Stacked | **None** | — | Highlight | Highlight | Highlight | Highlight | Highlight |
| Profit Bar | **None** | Highlight | — | Highlight | Highlight | Highlight | Highlight |
| Discount Column | **None** | Highlight | Highlight | — | Highlight | Highlight | Highlight |
| Matrix | **Filter** | **Filter** | **Filter** | **Filter** | — | **Filter** | **Filter** |
| Volume Stacked | **None** | Highlight | Highlight | Highlight | Highlight | — | Highlight |
| Share Line | **None** | Highlight | Highlight | Highlight | Highlight | Highlight | — |
| Slicers | Filter | Filter | Filter | Filter | Filter | Filter | Filter |

**Key decisions:**
- Matrix is the only chart that FILTERS (clicking a cell = specific MemberType + RoomType intersection)
- All other charts only HIGHLIGHT — this page is comparative, so filtering one chart removes the comparison context
- KPI Cards are NEVER affected by chart clicks (only slicers and MemberType slicer)

---

## 7. Conditional Formatting

| Visual | Element | Type | Rules |
|--------|---------|------|-------|
| Card 3 (Member %) | Callout color | Rules | >55%=#2E8B57, 45-55%=#1B365D, <45%=#DC3545 |
| Revenue Stacked | Segment fill | By Legend | Member=#1B365D, Non-Member=#C4A265 |
| Profit Bar (Revenue) | Bar fill | Fixed | #1B365D |
| Profit Bar (Cost) | Bar fill | Fixed | #DC3545 @ 40% transparency |
| Profit Bar (Profit) | Bar fill | Fixed | #2E8B57 |
| Volume Stacked | Segment fill | By Legend | Member=#1B365D, Non-Member=#C4A265 |
| Matrix (Bookings) | Background | Color scale | White→Navy gradient |
| Member Share Line | Line color | Fixed | #1B365D |

---

## 8. Drill-Through

No outbound drill-through on this page. The Matrix does not trigger hotel drill-through since rows are MemberType (not Hotel_Name).

If hotel-level member analysis is needed, users should navigate to Executive Summary → filter by hotel → then come to this page (Year/Hotel slicers are synced).

---

## 9. Bookmarks

| Bookmark | Button Position | Action |
|----------|----------------|--------|
| BM-MembersOnly | x:1690, y:150, w:120, h:28 | MemberType slicer = "Member" |
| BM-NonMembers | x:1690, y:182, w:120, h:28 | MemberType slicer = "Non-Member" |
| BM-ResetMember | x:1690, y:214, w:120, h:28 | Clear MemberType + RoomType slicers |

Button styling: Segoe UI 9pt, #F5F6FA fill, #1B365D text, radius 4px. Gold hover.

---

## 10. Color Palette (This Page)

| Element | Color | Hex | Meaning |
|---------|-------|-----|---------|
| Member (all visuals) | Navy | #1B365D | Loyalty members |
| Non-Member (all visuals) | Gold | #C4A265 | Non-loyalty guests |
| Revenue bars | Navy | #1B365D | Primary financial |
| Cost bars | Red (light) | #DC3545 @ 40% | Expense |
| Profit bars | Green | #2E8B57 | Positive outcome |
| Avg Discount | Gold | #C4A265 | Discount metric |
| Total Discounts | Navy | #1B365D | Absolute discount |
| Parity line (50%) | Red dashed | #DC3545 | Target reference |
| Matrix heatmap max | Navy | #1B365D | High value |
| Matrix heatmap min | White | #FFFFFF | Low value |
| Card backgrounds | White | #FFFFFF | — |
| Page background | Cool Gray | #F5F6FA | — |

---

## 11. Build Steps

### Step 1: Create Page
1. Add page → Rename "Member vs Non-Member" → Canvas 1920×1080 → BG: #F5F6FA

### Step 2: Navigation Bar
1. Copy from previous page. Set "Membership" button active (Gold fill).

### Step 3: Header
1. Text box: "Member vs Non-Member Analysis" — 20pt Bold #1B365D
2. Text box: "Loyalty Program Intelligence" — 11pt #6C757D
3. Line at y:96

### Step 4: Build 5 KPI Cards
1. Card 1: [Member Revenue] → x:20, y:105, w:230, h:115 → Navy accent bar
2. Card 2: [Non-Member Revenue] → x:270 → Gold accent bar
3. Card 3: [Member Booking %] → x:520 → Navy accent, conditional callout color
4. Card 4: [Member ADR] → x:770 → Navy accent
5. Card 5: [Non-Member ADR] → x:1020 → Gold accent
6. Format all: White bg, shadow, Calibri 22pt Bold callout

### Step 5: Slicers
1. Add Year dropdown (synced), Hotel dropdown, RoomType tiles, MemberType tiles
2. Position per Section 4.4

### Step 6: Revenue 100% Stacked Column
1. Visualizations → 100% Stacked Column Chart
2. X: dim_Date[Year], Y: [Total Revenue], Legend: MemberType
3. Tooltips: [Member Revenue], [Non-Member Revenue], [Member Booking %]
4. Position: x:20, y:230, w:940, h:280
5. Colors: Member=#1B365D, Non-Member=#C4A265
6. Data labels: ON, inside center, white, percentage format

### Step 7: Profit Comparison Bar
1. Visualizations → Clustered Bar Chart
2. Y: MemberType, X: [Total Revenue] + [Total Cost] + [Net Profit]
3. Tooltips: [Profit Margin %], [Successful Bookings], [ADR]
4. Position: x:980, y:230, w:920, h:280
5. Colors: Revenue=Navy, Cost=Red@40%, Profit=Green
6. Legend ON, data labels ON

### Step 8: Discount Analysis Column
1. Visualizations → Clustered Column (or Combo: Column + Line)
2. X: MemberType, Y: [Avg Discount %] + [Total Discounts]
3. Tooltips: [Total Revenue], [Successful Bookings], [Revenue Per Booking]
4. Position: x:20, y:520, w:940, h:240
5. Colors: AvgDisc=Gold, TotalDisc=Navy

### Step 9: Room × Member Matrix
1. Visualizations → Matrix
2. Rows: MemberType, Columns: RoomType
3. Values: [Successful Bookings], [Total Revenue], [ADR]
4. Position: x:980, y:520, w:920, h:240
5. Apply heatmap conditional formatting on [Successful Bookings]

### Step 10: Booking Volume Stacked Column
1. Visualizations → Stacked Column
2. X: Year, Y: [Successful Bookings], Legend: MemberType
3. Tooltips: [Member Booking %], [Total Revenue]
4. Position: x:20, y:770, w:940, h:230
5. Colors: Member=Navy, Non-Member=Gold

### Step 11: Member Share Line
1. Visualizations → Line Chart
2. X: dim_Date[Year], Y: [Member Booking %]
3. Tooltips: [Successful Bookings], [Member Revenue], [Non-Member Revenue]
4. Position: x:980, y:770, w:920, h:230
5. Line: Navy, 3px, markers ON
6. Y-Axis range: 40%–70%
7. Analytics → Constant Line at 50% (Red dashed, label "Parity")

### Step 12: Page-Level Filter
1. Filters pane → PaymentStatus → "Paid" → Lock → Hide

### Step 13: Configure Interactions
1. Per Section 6: Matrix = Filter; all others = Highlight; KPIs = None from charts

### Step 14: Footer + Bookmarks
1. Copy footer. Add 3 bookmark buttons per Section 9.

### Step 15: Verify
- [ ] Cards 1+2 sum ≈ Total Revenue on Exec page (minor rounding OK)
- [ ] Card 3 shows ~55% (matches source: 55,031 members / 100,000 total)
- [ ] 100% stacked columns show Navy + Gold segments summing to 100% each year
- [ ] Profit bars show 3 measures per MemberType row
- [ ] Discount chart shows both metrics side by side
- [ ] Matrix shows 2 rows × 3 columns with heatmap
- [ ] Volume stacked shows growing/stable trend
- [ ] Member share line is above 50% parity line
- [ ] MemberType slicer filters all visuals correctly
- [ ] Clicking Matrix cell filters other visuals

---

## 12. Business Insights This Page Reveals

| Insight | Visual Source |
|---------|-------------|
| Which segment contributes more revenue? | KPI Cards 1 & 2, Revenue stacked |
| Are members more profitable per booking? | Profit comparison (green bars) |
| Do members get significantly more discounts? | Discount analysis columns |
| Is the loyalty program growing? | Member Share trend line (above 50%?) |
| What room types do members prefer? | Matrix (are members in more Suites?) |
| Is member ADR higher or lower? | KPI Cards 4 & 5 comparison |
| Is the revenue split changing over time? | 100% stacked (is Navy growing?) |
| How does booking volume split trend? | Volume stacked columns year over year |

---

*End of Member vs Non-Member Analysis Page Design*
