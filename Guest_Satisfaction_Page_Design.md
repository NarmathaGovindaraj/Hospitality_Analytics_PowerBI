# Guest Satisfaction Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Guest Satisfaction |
| Page Order | 6th page (after Channel Analysis) |
| Canvas Size | 1920 × 1080 px |
| Background Color | #F5F6FA (Light Cool Gray) |
| Page-Level Filter | None (satisfaction applies to ALL bookings regardless of PaymentStatus) |
| Business Objective | Monitor guest experience quality through ratings, feedback analysis, and satisfaction trends to identify service improvement areas |

---

## 2. Business Questions Answered

1. What is the overall Guest Satisfaction Score across the portfolio?
2. How do the three rating dimensions (Overall, Staff, Cleanliness) compare?
3. What are the most common feedback categories — positive and negative?
4. Which hotels have the highest and lowest satisfaction?
5. Is guest satisfaction improving or declining over time?
6. What percentage of feedback is positive vs negative?
7. How does satisfaction vary by hotel category?
8. Which specific areas need immediate attention?

---

## 3. Wireframe (1920 × 1080)

```
┌══════════════════════════════════════════════════════════════════════════════════════┐
║ y:0  NAV BAR (h:44) #1B365D                                                        ║
║ [Home][Executive][Revenue][Manager][Channels][●Satisfaction][Membership]             ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:44  PAGE HEADER (h:52) Background: #FFFFFF                                         ║
║                                                                                      ║
║  "Guest Satisfaction & Feedback"              "Service Quality Monitoring"           ║
║  ────────────────────────────── (1px #E0E0E0) ──────────────────────────────────── ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:100  KPI ROW (h:130)                                                               ║
║                                                                                      ║
║  ┌─────────────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────────┐ │SLICERS     ║
║  │     GSS         │ │Overall │ │ Staff  │ │Clean-  │ │Positive:72%│ │Year   [▼]  ║
║  │   GAUGE         │ │Rating  │ │Rating  │ │liness  │ │Negative:28%│ │Hotel  [▼]  ║
║  │    4.11         │ │  4.13  │ │  4.10  │ │  4.09  │ │            │ │Category    ║
║  │  ╭──────╮       │ │        │ │        │ │        │ │ [████░░░░] │ │[L][H][B][R]║
║  │  │target│       │ │        │ │        │ │        │ │            │ │Sentiment   ║
║  │  │ 4.0  │       │ │        │ │        │ │        │ │            │ │[Pos][Neg]  ║
║  │  ╰──────╯       │ │        │ │        │ │        │ │            │ │            ║
║  └─────────────────┘ └────────┘ └────────┘ └────────┘ └────────────┘ │            ║
║  x:20 w:320         x:360     x:540      x:720      x:900           │x:1450      ║
║                      w:160     w:160      w:160      w:320           │w:200       ║
╠══════════════════════════════════════════════╦═══════════════════════╩════════════╣
║ y:240  ROW 1 (h:340)                         ║                                    ║
║                                              ║  GSS BY HOTEL (Horizontal Bar)     ║
║  FEEDBACK CATEGORIES (Horizontal Bar)        ║  x:980, y:240, w:920, h:340        ║
║  x:20, y:240, w:940, h:340                   ║                                    ║
║                                              ║  Y: dim_Hotel[Hotel_Name]           ║
║  Y: dim_Feedback[Feedback]                   ║  X: [GSS]                          ║
║  X: Count of BookingID                       ║  Constant line at 4.0 (target)     ║
║  Legend: dim_Feedback[FeedbackSentiment]      ║  Sort: GSS descending              ║
║                                              ║  Data labels: ON                   ║
║  Sort: Count descending                      ║  Conditional color: gradient       ║
║  Colors: Positive=Green, Negative=Red        ║                                    ║
║                                              ║                                    ║
╠══════════════════════════════════════════════╬════════════════════════════════════╣
║ y:590  ROW 2 (h:230)                         ║                                    ║
║                                              ║  SATISFACTION BY CATEGORY           ║
║  RATING TREND (Multi-Line Chart)             ║  (Clustered Bar)                   ║
║  x:20, y:590, w:940, h:230                   ║  x:980, y:590, w:920, h:230        ║
║                                              ║                                    ║
║  X: dim_Date[YearQuarter]                    ║  Y: dim_Hotel[Category]            ║
║  Lines: [Avg Rating] — Navy                  ║  X: [GSS]                          ║
║         [Avg Staff Rating] — Gold            ║  Fixed colors per category          ║
║         [Avg Cleanliness Rating] — Teal      ║  Data labels: ON                   ║
║                                              ║  Constant line at 4.0              ║
║                                              ║                                    ║
╠══════════════════════════════════════════════╬════════════════════════════════════╣
║ y:830  ROW 3 (h:170)                         ║                                    ║
║                                              ║  RATING HEATMAP (Matrix)           ║
║  FEEDBACK TREND (Stacked Area)               ║  x:980, y:830, w:920, h:170        ║
║  x:20, y:830, w:940, h:170                   ║                                    ║
║                                              ║  Rows: dim_Hotel[Hotel_Name]        ║
║  X: dim_Date[YearQuarter]                    ║  Values: [Avg Rating],             ║
║  Y: Count of BookingID                       ║    [Avg Staff Rating],             ║
║  Legend: dim_Feedback[FeedbackSentiment]      ║    [Avg Cleanliness Rating], [GSS] ║
║  Colors: Positive=#2E8B57, Negative=#DC3545  ║  Conditional: Background heatmap   ║
║                                              ║                                    ║
╠══════════════════════════════════════════════╩════════════════════════════════════╣
║ y:1010  FOOTER (h:30)                                                             ║
║ [Last Refresh]           © 2026 Taj Hotels           Internal Use Only             ║
╚══════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element Specifications

### 4.1 Navigation Bar
Active button = "Satisfaction" (6th position): Fill #C4A265, Text #0D1B2A.
All others: Transparent, White text. Same structure as other pages.

### 4.2 Page Header

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Title | Text Box | 20 | 50 | 500 | 30 | "Guest Satisfaction & Feedback" | Segoe UI 20pt Bold #1B365D |
| Subtitle | Text Box | 20 | 76 | 350 | 18 | "Service Quality Monitoring" | Segoe UI 11pt #6C757D |
| Divider | Line | 20 | 96 | 1880 | 1 | — | #E0E0E0 |

### 4.3 GSS Gauge

| Property | Value |
|----------|-------|
| Type | Gauge |
| Position | x:20, y:105, w:320, h:125 |
| Background | #FFFFFF, border 1px #E8E8E8, radius 8px, shadow |
| Title | "Guest Satisfaction Score" — Segoe UI 11pt SemiBold #1B365D |

**Field Wells:**

| Well | Field |
|------|-------|
| Value | [GSS] (M21) |
| Minimum value | 1 (fixed) |
| Maximum value | 5 (fixed) |
| Target value | 4.0 (fixed constant) |

**Format:**

| Setting | Value |
|---------|-------|
| Gauge fill color | Gradient: Red (1.0) → Amber (3.0) → Green (4.5) |
| Target line | ON, color #1B365D, thickness 3px |
| Target label | "Target: 4.0" — Segoe UI 9pt |
| Callout value | ON, Calibri 28pt Bold |
| Callout color | Conditional: ≥4.0=#2E8B57, 3.0-3.99=#FFC107, <3.0=#DC3545 |
| Data label (value) | 0.00 format |
| Padding | 12px |

**Gauge color bands (via conditional formatting or visual settings):**
- 1.0 – 2.5: #DC3545 (Red)
- 2.5 – 3.5: #FFC107 (Amber)
- 3.5 – 5.0: #2E8B57 (Green)

### 4.4 Rating Cards (3 cards)

| Card | Measure | x | y | w | h |
|------|---------|---|---|---|---|
| Overall Rating | [Avg Rating] (M22) | 360 | 105 | 160 | 125 |
| Staff Rating | [Avg Staff Rating] (M23) | 540 | 105 | 160 | 125 |
| Cleanliness Rating | [Avg Cleanliness Rating] (M24) | 720 | 105 | 160 | 125 |

**Styling (all 3):**

| Setting | Value |
|---------|-------|
| Background | #FFFFFF |
| Border | 1px #E8E8E8, radius 8px |
| Shadow | ON |
| Top accent | 4px Gold (#C4A265) |
| Callout font | Calibri 22pt Bold |
| Callout color | Conditional: ≥4.0=#2E8B57, 3.0-3.99=#FFC107, <3.0=#DC3545 |
| Category label | "Overall Rating" / "Staff Rating" / "Cleanliness" |
| Category font | Segoe UI 9pt #6C757D |
| Subtitle | Star visualization: "⭐⭐⭐⭐" for ≥4.0 (use [GSS Band] or static) |

### 4.5 Positive/Negative Split Card

| Property | Value |
|----------|-------|
| Type | Multi-row Card (or two Cards side by side) |
| Position | x:900, y:105, w:320, h:125 |
| Background | #FFFFFF, border, radius 8px, shadow, gold accent |

**Option A: Two cards side-by-side inside a container:**

| Sub-card | Measure | Format | Color |
|----------|---------|--------|-------|
| Left | [Positive Feedback %] (M25) | "0.0% Positive" | #2E8B57 (Green) |
| Right | [Negative Feedback %] (M26) | "0.0% Negative" | #DC3545 (Red) |

**Option B: 100% Stacked Bar (single bar):**
- Legend: FeedbackSentiment
- Values: COUNTROWS(dim_Feedback)
- Colors: Positive=#2E8B57, Negative=#DC3545
- Data labels: Inside, white, percentage
- Size: w:320, h:50 (compact within the card area)

### 4.6 Slicers

| Slicer | Field | x | y | w | h | Style |
|--------|-------|---|---|---|---|-------|
| Year | dim_Date[Year] | 1450 | 105 | 190 | 38 | Dropdown |
| Hotel | dim_Hotel[Hotel_Name] | 1450 | 150 | 190 | 38 | Dropdown (search ON) |
| Category | dim_Hotel[Category] | 1450 | 195 | 190 | 32 | Horizontal tiles (4) |
| Sentiment | dim_Feedback[FeedbackSentiment] | 1660 | 105 | 130 | 32 | Horizontal tiles (2) |

**Slicer format:** White background, #E0E0E0 border, 6px radius. Selected tile: #C4A265 fill, white text.

---

### 4.7 Feedback Categories (Horizontal Bar Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Bar Chart |
| Position | x:20, y:240, w:940, h:340 |
| Background | #FFFFFF, border 1px #E8E8E8, radius 8px, shadow |
| Title | "Feedback Category Distribution" — Segoe UI 14pt SemiBold #1B365D |
| Subtitle | "Colored by sentiment" — Segoe UI 9pt Italic #6C757D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | dim_Feedback[Feedback] | 10 feedback categories |
| X-Axis (Values) | Count of dim_Feedback[BookingID] | Booking count per category |
| Legend | dim_Feedback[FeedbackSentiment] | Colors bars by Positive/Negative |
| Tooltips | [Positive Feedback %] | Overall context |
| Tooltips | [GSS] | Score when that feedback occurs |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | Descending by Count (highest frequency first) |
| Bar colors by Legend: | |
| — Positive | #2E8B57 (Green) |
| — Negative | #DC3545 (Red) |
| Bar spacing | 15% |
| Y-Axis (category) font | Segoe UI 10pt #333333 |
| Y-Axis title | OFF |
| X-Axis font | Calibri 9pt #6C757D |
| X-Axis title | OFF |
| X-Axis gridlines | ON, #F0F0F0, dashed |
| Data labels | ON, outside end, Calibri 9pt Bold, color matches bar |
| Data labels format | #,##0 |
| Legend | OFF (colors are self-explanatory with only 2 categories; title says "colored by sentiment") |
| Padding | 16px |

**Expected bar order (descending by count):**
1. Good service (Green)
2. Very good (Green)
3. Check-in was slow (Red)
4. Excellent stay (Green)
5. Average (Red)
6. Room was clean and comfortable (Green)
7. Could be better (Red)
8. Food quality was great (Green)
9. Exceptional hospitality (Green)
10. Will visit again (Green)

---

### 4.8 GSS by Hotel (Horizontal Bar Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Bar Chart |
| Position | x:980, y:240, w:920, h:340 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Guest Satisfaction by Hotel" — Segoe UI 14pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | dim_Hotel[Hotel_Name] | 7 hotels |
| X-Axis (Values) | [GSS] (M21) | Average GSS per hotel |
| Tooltips | [Avg Rating] (M22) | Overall component |
| Tooltips | [Avg Staff Rating] (M23) | Staff component |
| Tooltips | [Avg Cleanliness Rating] (M24) | Cleanliness component |
| Tooltips | [Positive Feedback %] (M25) | Positive share |
| Tooltips | [Successful Bookings] (M13) | Volume context |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | Descending by [GSS] |
| Bar color | Conditional gradient: Low GSS=#FFC107 → High GSS=#2E8B57 |
| Y-Axis font | Segoe UI 11pt #333333 |
| Y-Axis title | OFF |
| X-Axis font | Calibri 9pt #6C757D |
| X-Axis title | OFF |
| X-Axis range | Minimum: 3.0, Maximum: 5.0 (focus on the relevant range) |
| Data labels | ON, outside end, Calibri 10pt Bold |
| Data labels format | 0.00 |
| Gridlines | ON, #F0F0F0 |
| Constant Line | Value=4.0, Color=#1B365D, Style=Dashed, Width=2px, Label="Target" |
| Padding | 16px |

**Adding constant line:**
1. Select chart → Analytics pane (magnifying glass)
2. Constant Line → Add Line → Value: 4.0
3. Color: #1B365D, Style: Dashed, Label: "Target: 4.0", Position: Behind bars

---

### 4.9 Rating Trend (Multi-Line Chart)

| Property | Value |
|----------|-------|
| Type | Line Chart |
| Position | x:20, y:590, w:940, h:230 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Rating Trend Over Time" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[YearQuarter] | Quarterly granularity |
| Y-Axis (Values) | [Avg Rating] (M22) | Line 1 |
| Y-Axis (Values) | [Avg Staff Rating] (M23) | Line 2 |
| Y-Axis (Values) | [Avg Cleanliness Rating] (M24) | Line 3 |
| Tooltips | [GSS] (M21) | Combined score |
| Tooltips | [Positive Feedback %] (M25) | Sentiment context |

**Format:**

| Setting | Value |
|---------|-------|
| Line 1 (Avg Rating) | Color: #1B365D (Navy), Width: 2.5px, Markers: Circle 5px |
| Line 2 (Avg Staff Rating) | Color: #C4A265 (Gold), Width: 2.5px, Markers: Diamond 5px |
| Line 3 (Avg Cleanliness) | Color: #17A2B8 (Teal), Width: 2.5px, Markers: Square 5px |
| X-Axis font | Segoe UI 9pt #6C757D, Rotation: -45° |
| Y-Axis font | Calibri 9pt #6C757D |
| Y-Axis format | 0.0 |
| Y-Axis range | Min: 3.0, Max: 5.0 |
| Gridlines | Horizontal, #F0F0F0, dashed |
| Legend | ON, position Top, Segoe UI 9pt |
| Legend labels | "Overall", "Staff", "Cleanliness" |
| Data labels | OFF (too dense for quarterly multi-line) |
| Constant Line | Value=4.0, Color=#DC3545, Style=Dotted, Label="Target" |
| Padding | 14px |

---

### 4.10 Satisfaction by Category (Clustered Bar Chart)

| Property | Value |
|----------|-------|
| Type | Clustered Bar Chart |
| Position | x:980, y:590, w:920, h:230 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Satisfaction by Hotel Category" — Segoe UI 13pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Y-Axis | dim_Hotel[Category] | 4 categories |
| X-Axis (Values) | [GSS] (M21) | Average GSS per category |
| Tooltips | [Avg Rating] (M22) | Component detail |
| Tooltips | [Avg Staff Rating] (M23) | Component detail |
| Tooltips | [Avg Cleanliness Rating] (M24) | Component detail |
| Tooltips | [Successful Bookings] (M13) | Volume |

**Format:**

| Setting | Value |
|---------|-------|
| Sort | Descending by [GSS] |
| Bar colors (fixed per category): | |
| — Luxury | #C4A265 (Gold) |
| — Heritage | #8B0000 (Deep Red) |
| — Business | #1B365D (Navy) |
| — Resort | #17A2B8 (Teal) |
| Y-Axis font | Segoe UI 11pt #333333 |
| X-Axis font | Calibri 9pt #6C757D |
| X-Axis range | Min: 3.5, Max: 4.5 (narrow range to show differences) |
| Data labels | ON, outside end, Calibri 10pt Bold, color matches bar |
| Data labels format | 0.00 |
| Constant Line | Value=4.0, Color=#1B365D, Dashed |
| Gridlines | ON, #F0F0F0 |
| Padding | 14px |

**Conditional formatting (rules-based bar color):**
1. Format → Bars → Color → fx → Rules
2. If Category = "Luxury" then #C4A265
3. If Category = "Heritage" then #8B0000
4. If Category = "Business" then #1B365D
5. If Category = "Resort" then #17A2B8

---

### 4.11 Feedback Sentiment Trend (Stacked Area Chart)

| Property | Value |
|----------|-------|
| Type | Stacked Area Chart |
| Position | x:20, y:830, w:940, h:170 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Feedback Sentiment Over Time" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| X-Axis | dim_Date[YearQuarter] | Quarterly |
| Y-Axis (Values) | Count of dim_Feedback[BookingID] | Volume per quarter |
| Legend | dim_Feedback[FeedbackSentiment] | Stacks: Positive + Negative |
| Tooltips | [Positive Feedback %] (M25) | Percentage |
| Tooltips | [Negative Feedback %] (M26) | Percentage |

**Format:**

| Setting | Value |
|---------|-------|
| Area colors: | |
| — Positive | #2E8B57 (Green), Transparency: 30% |
| — Negative | #DC3545 (Red), Transparency: 30% |
| Line colors | Same as area (solid) |
| X-Axis font | Segoe UI 8pt #6C757D, Rotation: -45° |
| Y-Axis font | Calibri 8pt #6C757D |
| Y-Axis title | OFF |
| Gridlines | OFF (compact chart) |
| Legend | ON, Top-Right, Segoe UI 9pt |
| Data labels | OFF (too compact) |
| Padding | 10px |

---

### 4.12 Rating Heatmap (Matrix Visual)

| Property | Value |
|----------|-------|
| Type | Matrix |
| Position | x:980, y:830, w:920, h:170 |
| Background | #FFFFFF, border, radius 8px, shadow |
| Title | "Rating Heatmap by Hotel" — Segoe UI 12pt SemiBold #1B365D |

**Field Wells:**

| Well | Field | Notes |
|------|-------|-------|
| Rows | dim_Hotel[Hotel_Name] | 7 hotels |
| Values | [Avg Rating] (M22) | Column 1 |
| Values | [Avg Staff Rating] (M23) | Column 2 |
| Values | [Avg Cleanliness Rating] (M24) | Column 3 |
| Values | [GSS] (M21) | Column 4 |

**Format:**

| Setting | Value |
|---------|-------|
| Style preset | Minimal (no row/column headers background) |
| Font (headers) | Segoe UI 9pt SemiBold #1B365D |
| Font (values) | Calibri 10pt #333333 |
| Column headers | "Overall", "Staff", "Cleanliness", "GSS" |
| Row height | 20px |
| Gridlines | ON, horizontal + vertical, #E8E8E8 |
| Values format | 0.00 |
| Padding | 8px |
| Column widths | Hotel_Name: 180px, each value column: ~140px |

**Conditional Formatting (Background Color — Heatmap):**

Apply to ALL 4 value columns:
1. Select matrix → Format → Cell elements → Background color → ON → fx
2. Format by: **Color scale**
3. What field: [Avg Rating] (or respective measure for each column)
4. Minimum: 3.0 → Color: #FFCCCC (Light Red)
5. Center: 4.0 → Color: #FFFFFF (White)
6. Maximum: 5.0 → Color: #C8E6C9 (Light Green)

Repeat for each value column with its respective measure.

**Result:** Cells with low ratings appear pinkish-red; cells with high ratings appear green; 4.0 is neutral white. This creates a visual heatmap effect.

---

### 4.13 Footer

Same as other pages:
| Element | x | y | Content | Format |
|---------|---|---|---------|--------|
| Last Refresh | 20 | 1020 | [Last Refresh] (M51) | Segoe UI 9pt #78909C |
| Copyright | 800 | 1020 | "© 2026 Taj Hotels India" | Centered |
| Internal | 1600 | 1020 | "Internal Use Only" | Right-aligned |

---

## 5. DAX Measures Used

| Visual | Measure(s) | ID |
|--------|-----------|-----|
| GSS Gauge | [GSS] | M21 |
| Rating Card 1 | [Avg Rating] | M22 |
| Rating Card 2 | [Avg Staff Rating] | M23 |
| Rating Card 3 | [Avg Cleanliness Rating] | M24 |
| Pos/Neg Split | [Positive Feedback %], [Negative Feedback %] | M25, M26 |
| Feedback Categories (tooltip) | [Positive Feedback %], [GSS] | M25, M21 |
| GSS by Hotel | [GSS] | M21 |
| GSS by Hotel (tooltips) | [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating], [Positive Feedback %], [Successful Bookings] | M22, M23, M24, M25, M13 |
| Rating Trend | [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating] | M22, M23, M24 |
| Rating Trend (tooltips) | [GSS], [Positive Feedback %] | M21, M25 |
| Satisfaction by Category | [GSS] | M21 |
| Satisfaction by Category (tooltips) | [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating], [Successful Bookings] | M22, M23, M24, M13 |
| Feedback Trend | Count (no custom measure — uses COUNTROWS context) | — |
| Feedback Trend (tooltips) | [Positive Feedback %], [Negative Feedback %] | M25, M26 |
| Heatmap Matrix | [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating], [GSS] | M22, M23, M24, M21 |
| Footer | [Last Refresh] | M51 |

**Total unique measures: 9** (M13, M21, M22, M23, M24, M25, M26, M51 + Count)

---

## 6. Cross-Filter Interactions

| Source (clicked) | → Gauge | → Cards | → Feedback Bar | → GSS by Hotel | → Trend | → Category Bar | → Sentiment Area | → Heatmap |
|-----------------|---------|---------|----------------|----------------|---------|----------------|-----------------|-----------|
| Feedback Bar | None | None | — | Highlight | Highlight | Highlight | **Filter** | Highlight |
| GSS by Hotel | None | None | **Filter** | — | **Filter** | Highlight | **Filter** | **Filter** |
| Rating Trend | None | None | Highlight | Highlight | — | Highlight | Highlight | Highlight |
| Category Bar | None | None | **Filter** | **Filter** | **Filter** | — | **Filter** | **Filter** |
| Sentiment Area | None | None | **Filter** | Highlight | Highlight | Highlight | — | Highlight |
| Heatmap | None | None | **Filter** | Highlight | Highlight | Highlight | Highlight | — |
| Slicers | Filter | Filter | Filter | Filter | Filter | Filter | Filter | Filter |

**Key decisions:**
- GSS by Hotel and Category Bar FILTER — they are primary analytical selectors
- Feedback Bar filters the Sentiment Area (shows composition change when selecting a category)
- Gauge and rating cards are NEVER affected by chart interactions (only slicers change them)
- Heatmap filters the feedback bar (clicking a hotel row shows that hotel's feedback breakdown)

---

## 7. Conditional Formatting Summary

| Visual | Element | Type | Rules |
|--------|---------|------|-------|
| GSS Gauge | Callout value | Color rules | ≥4.0 Green, 3.0-3.99 Amber, <3.0 Red |
| Rating Card 1-3 | Callout value | Color rules | ≥4.0 Green, 3.0-3.99 Amber, <3.0 Red |
| GSS by Hotel bar | Bar fill | Gradient | Low=#FFC107(Amber) → High=#2E8B57(Green) |
| Satisfaction by Category | Bar fill | Rules | Fixed per category (Gold/Red/Navy/Teal) |
| Heatmap Matrix (all columns) | Background | Color scale | Min(3.0)=#FFCCCC → Center(4.0)=#FFFFFF → Max(5.0)=#C8E6C9 |
| Feedback Categories bar | Bar color | By Legend | Positive=#2E8B57, Negative=#DC3545 |
| Sentiment Area | Area fill | By Legend | Positive=#2E8B57, Negative=#DC3545 |

---

## 8. Drill-Through

| Source Visual | Trigger | Target | Filter Field |
|--------------|---------|--------|--------------|
| GSS by Hotel bar | Right-click hotel name | "Hotel Drillthrough" page | dim_Hotel[Hotel_Name] |

No other drill-through configured on this page.

---

## 9. Bookmarks

| Bookmark | Button Position | Action |
|----------|----------------|--------|
| BM-ShowNegativeOnly | x:1660, y:155, w:120, h:28 | Sets Sentiment slicer to "Negative" |
| BM-ResetSatisfaction | x:1660, y:190, w:120, h:28 | Clears all slicers on this page |

Button style: Segoe UI 9pt, #F5F6FA fill, #1B365D text, radius 4px. Gold hover.

---

## 10. Build Steps

### Step 1: Create Page
1. Add page → Rename "Guest Satisfaction" → Canvas 1920×1080 → BG: #F5F6FA

### Step 2: Navigation Bar
1. Copy from previous page. Set "Satisfaction" button active (Gold fill).

### Step 3: Header
1. Text box: "Guest Satisfaction & Feedback" → Segoe UI 20pt Bold #1B365D
2. Text box: "Service Quality Monitoring" → Segoe UI 11pt #6C757D
3. Line divider at y:96

### Step 4: GSS Gauge
1. Visualizations → Gauge
2. Value: [GSS], Min: 1, Max: 5, Target: 4.0
3. Position: x:20, y:105, w:320, h:125
4. Format: Callout 28pt, conditional color, target line ON

### Step 5: Rating Cards (3)
1. Add 3 Card visuals: [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating]
2. Position per Section 4.4
3. Format: White, shadow, gold accent, conditional value color

### Step 6: Positive/Negative Split
1. Add 2 small cards ([Positive Feedback %] in green, [Negative Feedback %] in red)
2. Position: x:900, y:105, w:320, h:125
3. Alternative: 100% stacked bar (horizontal, single bar)

### Step 7: Slicers
1. Add Year dropdown, Hotel dropdown, Category tiles, Sentiment tiles
2. Position per Section 4.6
3. Format: White bg, selected=Gold

### Step 8: Feedback Categories Bar Chart
1. Visualizations → Clustered Bar
2. Y: dim_Feedback[Feedback], X: Count of BookingID, Legend: FeedbackSentiment
3. Position: x:20, y:240, w:940, h:340
4. Format: Sort desc, Green/Red by legend, data labels ON

### Step 9: GSS by Hotel Bar Chart
1. Visualizations → Clustered Bar
2. Y: dim_Hotel[Hotel_Name], X: [GSS]
3. Tooltips: [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating], [Positive Feedback %], [Successful Bookings]
4. Position: x:980, y:240, w:920, h:340
5. X-Axis min: 3.0, max: 5.0
6. Conditional gradient on bars
7. Analytics → Constant line at 4.0

### Step 10: Rating Trend (Multi-Line)
1. Visualizations → Line Chart
2. X: dim_Date[YearQuarter]
3. Y: [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating] (all three in Y-Values)
4. Tooltips: [GSS], [Positive Feedback %]
5. Position: x:20, y:590, w:940, h:230
6. Format: 3 line colors (Navy, Gold, Teal), markers ON, legend top
7. Y-Axis range: 3.0–5.0
8. Constant line at 4.0

### Step 11: Satisfaction by Category Bar
1. Visualizations → Clustered Bar
2. Y: Category, X: [GSS]
3. Position: x:980, y:590, w:920, h:230
4. Apply rules-based bar color per category
5. Constant line at 4.0

### Step 12: Feedback Sentiment Trend (Stacked Area)
1. Visualizations → Stacked Area Chart
2. X: dim_Date[YearQuarter], Y: Count of BookingID, Legend: FeedbackSentiment
3. Tooltips: [Positive Feedback %], [Negative Feedback %]
4. Position: x:20, y:830, w:940, h:170
5. Colors: Positive=#2E8B57, Negative=#DC3545

### Step 13: Rating Heatmap Matrix
1. Visualizations → Matrix
2. Rows: dim_Hotel[Hotel_Name]
3. Values: [Avg Rating], [Avg Staff Rating], [Avg Cleanliness Rating], [GSS]
4. Position: x:980, y:830, w:920, h:170
5. Apply conditional formatting → Background color → Color scale for each column:
   - Min (3.0): #FFCCCC, Center (4.0): #FFFFFF, Max (5.0): #C8E6C9

### Step 14: Configure Interactions
1. Per Section 6 table. Key: GSS by Hotel and Category Bar = Filter mode.

### Step 15: Footer + Bookmarks
1. Copy footer. Add 2 bookmark buttons.

### Step 16: Verify
- [ ] Gauge shows correct GSS with target at 4.0
- [ ] 3 rating cards have conditional colors
- [ ] Feedback bar shows 10 categories, colored by sentiment
- [ ] GSS by Hotel sorted correctly with constant line
- [ ] Rating trend shows 3 distinct lines
- [ ] Category bar has fixed category colors
- [ ] Heatmap matrix shows color-coded cells
- [ ] Sentiment trend shows green/red stacked areas
- [ ] Slicers filter all visuals
- [ ] Clicking a hotel bar filters feedback chart

---

*End of Guest Satisfaction Page Design*
