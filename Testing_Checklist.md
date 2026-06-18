# Phase 4: Testing & Validation
## Complete Testing Checklist — Hospitality Analytics Dashboard

| Field | Value |
|-------|-------|
| Phase | Phase 4 — Testing & Validation |
| Version | 1.0 |
| Date | June 2026 |
| Tester | [Name] |
| Environment | Power BI Desktop + Power BI Service |

---

## 1. KPI Validation Test Cases

### 1.1 Revenue Measures

| TC# | Test Case | Measure | Expected Value (Full Dataset, All Paid) | Validation Method | Pass/Fail |
|-----|-----------|---------|----------------------------------------|-------------------|-----------|
| TC-001 | Total Revenue matches source | [Total Revenue] | SUM of Finance[TotalRevenue] WHERE PaymentStatus="Paid" | Compare DAX card with Python/Excel calculation on source CSV | |
| TC-002 | Total Revenue excludes Failed | [Total Revenue] | Should NOT include 2,053 Failed bookings | Filter PaymentStatus=Failed, verify revenue = 0 | |
| TC-003 | Total Revenue excludes Pending | [Total Revenue] | Should NOT include 7,856 Pending bookings | Filter PaymentStatus=Pending, verify revenue = 0 | |
| TC-004 | Total Cost matches source | [Total Cost] | SUM of Finance[Cost] for 90,091 Paid bookings | Cross-check with source file | |
| TC-005 | Net Profit formula correct | [Net Profit] | Revenue − Cost − AddSvcCost + AddSvcRev − Discounts | Manually calculate for 5 sample bookings, compare | |
| TC-006 | Profit Margin % calculation | [Profit Margin %] | Net Profit / Total Revenue × 100 | Verify M04 = M03/M01 × 100 | |
| TC-007 | ADR calculation | [ADR] | Total Revenue / SUM(NightsStayed) for Paid | Manual: Revenue ÷ Room Nights | |
| TC-008 | Revenue Per Booking | [Revenue Per Booking] | Total Revenue / Successful Bookings count | Verify M06 = M01/M13 | |
| TC-009 | Total Tax | [Total Tax] | SUM(TaxAmount) for Paid bookings | Source file cross-check | |
| TC-010 | Total Discounts | [Total Discounts] | SUM(DiscountsGiven) for Paid bookings | Source file cross-check | |

### 1.2 Booking Measures

| TC# | Test Case | Measure | Expected Value | Validation Method | Pass/Fail |
|-----|-----------|---------|----------------|-------------------|-----------|
| TC-011 | Total Bookings = 100,000 | [Total Bookings] | 100,000 | COUNTROWS of fact table | |
| TC-012 | Successful Bookings = 90,091 | [Successful Bookings] | 90,091 | Count WHERE Paid | |
| TC-013 | Failed Rate = ~2.05% | [Failed Booking Rate %] | 2053/100000 × 100 = 2.053% | Calculator verification | |
| TC-014 | Pending Rate = ~7.86% | [Pending Booking Rate %] | 7856/100000 × 100 = 7.856% | Calculator verification | |
| TC-015 | Avg Nights Stayed | [Avg Nights Stayed] | AVERAGE(NightsStayed) for Paid | Source cross-check | |
| TC-016 | Total Room Nights | [Total Room Nights] | SUM(NightsStayed) for Paid | Source cross-check | |
| TC-017 | Unique Guests = 200 | [Unique Guests] | 200 (with no filters) | DISTINCTCOUNT(CustomerID) | |
| TC-018 | Repeat Guest Rate | [Repeat Guest Rate %] | All 200 guests have multiple bookings → ~100% | Verify logic | |

### 1.3 Satisfaction Measures

| TC# | Test Case | Measure | Expected Value | Validation Method | Pass/Fail |
|-----|-----------|---------|----------------|-------------------|-----------|
| TC-019 | GSS = avg of 3 ratings | [GSS] | (AvgRating + AvgStaff + AvgClean) / 3 | Verify against source averages | |
| TC-020 | Rating range valid | [Avg Rating] | Between 1.5 and 5.0 | Check min/max | |
| TC-021 | Staff Rating range | [Avg Staff Rating] | Between 1.0 and 5.0 | Check min/max | |
| TC-022 | Cleanliness range | [Avg Cleanliness Rating] | Between 1.0 and 5.0 | Check min/max | |
| TC-023 | Positive + Negative = 100% | [Positive Feedback %] + [Negative Feedback %] | 100% | Sum must equal 100 | |
| TC-024 | Sentiment classification | Feedback counts | 7 positive categories + 3 negative = 10 total | Verify all categories classified | |

### 1.4 Time Intelligence Measures

| TC# | Test Case | Measure | Expected Value | Validation Method | Pass/Fail |
|-----|-----------|---------|----------------|-------------------|-----------|
| TC-025 | Revenue PY for 2023 | [Revenue PY] with Year=2023 | Should equal Total Revenue for 2022 | Compare values | |
| TC-026 | YoY Growth % logic | [YoY Revenue Growth %] | (2023 Rev − 2022 Rev) / 2022 Rev × 100 | Manual calculation | |
| TC-027 | Revenue YTD Jan–Jun | [Revenue YTD] in June | Sum of Jan–Jun revenue | Manual sum | |
| TC-028 | Revenue MTD correct | [Revenue MTD] | Revenue from 1st of month to selected date | Filter and verify | |
| TC-029 | Running Total accumulates | [Running Total Revenue] | Cumulative — monotonically increasing | Visual inspection of line chart | |
| TC-030 | Fiscal YTD uses March 31 | [Revenue Fiscal YTD] | Accumulates from April 1, resets April 1 | Check FY boundary behavior | |
| TC-031 | QoQ Growth % | [QoQ Revenue Growth %] | (Current Q − Prior Q) / Prior Q × 100 | Manual for Q2 2023 vs Q1 2023 | |

### 1.5 Comparative Measures

| TC# | Test Case | Measure | Expected Value | Validation Method | Pass/Fail |
|-----|-----------|---------|----------------|-------------------|-----------|
| TC-032 | Member + NonMember = Total | [Member Revenue] + [Non-Member Revenue] | = [Total Revenue] | Verify sum | |
| TC-033 | Member Booking % ~55% | [Member Booking %] | 55031/100000 × 100 = 55.03% | Source data verification | |
| TC-034 | Revenue Rank = 1 for top hotel | [Hotel Revenue Rank] | Highest revenue hotel gets rank 1 | Sort and verify | |
| TC-035 | Revenue % of Total sums to 100 | [Revenue % of Total] | Sum across all hotels = 100% | Matrix verification | |

---

## 2. Data Reconciliation Checks

### 2.1 Row Count Reconciliation

| Check# | Table | Expected Rows | Actual Rows | Source File Rows | Match? |
|--------|-------|---------------|-------------|------------------|--------|
| DR-001 | fct_Bookings | 100,000 | [check] | Hospitality_Data_Global_fct.csv: 100,000 | |
| DR-002 | dim_Finance | 100,000 | [check] | Finance_dim.csv: 100,000 | |
| DR-003 | dim_Feedback | 100,000 | [check] | Feedback_dim.csv: 100,000 | |
| DR-004 | dim_OnlineBooking | 100,000 | [check] | OnlineBooking_dim.csv: 100,000 | |
| DR-005 | dim_Hotel | 7 | [check] | Hotel_List_dim.xlsx: 7 | |
| DR-006 | dim_Manager | 7 | [check] | Manager_Details_dim.xlsx: 7 | |
| DR-007 | dim_Date | 1,826 | [check] | Generated (2021-01-01 to 2025-12-31) | |

### 2.2 Key Integrity Reconciliation

| Check# | Validation | Expected | Method | Pass/Fail |
|--------|-----------|----------|--------|-----------|
| DR-008 | All fct BookingIDs exist in dim_Finance | 0 orphans | Left anti-join | |
| DR-009 | All fct BookingIDs exist in dim_Feedback | 0 orphans | Left anti-join | |
| DR-010 | All fct BookingIDs exist in dim_OnlineBooking | 0 orphans | Left anti-join | |
| DR-011 | All fct HotelNames exist in dim_Hotel | 0 orphans | Relationship validation | |
| DR-012 | All dim_Hotel Hotel_Names exist in dim_Manager | 7 matches | Check completeness | |
| DR-013 | No duplicate BookingIDs in fct_Bookings | 0 duplicates | COUNTROWS vs DISTINCTCOUNT | |
| DR-014 | No duplicate BookingIDs in dim_Finance | 0 duplicates | COUNTROWS vs DISTINCTCOUNT | |

### 2.3 Value Range Reconciliation

| Check# | Column | Expected Range | Validation | Pass/Fail |
|--------|--------|----------------|------------|-----------|
| DR-015 | Age | 18–85 | MIN/MAX check | |
| DR-016 | NightsStayed | 1–5 | MIN/MAX check | |
| DR-017 | RatePerNight | 1,140–102,881 | MIN/MAX check | |
| DR-018 | Rating | 1.5–5.0 | MIN/MAX check | |
| DR-019 | StaffRating | 1.0–5.0 | MIN/MAX check | |
| DR-020 | CleanlinessRating | 1.0–5.0 | MIN/MAX check | |
| DR-021 | TotalRevenue | 4,720–474,397 | MIN/MAX check | |
| DR-022 | DiscountPercent | 0, 5, 10, 15, 20 only | DISTINCT check | |
| DR-023 | CheckInDate range | 2021-01-01 to 2024-12-31 | MIN/MAX check | |
| DR-024 | dim_Date range | 2021-01-01 to 2025-12-31 | MIN/MAX check | |

### 2.4 Aggregate Reconciliation (Python/Excel vs Power BI)

| Check# | Metric | Calculate in Python/Excel | Compare to Power BI | Tolerance | Pass/Fail |
|--------|--------|--------------------------|---------------------|-----------|-----------|
| DR-025 | Total Revenue (Paid) | `df[df.PaymentStatus=='Paid'].TotalRevenue.sum()` | [Total Revenue] card | ±₹1 | |
| DR-026 | Total Bookings per Hotel | `df.groupby('HotelName').size()` | Hotel bar chart values | Exact | |
| DR-027 | Avg Rating overall | `df.Rating.mean()` | [Avg Rating] card | ±0.01 | |
| DR-028 | Member count | `df[df.MemberType=='Member'].shape[0]` | 55,031 | Exact | |
| DR-029 | Channel distribution | `df.Channel.value_counts()` | Channel donut | Exact | |

---

## 3. DAX Measure Validation Scenarios

### 3.1 Boundary & Edge Case Tests

| TC# | Scenario | Setup | Expected Behavior | Pass/Fail |
|-----|----------|-------|-------------------|-----------|
| DX-001 | No data in context (empty filter) | Apply impossible filter (e.g., City="London") | All measures return 0 or BLANK, no errors | |
| DX-002 | Single booking context | Filter to 1 BookingID | Revenue = that booking's TotalRevenue | |
| DX-003 | DIVIDE by zero | Filter to period with 0 PY revenue | YoY Growth % = 0 (not error) | |
| DX-004 | First year (2021) has no PY | Year = 2021 | Revenue PY = BLANK, YoY = 0 | |
| DX-005 | All bookings Failed | Filter PaymentStatus = Failed | Total Revenue = 0, all financial = 0 | |
| DX-006 | Single hotel context | Filter Hotel = Taj Bengal | ~14,283 bookings, proportional revenue | |
| DX-007 | GSS with all 5.0 ratings | If a subset has perfect ratings | GSS = 5.0 | |

### 3.2 Cross-Measure Consistency Tests

| TC# | Test | Rule | Pass/Fail |
|-----|------|------|-----------|
| DX-008 | Revenue = Member + NonMember | [Total Revenue] = [Member Revenue] + [Non-Member Revenue] | |
| DX-009 | Positive% + Negative% = 100 | [Positive Feedback %] + [Negative Feedback %] = 100 | |
| DX-010 | Profit Margin from components | [Profit Margin %] = [Net Profit] / [Total Revenue] × 100 | |
| DX-011 | ADR × Room Nights ≈ Revenue | [ADR] × [Total Room Nights] ≈ [Total Revenue] (within rounding) | |
| DX-012 | Bookings = Successful + Pending + Failed | 100,000 = 90,091 + 7,856 + 2,053 | |
| DX-013 | Revenue Per Booking × Bookings ≈ Revenue | [Revenue Per Booking] × [Successful Bookings] ≈ [Total Revenue] | |

### 3.3 Time Intelligence Validation

| TC# | Test | Setup | Expected | Pass/Fail |
|-----|------|-------|----------|-----------|
| DX-014 | YTD resets at year boundary | Compare Dec 31 vs Jan 1 | YTD drops to near-zero on Jan 1 | |
| DX-015 | Fiscal YTD resets April 1 | Compare Mar 31 vs Apr 1 | Fiscal YTD drops on Apr 1 | |
| DX-016 | Running Total never decreases | Scan all months | Monotonically increasing (unless filter applied) | |
| DX-017 | MTD on day 1 of month | Select 1st of any month | MTD = that single day's revenue | |
| DX-018 | SAMEPERIODLASTYEAR returns blank for 2021 | Year = 2021 | Revenue PY = BLANK | |

---

## 4. Row-Level Security Test Matrix

| TC# | User Email | Role | Hotels Visible | Bookings Visible | Revenue Visible | Pass/Fail |
|-----|-----------|------|---------------|-----------------|-----------------|-----------|
| RLS-001 | aman.mehta@tajhotels.com | HotelManager | Taj Mahal Palace only | ~14,270 (Paid ~12,860) | Mumbai only | |
| RLS-002 | kavya.shah@tajhotels.com | HotelManager | Taj Lake Palace only | ~14,148 | Udaipur only | |
| RLS-003 | rohan.das@tajhotels.com | HotelManager | Taj Falaknuma Palace only | ~14,448 | Hyderabad only | |
| RLS-004 | sneha.roy@tajhotels.com | HotelManager | Taj Bengal only | ~14,283 | Kolkata only | |
| RLS-005 | arjun.rao@tajhotels.com | HotelManager | Taj West End only | ~14,209 | Bengaluru only | |
| RLS-006 | priya.nair@tajhotels.com | HotelManager | Taj Exotica only | ~14,378 | Goa only | |
| RLS-007 | sanjay.iyer@tajhotels.com | HotelManager | Taj Coromandel only | ~14,264 | Chennai only | |
| RLS-008 | executive@tajhotels.com | None (no role) | All 7 hotels | 100,000 | Full portfolio | |
| RLS-009 | unknown@other.com | HotelManager | No match | 0 bookings | ₹0 | |

### RLS Detailed Validation

| TC# | Test Case | Validation Steps | Expected | Pass/Fail |
|-----|-----------|-----------------|----------|-----------|
| RLS-010 | Manager cannot see other hotels in slicers | Login as MGR001, check Hotel slicer | Only "Taj Mahal Palace" visible | |
| RLS-011 | Revenue sum under RLS matches hotel total | Login as MGR001, check [Total Revenue] | Equals SUM(TotalRevenue) WHERE Hotel=TajMahalPalace AND Paid | |
| RLS-012 | Drill-through respects RLS | Login as MGR001, drill to Hotel detail | Cannot see other hotels' data | |
| RLS-013 | Export respects RLS | Login as MGR001, export to Excel | Only Mumbai data in export | |
| RLS-014 | Manager View title correct | Login as MGR001, check Page 3 | Shows "Aman Mehta — Taj Mahal Palace" | |
| RLS-015 | Feedback filtered by RLS | Login as MGR001, check Page 5 | Only feedback for Taj Mahal Palace | |
| RLS-016 | Finance filtered by RLS | Login as MGR001, check revenue | Only finance records for that hotel's bookings | |
| RLS-017 | No cross-hotel leakage in matrix | Login as MGR001, check Hotel×Channel matrix | Only 1 row (their hotel) | |

---

## 5. Filter & Slicer Validation

| TC# | Test Case | Page | Steps | Expected Result | Pass/Fail |
|-----|-----------|------|-------|-----------------|-----------|
| SL-001 | Year slicer filters all visuals | Page 1 | Select Year=2023 | All cards/charts show 2023 data only | |
| SL-002 | Hotel slicer filters all visuals | Page 1 | Select Hotel=Taj Bengal | All metrics for Kolkata only | |
| SL-003 | Category slicer works | Page 1 | Select Category=Luxury | Only Taj Mahal Palace + Taj Coromandel | |
| SL-004 | Quarter slicer filters correctly | Page 1 | Select Q1 | Only Jan–Mar data | |
| SL-005 | Room Type slicer on Page 2 | Page 2 | Select Suite | Revenue/ADR for Suite only | |
| SL-006 | Channel slicer on Page 4 | Page 4 | Select App | Only App bookings in charts | |
| SL-007 | Sentiment slicer on Page 5 | Page 5 | Select Negative | Only negative feedback shown | |
| SL-008 | MemberType slicer on Page 6 | Page 6 | Select Member | Only member data | |
| SL-009 | Synced Year slicer across pages | All | Select 2022 on Page 1, navigate to Page 2 | Page 2 also shows 2022 | |
| SL-010 | Clear slicer returns all data | Page 1 | Clear Year slicer | Revenue = full dataset total | |
| SL-011 | Multi-select works | Page 1 | Select 2022 + 2023 in Year | Combined 2-year data | |
| SL-012 | No slicer on Manager page | Page 3 | Visual inspection | No slicers present (RLS only) | |
| SL-013 | Slicer "Select All" works | Page 1 | Click "Select All" on Hotel | Same as no filter applied | |

---

## 6. Drill-Through & Bookmark Testing

### 6.1 Drill-Through Tests

| TC# | Test Case | Source Page | Trigger | Target Page | Expected | Pass/Fail |
|-----|-----------|------------|---------|-------------|----------|-----------|
| DT-001 | Hotel drill-through from bar chart | Page 1 | Right-click "Taj Bengal" bar | Hotel Drillthrough | All visuals show Taj Bengal data | |
| DT-002 | Hotel drill-through from Page 2 | Page 2 | Right-click hotel in visual | Hotel Drillthrough | Correct hotel data | |
| DT-003 | Hotel drill-through preserves year filter | Page 1 | Year=2023, then drill to hotel | Hotel Drillthrough | Shows 2023 data for that hotel | |
| DT-004 | Back button returns to source | Hotel Drillthrough | Click Back button | Previous page | Returns to exact source page | |
| DT-005 | Drill-through page is hidden | Report navigation | Try to navigate | N/A | Page not visible in tab bar | |
| DT-006 | Drill-through not available for non-hotel fields | Page 1 | Right-click on Channel donut | No drill option | No "Drill through" option appears | |

### 6.2 Bookmark Tests

| TC# | Test Case | Bookmark | Steps | Expected | Pass/Fail |
|-----|-----------|----------|-------|----------|-----------|
| BK-001 | Reset All clears filters | BM-01 | Click "Reset All" button | All slicers cleared, full data | |
| BK-002 | Current Year sets 2024 | BM-02 | Click "2024" bookmark | Year slicer = 2024 only | |
| BK-003 | Luxury filter applies | BM-03 | Click "Luxury" bookmark | Category = Luxury, 2 hotels visible | |
| BK-004 | Bookmarks persist across pages | BM-02 | Apply on Page 1, navigate to Page 2 | Year=2024 still active on Page 2 | |

---

## 7. Performance Testing Checklist

| TC# | Test Case | Target | Actual | Pass/Fail |
|-----|-----------|--------|--------|-----------|
| PF-001 | Initial report load time | < 5 seconds | [measure] | |
| PF-002 | Page navigation switch time | < 2 seconds | [measure] | |
| PF-003 | Slicer selection response time | < 1 second | [measure] | |
| PF-004 | Drill-through navigation time | < 2 seconds | [measure] | |
| PF-005 | Cross-filter highlight response | < 1 second | [measure] | |
| PF-006 | Export to Excel time (under RLS) | < 10 seconds | [measure] | |
| PF-007 | .pbix file size | < 30 MB | [check] | |
| PF-008 | Data refresh time (Close & Apply) | < 60 seconds | [measure] | |
| PF-009 | Visual rendering (Page 1 with all cards) | No blank flash | [observe] | |
| PF-010 | Memory usage during interaction | < 500 MB RAM | [Task Manager] | |

### Performance Optimization Verification

| Check | Item | Expected | Pass/Fail |
|-------|------|----------|-----------|
| PO-001 | Auto date/time disabled | Options → Data Load → unchecked | |
| PO-002 | Import mode (not DirectQuery) | All tables in Import mode | |
| PO-003 | No unnecessary bidirectional filters | Only R3 needs bidirectional for RLS | |
| PO-004 | Pre-calculated columns exist | NetProfit, GSS, IsPaid in PQ | |
| PO-005 | Unused columns hidden | FirstName, LastName, GuestEmail hidden | |
| PO-006 | Visuals per page ≤ 10 | Count each page | |

---

## 8. User Acceptance Testing (UAT) Checklist

### 8.1 Executive Stakeholder UAT

| TC# | Acceptance Criteria | Stakeholder | Status | Comments |
|-----|--------------------:|-------------|--------|----------|
| UAT-001 | Executive Summary shows portfolio KPIs | Hospitality Head | | |
| UAT-002 | Revenue trend visible for 2021–2025 | Hospitality Head | | |
| UAT-003 | All 7 hotels visible in bar chart | Hospitality Head | | |
| UAT-004 | Can filter by year, hotel, category | Hospitality Head | | |
| UAT-005 | YoY growth percentage is accurate | Finance Team | | |
| UAT-006 | Net Profit matches expected formula | Finance Team | | |
| UAT-007 | Channel mix matches business knowledge | Marketing Team | | |
| UAT-008 | GSS scores align with known feedback | Guest Experience | | |
| UAT-009 | Member vs Non-Member split is correct | Strategy Team | | |
| UAT-010 | Reports are internal-facing only | BI Lead | | |

### 8.2 Hotel Manager UAT

| TC# | Acceptance Criteria | Tester (Role) | Status | Comments |
|-----|--------------------:|---------------|--------|----------|
| UAT-011 | Manager sees only their hotel | Each MGR | | |
| UAT-012 | Revenue matches their internal records | Each MGR | | |
| UAT-013 | Feedback shown is for their property | Each MGR | | |
| UAT-014 | Cannot see other hotels' data | Each MGR | | |
| UAT-015 | Dynamic title shows correct name & hotel | Each MGR | | |
| UAT-016 | Monthly trend reflects their hotel | Each MGR | | |
| UAT-017 | Room type mix matches known distribution | Each MGR | | |

### 8.3 Technical UAT

| TC# | Acceptance Criteria | Reviewer | Status | Comments |
|-----|--------------------:|----------|--------|----------|
| UAT-018 | 20+ distinct DAX functions used | BI Lead | | |
| UAT-019 | Canvas size = 1920×1080 | BI Lead | | |
| UAT-020 | Font = Segoe UI (text) + Calibri (numbers) | BI Lead | | |
| UAT-021 | Color palette is visually balanced | BI Lead | | |
| UAT-022 | RLS implemented with USERPRINCIPALNAME() | BI Lead | | |
| UAT-023 | Date table supports time intelligence | BI Lead | | |
| UAT-024 | All relationships correctly configured | BI Lead | | |
| UAT-025 | No circular dependency warnings | BI Lead | | |
| UAT-026 | Deliverable is .pbix file | BI Lead | | |
| UAT-027 | Report refreshes without error | BI Lead | | |

---

## 9. Defect Logging Template

| Defect ID | Date | Severity | Page | Description | Steps to Reproduce | Expected | Actual | Assigned To | Status | Resolution | Resolved Date |
|-----------|------|----------|------|-------------|-------------------|----------|--------|-------------|--------|------------|---------------|
| DEF-001 | | Critical/High/Medium/Low | | | | | | | Open/In Progress/Resolved/Closed | | |
| DEF-002 | | | | | | | | | | | |
| DEF-003 | | | | | | | | | | | |

### Severity Definitions

| Severity | Definition | Response Time |
|----------|-----------|---------------|
| Critical | RLS failure (data leakage), complete page failure, wrong revenue totals | Immediate fix required |
| High | KPI calculation error, missing visuals, slicer not working | Fix within 24 hours |
| Medium | Formatting issues, minor calculation variance (<1%), tooltip missing | Fix within 3 days |
| Low | Cosmetic issues, color preferences, label alignment | Fix in next release |

---

## 10. Final Sign-Off Checklist

### 10.1 Pre-Deployment Verification

| # | Checkpoint | Verified By | Date | Sign-Off |
|---|-----------|-------------|------|----------|
| 1 | All 100,000 records loaded correctly | Data Analyst | | ☐ |
| 2 | All 6 report pages render without error | BI Developer | | ☐ |
| 3 | All 51 DAX measures return correct values | BI Developer | | ☐ |
| 4 | RLS tested for all 7 managers + executive | Security Lead | | ☐ |
| 5 | Drill-through works correctly | QA Tester | | ☐ |
| 6 | Slicers sync across pages | QA Tester | | ☐ |
| 7 | Performance within acceptable limits | BI Developer | | ☐ |
| 8 | No Critical or High defects open | Test Lead | | ☐ |
| 9 | Data reconciliation passed (source vs report) | Data Analyst | | ☐ |
| 10 | Technical requirements met (20+ DAX, fonts, canvas) | BI Lead | | ☐ |

### 10.2 Stakeholder Approval

| Stakeholder | Role | Approval Status | Date | Signature |
|-------------|------|-----------------|------|-----------|
| [Name] | Project Owner | Pending / Approved / Rejected | | |
| [Name] | Hospitality Head (Business Sponsor) | Pending / Approved / Rejected | | |
| [Name] | BI Lead (Technical Reviewer) | Pending / Approved / Rejected | | |
| [Name] | Finance Representative | Pending / Approved / Rejected | | |
| [Name] | Guest Experience Representative | Pending / Approved / Rejected | | |

### 10.3 Go/No-Go Criteria

| Criterion | Threshold | Status |
|-----------|-----------|--------|
| Critical defects | 0 open | |
| High defects | 0 open | |
| Medium defects | ≤ 3 open (with workaround documented) | |
| KPI accuracy | 100% match with source data | |
| RLS security | 100% pass (all 9 test cases) | |
| Performance | All pages < 5 sec load | |
| Stakeholder approval | All 3 mandatory approvers signed | |
| UAT completion | All UAT-001 to UAT-027 passed | |

### 10.4 Final Decision

| Decision | Date | Authority |
|----------|------|-----------|
| ☐ GO — Proceed to Deployment | | Project Owner |
| ☐ NO-GO — Return to Construction (defects found) | | Project Owner |
| ☐ CONDITIONAL GO — Deploy with known issues documented | | Project Owner + BI Lead |

---

*End of Phase 4: Testing & Validation*
*Awaiting approval to proceed to Phase 5: Deployment & Documentation.*


---

## Appendix A: Reference Validation Values

These values were calculated directly from the source CSV files using Python (pandas).
Use these as the ground truth when validating Power BI DAX measures.

### A.1 Booking Counts

| Metric | Expected Value |
|--------|----------------|
| Total Bookings (all statuses) | 100,000 |
| Paid Bookings | 90,091 |
| Pending Bookings | 7,856 |
| Failed Bookings | 2,053 |
| Unique Guests (CustomerIDs) | 200 |

### A.2 Revenue & Financial (Paid Bookings Only)

| Metric | Expected Value |
|--------|----------------|
| Total Revenue | ₹5,720,964,070.45 |
| Total Cost | ₹2,573,778,772.00 |
| Total Tax Collected | ₹460,755,271.57 |
| Total Discounts Given | ₹572,499,140.80 |
| Additional Service Revenue | ₹112,460,000.00 |
| Additional Service Cost | ₹60,733,100.00 |
| Net Profit | ₹2,626,413,057.65 |
| Profit Margin % | 45.91% |
| Total Room Nights Sold | 202,568 |
| ADR (Average Daily Rate) | ₹28,242.19 |
| Revenue Per Booking | ₹63,502.06 |
| Avg Nights Stayed | 2.2485 |

### A.3 Guest Satisfaction (All Bookings)

| Metric | Expected Value |
|--------|----------------|
| Avg Overall Rating | 4.1300 |
| Avg Staff Rating | 4.1007 |
| Avg Cleanliness Rating | 4.0888 |
| GSS (simple average of 3) | 4.1065 |

### A.4 Revenue by Hotel (Paid Only)

| Hotel | Expected Revenue |
|-------|------------------|
| Taj Lake Palace | ₹1,290,094,899.18 |
| Taj Falaknuma Palace | ₹1,223,201,842.81 |
| Taj Coromandel | ₹845,113,004.84 |
| Taj Exotica | ₹814,482,350.00 |
| Taj Mahal Palace | ₹733,959,490.61 |
| Taj West End | ₹533,250,694.16 |
| Taj Bengal | ₹280,861,788.85 |
| **TOTAL** | **₹5,720,964,070.45** |

### A.5 Member vs Non-Member (Paid Only)

| Metric | Member | Non-Member |
|--------|--------|------------|
| Revenue | ₹2,645,934,998.67 | ₹3,075,029,071.78 |
| Booking Count (all) | 55,031 | 44,969 |
| Member Booking % | 55.03% | 44.97% |

### A.6 Channel Distribution (All Bookings)

| Channel | Count | Percentage |
|---------|-------|------------|
| Website | 45,077 | 45.08% |
| App | 25,219 | 25.22% |
| Agent | 16,852 | 16.85% |
| ThirdParty | 12,852 | 12.85% |

### A.7 Validation Tolerance

| Measure Type | Acceptable Tolerance |
|-------------|---------------------|
| Count measures | Exact match (0 tolerance) |
| Revenue/Cost sums | ±₹1 (rounding only) |
| Averages | ±0.01 |
| Percentages | ±0.1% |
| Rankings | Exact match |
