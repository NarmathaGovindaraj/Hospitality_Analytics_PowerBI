# Channel Analysis Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Channel Analysis |
| Page Order | 5th page (after Hotel Manager View) |
| Canvas Size | 1920 × 1080 px |
| Background Color | #F5F6FA (Light Cool Gray) |
| Page-Level Filter | None (show all bookings for channel volume; financial visuals use [Total Revenue] which already filters Paid) |
| Business Objective | Analyze booking channel effectiveness, device preferences, channel revenue contribution, and trends to inform marketing and digital investment strategy |

---

## 2. Business Questions Answered

1. What percentage of bookings come from each channel (Website/App/Agent/ThirdParty)?
2. Is mobile or desktop dominant — and by how much?
3. How have channel preferences changed over time?
4. Which channels generate the most revenue (not just volume)?
5. What is the average revenue per booking by channel?
6. How does the channel mix vary across hotels?
7. What is the booking lead time by channel?
8. Which device type yields higher ADR?

---

## 3. Wireframe (1920 × 1080)

```
┌══════════════════════════════════════════════════════════════════════════════════════┐
║ y:0  NAV BAR (h:44) #1B365D                                                        ║
║ [Home][Executive][Revenue][Manager][●Channels][Satisfaction][Membership]              ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:44  PAGE HEADER (h:52) Background: #FFFFFF                                         ║
║                                                                                      ║
║  "Booking Source & Channel Analysis"          "Marketing Channel Intelligence"      ║
║  ────────────────────────────── (1px #E0E0E0) ──────────────────────────────────── ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:100  KPI + DONUT ROW (h:190)                                                       ║
║                                                                                      ║
║  ┌──────────────────────────┐  ┌──────────────────────────┐  │ SLICERS              ║
║  │  CHANNEL DISTRIBUTION    │  │  DEVICE TYPE SPLIT        │  │                      ║
║  │  (Donut Chart)           │  │  (Donut Chart)            │  │ Year      [▼]        ║
║  │                          │  │                           │  │ Hotel     [▼]        ║
║  │   Website  45%           │  │    Mobile   62%           │  │ Channel             ║
║  │   App      25%           │  │    Desktop  38%           │  │ [Web][App][Agt][3rd] ║
║  │   Agent    17%           │  │                           │  │ Device              ║
║  │   3rdParty 13%           │  │                           │  │ [Mobile][Desktop]    ║
║  │                          │  │                           │  │                      ║
║  │  x:20, w:520, h:185      │  │  x:560, w:520, h:185      │  │ x:1500, w:200       ║
║  └──────────────────────────┘  └──────────────────────────┘  │                      ║
║                                                               │                      ║
╠═══════════════════════════════════════════════════════════════╩══════════════════════╣
║ y:300  ROW 2 — CHANNEL TREND (h:290)                                                 ║
║                                                                                      ║
║  CHANNEL TREND OVER TIME (Stacked Area Chart)                                       ║
║  x:20, y:300, w:1880, h:290                                                         ║
║                                                                                      ║
║  X-Axis: dim_Date[YearQuarter]                                                      ║
║  Y-Axis: [Total Bookings]                                                           ║
║  Legend: fct_Bookings[Channel]                                                      ║
║  Colors: Website=#1B365D, App=#17A2B8, Agent=#C4A265, ThirdParty=#6C757D            ║
║                                                                                      ║
╠════════════════════════════════════════════╦═════════════════════════════════════════╣
║ y:600  ROW 3 (h:200)                      ║                                          ║
║                                           ║  REVENUE PER BOOKING BY CHANNEL          ║
║  CHANNEL REVENUE (Clustered Bar)          ║  (Clustered Column Chart)                ║
║  x:20, y:600, w:940, h:200               ║  x:980, y:600, w:920, h:200              ║
║                                           ║                                          ║
║  Y: fct_Bookings[Channel]                 ║  X: fct_Bookings[Channel]                ║
║  X: [Total Revenue]                       ║  Y: [Revenue Per Booking]                ║
║  Sort: Descending                         ║  Data labels: ON                         ║
║  Data labels: ON                          ║  Colors: per-channel fixed               ║
║                                           ║                                          ║
╠════════════════════════════════════════════╬═════════════════════════════════════════╣
║ y:810  ROW 4 (h:190)                      ║                                          ║
║                                           ║  DEVICE ADR COMPARISON                   ║
║  HOTEL × CHANNEL MATRIX                   ║  (Clustered Column)                      ║
║  x:20, y:810, w:940, h:190               ║  x:980, y:810, w:450, h:190              ║
║                                           ║                                          ║
║  Rows: dim_Hotel[Hotel_Name]              ║  X: fct_Bookings[DeviceType]             ║
║  Columns: fct_Bookings[Channel]           ║  Y: [ADR]                                ║
║  Values: [Total Bookings]                 ║                                          ║
║  Conditional: Background heatmap          ║                                          ║
║                                           ║──────────────────────────────────────────║
║                                           ║  AVG LEAD TIME BY CHANNEL                ║
║                                           ║  (Clustered Bar)                          ║
║                                           ║  x:1450, y:810, w:450, h:190             ║
║                                           ║                                          ║
║                                           ║  Y: Channel                              ║
║                                           ║  X: [Avg Booking Lead Time]              ║
║                                           ║                                          ║
╠════════════════════════════════════════════╩═════════════════════════════════════════╣
║ y:1010  FOOTER (h:30)                                                                ║
║ [Last Refresh]           © 2026 Taj Hotels           Internal Use Only                ║
╚══════════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element Specifications

### 4.1 Navigation Bar
Active button = "Channels" (5th position): Fill #C4A265, Text #0D1B2A.
All others: Transparent, White text.

### 4.2 Page Header

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Title | Text Box | 20 | 50 | 550 | 30 | "Booking Source & Channel Analysis" | Segoe UI 20pt Bold #1B365D |
| Subtitle | Text Box | 20 | 76 | 400 | 18 | "Marketing Channel Intelligence" | Segoe UI 11pt #6C757D |
| Divider | Line | 20 | 96 | 1880 | 1 | — | #E0E0E0 |

### 4.3 Channel Distribution (Donut Chart)

| Property | Value |
|----------|-------|
| Type | Donut Chart |
| Position | x:20, y:105, w:520, h:185 |
| Background | #FFFFFF, border 1px #E8E8E8, radius 8px, shadow |
| Title | "Booking Channel Mix" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field |
|------|-------|
| Legend | fct_Bookings[Channel] |
| Values | [Total Bookings] (M12) |
| Tooltips | [Total Revenue] (M01) |
| Tooltips | [Revenue Per Booking] (M06) |
| Tooltips | [Successful Bookings] (M13) |

**Format:**

| Setting | Value |
|---------|-------|
| Inner radius | 55% |
| Slice: Website | #1B365D (Navy) |
| Slice: App | #17A2B8 (Teal) |
| Slice: Agent | #C4A265 (Gold) |
| Slice: ThirdParty | #6C757D (Gray) |
| Detail labels | ON, Category + Percent of total |
| Detail label font | Segoe UI 9pt #333333 |
| Detail label position | Outside |
| Legend | OFF (labels sufficient for 4 values) |
| Padding | 10px |

### 4.4 Device Type Split (Donut Chart)

| Property | Value |
|----------|-------|
| Type | Donut Chart |
| Position | x:560, y:105, w:520, h:185 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Device Type Split" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field |
|------|-------|
| Legend | fct_Bookings[DeviceType] |
| Values | [Total Bookings] (M12) |
| Tooltips | [Total Revenue] (M01) |
| Tooltips | [ADR] (M05) |

**Format:**

| Setting | Value |
|---------|-------|
| Inner radius | 55% |
| Slice: Mobile | #1B365D (Navy) |
| Slice: Desktop | #C4A265 (Gold) |
| Detail labels | ON, Category + Percent |
| Detail label font | Segoe UI 10pt |
| Legend | OFF |
| Padding | 10px |

### 4.5 Slicers

| Slicer | Field | x | y | w | h | Style |
|--------|-------|---|---|---|---|-------|
| Year | dim_Date[Year] | 1500 | 105 | 190 | 38 | Dropdown |
| Hotel | dim_Hotel[Hotel_Name] | 1500 | 150 | 190 | 38 | Dropdown (search ON) |
| Channel | fct_Bookings[Channel] | 1500 | 198 | 190 | 32 | Horizontal tiles (4) |
| Device | fct_Bookings[DeviceType] | 1710 | 198 | 130 | 32 | Horizontal tiles (2) |

Slicer format: White bg, #E0E0E0 border, 6px radius. Selected: #C4A265 fill, white text.

---

### 4.6 Channel Trend Over Time (Stacked Area Chart)

| Property | Value |
|----------|-------|
| Type | Stacked Area Chart |
| Position | x:20, y:300, w:1880, h:290 |
| Background | #FFFFFF, border 1px #E8E8E8, radius 8px, shadow |
| Title | "Channel Trend Over Time" — Segoe UI 14pt SemiBold #1B365D |
| Subtitle | "Quarterly booking volume by channel" — Segoe UI 9pt Italic #6C757D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[YearQuarter] | Quarterly aggregation |
| Y-Axis (Values) | [Total Bookings] (M12) | Volume stacked by channel |
| Legend | fct_Bookings[Channel] | 4 stacked areas |
| Tooltips | [Total Revenue] (M01) | Revenue at that point |
| Tooltips | [Revenue Per Booking] (M06) | Avg ticket at that point |

**Format:**

| Setting | Value |
|---------|-------|
| Area colors (stacked bottom-to-top): | |
| — Website (bottom) | #1B365D (Navy), Transparency: 20% |
| — App | #17A2B8 (Teal), Transparency: 20% |
| — Agent | #C4A265 (Gold), Transparency: 20% |
| — ThirdParty (top) | #6C757D (Gray), Transparency: 20% |
| Line colors | Same as area, solid, 2px width |
| X-Axis font | Segoe UI 9pt #6C757D, Rotation: -45° |
| X-Axis title | OFF |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis title | OFF |
| Y-Axis format | #,##0 |
| Y-Axis gridlines | ON, #F0F0F0, dashed |
| Legend | ON, position Top-Right, Segoe UI 9pt |
| Data labels | OFF (too dense for stacked area) |
| Padding | 16px |

---

### 4.7 Channel Revenue (Clustered Bar Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Bar Chart |
| Position | x:20, y:600, w:940, h:200 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Revenue by Channel" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | fct_Bookings[Channel] | 4 channels |
| X-Axis (Values) | [Total Revenue] (M01) | Revenue per channel |
| Tooltips | [Successful Bookings] (M13) | Paid volume |
| Tooltips | [Revenue Per Booking] (M06) | Avg ticket |
| Tooltips | [Profit Margin %] (M04) | Profitability |
| Tooltips | [ADR] (M05) | Rate |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | Descending by [Total Revenue] |
| Bar colors (fixed per channel): | |
| — Website | #1B365D |
| — App | #17A2B8 |
| — Agent | #C4A265 |
| — ThirdParty | #6C757D |
| Y-Axis font | Segoe UI 11pt #333333 |
| Y-Axis title | OFF |
| X-Axis font | Calibri 9pt #6C757D |
| X-Axis title | OFF |
| X-Axis display units | Billions |
| X-Axis format | ₹0.0B |
| Gridlines | ON, #F0F0F0 |
| Data labels | ON, outside end, Calibri 10pt Bold |
| Data labels color | Matches bar color |
| Data labels format | ₹#,##0,,"M" |
| Padding | 14px |

**Conditional formatting (rules-based bar color):**
1. Format → Bars → Color → fx → Rules
2. If Channel = "Website" then #1B365D
3. If Channel = "App" then #17A2B8
4. If Channel = "Agent" then #C4A265
5. If Channel = "ThirdParty" then #6C757D

---

### 4.8 Revenue Per Booking by Channel (Clustered Column Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Column Chart |
| Position | x:980, y:600, w:920, h:200 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Avg Revenue Per Booking by Channel" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | fct_Bookings[Channel] | 4 channels |
| Y-Axis (Values) | [Revenue Per Booking] (M06) | Average ticket size |
| Tooltips | [Total Revenue] (M01) | Total context |
| Tooltips | [Successful Bookings] (M13) | Volume context |
| Tooltips | [ADR] (M05) | Rate context |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | Descending by [Revenue Per Booking] |
| Column colors (fixed per channel): | |
| — Website | #1B365D |
| — App | #17A2B8 |
| — Agent | #C4A265 |
| — ThirdParty | #6C757D |
| Column width | 60% |
| X-Axis font | Segoe UI 10pt #333333 |
| X-Axis title | OFF |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis title | OFF |
| Y-Axis format | ₹#,##0 |
| Y-Axis gridlines | ON, #F0F0F0 |
| Data labels | ON, above columns, Calibri 10pt Bold #1B365D |
| Data labels format | ₹#,##0 |
| Padding | 14px |

**Conditional formatting (rules-based column color):**
Same rules as Channel Revenue bar (Section 4.7).

---

### 4.9 Hotel × Channel Matrix

| Property | Value |
|----------|-------|
| Type | Matrix |
| Position | x:20, y:810, w:940, h:190 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Bookings by Hotel & Channel" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Rows | dim_Hotel[Hotel_Name] | 7 hotels |
| Columns | fct_Bookings[Channel] | 4 channels as column headers |
| Values | [Total Bookings] (M12) | Count in each cell |

**Format:**

| Setting | Value |
|---------|-------|
| Style preset | Minimal |
| Header font | Segoe UI 9pt SemiBold #1B365D |
| Row font | Segoe UI 9pt #333333 |
| Value font | Calibri 9pt #333333 |
| Values format | #,##0 |
| Row height | 22px |
| Column widths | Hotel: 180px, each channel: ~160px |
| Gridlines | ON, both directions, #E8E8E8 |
| Row subtotals | OFF |
| Column subtotals | ON (Grand Total column on right) |
| Padding | 8px |

**Conditional Formatting (Background Heatmap):**
1. Format → Cell elements → Background color → ON → fx
2. Format by: Color scale
3. What field: [Total Bookings]
4. Minimum value: Color #FFFFFF (White) — lowest count
5. Maximum value: Color #1B365D (Navy) — highest count
6. Font color: Auto (white text on dark backgrounds)

---

### 4.10 Device ADR Comparison (Clustered Column Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Column Chart |
| Position | x:980, y:810, w:450, h:190 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "ADR by Device" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | fct_Bookings[DeviceType] | Mobile, Desktop |
| Y-Axis (Values) | [ADR] (M05) | Average Daily Rate per device |
| Tooltips | [Total Revenue] (M01) | Revenue context |
| Tooltips | [Successful Bookings] (M13) | Volume |

**Format:**

| Setting | Value |
|---------|-------|
| Column colors: | |
| — Mobile | #1B365D (Navy) |
| — Desktop | #C4A265 (Gold) |
| Column width | 50% |
| X-Axis font | Segoe UI 10pt #333333 |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis format | ₹#,##0 |
| Y-Axis gridlines | ON, #F0F0F0 |
| Data labels | ON, above, Calibri 11pt Bold #1B365D |
| Data labels format | ₹#,##0 |
| Padding | 10px |

---

### 4.11 Avg Booking Lead Time by Channel (Clustered Bar Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Bar Chart |
| Position | x:1450, y:810, w:450, h:190 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Booking Lead Time (days)" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | fct_Bookings[Channel] | 4 channels |
| X-Axis (Values) | [Avg Booking Lead Time] (M20) | Days between booking and check-in |
| Tooltips | [Total Bookings] (M12) | Volume context |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | Descending by [Avg Booking Lead Time] |
| Bar colors (fixed per channel): | |
| — Website | #1B365D |
| — App | #17A2B8 |
| — Agent | #C4A265 |
| — ThirdParty | #6C757D |
| Y-Axis font | Segoe UI 9pt #333333 |
| X-Axis font | Calibri 9pt #6C757D |
| X-Axis format | 0 "days" |
| Gridlines | ON, #F0F0F0 |
| Data labels | ON, outside end, Calibri 9pt Bold |
| Data labels format | 0 "d" |
| Padding | 10px |

---

### 4.12 Footer

Same as other pages: [Last Refresh] at x:20, copyright centered, internal note right-aligned, line at y:1010.

---

## 5. DAX Measures Used

| Visual | Measure(s) | ID |
|--------|-----------|-----|
| Channel Donut | [Total Bookings] | M12 |
| Channel Donut (tooltips) | [Total Revenue], [Revenue Per Booking], [Successful Bookings] | M01, M06, M13 |
| Device Donut | [Total Bookings] | M12 |
| Device Donut (tooltips) | [Total Revenue], [ADR] | M01, M05 |
| Channel Trend | [Total Bookings] | M12 |
| Channel Trend (tooltips) | [Total Revenue], [Revenue Per Booking] | M01, M06 |
| Channel Revenue Bar | [Total Revenue] | M01 |
| Channel Revenue (tooltips) | [Successful Bookings], [Revenue Per Booking], [Profit Margin %], [ADR] | M13, M06, M04, M05 |
| Revenue Per Booking Column | [Revenue Per Booking] | M06 |
| Revenue Per Booking (tooltips) | [Total Revenue], [Successful Bookings], [ADR] | M01, M13, M05 |
| Hotel×Channel Matrix | [Total Bookings] | M12 |
| Device ADR Column | [ADR] | M05 |
| Device ADR (tooltips) | [Total Revenue], [Successful Bookings] | M01, M13 |
| Lead Time Bar | [Avg Booking Lead Time] | M20 |
| Lead Time (tooltips) | [Total Bookings] | M12 |
| Footer | [Last Refresh] | M51 |

**Unique measures on this page: 9** (M01, M04, M05, M06, M12, M13, M20, M51)

---

## 6. Cross-Filter Interactions

| Source (clicked) | → Channel Donut | → Device Donut | → Trend Area | → Revenue Bar | → Rev/Booking Col | → Matrix | → Device ADR | → Lead Time |
|-----------------|-----------------|----------------|--------------|---------------|-------------------|----------|--------------|-------------|
| Channel Donut | — | Highlight | **Filter** | **Filter** | **Filter** | **Filter** | Highlight | **Filter** |
| Device Donut | Highlight | — | Highlight | Highlight | Highlight | Highlight | **Filter** | Highlight |
| Trend Area | Highlight | Highlight | — | Highlight | Highlight | Highlight | Highlight | Highlight |
| Revenue Bar | **Filter** | Highlight | **Filter** | — | **Filter** | **Filter** | Highlight | **Filter** |
| Rev/Booking Col | **Filter** | Highlight | **Filter** | **Filter** | — | **Filter** | Highlight | **Filter** |
| Matrix | **Filter** | Highlight | **Filter** | **Filter** | **Filter** | — | Highlight | **Filter** |
| Device ADR | Highlight | **Filter** | Highlight | Highlight | Highlight | Highlight | — | Highlight |
| Lead Time | **Filter** | Highlight | **Filter** | **Filter** | **Filter** | **Filter** | Highlight | — |
| Slicers | Filter | Filter | Filter | Filter | Filter | Filter | Filter | Filter |

**Key decisions:**
- Channel Donut FILTERS channel-specific visuals (Revenue bar, Trend, Matrix, Lead Time) — primary selector
- Revenue Bar and Rev/Booking Column also FILTER to allow focused analysis
- Device Donut only FILTERS the Device ADR chart (its direct pair)
- Trend Area only HIGHLIGHTS (maintains time overview)
- Matrix FILTERS everything when a cell is clicked (hotel+channel intersection)

---

## 7. Conditional Formatting

| Visual | Element | Type | Rules |
|--------|---------|------|-------|
| Channel Revenue Bar | Bar fill | Rules-based | Website=#1B365D, App=#17A2B8, Agent=#C4A265, ThirdParty=#6C757D |
| Rev/Booking Column | Column fill | Rules-based | Same channel colors as above |
| Lead Time Bar | Bar fill | Rules-based | Same channel colors |
| Hotel×Channel Matrix | Cell background | Color scale (gradient) | Min=White(#FFFFFF) → Max=Navy(#1B365D) |
| Device ADR Column | Column fill | Rules-based | Mobile=#1B365D, Desktop=#C4A265 |

---

## 8. Drill-Through

| Source Visual | Trigger | Target Page | Filter Field |
|--------------|---------|-------------|--------------|
| Hotel×Channel Matrix (row click) | Right-click hotel name | "Hotel Drillthrough" | dim_Hotel[Hotel_Name] |

No other drill-through on this page.

---

## 9. Bookmarks

| Bookmark | Button Position | Action |
|----------|----------------|--------|
| BM-WebsiteOnly | x:1710, y:105, w:100, h:28 | Channel slicer = Website |
| BM-MobileOnly | x:1710, y:140, w:100, h:28 | Device slicer = Mobile |
| BM-ResetChannel | x:1710, y:175, w:100, h:28 | Clear Channel + Device slicers |

Button styling: Segoe UI 9pt, #F5F6FA fill, #1B365D text, radius 4px. Gold on hover.

---

## 10. Color Palette (This Page)

| Element | Color | Hex |
|---------|-------|-----|
| Website (all visuals) | Navy | #1B365D |
| App (all visuals) | Teal | #17A2B8 |
| Agent (all visuals) | Gold | #C4A265 |
| ThirdParty (all visuals) | Gray | #6C757D |
| Mobile device | Navy | #1B365D |
| Desktop device | Gold | #C4A265 |
| Matrix heatmap max | Navy | #1B365D |
| Matrix heatmap min | White | #FFFFFF |
| Card backgrounds | White | #FFFFFF |
| Page background | Cool Gray | #F5F6FA |

---

## 11. Build Steps

### Step 1: Create Page
1. Add page → Rename "Channel Analysis" → Canvas 1920×1080 → BG: #F5F6FA

### Step 2: Navigation Bar
1. Copy from previous page. Set "Channels" button active (Gold fill).

### Step 3: Header
1. Text box: "Booking Source & Channel Analysis" — 20pt Bold #1B365D
2. Text box: "Marketing Channel Intelligence" — 11pt #6C757D
3. Line at y:96

### Step 4: Channel Donut
1. Visualizations → Donut Chart
2. Legend: fct_Bookings[Channel], Values: [Total Bookings]
3. Tooltips: [Total Revenue], [Revenue Per Booking], [Successful Bookings]
4. Position: x:20, y:105, w:520, h:185
5. Format: Inner radius 55%, set 4 slice colors, detail labels ON

### Step 5: Device Donut
1. Visualizations → Donut Chart
2. Legend: fct_Bookings[DeviceType], Values: [Total Bookings]
3. Tooltips: [Total Revenue], [ADR]
4. Position: x:560, y:105, w:520, h:185
5. Format: Mobile=#1B365D, Desktop=#C4A265

### Step 6: Slicers
1. Add 4 slicers per Section 4.5
2. Format: White, border, selected=Gold

### Step 7: Channel Trend (Stacked Area)
1. Visualizations → Stacked Area Chart
2. X: dim_Date[YearQuarter], Y: [Total Bookings], Legend: Channel
3. Tooltips: [Total Revenue], [Revenue Per Booking]
4. Position: x:20, y:300, w:1880, h:290
5. Format: 4 area colors with 20% transparency, legend Top-Right

### Step 8: Channel Revenue Bar
1. Visualizations → Clustered Bar
2. Y: Channel, X: [Total Revenue]
3. Tooltips: [Successful Bookings], [Revenue Per Booking], [Profit Margin %], [ADR]
4. Position: x:20, y:600, w:940, h:200
5. Sort descending, apply conditional bar colors, data labels ON

### Step 9: Revenue Per Booking Column
1. Visualizations → Clustered Column
2. X: Channel, Y: [Revenue Per Booking]
3. Tooltips: [Total Revenue], [Successful Bookings], [ADR]
4. Position: x:980, y:600, w:920, h:200
5. Apply channel colors, data labels ON

### Step 10: Hotel × Channel Matrix
1. Visualizations → Matrix
2. Rows: Hotel_Name, Columns: Channel, Values: [Total Bookings]
3. Position: x:20, y:810, w:940, h:190
4. Apply heatmap conditional formatting on background

### Step 11: Device ADR Column
1. Visualizations → Clustered Column
2. X: DeviceType, Y: [ADR]
3. Tooltips: [Total Revenue], [Successful Bookings]
4. Position: x:980, y:810, w:450, h:190
5. Colors: Mobile=#1B365D, Desktop=#C4A265

### Step 12: Lead Time Bar
1. Visualizations → Clustered Bar
2. Y: Channel, X: [Avg Booking Lead Time]
3. Tooltips: [Total Bookings]
4. Position: x:1450, y:810, w:450, h:190
5. Apply channel colors, data labels format "0 d"

### Step 13: Configure Interactions
1. Per Section 6 table
2. Key: Channel Donut and Revenue Bar = Filter; Trend Area = Highlight only

### Step 14: Footer + Bookmarks
1. Copy footer from previous pages
2. Add 3 bookmark buttons per Section 9

### Step 15: Verify
- [ ] Channel donut shows 4 slices with correct percentages (Web 45%, App 25%, Agent 17%, 3rd 13%)
- [ ] Device donut shows Mobile/Desktop split
- [ ] Trend chart shows stacked areas over quarters
- [ ] Revenue bar sorted descending with correct channel colors
- [ ] Rev/Booking columns show avg ticket per channel
- [ ] Matrix shows 7 rows × 4 columns with heatmap coloring
- [ ] Device ADR shows 2 columns
- [ ] Lead time shows days per channel
- [ ] Clicking Channel donut filters revenue bar and matrix
- [ ] Slicers filter all visuals

---

## 12. Business Insights This Page Reveals

| Insight | Visual Source |
|---------|-------------|
| Which channel dominates bookings? | Channel donut (Website ~45%) |
| Is App channel growing over time? | Trend area (watch Teal layer expand) |
| Which channel generates most revenue? | Revenue bar (may differ from volume leader) |
| Which channel has highest ticket size? | Rev/Booking columns |
| Are direct channels (Web/App) more profitable than indirect (Agent/3rd)? | Revenue bar vs donut comparison |
| Does device type affect spending? | Device ADR comparison |
| Do certain hotels rely more on agents? | Matrix heatmap (darker cells = higher reliance) |
| How far in advance do guests book by channel? | Lead time bar (Agents may book further ahead) |

---

*End of Channel Analysis Page Design*
