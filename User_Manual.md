# User Manual
## Hospitality Analytics Dashboard — End-User Guide

---

## Welcome

The Hospitality Analytics Dashboard provides interactive insights into the performance
of our 7 Taj hotel properties across India. This guide will help you navigate the
dashboard and get the most out of the analytics available to you.

---

## 1. Accessing the Dashboard

### Option A: Via Power BI App
1. Open your browser → go to **app.powerbi.com**
2. Sign in with your organizational email
3. Click **Apps** in the left navigation
4. Click **Hospitality Analytics**
5. The Executive Summary page loads first

### Option B: Via Direct Link
Click the link provided by your BI team:
`[App URL provided by administrator]`

### Option C: Via Power BI Mobile
1. Download "Power BI" app from App Store / Google Play
2. Sign in with your organizational email
3. Find "Hospitality Analytics" in your apps
4. Tap to open

---

## 2. Navigation

The dashboard has 6 main pages, accessible via the navigation bar at the top:

| Page | Icon | Purpose | Best For |
|------|------|---------|----------|
| Executive Summary | 📊 | Portfolio overview | Quick status check |
| Revenue & Trends | 📈 | Revenue deep dive | Financial analysis |
| Hotel Manager View | 🏨 | Your property | Managers (RLS) |
| Channel Analysis | 🌐 | Booking sources | Marketing insights |
| Guest Satisfaction | ⭐ | Ratings & feedback | Quality monitoring |
| Member vs Non-Member | 👥 | Loyalty analysis | Strategy decisions |

**To navigate:** Click any button in the top navigation bar.

---

## 3. Using Slicers (Filters)

Slicers appear on the right side of most pages. They filter all visuals on the page.

### How to use slicers:
- **Dropdown slicers (Year, Hotel):** Click the dropdown arrow → select value(s)
- **Button slicers (Category, Channel):** Click one or more buttons (Ctrl+click for multiple)
- **Clear a slicer:** Click the eraser icon (↺) on the slicer header
- **Select All:** Click "Select all" checkbox in dropdown slicers

### Common filter combinations:
| Goal | Set These Slicers |
|------|-------------------|
| See 2024 performance | Year = 2024 |
| Compare luxury hotels | Category = Luxury |
| Focus on one hotel | Hotel = [select hotel] |
| See Q1 only | Quarter = Q1 |

**Tip:** Slicers sync across pages. If you select Year = 2023 on Page 1,
it remains active when you navigate to Page 2.

---

## 4. Interacting with Visuals

### Click on a chart element
- **Bar/column:** Clicking a bar filters other visuals to that selection
- **Donut slice:** Clicking highlights related data in other visuals
- **Line chart point:** Hover for tooltip details

### Right-click for more options
- **Drill through:** Right-click a hotel name → "Drill through → Hotel Detail"
- **Export data:** Right-click → "Export data" to download the underlying data
- **Show as table:** Right-click → "Show as a table" to see raw numbers

### Hover for tooltips
Move your mouse over any data point to see additional details in a popup tooltip.

---

## 5. Understanding the KPIs

### Revenue KPIs
| KPI | What It Means | Good If |
|-----|---------------|---------|
| Total Revenue | Total income from paid bookings | ↑ Increasing year over year |
| Net Profit | Revenue minus all costs and discounts | Positive and growing |
| Profit Margin % | Profit as % of revenue | > 40% |
| ADR | Average revenue per room-night sold | ↑ Increasing (higher rates) |

### Booking KPIs
| KPI | What It Means | Good If |
|-----|---------------|---------|
| Total Bookings | Number of booking transactions | ↑ Growing demand |
| Successful Bookings | Bookings that were paid | Close to Total Bookings |
| Failed Rate % | Payment failures | < 3% |

### Satisfaction KPIs
| KPI | What It Means | Good If |
|-----|---------------|---------|
| GSS | Guest Satisfaction Score (avg of 3 ratings) | ≥ 4.0 |
| Staff Rating | Guest rating of staff service | ≥ 4.0 |
| Cleanliness Rating | Guest rating of hygiene | ≥ 4.0 |
| Positive Feedback % | % of positive reviews | > 70% |

### Growth KPIs
| KPI | What It Means | Good If |
|-----|---------------|---------|
| YoY Growth % | Revenue change vs same period last year | Positive (▲) |
| QoQ Growth % | Revenue change vs previous quarter | Positive |

---

## 6. Page-by-Page Guide

### Page 1: Executive Summary
**What you see:** Top-level KPIs, revenue trend line, hotel comparison, category and channel breakdowns.
**Key actions:**
- Check the 5 KPI cards for quick health check
- Look at Revenue by Year line to spot trends
- Compare hotels in the bar chart (sorted by revenue)
- Use slicers to filter to specific year/category

### Page 2: Revenue & Trends
**What you see:** Detailed revenue analysis with monthly trends and drill-down.
**Key actions:**
- Click on the area chart to drill down: Year → Quarter → Month
- Compare revenue vs cost in the combo chart
- Check profit margin trend (should stay above 30% line)
- Use Room Type slicer to see Suite vs Deluxe vs Standard

### Page 3: Hotel Manager View
**What you see:** YOUR hotel's performance only (filtered automatically by your login).
**Key actions:**
- Review your KPI cards (revenue, bookings, GSS, profit)
- Check monthly revenue trend for seasonal patterns
- Review top feedback issues — act on negative items
- Monitor payment method mix

**Note:** If you see blank data, contact the BI team — your email may not be configured in the security settings.

### Page 4: Channel Analysis
**What you see:** How bookings arrive (Website, App, Agent, ThirdParty).
**Key actions:**
- See which channel dominates (Website = ~45%)
- Check mobile vs desktop split
- Look at channel trend — is App growing?
- Check revenue per channel (not just volume)

### Page 5: Guest Satisfaction
**What you see:** Ratings, feedback categories, satisfaction by hotel and over time.
**Key actions:**
- GSS gauge shows overall score vs 4.0 target
- Feedback bar chart shows most common categories
- Look for Red bars (negative feedback) — these need attention
- Compare hotels — which needs improvement?

### Page 6: Member vs Non-Member
**What you see:** Financial comparison between loyalty members and non-members.
**Key actions:**
- Compare revenue contribution (Member vs Non-Member)
- Check if members get more discounts (they should)
- Compare ADR — are members paying more or less?
- Look at room type preference differences

---

## 7. Drill-Through (Deep Dive)

To see detailed data for a specific hotel:
1. Right-click on any hotel name in a chart
2. Select **Drill through → Hotel Drillthrough**
3. A detailed page opens with that hotel's full metrics
4. Click the **← Back** button (top-left) to return

---

## 8. Exporting Data

### Export a visual to Excel:
1. Hover over the visual
2. Click the **⋯** (more options) icon
3. Select **Export data**
4. Choose format (Excel/CSV)
5. Click **Export**

### Export entire page to PDF/PowerPoint:
1. File → **Export** → PDF or PowerPoint
2. Choose pages to include
3. Download

**Note:** Exports respect your RLS security. Managers only export their hotel's data.

---

## 9. Troubleshooting

| Problem | Solution |
|---------|----------|
| Dashboard shows blank data | Your RLS role may not be configured. Contact BI team. |
| Slicers don't seem to work | Check if another slicer is restricting data. Click "Reset All" bookmark. |
| Numbers look wrong | Check which slicers are active. Clear all filters and compare to expected totals. |
| Page won't load | Refresh your browser (F5). If persists, contact BI team. |
| Can't find the app | Go to Apps → Get Apps → search "Hospitality" |
| Export blocked | Your role may not have export permissions. Contact BI Admin. |

---

## 10. Data Refresh Schedule

| Property | Value |
|----------|-------|
| Refresh frequency | Daily |
| Refresh time | 6:00 AM IST |
| Data coverage | January 2021 – September 2025 |
| Last refresh visible | Bottom-right footer of each page |

**Note:** Data shown is as of the last refresh. If you need real-time data, contact the BI team.

---

## 11. Getting Help

| Type | Contact |
|------|---------|
| Technical issues | bi.team@tajhotels.com |
| Data questions | data.team@tajhotels.com |
| Access requests | it.helpdesk@tajhotels.com |
| Feature requests | project.owner@tajhotels.com |

---

## 12. Glossary

| Term | Definition |
|------|-----------|
| ADR | Average Daily Rate — revenue per room-night |
| GSS | Guest Satisfaction Score — average of 3 ratings |
| RLS | Row-Level Security — restricts data by user login |
| YoY | Year-over-Year comparison |
| QoQ | Quarter-over-Quarter comparison |
| YTD | Year-to-Date (cumulative from Jan 1) |
| FY | Fiscal Year (April–March in India) |
| PII | Personally Identifiable Information (protected data) |
| Slicer | Interactive filter control on the dashboard |
| Drill-through | Navigate to detail page for a specific item |
