# Booking Drillthrough Page Design
## Complete Implementation-Ready Specification

---

## 1. Page Properties

| Property | Value |
|----------|-------|
| Page Name | Booking Detail |
| Page Visibility | **Hidden** (not in navigation or tab strip) |
| Canvas Size | 1920 × 1080 px |
| Background Color | #F5F6FA |
| Drill-Through Filter Field | fct_Bookings[BookingID] |
| Trigger | Right-click a BookingID in any table/matrix visual → "Drill through → Booking Detail" |
| Purpose | Full detail view of a single booking transaction for auditing or investigation |

---

## 2. Drill-Through Configuration

### 2.1 Filter Field Setup

1. Navigate to "Booking Detail" page
2. Drag **fct_Bookings[BookingID]** into "Add drill-through fields here"
3. Set "Keep all filters" = **ON**

### 2.2 Source Pages

| Source Page | Triggering Visual | Notes |
|-------------|-------------------|-------|
| Hotel Manager View | Feedback Table (if BookingID added as a column) | Managers investigate specific bookings |
| Hotel Drillthrough | Feedback Table (if BookingID column added) | Deep-dive from hotel detail |

**Note:** This drill-through is optional and used for detailed auditing. It requires BookingID to be visible in a table visual on the source page. If no source tables expose BookingID, this page can be accessed via a dedicated "Booking Lookup" slicer on this page itself (see Section 4.3).

---

## 3. Wireframe (1920 × 1080)

```
┌══════════════════════════════════════════════════════════════════════════════════════┐
║ y:0  HEADER BAR (h:70) Background: #1B365D                                          ║
║                                                                                      ║
║  [← Back]   "Booking #43421"          "Taj Mahal Palace | Mumbai | Luxury"          ║
║             Segoe UI 22pt Bold White    Segoe UI 12pt #B0BEC5                        ║
║                                                                                      ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:75  BOOKING LOOKUP (h:50) — Optional search slicer                                 ║
║                                                                                      ║
║  "Booking ID:" [___________▼]  (Slicer: fct_Bookings[BookingID], dropdown+search)   ║
║                                                                                      ║
╠═══════════════════════════════════╦══════════════════════════════════════════════════╣
║ y:130  SECTION 1: GUEST INFO      ║  SECTION 2: STAY DETAILS                        ║
║ (h:220)                           ║  (h:220)                                         ║
║                                   ║                                                  ║
║ ┌────────────────────────────┐    ║ ┌────────────────────────────────────────────┐  ║
║ │ GUEST INFORMATION          │    ║ │ STAY DETAILS                               │  ║
║ │ (Multi-Row Card)           │    ║ │ (Multi-Row Card)                           │  ║
║ │                            │    ║ │                                            │  ║
║ │ Guest Name: Tanya Verma    │    ║ │ Hotel: Taj Mahal Palace                    │  ║
║ │ Gender: Female             │    ║ │ City: Mumbai                               │  ║
║ │ Age: 33                    │    ║ │ Category: Luxury                           │  ║
║ │ Country: India             │    ║ │ Room Type: Standard                        │  ║
║ │ Member Type: Non-Member    │    ║ │ Check-In: 21-Jan-2023                      │  ║
║ │ Customer ID: CUST00001     │    ║ │ Check-Out: 23-Jan-2023                     │  ║
║ │                            │    ║ │ Nights Stayed: 2                           │  ║
║ │                            │    ║ │ Rate Per Night: ₹24,020                    │  ║
║ │ x:20, y:130, w:920, h:220 │    ║ │ Room Revenue: ₹48,040                     │  ║
║ └────────────────────────────┘    ║ │                                            │  ║
║                                   ║ │ x:960, y:130, w:940, h:220                 │  ║
║                                   ║ └────────────────────────────────────────────┘  ║
╠═══════════════════════════════════╬══════════════════════════════════════════════════╣
║ y:360  SECTION 3: FINANCIALS      ║  SECTION 4: BOOKING & CHANNEL                   ║
║ (h:220)                           ║  (h:220)                                         ║
║                                   ║                                                  ║
║ ┌────────────────────────────┐    ║ ┌────────────────────────────────────────────┐  ║
║ │ FINANCIAL DETAILS          │    ║ │ BOOKING & CHANNEL INFO                     │  ║
║ │ (Multi-Row Card)           │    ║ │ (Multi-Row Card)                           │  ║
║ │                            │    ║ │                                            │  ║
║ │ Total Revenue: ₹53,469     │    ║ │ Channel: Website                           │  ║
║ │ Cost: ₹21,459              │    ║ │ Device Type: Mobile                        │  ║
║ │ Tax Amount: ₹5,729         │    ║ │ Payment Method: Credit Card                │  ║
║ │ Discount %: 0%             │    ║ │ Payment Status: Paid ✓                     │  ║
║ │ Discount Given: ₹0         │    ║ │ Booking Date: 15-Dec-2022                  │  ║
║ │ Add. Service Rev: ₹500     │    ║ │ Lead Time: 37 days                         │  ║
║ │ Add. Service Cost: ₹800    │    ║ │                                            │  ║
║ │ Net Profit: ₹31,510        │    ║ │                                            │  ║
║ │ Profit Margin: 58.9%       │    ║ │                                            │  ║
║ │                            │    ║ │                                            │  ║
║ │ x:20, y:360, w:920, h:220 │    ║ │ x:960, y:360, w:940, h:220                │  ║
║ └────────────────────────────┘    ║ └────────────────────────────────────────────┘  ║
╠═══════════════════════════════════╩══════════════════════════════════════════════════╣
║ y:590  SECTION 5: FEEDBACK (h:180)                                                   ║
║                                                                                      ║
║ ┌────────────────────────────────────────────────────────────────────────────────┐  ║
║ │ GUEST FEEDBACK                                                                 │  ║
║ │ (Multi-Row Card or Table — single row)                                         │  ║
║ │                                                                                │  ║
║ │ Overall Rating: 4.50        Staff Rating: 4.80       Cleanliness: 4.60         │  ║
║ │ GSS Score: 4.63             Feedback: "Good service"  Sentiment: Positive      │  ║
║ │ Submitted Date: 29-Jan-2023                                                    │  ║
║ │                                                                                │  ║
║ │ x:20, y:590, w:1880, h:180                                                    │  ║
║ └────────────────────────────────────────────────────────────────────────────────┘  ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:780  SECTION 6: VISUAL SUMMARY (h:220)                                             ║
║                                                                                      ║
║  ┌──────────────────────┐ ┌──────────────────────┐ ┌──────────────────────────────┐ ║
║  │ PROFIT GAUGE          │ │ RATING GAUGE          │ │ BOOKING TIMELINE             │ ║
║  │ x:20, y:780           │ │ x:660, y:780          │ │ (Card showing key dates)     │ ║
║  │ w:620, h:220          │ │ w:620, h:220          │ │ x:1300, y:780, w:600, h:220  │ ║
║  │                       │ │                       │ │                              │ ║
║  │ Value: NetProfit       │ │ Value: GSS            │ │ Booked: 15-Dec-2022         │ ║
║  │ Min: 0                │ │ Min: 1, Max: 5        │ │ Check-In: 21-Jan-2023       │ ║
║  │ Max: TotalRevenue     │ │ Target: 4.0           │ │ Check-Out: 23-Jan-2023      │ ║
║  │ Target: 40% of Rev    │ │                       │ │ Feedback: 29-Jan-2023       │ ║
║  │                       │ │                       │ │ Lead Time: 37 days          │ ║
║  └──────────────────────┘ └──────────────────────┘ └──────────────────────────────┘ ║
╠══════════════════════════════════════════════════════════════════════════════════════╣
║ y:1010  FOOTER (h:30)                                                                ║
║ [← Back]         [Last Refresh]         Internal Use Only                            ║
╚══════════════════════════════════════════════════════════════════════════════════════╝
```

---

## 4. Element Specifications

### 4.1 Header Bar

| Element | Type | x | y | w | h | Content | Format |
|---------|------|---|---|---|---|---------|--------|
| Background | Rectangle | 0 | 0 | 1920 | 70 | — | Fill: #1B365D |
| Back Button | Button (Back) | 20 | 15 | 100 | 40 | "← Back" | White text, transparent, border |
| Booking ID | Card | 140 | 10 | 400 | 30 | fct_Bookings[BookingID] (from context) | Segoe UI 22pt Bold White, prefix "Booking #" |
| Hotel + Location | Card | 140 | 42 | 600 | 20 | Concatenation of Hotel + City + Category | Segoe UI 12pt #B0BEC5 |

### 4.2 Alternative: Booking ID Display Using Measure

Since the drill-through context provides BookingID, create a display measure:

```dax
Booking ID Display = 
"Booking #" & FORMAT(SELECTEDVALUE(fct_Bookings[BookingID], 0), "#####")
```

And for hotel context:
```dax
Booking Hotel Context = 
SELECTEDVALUE(fct_Bookings[HotelName], "") & " | " &
SELECTEDVALUE(fct_Bookings[City], "") & " | " &
SELECTEDVALUE(fct_Bookings[Category], "")
```

### 4.3 Booking Lookup Slicer (Optional Standalone Access)

| Property | Value |
|----------|-------|
| Type | Slicer |
| Position | x:120, y:78, w:300, h:40 |
| Field | fct_Bookings[BookingID] |
| Style | Dropdown with search enabled |
| Label | "Booking ID:" (text box beside it) |
| Purpose | Allows direct navigation without drill-through from another page |

### 4.4 Section 1: Guest Information (Multi-Row Card)

| Property | Value |
|----------|-------|
| Type | Multi-Row Card (or Table with single row) |
| Position | x:20, y:130, w:920, h:220 |
| Background | White, border #E8E8E8, radius 8px, shadow |
| Title | "Guest Information" — Segoe UI 13pt SemiBold #1B365D |

**Fields in Values well (displayed as rows):**

| # | Field | Format | Source |
|---|-------|--------|--------|
| 1 | fct_Bookings[GuestName] | Text | "Tanya Verma" |
| 2 | fct_Bookings[Gender] | Text | "Female" |
| 3 | fct_Bookings[Age] | Integer | 33 |
| 4 | fct_Bookings[Country] | Text | "India" |
| 5 | fct_Bookings[MemberType] | Text | "Non-Member" |
| 6 | fct_Bookings[CustomerID] | Text | "CUST00001" |

**Format:**
- Category label: Segoe UI 10pt SemiBold #1B365D (field names)
- Data label: Segoe UI 11pt Regular #333333 (values)
- Row spacing: 28px
- Padding: 16px

### 4.5 Section 2: Stay Details (Multi-Row Card)

| Property | Value |
|----------|-------|
| Type | Multi-Row Card |
| Position | x:960, y:130, w:940, h:220 |
| Background | White, border, radius 8px, shadow |
| Title | "Stay Details" |

**Fields:**

| # | Field | Format |
|---|-------|--------|
| 1 | fct_Bookings[HotelName] | Text |
| 2 | fct_Bookings[City] | Text |
| 3 | fct_Bookings[Category] | Text |
| 4 | fct_Bookings[RoomType] | Text |
| 5 | fct_Bookings[CheckInDate] | DD-MMM-YYYY |
| 6 | fct_Bookings[CheckOutDate] | DD-MMM-YYYY |
| 7 | fct_Bookings[NightsStayed] | #0 |
| 8 | fct_Bookings[RatePerNight] | ₹#,##0 |
| 9 | fct_Bookings[RoomRevenue] | ₹#,##0 |

### 4.6 Section 3: Financial Details (Multi-Row Card)

| Property | Value |
|----------|-------|
| Type | Multi-Row Card |
| Position | x:20, y:360, w:920, h:220 |
| Background | White, border, radius 8px, shadow |
| Title | "Financial Details" |

**Fields:**

| # | Field | Format | Source Table |
|---|-------|--------|-------------|
| 1 | dim_Finance[TotalRevenue] | ₹#,##0.00 | dim_Finance |
| 2 | dim_Finance[Cost] | ₹#,##0 | dim_Finance |
| 3 | dim_Finance[TaxAmount] | ₹#,##0.00 | dim_Finance |
| 4 | dim_Finance[DiscountPercent] | 0"%" | dim_Finance |
| 5 | dim_Finance[DiscountsGiven] | ₹#,##0.00 | dim_Finance |
| 6 | dim_Finance[AdditionalServiceRevenue] | ₹#,##0 | dim_Finance |
| 7 | dim_Finance[AdditionalServiceCost] | ₹#,##0 | dim_Finance |
| 8 | dim_Finance[NetProfit] | ₹#,##0.00 | dim_Finance |
| 9 | dim_Finance[ProfitMarginPct] | 0.0"%" | dim_Finance |

**Conditional formatting:**
- NetProfit: Green (#2E8B57) if positive, Red (#DC3545) if negative
- ProfitMarginPct: Green if >40%, Amber if 20-40%, Red if <20%

### 4.7 Section 4: Booking & Channel Info (Multi-Row Card)

| Property | Value |
|----------|-------|
| Type | Multi-Row Card |
| Position | x:960, y:360, w:940, h:220 |
| Background | White, border, radius 8px, shadow |
| Title | "Booking & Channel Information" |

**Fields:**

| # | Field | Format | Source |
|---|-------|--------|--------|
| 1 | fct_Bookings[Channel] | Text | fct_Bookings |
| 2 | fct_Bookings[DeviceType] | Text | fct_Bookings |
| 3 | fct_Bookings[PaymentMethod] | Text | fct_Bookings |
| 4 | fct_Bookings[PaymentStatus] | Text | fct_Bookings |
| 5 | dim_OnlineBooking[BookingDate] | DD-MMM-YYYY | dim_OnlineBooking |

**Conditional formatting on PaymentStatus:**
- "Paid" → Green text + "✓" suffix
- "Pending" → Amber text + "⏳" suffix
- "Failed" → Red text + "✗" suffix

### 4.8 Section 5: Feedback (Full-Width Card)

| Property | Value |
|----------|-------|
| Type | Multi-Row Card |
| Position | x:20, y:590, w:1880, h:180 |
| Background | White, border, radius 8px, shadow |
| Title | "Guest Feedback" |

**Fields:**

| # | Field | Format | Source |
|---|-------|--------|--------|
| 1 | dim_Feedback[Rating] | 0.00 | dim_Feedback |
| 2 | dim_Feedback[StaffRating] | 0.00 | dim_Feedback |
| 3 | dim_Feedback[CleanlinessRating] | 0.00 | dim_Feedback |
| 4 | dim_Feedback[GuestSatisfactionScore] | 0.00 | dim_Feedback |
| 5 | dim_Feedback[Feedback] | Text | dim_Feedback |
| 6 | dim_Feedback[FeedbackSentiment] | Text | dim_Feedback |
| 7 | dim_Feedback[SubmittedDate] | DD-MMM-YYYY | dim_Feedback |

**Conditional:** Sentiment = "Negative" → Red text; "Positive" → Green text.

### 4.9 Section 6: Visual Summary

**Profit Gauge:**
| Property | Value |
|----------|-------|
| Type | Gauge |
| Position | x:20, y:780, w:620, h:220 |
| Value | dim_Finance[NetProfit] |
| Min | 0 |
| Max | dim_Finance[TotalRevenue] |
| Target | dim_Finance[TotalRevenue] * 0.4 (40% profit target) |
| Title | "Profit vs Revenue" |

**GSS Gauge:**
| Property | Value |
|----------|-------|
| Type | Gauge |
| Position | x:660, y:780, w:620, h:220 |
| Value | dim_Feedback[GuestSatisfactionScore] |
| Min | 1 |
| Max | 5 |
| Target | 4.0 |
| Title | "Guest Satisfaction Score" |
| Callout color | ≥4.0=#2E8B57, 3.0-3.99=#FFC107, <3.0=#DC3545 |

**Booking Timeline (Card):**
| Property | Value |
|----------|-------|
| Type | Multi-Row Card |
| Position | x:1300, y:780, w:600, h:220 |
| Title | "Booking Timeline" |
| Fields | BookingDate, CheckInDate, CheckOutDate, SubmittedDate (feedback), calculated lead time |

---

## 5. DAX Measures / Fields Used

| Section | Fields / Measures |
|---------|-------------------|
| Header | [Booking ID Display], [Booking Hotel Context] (new) |
| Guest Info | GuestName, Gender, Age, Country, MemberType, CustomerID |
| Stay Details | HotelName, City, Category, RoomType, CheckInDate, CheckOutDate, NightsStayed, RatePerNight, RoomRevenue |
| Financial | TotalRevenue, Cost, TaxAmount, DiscountPercent, DiscountsGiven, AddSvcRevenue, AddSvcCost, NetProfit, ProfitMarginPct |
| Booking Info | Channel, DeviceType, PaymentMethod, PaymentStatus, BookingDate |
| Feedback | Rating, StaffRating, CleanlinessRating, GSS, Feedback, Sentiment, SubmittedDate |
| Gauges | NetProfit, TotalRevenue, GSS |

---

## 6. Build Steps

### Step 1: Create Hidden Page
1. Add new page → Rename "Booking Detail"
2. Canvas: 1920×1080, BG: #F5F6FA
3. **Right-click tab → Hide page**

### Step 2: Configure Drill-Through
1. Drag **fct_Bookings[BookingID]** to "Add drill-through fields here"
2. Keep all filters = ON

### Step 3: Header
1. Add rectangle (1920×70) fill #1B365D
2. Insert → Buttons → Back → x:20, y:15, white outline
3. Add Card for [Booking ID Display] → 22pt Bold White, transparent bg
4. Add Card for [Booking Hotel Context] → 12pt #B0BEC5

### Step 4: Optional Lookup Slicer
1. Add slicer → fct_Bookings[BookingID] → Dropdown with search
2. Position x:120, y:78, w:300, h:40
3. This allows standalone access without drill-through

### Step 5: Build Section 1 — Guest Info
1. Visualizations → Multi-Row Card
2. Drag: GuestName, Gender, Age, Country, MemberType, CustomerID
3. Position x:20, y:130, w:920, h:220
4. Format: White bg, title "Guest Information"

### Step 6: Build Section 2 — Stay Details
1. Multi-Row Card → Drag all stay fields
2. Position x:960, y:130, w:940, h:220
3. Format dates as DD-MMM-YYYY, currency as ₹#,##0

### Step 7: Build Section 3 — Financials
1. Multi-Row Card → Drag all Finance fields
2. Position x:20, y:360, w:920, h:220
3. Conditional formatting on NetProfit and ProfitMarginPct

### Step 8: Build Section 4 — Booking Channel
1. Multi-Row Card → Drag Channel, DeviceType, PaymentMethod, PaymentStatus, BookingDate
2. Position x:960, y:360, w:940, h:220
3. Conditional on PaymentStatus

### Step 9: Build Section 5 — Feedback
1. Multi-Row Card → Drag all Feedback fields
2. Position x:20, y:590, w:1880, h:180
3. Conditional on Sentiment

### Step 10: Build Section 6 — Gauges + Timeline
1. Add Profit Gauge (x:20, y:780)
2. Add GSS Gauge (x:660, y:780)
3. Add Timeline card (x:1300, y:780)

### Step 11: Footer
1. Add Back button + [Last Refresh] card at bottom

### Step 12: Test
1. Go to a source page with a table showing BookingID
2. Right-click a BookingID → verify "Drill through → Booking Detail" appears
3. Click → verify all 6 sections populate with correct single-booking data
4. Click Back → returns to source
5. Test BookingID slicer (type an ID, verify data appears)

---

*End of Booking Drillthrough Page Design*
