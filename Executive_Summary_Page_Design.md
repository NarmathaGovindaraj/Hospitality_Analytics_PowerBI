# Executive Summary Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Executive Summary |
| Page Order | 2nd page (after Home) |
| Canvas Width | 1920 px |
| Canvas Height | 1080 px |
| Background Color | #F5F6FA (Light Cool Gray) |
| Page Type | Report page (visible in navigation) |
| Default Filters | PaymentStatus = "Paid" (page-level filter, hidden) |

---

## 2. Business Purpose

This page answers the following questions for senior leadership:
1. What is the total revenue and profit across the portfolio?
2. How is revenue trending year-over-year?
3. Which hotels generate the most revenue?
4. What is the revenue split by hotel category?
5. Which booking channels drive the most volume?
6. How does the current period compare to last year?
7. What is the guest satisfaction level?

---

## 3. Complete Wireframe (1920 × 1080)

```
┌════════════════════════════════════════════════════════════════════════════════════════┐
║ y:0 ─ NAVIGATION BAR (h:44)  Background: #1B365D (Solid Navy)                        ║
║                                                                                       ║
║  [🏠Home] [●Executive] [Revenue] [Manager] [Channels] [Satisfaction] [Membership]    ║
║           (active=Gold)                                                               ║
╠════════════════════════════════════════════════════════════════════════════════════════╣
║ y:44 ─ PAGE HEADER (h:56)  Background: #FFFFFF                                        ║
║                                                                                       ║
║  "Executive Summary"                              "Portfolio Performance Overview"    ║
║  (Segoe UI 20pt Bold #1B365D)                     (Segoe UI 11pt #6C757D)            ║
║                                                                                       ║
║  ─────────────────────────────────────── (1px line #E0E0E0) ─────────────────────────║
╠════════════════════════════════════════════════════════════════════════════════════════╣
║ y:100 ─ KPI CARDS ROW (h:130)                                                         ║
║                                                                                       ║
║  x:20        x:290       x:560       x:830       x:1100      ║ x:1400 SLICERS        ║
║  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐ ║ ┌──────────────────┐  ║
║  │ 💰     │  │ 📋     │  │ 🛏️     │  │ 📊     │  │ ⭐     │ ║ │ Year        [▼]  │  ║
║  │₹5.72B  │  │ 90,091 │  │₹28,242 │  │₹2.63B  │  │ 4.11  │ ║ │ ─────────────── │  ║
║  │Total   │  │Success-│  │ ADR    │  │ Net    │  │ Guest │ ║ │ Hotel       [▼]  │  ║
║  │Revenue │  │ful     │  │        │  │ Profit │  │ Satis-│ ║ │ ─────────────── │  ║
║  │▲12.3%  │  │Bookings│  │per     │  │45.9%   │  │faction│ ║ │ Category        │  ║
║  │ YoY    │  │        │  │room-   │  │margin  │  │Score  │ ║ │ [L][H][B][R]    │  ║
║  │        │  │        │  │night   │  │        │  │V.Good │ ║ │ ─────────────── │  ║
║  └────────┘  └────────┘  └────────┘  └────────┘  └────────┘ ║ │ City        [▼]  │  ║
║  w:250       w:250       w:250       w:250       w:250      ║ │ ─────────────── │  ║
║                                                              ║ │ Quarter         │  ║
║                                                              ║ │ [Q1][Q2][Q3][Q4]│  ║
║                                                              ║ └──────────────────┘  ║
╠══════════════════════════════════════════════════╦═══════════╩════════════════════════╣
║ y:240 ─ CHART ROW 1 (h:380)                     ║                                    ║
║                                                  ║                                    ║
║  REVENUE BY YEAR (Line + Area Chart)             ║  REVENUE BY HOTEL (H-Bar Chart)    ║
║                                                  ║                                    ║
║  x:20, y:240                                     ║  x:960, y:240                      ║
║  w:920, h:380                                    ║  w:940, h:380                      ║
║                                                  ║                                    ║
║       ₹1.5B ┤                          ╱        ║  Taj Lake Palace    ████████████    ║
║             │                       ╱            ║  Taj Falaknuma      ███████████     ║
║       ₹1.2B ┤                    ╱               ║  Taj Coromandel     ████████        ║
║             │                 ╱                  ║  Taj Exotica        ███████         ║
║       ₹0.9B ┤              ╱                     ║  Taj Mahal Palace   ██████          ║
║             │           ╱                        ║  Taj West End       █████           ║
║       ₹0.6B ┤        ╱                           ║  Taj Bengal         ███             ║
║             │     ╱                              ║                                    ║
║             └──────┬──────┬──────┬──────┬────    ║  Sort: Descending by revenue       ║
║                  2021   2022   2023   2024        ║  Data labels: ₹ values             ║
║                                                  ║                                    ║
║  Line: Current Year Revenue (Navy #1B365D)       ║                                    ║
║  Dashed: Prior Year Revenue (Gray #B0BEC5)       ║                                    ║
║  Area fill: Gold gradient (20% opacity)          ║                                    ║
║                                                  ║                                    ║
╠══════════════════════════════════════════════════╬════════════════════════════════════╣
║ y:630 ─ CHART ROW 2 (h:380)                     ║                                    ║
║                                                  ║                                    ║
║  REVENUE BY CATEGORY (Donut Chart)               ║  CHANNEL DISTRIBUTION (Donut)      ║
║                                                  ║                                    ║
║  x:20, y:630                                     ║  x:960, y:630                      ║
║  w:920, h:370                                    ║  w:940, h:370                      ║
║                                                  ║                                    ║
║         ╭───────────────╮                        ║        ╭───────────────╮           ║
║        │    Luxury     │  Heritage               ║       │   Website    │  App        ║
║       │     32%       │    28%                   ║      │    45%      │   25%        ║
║        │             │                           ║       │            │              ║
║        │  Business   │    Resort                 ║       │   Agent    │  ThirdParty  ║
║         ╰──────22%────╯     18%                  ║        ╰────17%────╯    13%       ║
║                                                  ║                                    ║
║  Center label: Total Revenue ₹5.72B              ║  Center label: 100,000 Bookings    ║
║                                                  ║                                    ║
╠══════════════════════════════════════════════════╩════════════════════════════════════╣
║ y:1010 ─ FOOTER (h:30)  Background: #FFFFFF, border-top 1px #E0E0E0                  ║
║                                                                                       ║
║  [Last Refresh] measure          © 2026 Taj Hotels          Internal Use Only         ║
║                                                                                       ║
╚═══════════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element-by-Element Specification

### 4.1 Navigation Bar

| Property | Value |
|----------|-------|
| Position | x: 0, y: 0 |
| Size | w: 1920, h: 44 |
| Background | #1B365D (Solid Navy) |

**Buttons:**

| # | Label | Icon | x | y | w | h | State | Action |
|---|-------|------|---|---|---|---|-------|--------|
| 1 | Home | 🏠 | 20 | 4 | 130 | 36 | Inactive | Navigate → Home |
| 2 | Executive | 📊 | 160 | 4 | 160 | 36 | **Active** | Current page |
| 3 | Revenue | 📈 | 330 | 4 | 140 | 36 | Inactive | Navigate → Revenue & Trends |
| 4 | Manager | 🏨 | 480 | 4 | 140 | 36 | Inactive | Navigate → Hotel Manager View |
| 5 | Channels | 🌐 | 630 | 4 | 140 | 36 | Inactive | Navigate → Channel Analysis |
| 6 | Satisfaction | ⭐ | 780 | 4 | 160 | 36 | Inactive | Navigate → Guest Satisfaction |
| 7 | Membership | 👥 | 950 | 4 | 160 | 36 | Inactive | Navigate → Member vs Non-Member |

**Button States:**
```
Active (current page):
  Fill: #C4A265 (Gold)
  Text: #0D1B2A (Dark Navy)
  Font: Segoe UI 11pt SemiBold
  Border-radius: 4px

Inactive:
  Fill: Transparent
  Text: #FFFFFF
  Font: Segoe UI 11pt Regular
  Border-radius: 4px

Hover (inactive):
  Fill: rgba(255,255,255,0.15)
  Text: #FFFFFF
  Border: 1px solid rgba(255,255,255,0.3)
```

### 4.2 Page Header

| Element | Type | x | y | w | h | Content | Formatting |
|---------|------|---|---|---|---|---------|------------|
| Title | Text Box | 20 | 50 | 400 | 32 | "Executive Summary" | Segoe UI 20pt Bold #1B365D |
| Subtitle | Text Box | 20 | 78 | 400 | 20 | "Portfolio Performance Overview" | Segoe UI 11pt Regular #6C757D |
| Divider | Shape (Line) | 20 | 96 | 1880 | 1 | — | Color: #E0E0E0, 1px solid |

---

### 4.3 KPI Card — Total Revenue (Card 1)

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 20, y: 105 |
| Size | w: 250, h: 125 |
| Field (Values well) | [Total Revenue] from _Measures table |
| Display Units | Billions (auto) |
| Format String | ₹#,##0.00,,"B" |

**Visual Format Settings:**

| Setting | Path in Format Pane | Value |
|---------|---------------------|-------|
| Callout value font | Callout value → Font | Calibri |
| Callout value size | Callout value → Text size | 28pt |
| Callout value color | Callout value → Color | #1B365D |
| Category label | Category label → Show | ON |
| Category label text | — | "Total Revenue" |
| Category label font | Category label → Font | Segoe UI |
| Category label size | Category label → Text size | 10pt |
| Category label color | Category label → Color | #6C757D |
| Background | General → Background | #FFFFFF |
| Border | General → Border | ON |
| Border color | — | #E8E8E8 |
| Border radius | — | 8px |
| Shadow | General → Shadow | ON, Preset: Bottom |
| Padding | — | 12px all sides |

**Subtitle (secondary value):**
Add a second Card or use "Reference labels" (new cards visual):
- Field: [Revenue Trend Icon] (shows "▲ 12.3%")
- Font: Calibri 11pt
- Conditional color: Rules-based (see Section 7)

**Decorative accent:**
- Shape (Rectangle): x=20, y=105, w=250, h=4
- Fill: #C4A265 (Gold)
- Border-radius: 8px 8px 0 0 (top corners only)
- Layer: In front of card, snapped to top edge

---

### 4.4 KPI Card — Successful Bookings (Card 2)

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 290, y: 105 |
| Size | w: 250, h: 125 |
| Field (Values) | [Successful Bookings] |
| Format String | #,##0 |
| Callout value | Calibri 28pt Bold #1B365D |
| Category label | "Successful Bookings" — Segoe UI 10pt #6C757D |
| Background | White, shadow, 8px radius |
| Gold accent bar | Same as Card 1 (4px top) |
| Subtitle | Static text: "Paid bookings" — 10pt #6C757D |

---

### 4.5 KPI Card — ADR (Card 3)

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 560, y: 105 |
| Size | w: 250, h: 125 |
| Field (Values) | [ADR] |
| Format String | ₹#,##0 |
| Callout value | Calibri 28pt Bold #1B365D |
| Category label | "ADR" — Segoe UI 10pt #6C757D |
| Background | White, shadow, 8px radius |
| Gold accent bar | 4px top #C4A265 |
| Subtitle | "Per room-night" — 10pt #6C757D |

---

### 4.6 KPI Card — Net Profit (Card 4)

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 830, y: 105 |
| Size | w: 250, h: 125 |
| Field (Values) | [Net Profit] |
| Format String | ₹#,##0.00,,"B" |
| Callout value | Calibri 28pt Bold #1B365D |
| Category label | "Net Profit" — Segoe UI 10pt #6C757D |
| Background | White, shadow, 8px radius |
| Gold accent bar | 4px top #C4A265 |
| Subtitle | [Profit Margin %] formatted as "45.9% margin" |
| Subtitle conditional | Green if >40%, Amber if 20-40%, Red if <20% |

---

### 4.7 KPI Card — GSS (Card 5)

| Property | Value |
|----------|-------|
| Visual Type | Card |
| Position | x: 1100, y: 105 |
| Size | w: 250, h: 125 |
| Field (Values) | [GSS] |
| Format String | 0.00 |
| Callout value | Calibri 28pt Bold |
| Callout value color | Conditional: ≥4.0=#2E8B57, 3.0-3.99=#FFC107, <3.0=#DC3545 |
| Category label | "Guest Satisfaction Score" — Segoe UI 10pt #6C757D |
| Background | White, shadow, 8px radius |
| Gold accent bar | 4px top #C4A265 |
| Subtitle | [GSS Band] — "Very Good" / "Excellent" / etc. |
| Subtitle color | Same conditional as callout value |

---

### 4.8 Slicer Panel

| Slicer | Field | x | y | w | h | Style | Default |
|--------|-------|---|---|---|---|-------|---------|
| Year | dim_Date[Year] | 1400 | 105 | 200 | 40 | Dropdown | All |
| Hotel | dim_Hotel[Hotel_Name] | 1400 | 155 | 200 | 40 | Dropdown (search ON) | All |
| Category | dim_Hotel[Category] | 1400 | 205 | 200 | 35 | Horizontal Tiles | All |
| City | dim_Hotel[City] | 1620 | 105 | 180 | 40 | Dropdown | All |
| Quarter | dim_Date[QuarterLabel] | 1620 | 155 | 180 | 35 | Horizontal Tiles | All |

**Slicer Formatting (All):**

| Setting | Value |
|---------|-------|
| Background | #FFFFFF |
| Border | 1px solid #E0E0E0 |
| Border radius | 6px |
| Header | ON |
| Header font | Segoe UI 9pt SemiBold #1B365D |
| Items font | Segoe UI 10pt Regular #333333 |
| Selection color | #C4A265 (Gold fill for selected tile) |
| Selected text color | #FFFFFF (on gold tiles) |
| Unselected tile | #F5F6FA fill, #333333 text |
| "Select All" | Enabled (dropdown slicers) |
| Search box | Enabled on Hotel slicer |
| Orientation (tiles) | Horizontal |
| Sync | Year + Hotel synced across all pages |

---

### 4.9 Revenue by Year (Line + Area Chart)

| Property | Value |
|----------|-------|
| Visual Type | Line Chart (with area shading) |
| Position | x: 20, y: 240 |
| Size | w: 920, h: 380 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[Year] | Categorical axis |
| Y-Axis (Values) | [Total Revenue] | Primary line |
| Secondary Y-Axis | [Revenue PY] | Dashed comparison line |
| Tooltips | [YoY Revenue Growth %] | Shows growth on hover |
| Tooltips | [Net Profit] | Profit context |
| Tooltips | [Successful Bookings] | Volume context |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Revenue by Year" |
| Title font | Segoe UI 14pt SemiBold #1B365D |
| Title alignment | Left |
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON, Preset: Bottom, Transparency: 80% |
| X-Axis label font | Segoe UI 10pt #6C757D |
| X-Axis title | OFF |
| Y-Axis label font | Calibri 10pt #6C757D |
| Y-Axis title | OFF |
| Y-Axis display units | Billions |
| Y-Axis format | ₹0.0B |
| Y-Axis gridlines | ON, color #F0F0F0, style dashed |
| Line 1 (Revenue) color | #1B365D (Navy) |
| Line 1 width | 3px |
| Line 1 markers | ON, circle, 8px, filled #1B365D |
| Line 2 (Revenue PY) color | #B0BEC5 (Gray) |
| Line 2 style | Dashed |
| Line 2 width | 2px |
| Line 2 markers | ON, diamond, 6px, filled #B0BEC5 |
| Area shading (Line 1) | ON |
| Area color | #C4A265, transparency 80% |
| Data labels (Line 1) | ON, Calibri 11pt Bold #1B365D, position Above |
| Data labels format | ₹#,##0.00,,"B" |
| Legend | ON, position Top-Right |
| Legend font | Segoe UI 9pt |
| Legend items | "Revenue", "Prior Year" |
| Padding | 16px all sides |

**Drill-down:** Enable hierarchy on X-Axis:
- Level 1: Year (default view)
- Level 2: YearQuarter (click drill-down)
- Level 3: MonthYear (click drill-down again)

To enable: Drag dim_Date[Year], dim_Date[YearQuarter], dim_Date[MonthYear] into X-Axis as hierarchy.

---

### 4.10 Revenue by Hotel (Horizontal Bar Chart)

| Property | Value |
|----------|-------|
| Visual Type | Clustered Bar Chart |
| Position | x: 960, y: 240 |
| Size | w: 940, h: 380 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | dim_Hotel[Hotel_Name] | Category axis (hotels) |
| X-Axis (Values) | [Total Revenue] | Bar length |
| Tooltips | [Hotel Revenue Rank] | Rank position |
| Tooltips | [Successful Bookings] | Volume context |
| Tooltips | [GSS] | Satisfaction context |
| Tooltips | [Profit Margin %] | Profitability context |
| Tooltips | [Revenue % of Total] | Share of portfolio |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Revenue by Property" |
| Title font | Segoe UI 14pt SemiBold #1B365D |
| Title alignment | Left |
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON, Preset: Bottom |
| Sort | Sort by [Total Revenue], Descending |
| Bar color | #1B365D (Navy) — all bars same color |
| Bar color (conditional) | Gradient: Highest=#1B365D, Lowest=#B0BEC5 |
| Bar border | None |
| Bar spacing | 20% of bar width |
| Y-Axis (category) font | Segoe UI 11pt #333333 |
| Y-Axis title | OFF |
| X-Axis labels | Calibri 9pt #6C757D |
| X-Axis title | OFF |
| X-Axis display units | Millions |
| X-Axis format | ₹0M |
| X-Axis gridlines | ON, #F0F0F0, dashed |
| Data labels | ON |
| Data labels position | Outside end |
| Data labels font | Calibri 10pt Bold #1B365D |
| Data labels format | ₹#,##0,,"M" |
| Padding | 16px |

**Drill-through action:** Right-clicking any hotel bar → Drill through → "Hotel Drillthrough" page (configured as drill-through target via dim_Hotel[Hotel_Name]).

---

### 4.11 Revenue by Category (Donut Chart)

| Property | Value |
|----------|-------|
| Visual Type | Donut Chart |
| Position | x: 20, y: 640 |
| Size | w: 920, h: 360 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Legend | dim_Hotel[Category] | Segments |
| Values | [Total Revenue] | Segment size |
| Tooltips | [Successful Bookings] | Volume per category |
| Tooltips | [Revenue % of Total] | Percentage |
| Tooltips | [ADR] | Rate per category |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Revenue by Category" |
| Title font | Segoe UI 14pt SemiBold #1B365D |
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON |
| Inner radius | 55% |
| Slice colors (MUST match exactly): | |
| — Luxury | #C4A265 (Gold) |
| — Heritage | #8B0000 (Deep Red) |
| — Business | #1B365D (Navy) |
| — Resort | #17A2B8 (Teal) |
| Detail labels | ON |
| Detail label style | Category, percent of total |
| Detail label font | Segoe UI 10pt |
| Detail label color | #333333 |
| Detail label position | Outside |
| Legend | ON, position Right |
| Legend font | Segoe UI 10pt |
| Center label | Not native; use overlaid text box: "Total Revenue" + measure if desired |
| Padding | 16px |

---

### 4.12 Channel Distribution (Donut Chart)

| Property | Value |
|----------|-------|
| Visual Type | Donut Chart |
| Position | x: 960, y: 640 |
| Size | w: 940, h: 360 |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Legend | fct_Bookings[Channel] | Segments |
| Values | [Total Bookings] | Segment size (booking count, ALL statuses) |
| Tooltips | [Total Revenue] | Revenue per channel |
| Tooltips | [Revenue Per Booking] | Avg ticket per channel |
| Tooltips | [Successful Bookings] | Paid count per channel |

**Visual Format:**

| Setting | Value |
|---------|-------|
| Title | "Booking Channel Distribution" |
| Title font | Segoe UI 14pt SemiBold #1B365D |
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON |
| Inner radius | 55% |
| Slice colors: | |
| — Website | #1B365D (Navy) |
| — App | #17A2B8 (Teal) |
| — Agent | #C4A265 (Gold) |
| — ThirdParty | #6C757D (Gray) |
| Detail labels | ON |
| Detail label style | Category, percent of total |
| Detail label font | Segoe UI 10pt |
| Detail label position | Outside |
| Legend | ON, position Right |
| Padding | 16px |

---

### 4.13 Footer

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Refresh stamp | Card (measure) | 20 | 1015 | 300 | 25 | [Last Refresh] | Segoe UI 9pt #78909C |
| Copyright | Text Box | 750 | 1015 | 400 | 25 | "© 2026 Taj Hotels India" | Segoe UI 9pt #78909C, centered |
| Internal note | Text Box | 1550 | 1015 | 350 | 25 | "Internal Use Only" | Segoe UI 9pt #78909C, right-aligned |
| Top border | Shape (Line) | 0 | 1010 | 1920 | 1 | — | #E0E0E0, 1px |

---

## 5. DAX Measures Used on This Page

| Visual | Measure(s) | Measure ID | Source Table |
|--------|-----------|------------|--------------|
| KPI Card 1 | [Total Revenue] | M01 | _Measures |
| KPI Card 1 (subtitle) | [Revenue Trend Icon] | M48 | _Measures |
| KPI Card 2 | [Successful Bookings] | M13 | _Measures |
| KPI Card 3 | [ADR] | M05 | _Measures |
| KPI Card 4 | [Net Profit] | M03 | _Measures |
| KPI Card 4 (subtitle) | [Profit Margin %] | M04 | _Measures |
| KPI Card 5 | [GSS] | M21 | _Measures |
| KPI Card 5 (subtitle) | [GSS Band] | M49 | _Measures |
| Revenue by Year (primary) | [Total Revenue] | M01 | _Measures |
| Revenue by Year (secondary) | [Revenue PY] | M27 | _Measures |
| Revenue by Year (tooltip) | [YoY Revenue Growth %] | M28 | _Measures |
| Revenue by Year (tooltip) | [Net Profit] | M03 | _Measures |
| Revenue by Year (tooltip) | [Successful Bookings] | M13 | _Measures |
| Revenue by Hotel (value) | [Total Revenue] | M01 | _Measures |
| Revenue by Hotel (tooltip) | [Hotel Revenue Rank] | M39 | _Measures |
| Revenue by Hotel (tooltip) | [Successful Bookings] | M13 | _Measures |
| Revenue by Hotel (tooltip) | [GSS] | M21 | _Measures |
| Revenue by Hotel (tooltip) | [Profit Margin %] | M04 | _Measures |
| Revenue by Hotel (tooltip) | [Revenue % of Total] | M40 | _Measures |
| Category Donut (value) | [Total Revenue] | M01 | _Measures |
| Category Donut (tooltip) | [Successful Bookings] | M13 | _Measures |
| Category Donut (tooltip) | [Revenue % of Total] | M40 | _Measures |
| Category Donut (tooltip) | [ADR] | M05 | _Measures |
| Channel Donut (value) | [Total Bookings] | M12 | _Measures |
| Channel Donut (tooltip) | [Total Revenue] | M01 | _Measures |
| Channel Donut (tooltip) | [Revenue Per Booking] | M06 | _Measures |
| Channel Donut (tooltip) | [Successful Bookings] | M13 | _Measures |
| Footer | [Last Refresh] | M51 | _Measures |

**Total unique measures on this page: 14**

---

## 6. Cross-Filter Interactions

Configure via Format tab → Edit interactions for each visual:

| Source Visual (when clicked) | → Revenue by Year | → Revenue by Hotel | → Category Donut | → Channel Donut | → KPI Cards |
|------------------------------|-------------------|--------------------|--------------------|-----------------|-------------|
| Revenue by Year (Line) | — (self) | **Highlight** | **Highlight** | **Highlight** | **None** |
| Revenue by Hotel (Bar) | **Filter** | — (self) | **Filter** | **Filter** | **Filter** |
| Category Donut | **Highlight** | **Filter** | — (self) | **Highlight** | **None** |
| Channel Donut | **Highlight** | **Highlight** | **Highlight** | — (self) | **None** |
| All Slicers | **Filter** | **Filter** | **Filter** | **Filter** | **Filter** |

**Rationale:**
- Hotel bar chart FILTERS all other visuals — enables focused property analysis
- Category donut FILTERS the hotel bar (shows only hotels in that category) but only highlights the line chart
- Line chart and Channel donut only HIGHLIGHT — keeps other visuals contextual
- KPI cards are set to "None" from chart interactions so they remain stable (only slicers affect them)

**How to configure in Power BI Desktop:**
1. Click source visual (e.g., Revenue by Hotel bar chart)
2. Format tab → **Edit interactions** (button appears)
3. On each target visual, icons appear: Filter | Highlight | None
4. Click the appropriate icon per the table above
5. Repeat for every source visual on the page

---

## 7. Conditional Formatting Rules

### 7.1 KPI Card 1 — Revenue Trend Subtitle

| Rule | Condition | Color |
|------|-----------|-------|
| 1 | [YoY Revenue Growth %] > 0 | #2E8B57 (Green) |
| 2 | [YoY Revenue Growth %] < 0 | #DC3545 (Red) |
| 3 | [YoY Revenue Growth %] = 0 or BLANK | #6C757D (Gray) |

**Implementation:** The [Revenue Trend Icon] measure (M48) already outputs the colored text with ▲/▼ symbols. Use a separate card visual overlaid on the main card, with font color set via conditional formatting rules.

### 7.2 KPI Card 5 — GSS Value Color

| Rule | Condition | Callout Value Color |
|------|-----------|---------------------|
| 1 | [GSS] >= 4.0 | #2E8B57 (Green) |
| 2 | [GSS] >= 3.0 AND < 4.0 | #FFC107 (Amber) |
| 3 | [GSS] < 3.0 | #DC3545 (Red) |

**Implementation:**
1. Select GSS Card visual
2. Format → Callout value → Color → **fx** (conditional formatting)
3. Format by: Rules
4. Add rules per table above
5. Click OK

### 7.3 Revenue by Hotel — Bar Color Gradient

| Rule | Type | Min Color | Max Color |
|------|------|-----------|-----------|
| Conditional bar fill | Gradient (field value) | #B0BEC5 (lowest revenue) | #1B365D (highest revenue) |

**Implementation:**
1. Select bar chart
2. Format → Bars → Color → **fx**
3. Format by: Gradient
4. What field: [Total Revenue]
5. Minimum color: #B0BEC5
6. Maximum color: #1B365D
7. Click OK

---

## 8. Drill-Through Configuration

| Action | Visual | Trigger | Target Page | Filter Field |
|--------|--------|---------|-------------|--------------|
| Hotel deep-dive | Revenue by Hotel (bar chart) | Right-click any bar | "Hotel Drillthrough" | dim_Hotel[Hotel_Name] |

**User experience:**
1. User sees Revenue by Hotel bar chart
2. Right-clicks on "Taj Lake Palace" bar
3. Context menu shows: "Drill through → Hotel Drillthrough"
4. Clicks it → navigates to hidden detail page filtered to Taj Lake Palace
5. Clicks **← Back** button on detail page to return

**Pre-requisite:** The "Hotel Drillthrough" page must have dim_Hotel[Hotel_Name] in its drill-through filters well.

---

## 9. Bookmarks

| Bookmark | Trigger | What It Does | Slicer State | Visual State |
|----------|---------|-------------|--------------|--------------|
| BM-Reset | "Reset" button (top-right area) | Clears all slicers | All = "Select All" | Default view |
| BM-2024 | "2024" button | Shows latest full year | Year = 2024 | Data filtered |
| BM-Luxury | "Luxury" button | Luxury hotels only | Category = Luxury | 2 hotels in bar |
| BM-Heritage | "Heritage" button | Heritage hotels only | Category = Heritage | 2 hotels in bar |

**Bookmark buttons position:** x: 1620, y: 50, arranged vertically (4 small buttons, 80×28 each)

**Button styling:**
```
Font: Segoe UI 9pt
Fill: #F5F6FA
Border: 1px #E0E0E0
Border-radius: 4px
Text: #1B365D
Hover: Fill #C4A265, Text #FFFFFF
```

**Implementation:**
1. View → Bookmarks pane → Add bookmark
2. Before adding: Set desired slicer state
3. Add → Rename (e.g., "BM-Reset")
4. Right-click bookmark → Update (Data=ON, Display=OFF, Current page=ON)
5. Create button: Insert → Buttons → Blank
6. Format → Action → Type: Bookmark → Select bookmark
7. Position button per layout

---

## 10. Page-Level Filter (Hidden)

| Filter | Field | Value | Visible to User? |
|--------|-------|-------|-------------------|
| Paid Only | fct_Bookings[PaymentStatus] | "Paid" | **No** (hidden) — only for financial visuals |

**Implementation:**
- Drag fct_Bookings[PaymentStatus] to **Filters on this page** (not visual-level)
- Set filter type: Basic → Select only "Paid"
- Lock filter: Click lock icon
- Hide filter: Click eye icon (hide from view)

**Exception:** The Channel Donut uses [Total Bookings] which counts ALL statuses (to show full demand). If you want it filtered to Paid only, change to [Successful Bookings] instead. Current spec uses [Total Bookings] intentionally to show overall channel demand.

---

## 11. Step-by-Step Build Instructions

### Step 1: Create Page
1. Click **+** tab in Power BI Desktop to add new page
2. Right-click tab → Rename → "Executive Summary"
3. Position as 2nd page (after Home)
4. Format → Canvas settings → Type: Custom → 1920 × 1080
5. Format → Canvas background → Color: #F5F6FA, Transparency: 0%

### Step 2: Build Navigation Bar
1. Insert → Shape → Rectangle: x=0, y=0, w=1920, h=44, Fill=#1B365D, No border
2. For each of 7 buttons:
   a. Insert → Buttons → Blank
   b. Position per Section 4.1 table
   c. Format → Text: Set icon + label
   d. Format → Fill: Set per state definitions
   e. Format → Action: Type=Page navigation, Destination=[page]
3. For "Executive" button: Set fill to Gold (#C4A265), text to #0D1B2A

### Step 3: Add Page Header
1. Insert → Text box → Type "Executive Summary"
2. Format: Segoe UI 20pt Bold #1B365D → Position: x=20, y=50
3. Insert → Text box → "Portfolio Performance Overview"
4. Format: Segoe UI 11pt #6C757D → Position: x=20, y=78
5. Insert → Shape → Line: x=20, y=96, w=1880, color=#E0E0E0

### Step 4: Build 5 KPI Cards
For each card (repeat 5 times with different measures):
1. Visualizations → Card visual
2. Drag measure into Values well
3. Position and size per Section 4.3–4.7
4. Format → Callout value: Calibri 28pt Bold #1B365D
5. Format → Category label: Segoe UI 10pt #6C757D
6. Format → Background: White
7. Format → Border: ON, #E8E8E8, rounded
8. Format → Shadow: ON
9. Add gold accent: Insert → Shape → Rectangle (4px tall, Gold, top of card)

### Step 5: Add Slicers
For each slicer (5 total):
1. Visualizations → Slicer
2. Drag field into Field well
3. Set style: Dropdown or Tile (per spec)
4. Position per Section 4.8 table
5. Format: Background white, border, fonts as specified
6. For Category/Quarter tiles: Set horizontal orientation
7. Configure sync: View → Sync slicers → enable for other pages

### Step 6: Build Revenue by Year Chart
1. Visualizations → Line Chart
2. Drag dim_Date[Year] → X-Axis
3. Drag [Total Revenue] → Y-Axis (Values)
4. Drag [Revenue PY] → Secondary Y-Axis (or use "Add to" secondary)
5. Drag tooltips: [YoY Revenue Growth %], [Net Profit], [Successful Bookings]
6. Position: x=20, y=240, w=920, h=380
7. Format per Section 4.9 table (colors, markers, area fill, labels)
8. Optional: Replace X-Axis with hierarchy (Year > YearQuarter > MonthYear) for drill-down

### Step 7: Build Revenue by Hotel Chart
1. Visualizations → Clustered Bar Chart
2. Drag dim_Hotel[Hotel_Name] → Y-Axis
3. Drag [Total Revenue] → X-Axis (Values)
4. Drag tooltips: [Hotel Revenue Rank], [Successful Bookings], [GSS], [Profit Margin %], [Revenue % of Total]
5. Position: x=960, y=240, w=940, h=380
6. Sort: Click visual → More options (⋯) → Sort by → [Total Revenue] → Descending
7. Format per Section 4.10 (gradient color, data labels, etc.)
8. Apply conditional formatting on bar color (gradient)

### Step 8: Build Category Donut
1. Visualizations → Donut Chart
2. Drag dim_Hotel[Category] → Legend
3. Drag [Total Revenue] → Values
4. Drag tooltips: [Successful Bookings], [Revenue % of Total], [ADR]
5. Position: x=20, y=640, w=920, h=360
6. Format → Slices → Set custom colors per category (Section 4.11)
7. Format → Detail labels: ON, Category + Percent

### Step 9: Build Channel Donut
1. Visualizations → Donut Chart
2. Drag fct_Bookings[Channel] → Legend
3. Drag [Total Bookings] → Values
4. Drag tooltips: [Total Revenue], [Revenue Per Booking], [Successful Bookings]
5. Position: x=960, y=640, w=940, h=360
6. Format → Slices → Set custom colors per channel (Section 4.12)
7. Format → Detail labels: ON, Category + Percent

### Step 10: Configure Interactions
1. Click Revenue by Year chart → Format tab → Edit interactions
2. Set all other visuals to "Highlight" or "None" per Section 6 table
3. Click Revenue by Hotel chart → Edit interactions → Set to "Filter" for all others
4. Repeat for Category Donut and Channel Donut

### Step 11: Add Conditional Formatting
1. Select GSS Card → Format → Callout value → Color → fx → Rules (Section 7.2)
2. Select Hotel Bar Chart → Format → Bars → Color → fx → Gradient (Section 7.3)

### Step 12: Add Page-Level Filter
1. Open Filters pane (right side)
2. Drag fct_Bookings[PaymentStatus] to "Filters on this page"
3. Select "Paid" only
4. Click lock icon → Click eye icon to hide

### Step 13: Add Footer
1. Insert → Card → [Last Refresh] measure → Position x=20, y=1015
2. Insert → Text boxes for copyright and internal note
3. Insert → Shape → Line at y=1010 (separator)

### Step 14: Add Bookmark Buttons
1. Set Year=2024 → View → Bookmarks → Add → Rename "BM-2024" → Update
2. Reset all slicers → Add bookmark "BM-Reset" → Update
3. Create 4 small buttons → Assign bookmark actions
4. Position at x=1620, y=50

### Step 15: Final Verification
- [ ] All 5 KPI cards show correct values
- [ ] Revenue line chart shows 4+ years of data
- [ ] Hotel bar chart sorted correctly (highest to lowest)
- [ ] Donut charts show correct category/channel colors
- [ ] Slicers filter all visuals correctly
- [ ] Clicking hotel bar → right-click shows drill-through option
- [ ] Conditional formatting colors are correct
- [ ] Page-level filter hides non-paid from financial measures
- [ ] Footer shows refresh date
- [ ] Navigation bar buttons all work

---

## 12. Business Insights This Page Should Reveal

| Insight Category | What Users Should Discover | Visual Source |
|-----------------|---------------------------|--------------|
| Revenue growth | Is revenue growing year-over-year? | Line chart trend direction |
| Top performer | Which hotel generates most revenue? | Bar chart (top position) |
| Underperformer | Which hotel needs attention? | Bar chart (bottom position) |
| Category mix | Is portfolio balanced across categories? | Category donut proportions |
| Channel dependency | Over-reliance on one channel? | Channel donut (Website=45% is high) |
| Profitability | Is profit margin healthy (>40%)? | KPI Card 4 subtitle |
| Guest satisfaction | Are guests happy (GSS ≥ 4.0)? | KPI Card 5 value and color |
| Comparison to prior year | Better or worse than last year? | Line chart dual-line, Revenue Trend Icon |

---

*End of Executive Summary Page Design*
