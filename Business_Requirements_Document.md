# Business Requirements Document (BRD)
## Hospitality Analytics Dashboard — India

| Field | Value |
|-------|-------|
| Project Title | Hospitality Analytics Dashboard - India |
| Version | 2.0 |
| Date | June 2026 |
| Methodology | AI-DLC (AI-Driven Development Lifecycle) |
| Phase | Phase 1 — Requirements Analysis |
| Client | KSR Datavizon Hospitality Analytics Challenge |
| Prepared By | Senior Power BI Architect / AI-DLC Lead |

---

## 1. Executive Summary

This project delivers a comprehensive Power BI analytics dashboard for a luxury hotel chain
operating 7 Taj properties across India. The solution provides multi-year performance tracking
(2021–2025), financial analysis, guest satisfaction monitoring, booking channel insights, and
membership analytics — secured with Row-Level Security (RLS) to restrict hotel managers to
their own property data while giving executives full cross-portfolio visibility.

**Key Numbers:**
- 7 Hotels | 7 Cities | 4 Categories
- 100,000 Booking Records
- 200 Unique Customers | 10 Countries
- Date Range: January 2021 – September 2025
- Revenue Range: ₹4,720 – ₹474,397 per booking
- 90.1% Payment Success Rate

---

## 2. Business Problem Statement

The hospitality chain lacks a centralized, interactive analytics platform to:
- Track revenue performance across properties and time periods
- Compare hotel efficiency by category, city, and manager
- Understand guest demographics and satisfaction drivers
- Analyze booking channel effectiveness and device preferences
- Evaluate the value of loyalty membership programs
- Enforce data security so managers see only their property's financials

The dashboard must solve these problems while meeting strict technical requirements
(20+ DAX functions, specific canvas size, fonts, and color standards).

---

## 3. Business Objectives

| # | Objective | Priority |
|---|-----------|----------|
| OBJ-01 | Track revenue over multiple years (2021–2025) | High |
| OBJ-02 | Compare hotel performance by city, category, and manager | High |
| OBJ-03 | Evaluate Member vs. Non-Member revenue contribution | High |
| OBJ-04 | Monitor guest satisfaction through feedback metrics | High |
| OBJ-05 | Restrict financial data via Row-Level Security by manager | High |
| OBJ-06 | Provide centralized dashboard for executives and managers | High |
| OBJ-07 | Analyze booking channels and device preferences | Medium |
| OBJ-08 | Track cost efficiency and profit margins | Medium |
| OBJ-09 | Identify discount impact on revenue | Medium |
| OBJ-10 | Enable time-based drill-down (Year → Quarter → Month) | Medium |

---

## 4. Scope

### 4.1 In Scope

- ETL from 6 structured data sources (4 CSV + 2 Excel)
- Data modeling using Star Schema in Power BI
- Power Query transformations for data cleansing and enrichment
- 20+ DAX measures for KPI calculations
- 6 interactive report pages
- Row-Level Security by Hotel Manager
- Publication to Power BI Service with scheduled refresh
- Date table for time intelligence (fiscal year support)
- Cross-filtering, drill-through, bookmarks, and synced slicers

### 4.2 Out of Scope

- Live database connections (data is file-based)
- Occupancy Rate and RevPAR (room inventory not available)
- Mobile-optimized layouts (desktop Full HD only)
- External API integrations
- Predictive analytics / ML models
- Multi-language support

---

## 5. Stakeholders & Reporting Needs

| Stakeholder | Role | Access Level | Key Reporting Needs |
|-------------|------|--------------|---------------------|
| Hospitality Head | Business Sponsor | Full (Executive) | Portfolio performance, YoY trends, category comparison |
| Project Owner | Strategic oversight | Full (Executive) | All KPIs, cross-hotel benchmarking |
| BI Lead | Technical Reviewer | Full (Executive) | Data model validation, DAX accuracy |
| Aman Mehta (MGR001) | Manager — Taj Mahal Palace | RLS (Mumbai only) | Property revenue, bookings, guest feedback |
| Kavya Shah (MGR002) | Manager — Taj Lake Palace | RLS (Udaipur only) | Property revenue, bookings, guest feedback |
| Rohan Das (MGR003) | Manager — Taj Falaknuma Palace | RLS (Hyderabad only) | Property revenue, bookings, guest feedback |
| Sneha Roy (MGR004) | Manager — Taj Bengal | RLS (Kolkata only) | Property revenue, bookings, guest feedback |
| Arjun Rao (MGR005) | Manager — Taj West End | RLS (Bengaluru only) | Property revenue, bookings, guest feedback |
| Priya Nair (MGR006) | Manager — Taj Exotica | RLS (Goa only) | Property revenue, bookings, guest feedback |
| Sanjay Iyer (MGR007) | Manager — Taj Coromandel | RLS (Chennai only) | Property revenue, bookings, guest feedback |
| Finance Team | Revenue analysts | Full (Executive) | Revenue, cost, profit, discounts, tax |
| Guest Experience Team | Quality assurance | Full (Executive) | GSS, staff ratings, cleanliness, feedback trends |
| Marketing Team | Channel strategy | Full (Executive) | Channel mix, device split, membership conversion |

---

## 6. Data Sources — Complete Inventory

### 6.1 Source File Summary

| # | File Name | Format | Records | Role | Join Key |
|---|-----------|--------|---------|------|----------|
| 1 | Hospitality_Data_Global_fct.csv | CSV (UTF-8) | 100,000 | Central Fact Table | BookingID |
| 2 | Finance_dim.csv | CSV (UTF-8) | 100,000 | Financial metrics | BookingID |
| 3 | Feedback_dim.csv | CSV (UTF-8) | 100,000 | Guest ratings & feedback | BookingID |
| 4 | OnlineBooking_dim.csv | CSV (Latin-1) | 100,000 | Booking channel details | BookingID |
| 5 | Hotel_List_dim.xlsx | Excel | 7 | Hotel master data | Hotel_Name |
| 6 | Manager_Details_dim.xlsx | Excel | 7 | Manager-to-hotel mapping | HotelName |

### 6.2 Fact Table: Hospitality_Data_Global_fct.csv

| Column | Data Type | Unique Values | Sample Values |
|--------|-----------|---------------|---------------|
| BookingID | Integer | 100,000 | 10001–109999 |
| CustomerID | Text | 200 | CUST00001–CUST00200 |
| FirstName | Text | 147 | Kavya, Raj, Priya, Aarav |
| LastName | Text | 78 | Verma, Sharma, Patel, Singh |
| GuestName | Text | 1,552 | Full name combination |
| Gender | Text | 2 | Male (51,836), Female (48,164) |
| Age | Integer | 68 | Range: 18–85 |
| Country | Text | 10 | India (40%), USA (15%), UK (8%), China (6%), Australia (6%), Japan (6%), Canada (5%), France (5%), Germany (5%), Brazil (4%) |
| RoomType | Text | 3 | Standard (49,986), Deluxe (34,953), Suite (15,061) |
| CheckInDate | Text (DD-MM-YYYY) | 1,704 | 01-01-2021 to 31-12-2024 |
| CheckOutDate | Text (DD-MM-YYYY) | 1,708 | 02-01-2021 to 05-01-2025 |
| NightsStayed | Integer | 5 | 1, 2, 3, 4, 5 |
| RatePerNight | Decimal | 39,318 | ₹1,140 – ₹102,881 |
| MemberType | Text | 2 | Member (55,031), Non-Member (44,969) |
| HotelName | Text | 7 | 7 Taj properties |
| City | Text | 7 | Mumbai, Udaipur, Hyderabad, Kolkata, Bengaluru, Goa, Chennai |
| Category | Text | 4 | Luxury, Heritage, Business, Resort |
| PaymentMethod | Text | 5 | Credit Card, Cash, UPI, Debit Card, Net Banking |
| Channel | Text | 4 | Website (45,077), App (25,219), Agent (16,852), ThirdParty (12,852) |
| DeviceType | Text | 2 | Mobile, Desktop |
| PaymentStatus | Text | 3 | Paid (90,091), Pending (7,856), Failed (2,053) |

### 6.3 Finance_dim.csv

| Column | Data Type | Range / Values |
|--------|-----------|----------------|
| BookingID | Integer | 100,000 unique (matches fact) |
| CustomerID | Text | CUST00001–CUST00200 |
| DiscountPercent | Integer | 0% (20,117), 5% (19,937), 10% (19,871), 15% (19,974), 20% (20,101) |
| TaxAmount | Decimal | Varies (Avg: ₹5,107) |
| TotalRevenue | Decimal | ₹4,720 – ₹474,397 (Avg: ₹63,434) |
| Cost | Integer | ₹1,923 – ₹204,276 (Avg: ₹28,529) |
| AdditionalServiceCost | Decimal | Avg: ₹675 |
| AdditionalServiceRevenue | Decimal | Avg: ₹1,248 |
| DiscountsGiven | Decimal | Avg: ₹6,331 |

### 6.4 Feedback_dim.csv

| Column | Data Type | Range / Values |
|--------|-----------|----------------|
| BookingID | Integer | 100,000 unique |
| CustomerID | Text | CUST00001–CUST00200 |
| Rating | Decimal | 1.5 – 5.0 (36 unique values) |
| Feedback | Text | 10 categories (see below) |
| SubmittedDate | Text (DD-MM-YYYY) | 2021–2025 |
| StaffRating | Decimal | 1.0 – 5.0 (40 unique values) |
| CleanlinessRating | Decimal | 1.0 – 5.0 (41 unique values) |

**Feedback Categories:**
| Category | Sentiment |
|----------|-----------|
| Good service | Positive |
| Excellent stay | Positive |
| Very good | Positive |
| Room was clean and comfortable | Positive |
| Food quality was great | Positive |
| Exceptional hospitality | Positive |
| Will visit again | Positive |
| Average | Negative |
| Could be better | Negative |
| Check-in was slow | Negative |

### 6.5 OnlineBooking_dim.csv

| Column | Data Type | Values |
|--------|-----------|--------|
| BookingID | Integer | 100,000 unique |
| CustomerID | Text | CUST00001–CUST00200 |
| Channel | Text | Website, App, Agent, ThirdParty |
| GuestEmail | Text | Email addresses (PII — must mask) |
| BookingDate | Text (DD-MM-YYYY) | Booking date |
| Location | Text | 7 hotel names |
| DeviceType | Text | Mobile, Desktop |

### 6.6 Hotel_List_dim.xlsx

| Hotel_Name | City | Category |
|------------|------|----------|
| Taj Mahal Palace | Mumbai | Luxury |
| Taj Lake Palace | Udaipur | Heritage |
| Taj Falaknuma Palace | Hyderabad | Heritage |
| Taj Bengal | Kolkata | Business |
| Taj West End | Bengaluru | Business |
| Taj Exotica | Goa | Resort |
| Taj Coromandel | Chennai | Luxury |

### 6.7 Manager_Details_dim.xlsx

| HotelName | ManagerID | ManagerName | ManagerEmail |
|-----------|-----------|-------------|--------------|
| Taj Mahal Palace | MGR001 | Aman Mehta | aman.mehta@tajhotels.com |
| Taj Lake Palace | MGR002 | Kavya Shah | kavya.shah@tajhotels.com |
| Taj Falaknuma Palace | MGR003 | Rohan Das | rohan.das@tajhotels.com |
| Taj Bengal | MGR004 | Sneha Roy | sneha.roy@tajhotels.com |
| Taj West End | MGR005 | Arjun Rao | arjun.rao@tajhotels.com |
| Taj Exotica | MGR006 | Priya Nair | priya.nair@tajhotels.com |
| Taj Coromandel | MGR007 | Sanjay Iyer | sanjay.iyer@tajhotels.com |

---

## 7. Functional Requirements

### FR-01: Executive Summary Dashboard
- Display top-level KPIs: Total Revenue, Net Profit, Successful Bookings, ADR, GSS
- Revenue trend by year (line chart)
- Revenue by hotel (bar chart, sorted descending)
- Revenue by category (donut chart)
- Channel distribution (donut chart)
- Slicers: Year, Quarter, Hotel, City, Category

### FR-02: Revenue & Trends (2021–2025)
- Monthly/quarterly revenue trend with drill-down (Year → Quarter → Month)
- YoY growth percentage
- Cost vs. Revenue comparison (combo chart)
- Revenue by Room Type (stacked bar)
- Profit margin trend over time
- Revenue YTD, QTD, MTD cards

### FR-03: Hotel Manager View (RLS-Secured)
- Dynamic title showing manager name and hotel
- Property-specific KPI cards (revenue, bookings, GSS, profit)
- Monthly revenue trend for the manager's hotel only
- Room type distribution
- Payment method breakdown
- Top feedback issues table
- Fully filtered by RLS — no manual slicer needed

### FR-04: Booking Source & Channel Analysis
- Channel distribution (Website 45%, App 25%, Agent 17%, ThirdParty 13%)
- Device type split (Mobile vs Desktop)
- Channel trend over time (stacked area)
- Channel-wise revenue comparison
- Hotel × Channel matrix
- Booking lead time analysis

### FR-05: Feedback & Guest Satisfaction Dashboard
- GSS gauge with target
- Individual rating cards (Overall, Staff, Cleanliness)
- Feedback category distribution (horizontal bar)
- GSS by Hotel comparison
- Rating trend over months (multi-line)
- Positive vs Negative feedback split
- Satisfaction by hotel category

### FR-06: Member vs. Non-Member Financial Summary
- Revenue cards for Member and Non-Member
- Revenue split (100% stacked bar by year)
- Net Profit comparison
- Discount analysis by membership type
- ADR comparison
- Room type preference by membership
- Booking volume trend by membership type

---

## 8. Non-Functional Requirements

| # | Requirement | Specification | Source |
|---|-------------|---------------|--------|
| NFR-01 | DAX Functions | Minimum 20 distinct DAX functions | General Instructions PDF |
| NFR-02 | Canvas Size | Default or Full HD (1920 × 1080 pixels) | General Instructions PDF |
| NFR-03 | Text Font | Segoe UI | General Instructions PDF |
| NFR-04 | Number Font | Calibri | General Instructions PDF |
| NFR-05 | Color Palette | Visually appealing and well-balanced | General Instructions PDF |
| NFR-06 | Security | RLS by Hotel Manager using USERPRINCIPALNAME() | BRD PDF |
| NFR-07 | PII Protection | Guest emails masked; names anonymized where needed | BRD PDF |
| NFR-08 | Access | Power BI Pro license for all stakeholders | BRD PDF |
| NFR-09 | Connectivity | Internet access for Power BI Service | BRD PDF |
| NFR-10 | Audience | Internal analysis only (not client-facing) | BRD PDF |
| NFR-11 | Deliverable | .pbix file | General Instructions PDF |
| NFR-12 | Performance | Dashboard loads in < 5 seconds for 100K records | Best practice |
| NFR-13 | Refresh | Auto-refresh enabled on Power BI Service | BRD PDF |

---

## 9. KPIs & Metrics (Summary)

| Category | KPI | Definition |
|----------|-----|------------|
| Revenue | Total Revenue | SUM of TotalRevenue for Paid bookings |
| Revenue | Net Profit | Revenue − Cost − AddSvcCost + AddSvcRevenue − Discounts |
| Revenue | Profit Margin % | Net Profit / Total Revenue × 100 |
| Revenue | ADR | Total Revenue / Total Room Nights Sold |
| Revenue | Total Discounts | SUM of DiscountsGiven |
| Revenue | Additional Service Revenue | SUM of AddSvcRevenue |
| Bookings | Total Bookings | Count of all BookingIDs |
| Bookings | Successful Bookings | Count where PaymentStatus = Paid |
| Bookings | Failed Rate % | Failed / Total × 100 |
| Bookings | Avg Nights Stayed | Average of NightsStayed |
| Bookings | Unique Guests | Distinct count of CustomerID |
| Satisfaction | GSS | Average of (Rating + StaffRating + CleanlinessRating) / 3 |
| Satisfaction | Avg Rating | Average of Rating |
| Satisfaction | Avg Staff Rating | Average of StaffRating |
| Satisfaction | Avg Cleanliness Rating | Average of CleanlinessRating |
| Satisfaction | Positive Feedback % | Positive categories / Total × 100 |
| Comparison | Member Revenue | Revenue for MemberType = Member |
| Comparison | Non-Member Revenue | Revenue for MemberType = Non-Member |
| Time | YoY Growth % | (Current − Prior) / Prior × 100 |
| Time | Revenue YTD / QTD / MTD | Time intelligence accumulations |

---

## 10. Security Requirements

| Requirement | Implementation |
|-------------|----------------|
| RLS by Manager | Single dynamic role filtering dim_Manager[ManagerEmail] = USERPRINCIPALNAME() |
| Executive Access | Users not assigned to any role see all data |
| PII Masking | GuestEmail column removed or masked in Power Query |
| Internal Only | Not published as public app; workspace access controlled |

---

## 11. Assumptions

| # | Assumption | Impact |
|---|------------|--------|
| A-01 | Each hotel has exactly one manager (1:1 mapping) | RLS design |
| A-02 | All reports are internal-only | No public embedding needed |
| A-03 | Power BI Pro licenses available for all users | Publishing & sharing |
| A-04 | BookingID is globally unique across all tables | Join integrity |
| A-05 | All financial values are in Indian Rupees (₹) | Formatting |
| A-06 | Date format is DD-MM-YYYY across all files | Power Query parsing |
| A-07 | PaymentStatus = "Paid" is the qualifier for revenue | Financial accuracy |
| A-08 | Failed/Pending bookings excluded from revenue calcs | Per stakeholder decision |
| A-09 | GSS = simple average of 3 ratings (no weighting) | Per stakeholder decision |
| A-10 | CheckInDate is the primary date for trends | Per stakeholder decision |
| A-11 | Room inventory data is unavailable | Skip Occupancy/RevPAR |
| A-12 | Net Profit = Revenue − Cost − AddSvcCost + AddSvcRev − Discounts | Per stakeholder decision |
| A-13 | 200 CustomerIDs represent returning guests | Repeat guest analysis possible |
| A-14 | Indian fiscal year (April–March) applies | FY in date table |

---

## 12. Constraints

| # | Constraint | Mitigation |
|---|------------|------------|
| C-01 | Data is static (CSV/Excel files, no live DB) | Manual refresh or scheduled from shared drive |
| C-02 | No room inventory → cannot calculate Occupancy/RevPAR | Excluded from scope |
| C-03 | OnlineBooking file has non-UTF-8 encoding | Load with Latin-1 (Windows-1252) encoding |
| C-04 | Finance schema differs from Data Dictionary | Use actual file structure, document deviation |
| C-05 | No Tablet DeviceType in data (despite dictionary mention) | Only Mobile/Desktop supported |
| C-06 | All dates in DD-MM-YYYY string format | Parse in Power Query |
| C-07 | Must use 20+ distinct DAX functions | Design measures intentionally |
| C-08 | Must deliver as .pbix file | No paginated reports |

---

## 13. Risks

| # | Risk | Probability | Impact | Mitigation |
|---|------|-------------|--------|------------|
| R-01 | RLS misconfiguration leaks cross-hotel data | Medium | High | Thorough testing with each manager email |
| R-02 | Date parsing errors for edge cases | Low | Medium | Validate all date columns post-transform |
| R-03 | Performance degradation with complex DAX | Low | Medium | Pre-calculate columns in Power Query where possible |
| R-04 | Data refresh fails on Service | Medium | Medium | Configure gateway, alert on failure |
| R-05 | Users confused by metric definitions | Low | Low | Add tooltips and documentation page |

---

## 14. Dependencies

| # | Dependency | Owner |
|---|------------|-------|
| D-01 | Power BI Desktop (latest version) | Developer |
| D-02 | Power BI Pro license (all users) | IT Admin |
| D-03 | Power BI Service workspace | IT Admin |
| D-04 | Data gateway (if files on network) | IT Admin |
| D-05 | Manager email addresses provisioned in Azure AD | IT Admin |
| D-06 | Source files accessible for refresh | Data Team |

---

## 15. Data Quality Issues Identified

| # | Issue | Table | Severity | Resolution |
|---|-------|-------|----------|------------|
| DQ-01 | Non-UTF-8 encoding (é, ô characters) | OnlineBooking_dim.csv | Medium | Load with encoding=1252 |
| DQ-02 | Schema mismatch vs Data Dictionary | Finance_dim.csv | Low | Use actual file columns; document deviation |
| DQ-03 | PaymentMode documented but absent in file | Finance_dim.csv | Low | Ignore — use PaymentMethod from fact table |
| DQ-04 | No Tablet DeviceType exists | Fact + OnlineBooking | Low | Report only Mobile/Desktop |
| DQ-05 | Dates as text strings (DD-MM-YYYY) | All tables | Medium | Parse in Power Query with custom logic |
| DQ-06 | PII exposure (guest emails) | OnlineBooking_dim.csv | High | Mask or remove in Power Query |
| DQ-07 | All bookings show single Country="India" in some blocks | Fact table | Low | Data generation artifact; actual data has 10 countries |
| DQ-08 | DiscountPercent perfectly evenly distributed (~20K each) | Finance_dim.csv | Low | Synthetic data — acceptable for analysis |

---

## 16. Clarifying Questions (Resolved)

| # | Question | Resolution |
|---|----------|------------|
| Q-01 | Room inventory for Occupancy/RevPAR? | SKIPPED — not available |
| Q-02 | Profit formula? | Revenue − Cost − AddSvcCost + AddSvcRev − Discounts |
| Q-03 | Include Failed/Pending in revenue? | NO — exclude |
| Q-04 | GSS weighting? | Simple average (equal weight) |
| Q-05 | Primary date for trends? | CheckInDate |
| Q-06 | Fiscal year convention? | Indian FY (April–March) |

---

## 17. Report Pages (Planned)

| # | Page Name | Primary Audience | Key Focus |
|---|-----------|------------------|-----------|
| 1 | Executive Summary | Senior Leadership | Portfolio-level KPIs and trends |
| 2 | Revenue & Trends | Finance, Executives | Multi-year revenue analysis |
| 3 | Hotel Manager View | Individual Managers (RLS) | Property-specific performance |
| 4 | Booking Source & Channel | Marketing | Channel effectiveness |
| 5 | Guest Satisfaction | Guest Experience Team | Ratings and feedback |
| 6 | Member vs Non-Member | Strategy, Finance | Membership value analysis |

---

## 18. Approval

| Name | Role | Status |
|------|------|--------|
| [Project Owner] | Project Owner | Pending |
| [Hospitality Head] | Business Sponsor | Pending |
| [BI Lead] | Technical Reviewer | Pending |

---

## 19. Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | June 2026 | AI-DLC Lead | Initial BRD |
| 2.0 | June 2026 | AI-DLC Lead | Complete rewrite with full data profiling, resolved questions |

---

*End of Phase 1: Requirements Analysis*
*Awaiting approval to proceed to Phase 2: Solution Design.*
