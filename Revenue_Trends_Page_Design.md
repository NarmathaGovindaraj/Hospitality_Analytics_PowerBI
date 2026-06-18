# Revenue & Trends Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Revenue & Trends |
| Page Order | 3rd page (after Executive Summary) |
| Canvas Width | 1920 px |
| Canvas Height | 1080 px |
| Background Color | #F5F6FA (Light Cool Gray) |
| Page Type | Report page (visible in navigation) |
| Default Page Filter | fct_Bookings[PaymentStatus] = "Paid" (hidden) |
| Business Objective | Deep multi-year revenue analysis with time drill-down, cost/profit comparison, room type breakdown, and margin trending |

---

## 2. Business Questions Answered

1. How has revenue grown year-over-year (2021–2025)?
2. What is the monthly/quarterly revenue pattern — is there seasonality?
3. What are the YTD, QTD, MTD revenue figures?
4. How do costs compare to revenue over time — is margin stable?
5. Which room types (Standard/Deluxe/Suite) contribute most?
6. What is the QoQ and YoY growth rate?
7. Is profit margin improving or declining over time?
8. What is the revenue trajectory — accelerating or decelerating?

---

## 3. Complete Wireframe (1920 × 1080)

```
┌══════════════════════════════════════════════════════════════════════════════════════════┐
║ y:0 ─ NAVIGATION BAR (h:44)  Background: #1B365D                                       ║
║                                                                                          ║
║  [Home] [Executive] [●Revenue] [Manager] [Channels] [Satisfaction] [Membership]         ║
║                      (active=Gold)                                                       ║
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║ y:44 ─ PAGE HEADER (h:52)  Background: #FFFFFF                                           ║
║                                                                                          ║
║  "Revenue & Trends (2021–2025)"                     "Financial Performance Analysis"    ║
║  Segoe UI 20pt Bold #1B365D                          Segoe UI 11pt #6C757D              ║
║  ────────────────────────────────────────── (1px #E0E0E0) ──────────────────────────── ║
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║ y:100 ─ KPI CARDS ROW (h:115)                                                            ║
║                                                                                          ║
║  x:20       x:250      x:480      x:710      x:940      x:1170 ║ x:1430 SLICERS       ║
║  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐║ ┌────────────────┐║
║  │▲+12.3%│  │₹5.72B │  │₹1.4B  │  │₹780M  │  │ 45.9% │  │₹28.2K │║ │ Year      [▼] │║
║  │ YoY   │  │Revenue │  │Revenue│  │Revenue│  │Profit │  │  ADR  │║ │ ────────────── │║
║  │Growth │  │  YTD   │  │  QTD  │  │  MTD  │  │Margin │  │       │║ │ Room Type     │║
║  │       │  │        │  │       │  │       │  │   %   │  │       │║ │ [Std][Dlx][St]│║
║  └───────┘  └───────┘  └───────┘  └───────┘  └───────┘  └───────┘║ │ ────────────── │║
║  w:210      w:210      w:210      w:210      w:210      w:210     ║ │ Category      │║
║                                                                    ║ │ [L][H][B][R]  │║
║                                                                    ║ │ ────────────── │║
║                                                                    ║ │ Quarter       │║
║                                                                    ║ │ [Q1][Q2][Q3][Q4]║
║                                                                    ║ └────────────────┘║
╠════════════════════════════════════════════════════════════════════╩══════════════════╣
║ y:225 ─ MAIN TREND CHART (h:330)                                                       ║
║                                                                                         ║
║  MONTHLY REVENUE TREND (Area Chart with drill-down)                                    ║
║  x:20, y:225, w:1880, h:330                                                           ║
║                                                                                         ║
║  ₹200M ┤                                                                              ║
║        │    ╱╲      ╱╲      ╱╲      ╱╲      ╱╲      ╱╲      ╱╲                       ║
║  ₹150M ┤  ╱  ╲  ╱╱  ╲╲  ╱╱  ╲╲  ╱╱  ╲╲  ╱╱  ╲╲  ╱╱  ╲╲  ╱╱                      ║
║        │╱    ╲╱      ╲╱      ╲╱      ╲╱      ╲╱      ╲╱                              ║
║  ₹100M ┤                                                                              ║
║        │                                                                               ║
║   ₹50M ┤                                                                              ║
║        └──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┬────                  ║
║            Jan 21  Jul 21  Jan 22  Jul 22  Jan 23  Jul 23  Jan 24  Jul 24             ║
║                                                                                         ║
║  Legend: ── Revenue (Navy)  ┄┄ Prior Year (Gray)                                       ║
║  Drill: Year → Quarter → Month (hierarchy enabled)                                    ║
║                                                                                         ║
╠═══════════════════════════════════════════════════╦═════════════════════════════════════╣
║ y:565 ─ CHART ROW 2 (h:220)                      ║                                     ║
║                                                   ║                                     ║
║  COST vs REVENUE (Combo Chart)                    ║  REVENUE BY ROOM TYPE               ║
║  x:20, y:565, w:920, h:220                        ║  (Stacked Column Chart)             ║
║                                                   ║  x:960, y:565, w:940, h:220         ║
║  Columns = [Total Revenue] (Navy)                 ║                                     ║
║  Line 1 = [Total Cost] (Red dashed)              ║  X: dim_Date[Year]                  ║
║  Line 2 = [Net Profit] (Green)                   ║  Y: [Total Revenue]                 ║
║  X-Axis: dim_Date[YearQuarter]                    ║  Legend: fct_Bookings[RoomType]      ║
║                                                   ║  Stacked: Standard | Deluxe | Suite  ║
║                                                   ║                                     ║
╠═══════════════════════════════════════════════════╬═════════════════════════════════════╣
║ y:795 ─ CHART ROW 3 (h:200)                      ║                                     ║
║                                                   ║                                     ║
║  PROFIT MARGIN TREND (Line Chart)                 ║  REVENUE BY CATEGORY (H-Bar)        ║
║  x:20, y:795, w:920, h:200                        ║  x:960, y:795, w:940, h:200         ║
║                                                   ║                                     ║
║  X: dim_Date[YearQuarter]                         ║  Y: dim_Hotel[Category]             ║
║  Y: [Profit Margin %]                            ║  X: [Total Revenue]                 ║
║  Constant line at 40% (target)                   ║  Data labels: ON                    ║
║  Area below 40% = light red                      ║  Sort: Descending                   ║
║                                                   ║                                     ║
╠═══════════════════════════════════════════════════╩═════════════════════════════════════╣
║ y:1005 ─ FOOTER (h:35)                                                                  ║
║  [Last Refresh]          © 2026 Taj Hotels India          Internal Use Only              ║
╚═════════════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element-by-Element Specification

### 4.1 Navigation Bar

Identical to Executive Summary page (see Executive_Summary_Page_Design.md Section 4.1) with one change:
- **Active button:** "Revenue" (position 3) gets Gold fill (#C4A265), dark text (#0D1B2A)
- All other buttons: Transparent fill, white text

### 4.2 Page Header

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Title | Text Box | 20 | 50 | 500 | 30 | "Revenue & Trends (2021–2025)" | Segoe UI 20pt Bold #1B365D |
| Subtitle | Text Box | 20 | 76 | 400 | 18 | "Financial Performance Analysis" | Segoe UI 11pt #6C757D |
| Divider | Shape (Line) | 20 | 96 | 1880 | 1 | — | #E0E0E0, 1px |

---

### 4.3 KPI Card 1 — YoY Revenue Growth %

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 20, y: 105 |
| Size | w: 210, h: 108 |
| Field (Values) | [YoY Revenue Growth %] |
| Format String | +0.0%;-0.0% |
| Callout font | Calibri 24pt Bold |
| Callout color | Conditional: >0 = #2E8B57, <0 = #DC3545, =0 = #6C757D |
| Category label | "YoY Revenue Growth" — Segoe UI 9pt #6C757D |
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON |
| Top accent | 4px #C4A265 (Gold bar) |
| Icon (above value) | ▲ or ▼ (embedded in [Revenue Trend Icon] measure — use separate card below main value if needed) |

### 4.4 KPI Card 2 — Revenue YTD

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 250, y: 105 |
| Size | w: 210, h: 108 |
| Field (Values) | [Revenue YTD] |
| Format String | ₹#,##0.00,,"B" |
| Callout font | Calibri 24pt Bold #1B365D |
| Category label | "Revenue YTD" — Segoe UI 9pt #6C757D |
| Background | #FFFFFF, border, shadow, gold accent |

### 4.5 KPI Card 3 — Revenue QTD

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 480, y: 105 |
| Size | w: 210, h: 108 |
| Field (Values) | [Revenue QTD] |
| Format String | ₹#,##0.0,,"B" |
| Callout font | Calibri 24pt Bold #1B365D |
| Category label | "Revenue QTD" — Segoe UI 9pt #6C757D |
| Background | #FFFFFF, border, shadow, gold accent |

### 4.6 KPI Card 4 — Revenue MTD

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 710, y: 105 |
| Size | w: 210, h: 108 |
| Field (Values) | [Revenue MTD] |
| Format String | ₹#,##0,,"M" |
| Callout font | Calibri 24pt Bold #1B365D |
| Category label | "Revenue MTD" — Segoe UI 9pt #6C757D |
| Background | #FFFFFF, border, shadow, gold accent |

### 4.7 KPI Card 5 — Profit Margin %

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 940, y: 105 |
| Size | w: 210, h: 108 |
| Field (Values) | [Profit Margin %] |
| Format String | 0.0"%" |
| Callout font | Calibri 24pt Bold |
| Callout color | Conditional: >40%=#2E8B57, 20-40%=#FFC107, <20%=#DC3545 |
| Category label | "Profit Margin %" — Segoe UI 9pt #6C757D |
| Background | #FFFFFF, border, shadow, gold accent |

### 4.8 KPI Card 6 — ADR

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 1170, y: 105 |
| Size | w: 210, h: 108 |
| Field (Values) | [ADR] |
| Format String | ₹#,##0 |
| Callout font | Calibri 24pt Bold #1B365D |
| Category label | "ADR (Avg Daily Rate)" — Segoe UI 9pt #6C757D |
| Background | #FFFFFF, border, shadow, gold accent |

---

### 4.9 Slicer Panel

| Slicer | Field | x | y | w | h | Style | Default |
|--------|-------|---|---|---|---|-------|---------|
| Year | dim_Date[Year] | 1430 | 105 | 190 | 38 | Dropdown | All |
| Room Type | fct_Bookings[RoomType] | 1430 | 152 | 190 | 32 | Horizontal Tiles (3) | All |
| Category | dim_Hotel[Category] | 1430 | 192 | 190 | 32 | Horizontal Tiles (4) | All |
| Quarter | dim_Date[QuarterLabel] | 1640 | 105 | 170 | 32 | Horizontal Tiles (4) | All |

**Tile Button Dimensions:**
- Room Type tiles: 3 items → each ~58px wide
- Category tiles: 4 items → each ~44px wide  
- Quarter tiles: 4 items → each ~40px wide

**Slicer Format (same as Executive Summary):**
- Background: #FFFFFF
- Border: 1px #E0E0E0, radius 6px
- Selected tile: #C4A265 fill, #FFFFFF text
- Unselected tile: #F5F6FA fill, #333333 text
- Header: ON, Segoe UI 9pt SemiBold #1B365D

**Sync configuration:**
- Year: Synced from global (all pages)
- Room Type: Synced with Page 6 (Member vs Non-Member)
- Category: Synced with Pages 1, 5
- Quarter: Synced with Page 1

---

### 4.10 Monthly Revenue Trend (Main Chart — Area + Line)

| Property | Value |
|----------|-------|
| Visual Type | Area Chart |
| Position | x: 20, y: 225 |
| Size | w: 1880, h: 330 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[Year], dim_Date[YearQuarter], dim_Date[MonthYear] | Hierarchy for drill-down |
| Y-Axis (Values) | [Total Revenue] | Primary series |
| Secondary Y-Axis | [Revenue PY] | Prior year comparison line |
| Tooltips | [YoY Revenue Growth %] | Growth context |
| Tooltips | [Net Profit] | Profit at each point |
| Tooltips | [Successful Bookings] | Volume context |
| Tooltips | [ADR] | Rate context |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Monthly Revenue Trend" |
| Title font | Segoe UI 14pt SemiBold #1B365D |
| Title alignment | Left |
| Subtitle | "Drill down: Year → Quarter → Month" |
| Subtitle font | Segoe UI 9pt Italic #6C757D |
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON, Preset Bottom, Transparency 85% |
| Padding | 20px |
| **Line 1 (Total Revenue):** | |
| Color | #1B365D (Navy) |
| Width | 3px |
| Style | Solid |
| Markers | ON, Circle, 6px, filled |
| Area fill | ON, #C4A265, transparency 85% (very light gold wash) |
| **Line 2 (Revenue PY):** | |
| Color | #B0BEC5 (Light Gray) |
| Width | 2px |
| Style | Dashed (- - -) |
| Markers | OFF |
| Area fill | OFF |
| **X-Axis:** | |
| Font | Segoe UI 9pt #6C757D |
| Title | OFF |
| Gridlines | OFF |
| **Y-Axis:** | |
| Font | Calibri 9pt #6C757D |
| Title | OFF |
| Display units | Auto (Millions for monthly, Billions for yearly) |
| Format | ₹0M or ₹0.0B |
| Gridlines | ON, #F0F0F0, dashed, 1px |
| Start from | 0 |
| **Secondary Y-Axis:** | |
| Show | ON |
| Title | "Prior Year" — Segoe UI 9pt #B0BEC5 |
| Font | Calibri 9pt #B0BEC5 |
| **Data Labels:** | |
| Show | ON (primary line only) |
| Font | Calibri 10pt Bold #1B365D |
| Position | Above |
| Format | ₹#,##0,,"M" (monthly) or ₹#,##0.0,,"B" (yearly) |
| Show only | Every point at Year level; every other at Quarter level; OFF at Month level |
| **Legend:** | |
| Show | ON |
| Position | Top Right |
| Font | Segoe UI 9pt |
| Items | "Revenue (Current)", "Revenue (Prior Year)" |
| **Drill-down:** | |
| Hierarchy | Year → YearQuarter → MonthYear |
| Drill mode | Expand all down one level / Go to next level |
| Drill icons | Show drill-up and drill-down icons in header |

---

### 4.11 Cost vs Revenue (Combo Chart)

| Property | Value |
|----------|-------|
| Visual Type | Line and Clustered Column Chart (Combo) |
| Position | x: 20, y: 565 |
| Size | w: 920, h: 220 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis (Shared) | dim_Date[YearQuarter] | Quarterly view |
| Column Y-Axis | [Total Revenue] | Revenue as columns |
| Line Y-Axis | [Total Cost] | Cost as line 1 |
| Line Y-Axis | [Net Profit] | Profit as line 2 |
| Tooltips | [Profit Margin %] | Margin at each quarter |
| Tooltips | [Total Discounts] | Discount impact |
| Tooltips | [Additional Service Revenue] | Ancillary contribution |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Revenue vs Cost Analysis" |
| Title font | Segoe UI 13pt SemiBold #1B365D |
| Background | #FFFFFF, border 1px #E8E8E8, radius 8px, shadow |
| Padding | 14px |
| **Columns (Revenue):** | |
| Color | #1B365D (Navy) |
| Column width | 60% |
| Border | None |
| Data labels | OFF (chart is dense) |
| **Line 1 (Total Cost):** | |
| Color | #DC3545 (Red) |
| Width | 2.5px |
| Style | Dashed |
| Markers | ON, diamond, 5px |
| **Line 2 (Net Profit):** | |
| Color | #2E8B57 (Green) |
| Width | 2.5px |
| Style | Solid |
| Markers | ON, circle, 5px |
| **X-Axis:** | |
| Font | Segoe UI 9pt #6C757D |
| Title | OFF |
| Rotation | -45° (angled for quarterly labels) |
| **Y-Axis (Column):** | |
| Font | Calibri 9pt #6C757D |
| Title | OFF |
| Display units | Billions |
| Gridlines | ON, #F0F0F0 |
| **Y-Axis (Line):** | |
| Show | ON (secondary axis) |
| Font | Calibri 9pt #6C757D |
| Title | OFF |
| **Legend:** | |
| Position | Top Right |
| Items | "Revenue", "Cost", "Net Profit" |
| Font | Segoe UI 9pt |

---

### 4.12 Revenue by Room Type (Stacked Column Chart)

| Property | Value |
|----------|-------|
| Visual Type | Stacked Column Chart |
| Position | x: 960, y: 565 |
| Size | w: 940, h: 220 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[Year] | Yearly comparison |
| Y-Axis (Values) | [Total Revenue] | Revenue per segment |
| Legend | fct_Bookings[RoomType] | Standard / Deluxe / Suite |
| Tooltips | [Revenue % of Total] | Share per room type |
| Tooltips | [Successful Bookings] | Volume per room type |
| Tooltips | [ADR] | Rate per room type |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Revenue by Room Type" |
| Title font | Segoe UI 13pt SemiBold #1B365D |
| Background | #FFFFFF, border, radius 8px, shadow |
| Padding | 14px |
| **Segment Colors:** | |
| Standard | #1B365D (Navy) |
| Deluxe | #C4A265 (Gold) |
| Suite | #17A2B8 (Teal) |
| **Columns:** | |
| Column width | 50% |
| Data labels | ON, inside segments, Calibri 9pt White (only for segments > 15% height) |
| Data labels format | ₹#,##0,,"M" |
| **X-Axis:** | |
| Font | Segoe UI 10pt #6C757D |
| Title | OFF |
| **Y-Axis:** | |
| Font | Calibri 9pt #6C757D |
| Title | OFF |
| Display units | Billions |
| Gridlines | ON, #F0F0F0 |
| **Legend:** | |
| Position | Top Right |
| Font | Segoe UI 9pt |

---

### 4.13 Profit Margin Trend (Line Chart with Target)

| Property | Value |
|----------|-------|
| Visual Type | Line Chart |
| Position | x: 20, y: 795 |
| Size | w: 920, h: 200 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[YearQuarter] | Quarterly granularity |
| Y-Axis (Values) | [Profit Margin %] | Margin trend |
| Tooltips | [Net Profit] | Absolute profit value |
| Tooltips | [Total Revenue] | Revenue context |
| Tooltips | [Total Cost] | Cost context |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Profit Margin % Trend" |
| Title font | Segoe UI 13pt SemiBold #1B365D |
| Background | #FFFFFF, border, radius 8px, shadow |
| Padding | 14px |
| **Line:** | |
| Color | #2E8B57 (Green) |
| Width | 3px |
| Markers | ON, circle, 7px, filled |
| **Area fill:** | |
| Fill | ON, color #2E8B57, transparency 90% |
| **Constant Line (Target):** | |
| Value | 40 (representing 40%) |
| Color | #DC3545 (Red) |
| Style | Dashed |
| Width | 2px |
| Label | "Target: 40%" — Segoe UI 9pt #DC3545 |
| **X-Axis:** | |
| Font | Segoe UI 9pt #6C757D |
| Title | OFF |
| Rotation | -45° |
| **Y-Axis:** | |
| Font | Calibri 9pt #6C757D |
| Title | OFF |
| Format | 0"%" |
| Minimum | 0 |
| Maximum | 70 |
| Gridlines | ON, #F0F0F0 |
| **Data Labels:** | |
| Show | ON |
| Font | Calibri 9pt Bold #2E8B57 |
| Format | 0.0"%" |
| Position | Above |

**Adding constant line in Power BI:**
1. Select line chart → Analytics pane (magnifying glass icon)
2. Constant Line → Add line
3. Value: 40
4. Color: #DC3545, Style: Dashed, Transparency: 0%
5. Data label: ON, text "Target", position Above

---

### 4.14 Revenue by Category (Horizontal Bar Chart)

| Property | Value |
|----------|-------|
| Visual Type | Clustered Bar Chart |
| Position | x: 960, y: 795 |
| Size | w: 940, h: 200 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | dim_Hotel[Category] | 4 categories |
| X-Axis (Values) | [Total Revenue] | Bar length |
| Tooltips | [Revenue % of Total] | Share |
| Tooltips | [Net Profit] | Category profitability |
| Tooltips | [Successful Bookings] | Volume |
| Tooltips | [GSS] | Satisfaction per category |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Revenue by Category" |
| Title font | Segoe UI 13pt SemiBold #1B365D |
| Background | #FFFFFF, border, radius 8px, shadow |
| Padding | 14px |
| Sort | Descending by [Total Revenue] |
| **Bar Colors (fixed per category):** | |
| Luxury | #C4A265 (Gold) |
| Heritage | #8B0000 (Deep Red) |
| Business | #1B365D (Navy) |
| Resort | #17A2B8 (Teal) |
| **Data Labels:** | |
| Show | ON |
| Position | Outside end |
| Font | Calibri 10pt Bold — color matches bar |
| Format | ₹#,##0.00,,"B" |
| **Y-Axis (Category):** | |
| Font | Segoe UI 11pt #333333 |
| Title | OFF |
| **X-Axis:** | |
| Font | Calibri 9pt #6C757D |
| Title | OFF |
| Gridlines | ON, #F0F0F0 |
| Display units | Billions |

---

### 4.15 Footer

Identical to Executive Summary (see that spec Section 4.13):
- [Last Refresh] card at x=20, y=1020
- Copyright text centered
- "Internal Use Only" right-aligned
- 1px #E0E0E0 line at y=1010

---

## 5. DAX Measures Used on This Page

| Visual | Measure | ID | Purpose |
|--------|---------|-----|---------|
| KPI Card 1 | [YoY Revenue Growth %] | M28 | Growth indicator |
| KPI Card 1 (icon) | [Revenue Trend Icon] | M48 | Arrow + percentage display |
| KPI Card 2 | [Revenue YTD] | M31 | Year accumulation |
| KPI Card 3 | [Revenue QTD] | M30 | Quarter accumulation |
| KPI Card 4 | [Revenue MTD] | M29 | Month accumulation |
| KPI Card 5 | [Profit Margin %] | M04 | Margin health |
| KPI Card 6 | [ADR] | M05 | Rate metric |
| Revenue Trend (primary) | [Total Revenue] | M01 | Main line |
| Revenue Trend (secondary) | [Revenue PY] | M27 | Comparison line |
| Revenue Trend (tooltip) | [YoY Revenue Growth %] | M28 | Growth at point |
| Revenue Trend (tooltip) | [Net Profit] | M03 | Profit at point |
| Revenue Trend (tooltip) | [Successful Bookings] | M13 | Volume at point |
| Revenue Trend (tooltip) | [ADR] | M05 | Rate at point |
| Combo Chart (column) | [Total Revenue] | M01 | Revenue bars |
| Combo Chart (line 1) | [Total Cost] | M02 | Cost comparison |
| Combo Chart (line 2) | [Net Profit] | M03 | Profit line |
| Combo Chart (tooltip) | [Profit Margin %] | M04 | Margin context |
| Combo Chart (tooltip) | [Total Discounts] | M08 | Discount impact |
| Combo Chart (tooltip) | [Additional Service Revenue] | M09 | Ancillary |
| Room Type Stacked | [Total Revenue] | M01 | Segment values |
| Room Type (tooltip) | [Revenue % of Total] | M40 | Share |
| Room Type (tooltip) | [Successful Bookings] | M13 | Volume |
| Room Type (tooltip) | [ADR] | M05 | Rate |
| Profit Margin Line | [Profit Margin %] | M04 | Trend line |
| Profit Margin (tooltip) | [Net Profit] | M03 | Absolute profit |
| Profit Margin (tooltip) | [Total Revenue] | M01 | Revenue context |
| Profit Margin (tooltip) | [Total Cost] | M02 | Cost context |
| Category Bar (value) | [Total Revenue] | M01 | Bar length |
| Category Bar (tooltip) | [Revenue % of Total] | M40 | Share |
| Category Bar (tooltip) | [Net Profit] | M03 | Profitability |
| Category Bar (tooltip) | [Successful Bookings] | M13 | Volume |
| Category Bar (tooltip) | [GSS] | M21 | Satisfaction |
| Footer | [Last Refresh] | M51 | Timestamp |

**Total unique measures: 14** (M01, M02, M03, M04, M05, M08, M09, M13, M21, M27, M28, M29, M30, M31, M40, M48, M51 = 17 measure references, 14 unique)

---

## 6. Cross-Filter Interactions

| Source (when clicked) | → Revenue Trend | → Combo Chart | → Room Type Stacked | → Margin Trend | → Category Bar | → KPI Cards |
|-----------------------|-----------------|---------------|---------------------|----------------|----------------|-------------|
| Revenue Trend (Area) | — (self) | Highlight | Highlight | Highlight | Highlight | **None** |
| Combo Chart | Highlight | — (self) | Highlight | Highlight | Highlight | **None** |
| Room Type Stacked | **Filter** | **Filter** | — (self) | **Filter** | **Filter** | **Filter** |
| Margin Trend | Highlight | Highlight | Highlight | — (self) | Highlight | **None** |
| Category Bar | **Filter** | **Filter** | **Filter** | **Filter** | — (self) | **Filter** |
| All Slicers | Filter | Filter | Filter | Filter | Filter | Filter |

**Key decisions:**
- Room Type and Category bar charts FILTER everything — they're analytical selectors
- Time-series charts only HIGHLIGHT — maintains overview context
- KPI Cards unaffected by chart clicks (only slicers change them) to keep top numbers stable

---

## 7. Conditional Formatting

### 7.1 YoY Growth Card (KPI Card 1)

| Condition | Callout Value Color |
|-----------|---------------------|
| Value > 0 | #2E8B57 (Green) |
| Value < 0 | #DC3545 (Red) |
| Value = 0 or BLANK | #6C757D (Gray) |

### 7.2 Profit Margin Card (KPI Card 5)

| Condition | Callout Value Color |
|-----------|---------------------|
| Value > 40 | #2E8B57 (Green) |
| Value 20–40 | #FFC107 (Amber) |
| Value < 20 | #DC3545 (Red) |

### 7.3 Profit Margin Trend Line — Area Shading

When margin drops below 40%, the area between the line and 40% target should appear light red. This is achieved by:
- The constant line at 40% (red dashed)
- Visual inspection — Power BI doesn't natively shade below a threshold, but the contrast between green line + red constant line communicates the gap clearly

### 7.4 Revenue by Category — Fixed Bar Colors

Use conditional formatting with rules (not gradient):
1. Select bar chart → Format → Bars → Color → **fx**
2. Format by: Rules
3. If Category = "Luxury" then #C4A265
4. If Category = "Heritage" then #8B0000
5. If Category = "Business" then #1B365D
6. If Category = "Resort" then #17A2B8

---

## 8. Drill-Through Configuration

| Visual | Right-Click Target | Drill-Through Page | Filter Field |
|--------|-------------------|--------------------|--------------|
| Category Bar | Any bar segment | "Hotel Drillthrough" | dim_Hotel[Category] is NOT used; instead, clicking a category and choosing drill-through passes via the filtered context |
| Revenue Trend | Not applicable | — | Time chart doesn't support hotel drill-through |

**Primary drill-through on this page:** The Category bar chart supports filtering to category → then the Hotel Drillthrough page shows hotels within that category. However, the drill-through filter field on the target page is dim_Hotel[Hotel_Name], so users should use the Executive Summary page for hotel-level drill-through.

**Alternative:** Add a small Hotel bar chart (or enable drill-through from slicer selection).

---

## 9. Bookmarks

| Bookmark | Button Label | Position | What It Does |
|----------|-------------|----------|--------------|
| BM-FullPeriod | "All Years" | x:1640, y:55, w:80, h:28 | Clears Year + Quarter slicers |
| BM-LatestYear | "2024" | x:1730, y:55, w:60, h:28 | Year=2024 |
| BM-AllRooms | "All Rooms" | x:1640, y:87, w:80, h:28 | Clears Room Type slicer |
| BM-SuiteOnly | "Suite" | x:1730, y:87, w:60, h:28 | RoomType=Suite |

**Button format:** Same as Executive Summary bookmark buttons (Segoe UI 9pt, #F5F6FA fill, #1B365D text, Gold on hover).

---

## 10. Color Palette Summary for This Page

| Element | Color | Hex |
|---------|-------|-----|
| Primary chart line | Navy | #1B365D |
| Prior Year reference | Light Gray | #B0BEC5 |
| Area fill (revenue) | Gold (low opacity) | #C4A265 @ 85% transparency |
| Cost line | Red | #DC3545 |
| Profit line | Green | #2E8B57 |
| Standard room | Navy | #1B365D |
| Deluxe room | Gold | #C4A265 |
| Suite room | Teal | #17A2B8 |
| Luxury category | Gold | #C4A265 |
| Heritage category | Deep Red | #8B0000 |
| Business category | Navy | #1B365D |
| Resort category | Teal | #17A2B8 |
| Profit margin line | Green | #2E8B57 |
| Target line | Red dashed | #DC3545 |
| Card backgrounds | White | #FFFFFF |
| Page background | Cool Gray | #F5F6FA |
| Grid lines | Very Light | #F0F0F0 |

---

## 11. Build Steps (Power BI Desktop)

### Step 1: Create Page
1. Click + tab → rename to "Revenue & Trends"
2. Position as 3rd page
3. Format → Canvas: Custom 1920×1080
4. Canvas background: #F5F6FA

### Step 2: Navigation Bar
1. Copy navigation bar from Executive Summary page (Ctrl+C → Ctrl+V)
2. Change active button: Set "Revenue" button fill=#C4A265, text=#0D1B2A
3. Set "Executive" button back to inactive (transparent fill, white text)

### Step 3: Page Header
1. Insert Text Box → "Revenue & Trends (2021–2025)" — Segoe UI 20pt Bold #1B365D
2. Insert Text Box → "Financial Performance Analysis" — Segoe UI 11pt #6C757D
3. Insert Shape Line → x=20, y=96, w=1880, color=#E0E0E0

### Step 4: Build 6 KPI Cards
For each card (positions in Section 4.3–4.8):
1. Visualizations → Card
2. Drag measure to Values
3. Format: Calibri 24pt Bold, white background, shadow, border
4. Add gold accent rectangle (4px) on top
5. Apply conditional formatting on Cards 1 and 5 (Section 7)

### Step 5: Add Slicers
1. Add Year dropdown (synced from global)
2. Add Room Type tiles (3 values)
3. Add Category tiles (4 values)
4. Add Quarter tiles (4 values)
5. Format all per Section 4.9

### Step 6: Build Revenue Trend (Main Chart)
1. Visualizations → Area Chart
2. X-Axis: Drag dim_Date[Year], then dim_Date[YearQuarter], then dim_Date[MonthYear] as hierarchy
3. Y-Axis: [Total Revenue]
4. Add [Revenue PY] to Secondary Y-Axis (or use "Line values" if available in combo)
5. Tooltips: Add [YoY Revenue Growth %], [Net Profit], [Successful Bookings], [ADR]
6. Size: w=1880, h=330, position x=20, y=225
7. Format per Section 4.10 (colors, markers, area fill, labels, legend)
8. Verify drill icons appear in visual header

### Step 7: Build Combo Chart (Cost vs Revenue)
1. Visualizations → Line and Clustered Column Chart
2. X-Axis: dim_Date[YearQuarter]
3. Column values: [Total Revenue]
4. Line values: [Total Cost], [Net Profit]
5. Tooltips: [Profit Margin %], [Total Discounts], [Additional Service Revenue]
6. Position: x=20, y=565, w=920, h=220
7. Format per Section 4.11

### Step 8: Build Room Type Stacked Column
1. Visualizations → Stacked Column Chart
2. X-Axis: dim_Date[Year]
3. Y-Axis: [Total Revenue]
4. Legend: fct_Bookings[RoomType]
5. Tooltips: [Revenue % of Total], [Successful Bookings], [ADR]
6. Position: x=960, y=565, w=940, h=220
7. Format → Set segment colors: Standard=#1B365D, Deluxe=#C4A265, Suite=#17A2B8

### Step 9: Build Profit Margin Trend
1. Visualizations → Line Chart
2. X-Axis: dim_Date[YearQuarter]
3. Y-Axis: [Profit Margin %]
4. Tooltips: [Net Profit], [Total Revenue], [Total Cost]
5. Position: x=20, y=795, w=920, h=200
6. Format → Line color #2E8B57, markers ON
7. Analytics pane → Constant Line → Value=40, Color=#DC3545, Style=Dashed

### Step 10: Build Category Bar Chart
1. Visualizations → Clustered Bar Chart
2. Y-Axis: dim_Hotel[Category]
3. X-Axis: [Total Revenue]
4. Tooltips: [Revenue % of Total], [Net Profit], [Successful Bookings], [GSS]
5. Position: x=960, y=795, w=940, h=200
6. Sort: Descending by value
7. Apply conditional formatting → Rules-based bar colors (Section 7.4)
8. Data labels: ON, outside end

### Step 11: Configure Interactions
1. Click each visual → Format → Edit interactions
2. Set per Section 6 table
3. Key: Room Type and Category = Filter; Time charts = Highlight; KPIs = None from charts

### Step 12: Add Page-Level Filter
1. Filters pane → Drag PaymentStatus → "Filters on this page"
2. Select "Paid" only → Lock → Hide

### Step 13: Add Footer + Bookmarks
1. Copy footer from Executive Summary
2. Create bookmarks per Section 9
3. Add 4 small buttons → assign bookmark actions

### Step 14: Final Check
- [ ] All 6 KPI cards populated
- [ ] Revenue trend shows multi-year data with drill-down working
- [ ] Combo chart shows 3 series (columns + 2 lines)
- [ ] Room Type stacked shows 3 colored segments
- [ ] Profit margin line visible with red target at 40%
- [ ] Category bars have correct fixed colors
- [ ] Slicers filter all visuals
- [ ] Interactions configured (charts highlight, bars filter)
- [ ] Page filter hides non-paid bookings

---

*End of Revenue & Trends Page Design*
