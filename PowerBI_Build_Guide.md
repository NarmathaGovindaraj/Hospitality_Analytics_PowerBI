# Power BI Desktop Build Guide
## Step-by-Step Implementation Instructions

---

## Prerequisites

- Power BI Desktop (latest version, June 2024+)
- Power BI Pro license (for publishing)
- Source data files in a local folder (e.g., `C:\Data\Hospitality_Datasets\`)
- 30–60 minutes build time

---

## Step 1: Initial Setup

### 1.1 Configure Options

1. Open Power BI Desktop
2. File → Options and settings → Options
3. **Global → Data Load:**
   - Uncheck "Auto date/time for new files"
   - Uncheck "Auto date/time"
4. **Global → Regional Settings:**
   - Set locale to "English (India)" for ₹ currency formatting
5. Click OK

### 1.2 Set Canvas Size

1. View tab → Page view → Actual size
2. File → Options → Report settings → (or Format pane later)
3. For each page: Format → Canvas settings → Custom: 1920 × 1080

---

## Step 2: Load Data (Power Query)

### 2.1 Load fct_Bookings

1. Home → Get Data → Text/CSV
2. Browse to `Hospitality_Data_Global_fct.csv`
3. Click **Transform Data** (opens Power Query Editor)
4. In Advanced Editor, paste the complete M code from `Power_Query_Transformations.md` Section 1
5. Rename query to: `fct_Bookings`

### 2.2 Load dim_Finance

1. In Power Query: Home → New Source → Text/CSV
2. Browse to `Finance_dim.csv`
3. Open Advanced Editor, paste M code from Section 2
4. Rename to: `dim_Finance`

### 2.3 Load dim_Feedback

1. Home → New Source → Text/CSV → `Feedback_dim.csv`
2. Advanced Editor → paste M code from Section 3
3. Rename to: `dim_Feedback`

### 2.4 Load dim_OnlineBooking

1. Home → New Source → Text/CSV → `OnlineBooking_dim.csv`
2. **Important:** In Advanced Editor, note encoding = 1252 (already in code)
3. Paste M code from Section 4
4. Rename to: `dim_OnlineBooking`

### 2.5 Load dim_Hotel

1. Home → New Source → Excel Workbook → `Hotel_List_dim.xlsx`
2. Select Sheet1 → Transform Data
3. Advanced Editor → paste M code from Section 5
4. Rename to: `dim_Hotel`

### 2.6 Load dim_Manager

1. Home → New Source → Excel Workbook → `Manager_Details_dim.xlsx`
2. Select Sheet1 → Transform Data
3. Advanced Editor → paste M code from Section 6
4. Rename to: `dim_Manager`

### 2.7 Create dim_Date

1. Home → New Source → Blank Query
2. Advanced Editor → paste M code from Section 7
3. Rename to: `dim_Date`

### 2.8 Apply Changes

1. Verify all 7 queries show green checkmarks (no errors)
2. Home → **Close & Apply**
3. Wait for data load to complete (~30 seconds for 100K records)

---

## Step 3: Build Data Model

### 3.1 Switch to Model View

Click the Model icon (3rd icon) on the left sidebar.

### 3.2 Create Relationships

Drag and drop to create each relationship:

| # | Drag From | Drop To | Then Configure |
|---|-----------|---------|----------------|
| R1 | fct_Bookings[CheckInDate] | dim_Date[Date] | Many:1, Single direction |
| R2 | fct_Bookings[HotelName] | dim_Hotel[Hotel_Name] | Many:1, Single direction |
| R3 | dim_Hotel[Hotel_Name] | dim_Manager[HotelName] | 1:1, **Both directions** |
| R4 | fct_Bookings[BookingID] | dim_Finance[BookingID] | 1:1, Both directions |
| R5 | fct_Bookings[BookingID] | dim_Feedback[BookingID] | 1:1, Both directions |
| R6 | fct_Bookings[BookingID] | dim_OnlineBooking[BookingID] | 1:1, Both directions |

**For R3 specifically:**
- Double-click the relationship line
- Set Cross filter direction: **Both**
- This is critical for RLS propagation

### 3.3 Mark Date Table

1. Click on dim_Date table in the model
2. Table tools tab → **Mark as date table**
3. Select column: **Date**
4. Click OK

### 3.4 Configure Sort-By Columns

1. Switch to Data View (table icon)
2. Select dim_Date table
3. Click MonthName column → Column tools → Sort by column → **Month**
4. Click MonthShort column → Sort by column → **Month**
5. Click WeekDayName column → Sort by column → **WeekDay**
6. Click QuarterLabel column → Sort by column → **Quarter**
7. Click MonthYear column → Sort by column → **Date**

### 3.5 Hide Technical Columns

In Model View, right-click and "Hide in report view":
- fct_Bookings: FirstName, LastName, IsPaid
- dim_Finance: CustomerID
- dim_Feedback: CustomerID
- dim_OnlineBooking: CustomerID, GuestEmail
- dim_Date: (none — all useful for slicers)

---

## Step 4: Create Measures

### 4.1 Create Measures Table

1. Modeling tab → New Table
2. Enter: `_Measures = ROW("Placeholder", BLANK())`
3. Press Enter
4. In Model View, hide the "Placeholder" column

### 4.2 Add All Measures

1. Click on _Measures table
2. Modeling tab → New Measure
3. Enter each measure from `DAX_Measures.md` one by one
4. After each measure, set the format:
   - Home tab → Format dropdown (or Measure tools → Format)

**Recommended order:**
1. First create: M01 (Total Revenue), M12 (Total Bookings), M13 (Successful Bookings)
2. Then: M02–M11 (other financial)
3. Then: M14–M20 (booking)
4. Then: M21–M26 (satisfaction)
5. Then: M27–M35 (time intelligence)
6. Then: M36–M47 (comparative, variance)
7. Finally: M48–M51 (display)

### 4.3 Organize into Display Folders

1. Select multiple measures (Ctrl+click)
2. Properties → Display folder:
   - `Revenue & Finance`: M01–M11
   - `Bookings`: M12–M20
   - `Satisfaction`: M21–M26
   - `Time Intelligence`: M27–M35
   - `Comparisons`: M36–M47
   - `Display`: M48–M51

---

## Step 5: Apply Theme & Formatting

### 5.1 Create Custom Theme

1. View tab → Themes → Customize current theme
2. Set colors:
   - Color 1 (Primary): #1B365D
   - Color 2: #C4A265
   - Color 3: #2E8B57
   - Color 4: #DC3545
   - Color 5: #6C757D
   - Color 6: #17A2B8
   - Color 7: #FFC107
   - Color 8: #8B0000
3. Set fonts:
   - Text: Segoe UI
   - Numbers: Calibri (set in individual visuals)
4. Save theme as: `Hospitality_Theme.json`

### 5.2 Canvas Settings

For each page:
1. Format pane → Canvas settings
2. Type: Custom
3. Width: 1920, Height: 1080
4. Canvas background: White (#FFFFFF)

---

## Step 6: Build Report Pages

### 6.1 Create Pages

1. Right-click page tab → Rename → "Executive Summary"
2. Click + to add pages:
   - "Revenue & Trends"
   - "Hotel Manager View"
   - "Channel Analysis"
   - "Guest Satisfaction"
   - "Member vs Non-Member"
   - "Hotel Drillthrough" (will hide later)

### 6.2 Build Each Page

Follow the exact specifications in `Dashboard_Page_Design.md`.

For each visual:
1. Click on empty canvas area
2. Visualizations pane → Select visual type
3. Drag fields into correct wells (Axis, Values, Legend, Tooltips)
4. Format pane → Set title, colors, data labels, etc.
5. Position and size using Format → Properties → Size & Position

### 6.3 Add Navigation Bar

On EVERY page:
1. Insert → Buttons → Blank (create 6 buttons)
2. Position: Top, spanning full width, y=0, h=40
3. For each button:
   - Text: Page name
   - Action: Page navigation → select target page
   - Default state: White background, Navy text
   - On hover: Light gray background
4. For active page button: Navy background, White text

### 6.4 Configure Drill-Through

**Hotel Drillthrough page:**
1. Select "Hotel Drillthrough" page
2. Add drill-through field: dim_Hotel[Hotel_Name] → drag to "Add drill-through fields here"
3. Build visuals per spec in Dashboard_Page_Design.md
4. Add Back button: Insert → Buttons → Back
5. Hide page: Right-click page tab → Hide page

---

## Step 7: Slicers & Interactions

### 7.1 Add Slicers

Follow the slicer strategy from Functional Specification Section 6.

### 7.2 Sync Slicers

1. View tab → **Sync slicers** pane
2. For Year slicer: Check sync + visible on all 6 pages
3. For Hotel slicer: Sync on pages 1,2,4,5,6; visible on same

### 7.3 Edit Interactions

On each page:
1. Select a visual
2. Format tab → **Edit interactions**
3. For each other visual, choose Filter / Highlight / None per the spec
4. Repeat for all visuals on the page

---

## Step 8: Conditional Formatting

### 8.1 KPI Card Colors

For GSS Card:
1. Select card → Format → Callout value → Color → **fx** (conditional formatting)
2. Rules:
   - If value >= 4.0 → Green (#2E8B57)
   - If value >= 3.0 AND < 4.0 → Amber (#FFC107)
   - If value < 3.0 → Red (#DC3545)

### 8.2 Matrix Heatmap

For Hotel×Channel Matrix:
1. Select matrix → Format → Cell elements → Background color → **fx**
2. Format by: Color scale
3. Minimum: White, Maximum: Navy (#1B365D)

---

## Step 9: Implement RLS

Follow `RLS_Implementation.md` exactly:
1. Modeling → Manage Roles → Create "HotelManager"
2. Table: dim_Manager, Filter: `[ManagerEmail] = USERPRINCIPALNAME()`
3. Test with View As for each manager

---

## Step 10: Bookmarks & Final Polish

### 10.1 Create Bookmarks

1. View tab → **Bookmarks** pane → Add:
   - "Reset All" — clear all slicers
   - "Current Year" — Year = 2024
   - "Luxury Focus" — Category = Luxury

### 10.2 Add Tooltips

1. Enable custom tooltips on Revenue line chart:
   - Format → Tooltip → Page → Select tooltip page (if created)

### 10.3 Final Checks

- [ ] All pages have navigation bar
- [ ] All numbers formatted correctly (₹, %, decimals)
- [ ] Slicers synced across pages
- [ ] Bar charts sorted descending
- [ ] Drill-through works from Pages 1,2,4,5 to Hotel Drillthrough
- [ ] Back button on drillthrough page works
- [ ] RLS tested for all 7 managers + executive (no role)
- [ ] No visual errors or blank pages
- [ ] Report title and footer on each page

---

## Step 11: Save & Publish

### 11.1 Save

1. File → Save As → `Hospitality_Analytics_Dashboard.pbix`
2. File size should be ~15-25 MB

### 11.2 Publish

1. Home → **Publish**
2. Select destination workspace
3. Wait for "Success" confirmation
4. Click "Open in Power BI" to verify

### 11.3 Configure in Service

1. Go to workspace → Dataset → Settings
2. Scheduled refresh → Configure (if applicable)
3. Dataset → Security → Add manager emails to "HotelManager" role
4. Verify RLS in Service using "Test as role"

---

## Performance Optimization Checklist

| # | Action | Impact | Done |
|---|--------|--------|------|
| 1 | Disable Auto date/time | Prevents hidden tables | |
| 2 | Use Import mode (not DirectQuery) | Full in-memory speed | |
| 3 | Pre-calculate NetProfit in Power Query | Avoids runtime SUMX | |
| 4 | Pre-calculate GSS in Power Query | Avoids runtime averaging | |
| 5 | Remove GuestEmail (or mask it) | Reduces high-cardinality | |
| 6 | Hide unused columns | Cleaner field list | |
| 7 | Limit visuals per page to 8-10 | Faster rendering | |
| 8 | Use DIVIDE() not / operator | Safe division | |
| 9 | Use VAR in complex measures | Avoid re-evaluation | |
| 10 | Set integer types where possible | Better compression | |
