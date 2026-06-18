# Power BI Home Page Design
## Hospitality Analytics Dashboard — Landing Page

---

## 1. Page Overview

| Property | Value |
|----------|-------|
| Page Name | Home |
| Canvas Size | 1920 × 1080 (Full HD) |
| Background | Gradient: #0D1B2A (Dark Navy) at top → #1B365D (Navy) at bottom |
| Purpose | Landing page — first impression, navigation hub, top-level KPIs |
| Audience | All users (executives, managers, analysts) |
| Auto-navigate | Set as default landing page in App settings |

---

## 2. Theme Specification

### 2.1 Colors

| Element | Color | Hex | Usage |
|---------|-------|-----|-------|
| Page Background | Dark Navy gradient | #0D1B2A → #1B365D | Full canvas |
| Header Bar | Solid Dark | #0D1B2A | Top 120px |
| Card Background | White | #FFFFFF | KPI cards |
| Card Accent (top border) | Gold | #C4A265 | 4px top border on cards |
| Navigation Button (default) | Semi-transparent White | rgba(255,255,255,0.1) | Button fill |
| Navigation Button (hover) | Gold | #C4A265 | Hover state |
| Navigation Button Text | White | #FFFFFF | Button labels |
| KPI Value Text | Dark Navy | #1B365D | Numbers on cards |
| KPI Label Text | Medium Gray | #6C757D | Labels below values |
| Subtitle/Body Text | Light Gray | #B0BEC5 | On dark background |
| Positive Indicator | Green | #2E8B57 | ▲ growth arrows |
| Negative Indicator | Red | #DC3545 | ▼ decline arrows |
| Divider Lines | Gold (thin) | #C4A265 | Separators |
| Footer Text | Muted | #78909C | Footer area |

### 2.2 Typography

| Element | Font | Size | Weight | Color |
|---------|------|------|--------|-------|
| Dashboard Title | Segoe UI | 28pt | Bold | #FFFFFF |
| Subtitle | Segoe UI | 14pt | Regular | #B0BEC5 |
| KPI Value | Calibri | 24pt | Bold | #1B365D |
| KPI Label | Segoe UI | 10pt | Regular | #6C757D |
| KPI Trend | Calibri | 11pt | SemiBold | Green/Red |
| Nav Button Text | Segoe UI | 12pt | SemiBold | #FFFFFF |
| Nav Button Icon | Segoe MDL2 Assets | 16pt | Regular | #FFFFFF |
| Section Header | Segoe UI | 16pt | SemiBold | #FFFFFF |
| Footer | Segoe UI | 9pt | Regular | #78909C |
| Slicer Header | Segoe UI | 10pt | SemiBold | #FFFFFF |

---

## 3. Complete Wireframe Layout

```
┌══════════════════════════════════════════════════════════════════════════════════┐
║ HEADER SECTION (y:0, h:120px) — Background: #0D1B2A                            ║
║                                                                                  ║
║  ┌──────┐                                                                        ║
║  │ LOGO │   HOSPITALITY ANALYTICS DASHBOARD          ┌─────────────────────┐    ║
║  │ TAJ  │   Performance Intelligence Suite           │ 🕐 Last Refresh:    │    ║
║  │64×64 │   Taj Hotels India                         │ 18-Jun-2026 06:00AM │    ║
║  └──────┘                                            └─────────────────────┘    ║
║                                                                                  ║
║  ═══════════════════════════ Gold line (#C4A265, 2px) ═══════════════════════    ║
╠══════════════════════════════════════════════════════════════════════════════════╣
║ NAVIGATION SECTION (y:120, h:80px) — Background: #0F2540                        ║
║                                                                                  ║
║  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌─────┐║
║  │ 📊        │ │ 📈        │ │ 🏨        │ │ 🌐        │ │ ⭐        │ │ 👥  │║
║  │ Executive │ │ Revenue & │ │ Hotel     │ │ Channel   │ │ Guest    │ │Member│║
║  │ Summary   │ │ Trends    │ │ Manager   │ │ Analysis  │ │ Ratings  │ │  vs  │║
║  │           │ │           │ │           │ │           │ │          │ │NonMbr│║
║  └───────────┘ └───────────┘ └───────────┘ └───────────┘ └───────────┘ └─────┘║
║                                                                                  ║
╠══════════════════════════════════════════════════════════════════════════════════╣
║ KPI CARDS SECTION (y:210, h:160px) — 5 Cards                                    ║
║                                                                                  ║
║  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐                   ║
║  │▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔│ │▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔│ │▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔│ ┌──────────────┐  ║
║  │ (gold border)   │ │ (gold border)   │ │ (gold border)   │ │ GLOBAL       │  ║
║  │                 │ │                 │ │                 │ │ SLICERS      │  ║
║  │  ₹5.72B        │ │   90,091        │ │  ₹28,242       │ │              │  ║
║  │  Total Revenue  │ │  Bookings       │ │  ADR            │ │ Year  ▼      │  ║
║  │  ▲ 12.3% YoY   │ │  ▲ 8.1% YoY    │ │  ▲ 5.2% YoY   │ │ Hotel ▼      │  ║
║  │                 │ │                 │ │                 │ │ Category     │  ║
║  └─────────────────┘ └─────────────────┘ └─────────────────┘ │ [L][H][B][R] │  ║
║  ┌─────────────────┐ ┌─────────────────┐                     │              │  ║
║  │▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔│ │▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔│                     └──────────────┘  ║
║  │ (gold border)   │ │ (gold border)   │                                        ║
║  │                 │ │                 │                                        ║
║  │  ₹2.63B        │ │   4.11          │                                        ║
║  │  Net Profit     │ │  GSS Score      │                                        ║
║  │  45.9% margin   │ │  ● Very Good    │                                        ║
║  │                 │ │                 │                                        ║
║  └─────────────────┘ └─────────────────┘                                        ║
║                                                                                  ║
╠══════════════════════════════════════════════════════════════════════════════════╣
║ EXECUTIVE SUMMARY SECTION (y:380, h:580px)                                       ║
║                                                                                  ║
║  ── "Portfolio Overview" ──────────────────── (Section header, white, 16pt) ──  ║
║                                                                                  ║
║  ┌─────────────────────────────────────────┐ ┌──────────────────────────────┐   ║
║  │                                         │ │                              │   ║
║  │  REVENUE TREND (Area Chart)             │ │  REVENUE BY HOTEL            │   ║
║  │                                         │ │  (Horizontal Bar Chart)      │   ║
║  │  X: dim_Date[Year]                      │ │                              │   ║
║  │  Y: [Total Revenue]                     │ │  Taj Lake Palace    ████████ │   ║
║  │  Fill: Gold gradient                    │ │  Taj Falaknuma      ███████  │   ║
║  │  Line: White                            │ │  Taj Coromandel     █████    │   ║
║  │                                         │ │  Taj Exotica        █████    │   ║
║  │  Background: transparent                │ │  Taj Mahal Palace   ████     │   ║
║  │  Gridlines: subtle gray                 │ │  Taj West End       ███      │   ║
║  │  Data labels: White, Calibri            │ │  Taj Bengal          ██      │   ║
║  │                                         │ │                              │   ║
║  │  h: 260px, w: 880px                     │ │  h: 260px, w: 880px         │   ║
║  └─────────────────────────────────────────┘ └──────────────────────────────┘   ║
║                                                                                  ║
║  ┌─────────────────────────────────────────┐ ┌──────────────────────────────┐   ║
║  │                                         │ │                              │   ║
║  │  CATEGORY SPLIT (Donut Chart)           │ │  QUICK STATS (Multi-row)     │   ║
║  │                                         │ │                              │   ║
║  │     ┌────────┐                          │ │  🏨 7 Hotels                 │   ║
║  │    ╱  Luxury  ╲   Heritage              │ │  🌍 10 Countries             │   ║
║  │   │   32%     │    28%                  │ │  📅 2021–2025                │   ║
║  │    ╲ Business ╱   Resort                │ │  👤 200 Guests               │   ║
║  │     └────────┘  22%    18%              │ │  💳 5 Payment Methods        │   ║
║  │                                         │ │  📱 Mobile: 62% | Desktop: 38%│  ║
║  │  Colors per category:                   │ │  🏷️ Members: 55%             │   ║
║  │  Luxury=#C4A265                         │ │                              │   ║
║  │  Heritage=#8B0000                       │ │  Background: semi-transparent│   ║
║  │  Business=#1B365D                       │ │  white card                  │   ║
║  │  Resort=#17A2B8                         │ │                              │   ║
║  │                                         │ │                              │   ║
║  │  h: 260px, w: 880px                     │ │  h: 260px, w: 880px         │   ║
║  └─────────────────────────────────────────┘ └──────────────────────────────┘   ║
║                                                                                  ║
╠══════════════════════════════════════════════════════════════════════════════════╣
║ FOOTER (y:1040, h:40px) — Background: #0D1B2A                                   ║
║                                                                                  ║
║  © 2026 Taj Hotels India | Hospitality Analytics v1.0 | Data as of: [Last Refresh]║
║  Powered by Power BI | Internal Use Only | Contact: bi.team@tajhotels.com        ║
║                                                                                  ║
╚══════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Detailed Element Specifications

### 4.1 Header Section

| Element | Type | Position (x, y) | Size (w × h) | Properties |
|---------|------|-----------------|--------------|------------|
| Company Logo | Image | 30, 28 | 64 × 64 | PNG with transparency; Taj Hotels logo |
| Dashboard Title | Text Box | 110, 25 | 600 × 35 | "HOSPITALITY ANALYTICS DASHBOARD" — Segoe UI 28pt Bold White |
| Subtitle Line 1 | Text Box | 110, 62 | 400 × 20 | "Performance Intelligence Suite" — Segoe UI 14pt #B0BEC5 |
| Subtitle Line 2 | Text Box | 110, 82 | 300 × 18 | "Taj Hotels India" — Segoe UI 12pt #78909C |
| Last Refresh | Card (measure) | 1650, 35 | 240 × 50 | [Last Refresh] measure — Segoe UI 10pt #B0BEC5 |
| Gold Divider | Shape (Line) | 30, 115 | 1860 × 2 | Color: #C4A265, 2px solid |

### 4.2 Navigation Section

| Button # | Label | Icon (Unicode) | Position (x, y) | Size (w × h) | Action |
|----------|-------|----------------|-----------------|--------------|--------|
| NAV-1 | Executive Summary | 📊 (U+1F4CA) | 80, 130 | 270 × 70 | Page navigation → "Executive Summary" |
| NAV-2 | Revenue & Trends | 📈 (U+1F4C8) | 370, 130 | 270 × 70 | Page navigation → "Revenue & Trends" |
| NAV-3 | Hotel Manager | 🏨 (U+1F3E8) | 660, 130 | 270 × 70 | Page navigation → "Hotel Manager View" |
| NAV-4 | Channel Analysis | 🌐 (U+1F310) | 950, 130 | 270 × 70 | Page navigation → "Channel Analysis" |
| NAV-5 | Guest Ratings | ⭐ (U+2B50) | 1240, 130 | 270 × 70 | Page navigation → "Guest Satisfaction" |
| NAV-6 | Member vs Non | 👥 (U+1F465) | 1530, 130 | 270 × 70 | Page navigation → "Member vs Non-Member" |

**Button Styling:**
```
Default State:
  Fill: rgba(255, 255, 255, 0.08)
  Border: 1px solid rgba(255, 255, 255, 0.2)
  Border-radius: 8px
  Text: White, centered

Hover State:
  Fill: rgba(196, 162, 101, 0.3) — Gold tint
  Border: 1px solid #C4A265
  Text: White
  Shadow: 0 4px 12px rgba(0,0,0,0.3)

Pressed State:
  Fill: #C4A265
  Text: #0D1B2A (dark)
```

### 4.3 KPI Cards Section

| Card # | KPI | Measure | Position (x, y) | Size (w × h) | Format | Subtitle |
|--------|-----|---------|-----------------|--------------|--------|----------|
| KPI-1 | Total Revenue | [Total Revenue] | 50, 220 | 320 × 140 | ₹#,##0,,"B" | [Revenue Trend Icon] |
| KPI-2 | Successful Bookings | [Successful Bookings] | 390, 220 | 320 × 140 | #,##0 | "Paid bookings" |
| KPI-3 | ADR | [ADR] | 730, 220 | 320 × 140 | ₹#,##0 | "Per room-night" |
| KPI-4 | Net Profit | [Net Profit] | 50, 370 | 320 × 140 | ₹#,##0,,"B" | [Profit Margin %] & "% margin" |
| KPI-5 | GSS | [GSS] | 390, 370 | 320 × 140 | 0.00 | [GSS Band] |

**KPI Card Styling:**

```
Card Container:
  Background: #FFFFFF
  Border-radius: 12px
  Shadow: 0 4px 20px rgba(0, 0, 0, 0.15)
  Top border: 4px solid #C4A265 (gold accent)
  Padding: 20px

Card Interior Layout:
  ┌─────────────────────────────┐
  │ ▔▔▔▔ (4px Gold top border) │
  │                             │
  │  [Icon]  KPI LABEL          │  ← 10pt Segoe UI, #6C757D
  │                             │
  │     ₹5.72B                  │  ← 24pt Calibri Bold, #1B365D
  │                             │
  │     ▲ 12.3% YoY            │  ← 11pt Calibri, Green/Red
  │                             │
  └─────────────────────────────┘
```

**Conditional Formatting on KPI Cards:**

| Card | Condition | Value Color | Subtitle Color |
|------|-----------|-------------|----------------|
| Revenue | YoY > 0 | #1B365D (Navy) | #2E8B57 (Green) |
| Revenue | YoY < 0 | #1B365D (Navy) | #DC3545 (Red) |
| Revenue | YoY = 0 | #1B365D (Navy) | #6C757D (Gray) |
| Profit | Margin > 40% | #1B365D | #2E8B57 |
| Profit | Margin 20–40% | #1B365D | #FFC107 (Amber) |
| Profit | Margin < 20% | #DC3545 | #DC3545 |
| GSS | Score ≥ 4.0 | #2E8B57 | #2E8B57 |
| GSS | Score 3.0–3.99 | #FFC107 | #FFC107 |
| GSS | Score < 3.0 | #DC3545 | #DC3545 |

### 4.4 Global Slicers Panel

| Slicer | Field | Position (x, y) | Size (w × h) | Style |
|--------|-------|-----------------|--------------|-------|
| Year | dim_Date[Year] | 1120, 230 | 200 × 45 | Dropdown, dark theme |
| Hotel | dim_Hotel[Hotel_Name] | 1120, 285 | 200 × 45 | Dropdown, dark theme |
| Category | dim_Hotel[Category] | 1120, 340 | 200 × 45 | Horizontal tiles (4) |

**Slicer Dark Theme Styling:**
```
Background: rgba(255, 255, 255, 0.05)
Border: 1px solid rgba(255, 255, 255, 0.15)
Border-radius: 6px
Font: Segoe UI 10pt
Text color: #FFFFFF
Dropdown arrow: #C4A265
Selected item highlight: #C4A265 background, #0D1B2A text
Header: ON, White, 10pt SemiBold
```

### 4.5 Executive Summary Charts

#### Revenue Trend (Area Chart)

| Property | Value |
|----------|-------|
| Position | x: 50, y: 530 |
| Size | 880 × 260 |
| X-Axis | dim_Date[Year] |
| Y-Axis | [Total Revenue] |
| Area Fill | Linear gradient: #C4A265 (top, 40% opacity) → transparent (bottom) |
| Line Color | #FFFFFF (2px) |
| Data Points | ON, White circles, 6px |
| Data Labels | ON, White, Calibri 11pt, above points |
| X-Axis Labels | White, Segoe UI 10pt |
| Y-Axis Labels | White, Calibri 10pt, format ₹#,##0,,"B" |
| Gridlines | Horizontal only, rgba(255,255,255,0.1), dashed |
| Background | Transparent |
| Title | "Revenue Trend" — White, Segoe UI 12pt SemiBold |
| Tooltip | [Total Revenue], [YoY Revenue Growth %], [Net Profit] |

#### Revenue by Hotel (Horizontal Bar Chart)

| Property | Value |
|----------|-------|
| Position | x: 960, y: 530 |
| Size | 880 × 260 |
| Y-Axis | dim_Hotel[Hotel_Name] |
| X-Axis | [Total Revenue] |
| Bar Color | Gradient from #C4A265 (longest) to rgba(196,162,101,0.5) (shortest) |
| Data Labels | ON, White, Calibri 11pt, end of bar, format ₹#,##0,,"M" |
| Sort | Descending by [Total Revenue] |
| Category Labels | White, Segoe UI 10pt |
| Gridlines | None |
| Background | Transparent |
| Title | "Revenue by Property" — White, Segoe UI 12pt SemiBold |
| Tooltip | [Total Revenue], [Hotel Revenue Rank], [GSS], [Successful Bookings] |

#### Category Split (Donut Chart)

| Property | Value |
|----------|-------|
| Position | x: 50, y: 810 |
| Size | 880 × 220 |
| Legend | dim_Hotel[Category] |
| Values | [Total Revenue] |
| Colors | Luxury=#C4A265, Heritage=#8B0000, Business=#1B365D, Resort=#17A2B8 |
| Inner Radius | 60% |
| Detail Labels | Category + percentage, White, 10pt |
| Center Text | Use custom visual or leave empty |
| Background | Transparent |
| Title | "Revenue by Category" — White, Segoe UI 12pt SemiBold |
| Legend Position | Right, White text |

#### Quick Stats Panel

| Property | Value |
|----------|-------|
| Position | x: 960, y: 810 |
| Size | 880 × 220 |
| Type | Multi-row Card or Text Box with measures |
| Background | rgba(255, 255, 255, 0.05), border-radius 12px |
| Border | 1px solid rgba(255, 255, 255, 0.1) |

**Stats Content (use Text Box or custom visual):**

| Icon | Stat | Value/Measure |
|------|------|---------------|
| 🏨 | Hotels | "7 Properties" (static text) |
| 🌍 | Guest Countries | "10 Countries" (static text) |
| 📅 | Data Period | "2021 – 2025" (static text) |
| 👤 | Unique Guests | [Unique Guests] (measure) |
| 💳 | Payment Methods | "5 Methods" (static text) |
| 📱 | Mobile Share | "62% Mobile" (or [Mobile %] measure) |
| 🏷️ | Member Share | [Member Booking %] (measure) |
| ⚡ | Success Rate | "90.1% Paid" (static or measure) |

### 4.6 Footer Section

| Element | Position (x, y) | Content | Font |
|---------|-----------------|---------|------|
| Copyright | 50, 1050 | "© 2026 Taj Hotels India | Hospitality Analytics v1.0" | Segoe UI 9pt #78909C |
| Data Status | 800, 1050 | [Last Refresh] measure | Segoe UI 9pt #78909C |
| Internal Note | 1400, 1050 | "Internal Use Only | bi.team@tajhotels.com" | Segoe UI 9pt #78909C |

---

## 5. Recommended Icons (Power BI Native)

For navigation buttons, use these alternatives if emoji rendering is inconsistent:

| Button | Unicode Emoji | Segoe MDL2 Alternative | Webdings Alt |
|--------|--------------|------------------------|--------------|
| Executive Summary | 📊 | \uE9F9 (BarChart) | — |
| Revenue & Trends | 📈 | \uE9FD (LineChart) | — |
| Hotel Manager | 🏨 | \uE825 (Home) | — |
| Channel Analysis | 🌐 | \uE774 (Globe) | — |
| Guest Ratings | ⭐ | \uE735 (FavoriteStar) | — |
| Member vs Non | 👥 | \uE77B (People) | — |

**Implementation tip:** Use Shape (rounded rectangle) + Text (icon character + label) layered together for each button. Group them for easy positioning.

---

## 6. Power BI Implementation Steps

### Step 1: Create Home Page

1. In Power BI Desktop, click **+** to add a new page
2. Rename to "Home"
3. Drag it to the leftmost position (first page)
4. Format → Canvas settings → Custom: 1920 × 1080

### Step 2: Set Dark Background

1. Format → Canvas background
2. Color: #0D1B2A
3. Transparency: 0%

### Step 3: Add Header

1. Insert → **Image** → Upload Taj logo (64×64 PNG)
2. Position: x=30, y=28
3. Insert → **Text box** → Type title "HOSPITALITY ANALYTICS DASHBOARD"
4. Format text: Segoe UI, 28pt, Bold, White
5. Insert → **Text box** → Subtitle lines
6. Insert → **Shape** (Line) → Gold (#C4A265), width 1860, y=115

### Step 4: Build Navigation Buttons

For each of the 6 navigation buttons:
1. Insert → **Buttons** → Blank
2. Set size: 270 × 70
3. Format → Style:
   - Default fill: Custom color with transparency
   - Border: ON, color rgba white
   - Border radius: 8
4. Format → Text:
   - Text: "[Icon] Button Label"
   - Font: Segoe UI 12pt SemiBold
   - Color: White
   - Alignment: Center
5. Format → Action:
   - Type: Page navigation
   - Destination: [select target page]
6. Position according to layout coordinates

### Step 5: Add KPI Cards

For each of the 5 KPI cards:
1. Insert visual → **Card**
2. Drag measure to Values well
3. Format:
   - Background: White
   - Border: ON, rounded, 12px radius
   - Shadow: ON
   - Callout value: Calibri 24pt Bold, #1B365D
   - Category label: Segoe UI 10pt, #6C757D
4. Add a **Shape** (thin rectangle) on top: 4px height, Gold (#C4A265), same width as card
5. Position per specification

### Step 6: Add Slicers (Dark Theme)

1. Add Year dropdown slicer: dim_Date[Year]
2. Add Hotel dropdown slicer: dim_Hotel[Hotel_Name]
3. Add Category button slicer: dim_Hotel[Category]
4. Format each:
   - Background: transparent/dark
   - Font color: White
   - Border: subtle white
   - Selection: Gold highlight

### Step 7: Add Executive Summary Charts

1. Add Area Chart (Revenue Trend):
   - X: dim_Date[Year], Y: [Total Revenue]
   - Format with transparent background, white text/lines
2. Add Horizontal Bar Chart (Revenue by Hotel):
   - Y: Hotel_Name, X: [Total Revenue]
   - Sort descending, gold bars
3. Add Donut Chart (Category):
   - Legend: Category, Values: [Total Revenue]
   - Custom colors per category
4. Add Quick Stats panel (Multi-row card or text boxes)

### Step 8: Add Footer

1. Insert → Text box
2. Add copyright, data refresh, and contact info
3. Format: Segoe UI 9pt, #78909C
4. Position at bottom

### Step 9: Configure Page as Landing

In Power BI App settings:
- Navigation → Set "Home" as the first page
- Or: Hide "Home" from navigation and set as default on app open

---

## 7. Mobile-Friendly Considerations

### 7.1 Mobile Layout

Power BI allows separate mobile layouts. For the Home page:

1. View → **Mobile layout**
2. Arrange elements vertically:

```
Mobile Layout (Phone: 360 × 640 logical)
┌────────────────────────────┐
│ Logo + Title (compact)     │  h: 60
├────────────────────────────┤
│ 2 KPI cards per row        │  h: 160
│ [Revenue] [Bookings]       │
│ [ADR]     [Profit]         │
│ [GSS]     [—empty—]        │
├────────────────────────────┤
│ Revenue Trend (full width) │  h: 200
├────────────────────────────┤
│ Navigation (3×2 grid)      │  h: 140
│ [Summary][Revenue][Manager]│
│ [Channel][Ratings][Member] │
├────────────────────────────┤
│ Year Slicer                │  h: 40
├────────────────────────────┤
│ Footer                     │  h: 30
└────────────────────────────┘
```

### 7.2 Mobile Best Practices

| Guideline | Implementation |
|-----------|---------------|
| Prioritize KPIs over charts | KPI cards at top of mobile layout |
| Use fewer visuals | Max 4-5 per mobile page |
| Larger touch targets | Buttons min 44×44px tap area |
| Avoid hover-dependent features | Tooltips not accessible on mobile |
| Test on actual phone | Power BI mobile app (iOS/Android) |
| Navigation simplified | Stack buttons vertically or use 2-column grid |

### 7.3 Responsive Design Notes

- All charts on dark backgrounds use White text (passes contrast on mobile)
- KPI card numbers are 24pt+ (readable on small screens)
- Navigation buttons have sufficient padding for finger taps
- Donut chart legends may need repositioning on mobile (below chart)

---

## 8. Interaction Rules (Home Page)

| Source Visual | Target Visual | Interaction |
|--------------|---------------|-------------|
| Revenue Trend (Area) | All KPI cards | No interaction (cards should be stable) |
| Revenue Trend (Area) | Hotel Bar, Category Donut | Highlight |
| Hotel Bar Chart | All visuals | Filter |
| Category Donut | All visuals | Highlight |
| Slicers (Year/Hotel/Category) | All visuals | Filter |
| Navigation Buttons | — | Page navigation (no filtering) |

---

## 9. DAX Measures Needed for Home Page

| Measure | Already Exists? | Notes |
|---------|-----------------|-------|
| [Total Revenue] | ✓ M01 | Card KPI-1 |
| [Successful Bookings] | ✓ M13 | Card KPI-2 |
| [ADR] | ✓ M05 | Card KPI-3 |
| [Net Profit] | ✓ M03 | Card KPI-4 |
| [GSS] | ✓ M21 | Card KPI-5 |
| [Revenue Trend Icon] | ✓ M48 | Subtitle on Revenue card |
| [Profit Margin %] | ✓ M04 | Subtitle on Profit card |
| [GSS Band] | ✓ M49 | Subtitle on GSS card |
| [Last Refresh] | ✓ M51 | Header + Footer |
| [Member Booking %] | ✓ M38 | Quick Stats panel |
| [Unique Guests] | ✓ M18 | Quick Stats panel |
| [Hotel Revenue Rank] | ✓ M39 | Tooltip on Hotel bar |

All measures already defined in Phase 3. No new DAX required.

---

## 10. Accessibility Compliance

| Requirement | Implementation |
|-------------|---------------|
| Color contrast (WCAG AA) | White text on dark background = 14.5:1 ratio (passes) |
| Alt text on images | Add alt text to logo: "Taj Hotels Company Logo" |
| Tab order | Set logical tab order: KPIs → Charts → Slicers → Navigation |
| Screen reader titles | All visuals have descriptive titles enabled |
| Keyboard navigation | All buttons accessible via Tab + Enter |
| No color-only indicators | Trend icons (▲/▼) supplement color coding |
