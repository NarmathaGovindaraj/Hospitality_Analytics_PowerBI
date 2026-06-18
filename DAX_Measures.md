# DAX Measures
## Complete Implementation-Ready Formulas

---

## Setup: Create Measures Table

```dax
_Measures = ROW("Placeholder", BLANK())
```
Create this as a calculated table. Hide the "Placeholder" column from Report view.
All measures below are added to this `_Measures` table.

---

## 1. Revenue & Financial Measures

### M01: Total Revenue
```dax
Total Revenue = 
CALCULATE(
    SUM(dim_Finance[TotalRevenue]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Sums TotalRevenue from Finance table, filtered to only Paid bookings via the relationship to fct_Bookings.
- **Business Purpose:** Primary revenue metric across all dashboards.
- **Dependencies:** Relationship R4 (fct↔Finance), PaymentStatus column.
- **Format:** ₹#,##0,,"M" (displays in millions)

### M02: Total Cost
```dax
Total Cost = 
CALCULATE(
    SUM(dim_Finance[Cost]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Total operational cost for paid bookings.
- **Business Purpose:** Cost tracking, profit margin calculation.
- **Dependencies:** R4, PaymentStatus.
- **Format:** ₹#,##0,,"M"

### M03: Net Profit
```dax
Net Profit = 
CALCULATE(
    SUMX(
        dim_Finance,
        dim_Finance[TotalRevenue]
        - dim_Finance[Cost]
        - dim_Finance[AdditionalServiceCost]
        + dim_Finance[AdditionalServiceRevenue]
        - dim_Finance[DiscountsGiven]
    ),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Row-by-row profit using the approved formula: Revenue − Cost − AddSvcCost + AddSvcRev − Discounts. SUMX iterates each row in dim_Finance.
- **Business Purpose:** Bottom-line profitability metric.
- **Dependencies:** R4, all Finance columns, PaymentStatus.
- **Format:** ₹#,##0,,"M"

### M04: Profit Margin %
```dax
Profit Margin % = 
DIVIDE(
    [Net Profit],
    [Total Revenue],
    0
) * 100
```
- **Explanation:** Net Profit as a percentage of Total Revenue. DIVIDE handles zero-division safely.
- **Business Purpose:** Efficiency metric — how much of revenue becomes profit.
- **Dependencies:** M01, M03.
- **Format:** 0.0"%"

### M05: ADR (Average Daily Rate)
```dax
ADR = 
DIVIDE(
    [Total Revenue],
    CALCULATE(
        SUM(fct_Bookings[NightsStayed]),
        fct_Bookings[PaymentStatus] = "Paid"
    ),
    0
)
```
- **Explanation:** Total Revenue divided by Total Room Nights Sold.
- **Business Purpose:** Average revenue earned per room-night — key hospitality metric.
- **Dependencies:** M01, NightsStayed column.
- **Format:** ₹#,##0

### M06: Revenue Per Booking
```dax
Revenue Per Booking = 
DIVIDE(
    [Total Revenue],
    [Successful Bookings],
    0
)
```
- **Explanation:** Average revenue per successful transaction.
- **Business Purpose:** Ticket size monitoring.
- **Dependencies:** M01, M12.
- **Format:** ₹#,##0

### M07: Total Tax Collected
```dax
Total Tax = 
CALCULATE(
    SUM(dim_Finance[TaxAmount]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Sum of tax amounts for paid bookings.
- **Business Purpose:** Tax compliance and reporting.
- **Dependencies:** R4, PaymentStatus.
- **Format:** ₹#,##0,,"M"

### M08: Total Discounts Given
```dax
Total Discounts = 
CALCULATE(
    SUM(dim_Finance[DiscountsGiven]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Total monetary discount value across paid bookings.
- **Business Purpose:** Monitor discount impact on revenue.
- **Dependencies:** R4, PaymentStatus.
- **Format:** ₹#,##0,,"M"

### M09: Additional Service Revenue
```dax
Additional Service Revenue = 
CALCULATE(
    SUM(dim_Finance[AdditionalServiceRevenue]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Revenue from spa, food, transport, and other ancillary services.
- **Business Purpose:** Track upselling effectiveness.
- **Dependencies:** R4, PaymentStatus.
- **Format:** ₹#,##0

### M10: Additional Service Cost
```dax
Additional Service Cost = 
CALCULATE(
    SUM(dim_Finance[AdditionalServiceCost]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Cost of providing additional services.
- **Business Purpose:** Monitor service delivery costs.
- **Dependencies:** R4, PaymentStatus.
- **Format:** ₹#,##0

### M11: Average Discount Percentage
```dax
Avg Discount % = 
CALCULATE(
    AVERAGE(dim_Finance[DiscountPercent]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Average discount tier offered to guests.
- **Business Purpose:** Pricing strategy monitoring.
- **Dependencies:** R4, PaymentStatus.
- **Format:** 0.0"%"

---

## 2. Booking & Operational Measures

### M12: Total Bookings
```dax
Total Bookings = 
COUNTROWS(fct_Bookings)
```
- **Explanation:** Count of all booking records regardless of payment status.
- **Business Purpose:** Total demand/volume metric.
- **Dependencies:** None.
- **Format:** #,##0

### M13: Successful Bookings
```dax
Successful Bookings = 
CALCULATE(
    COUNTROWS(fct_Bookings),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Count of paid bookings only.
- **Business Purpose:** Actual converted bookings used in revenue analysis.
- **Dependencies:** PaymentStatus column.
- **Format:** #,##0

### M14: Failed Booking Rate %
```dax
Failed Booking Rate % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(fct_Bookings),
        fct_Bookings[PaymentStatus] = "Failed"
    ),
    [Total Bookings],
    0
) * 100
```
- **Explanation:** Percentage of bookings that resulted in payment failure.
- **Business Purpose:** Payment gateway health monitoring.
- **Dependencies:** M12.
- **Format:** 0.0"%"

### M15: Pending Booking Rate %
```dax
Pending Booking Rate % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(fct_Bookings),
        fct_Bookings[PaymentStatus] = "Pending"
    ),
    [Total Bookings],
    0
) * 100
```
- **Explanation:** Percentage of bookings still pending payment.
- **Business Purpose:** Revenue leakage and conversion monitoring.
- **Dependencies:** M12.
- **Format:** 0.0"%"

### M16: Average Nights Stayed
```dax
Avg Nights Stayed = 
CALCULATE(
    AVERAGE(fct_Bookings[NightsStayed]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Average length of stay for completed bookings.
- **Business Purpose:** Guest behavior analysis.
- **Dependencies:** PaymentStatus.
- **Format:** 0.0

### M17: Total Room Nights Sold
```dax
Total Room Nights = 
CALCULATE(
    SUM(fct_Bookings[NightsStayed]),
    fct_Bookings[PaymentStatus] = "Paid"
)
```
- **Explanation:** Total nights sold — denominator for ADR.
- **Business Purpose:** Volume metric for rate analysis.
- **Dependencies:** PaymentStatus.
- **Format:** #,##0

### M18: Unique Guests
```dax
Unique Guests = 
DISTINCTCOUNT(fct_Bookings[CustomerID])
```
- **Explanation:** Count of unique customer IDs in current context.
- **Business Purpose:** Guest base size monitoring.
- **Dependencies:** None.
- **Format:** #,##0

### M19: Repeat Guest Rate %
```dax
Repeat Guest Rate % = 
VAR TotalGuests = DISTINCTCOUNT(fct_Bookings[CustomerID])
VAR RepeatGuests =
    COUNTROWS(
        FILTER(
            ADDCOLUMNS(
                VALUES(fct_Bookings[CustomerID]),
                "BookingCount", CALCULATE(COUNTROWS(fct_Bookings))
            ),
            [BookingCount] > 1
        )
    )
RETURN
    DIVIDE(RepeatGuests, TotalGuests, 0) * 100
```
- **Explanation:** Percentage of guests who have made more than one booking. Uses VALUES to get distinct customers, ADDCOLUMNS to count bookings per guest, FILTER to keep repeats.
- **Business Purpose:** Loyalty and retention measurement.
- **Dependencies:** None.
- **Format:** 0.0"%"
- **DAX Functions Used:** VAR, DISTINCTCOUNT, COUNTROWS, FILTER, ADDCOLUMNS, VALUES, CALCULATE, DIVIDE

### M20: Avg Booking Lead Time
```dax
Avg Booking Lead Time = 
AVERAGEX(
    fct_Bookings,
    DATEDIFF(
        RELATED(dim_OnlineBooking[BookingDate]),
        fct_Bookings[CheckInDate],
        DAY
    )
)
```
- **Explanation:** Average days between booking date and check-in date.
- **Business Purpose:** Understanding booking behavior and planning windows.
- **Dependencies:** R6 (fct↔OnlineBooking), BookingDate, CheckInDate.
- **Format:** 0 "days"

---

## 3. Guest Satisfaction Measures

### M21: Guest Satisfaction Score (GSS)
```dax
GSS = 
AVERAGE(dim_Feedback[GuestSatisfactionScore])
```
- **Explanation:** Average of the pre-calculated GSS column (which is itself the mean of 3 ratings).
- **Business Purpose:** Primary quality metric across all properties.
- **Dependencies:** R5, GuestSatisfactionScore column in dim_Feedback.
- **Format:** 0.00

### M22: Average Overall Rating
```dax
Avg Rating = 
AVERAGE(dim_Feedback[Rating])
```
- **Explanation:** Average of guest overall rating.
- **Business Purpose:** Individual rating component tracking.
- **Dependencies:** R5.
- **Format:** 0.00

### M23: Average Staff Rating
```dax
Avg Staff Rating = 
AVERAGE(dim_Feedback[StaffRating])
```
- **Explanation:** Average of staff service rating.
- **Business Purpose:** Staff performance monitoring per hotel.
- **Dependencies:** R5.
- **Format:** 0.00

### M24: Average Cleanliness Rating
```dax
Avg Cleanliness Rating = 
AVERAGE(dim_Feedback[CleanlinessRating])
```
- **Explanation:** Average of cleanliness/hygiene rating.
- **Business Purpose:** Housekeeping quality monitoring.
- **Dependencies:** R5.
- **Format:** 0.00

### M25: Positive Feedback %
```dax
Positive Feedback % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(dim_Feedback),
        dim_Feedback[FeedbackSentiment] = "Positive"
    ),
    COUNTROWS(dim_Feedback),
    0
) * 100
```
- **Explanation:** Percentage of feedback classified as positive sentiment.
- **Business Purpose:** Quick gauge of overall guest happiness.
- **Dependencies:** R5, FeedbackSentiment column.
- **Format:** 0.0"%"

### M26: Negative Feedback %
```dax
Negative Feedback % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(dim_Feedback),
        dim_Feedback[FeedbackSentiment] = "Negative"
    ),
    COUNTROWS(dim_Feedback),
    0
) * 100
```
- **Explanation:** Percentage of negative sentiment feedback.
- **Business Purpose:** Service issue identification.
- **Dependencies:** R5, FeedbackSentiment column.
- **Format:** 0.0"%"

---

## 4. Time Intelligence Measures

### M27: Revenue Previous Year
```dax
Revenue PY = 
CALCULATE(
    [Total Revenue],
    SAMEPERIODLASTYEAR(dim_Date[Date])
)
```
- **Explanation:** Total Revenue for the same period in the previous year.
- **Business Purpose:** Year-over-year comparison baseline.
- **Dependencies:** M01, dim_Date, R1.
- **Format:** ₹#,##0,,"M"

### M28: YoY Revenue Growth %
```dax
YoY Revenue Growth % = 
VAR CurrentRevenue = [Total Revenue]
VAR PriorRevenue = [Revenue PY]
RETURN
    DIVIDE(CurrentRevenue - PriorRevenue, PriorRevenue, 0) * 100
```
- **Explanation:** Percentage growth compared to same period last year.
- **Business Purpose:** Growth tracking — most critical trend metric.
- **Dependencies:** M01, M27.
- **Format:** +0.0%;-0.0%

### M29: Revenue MTD (Month-to-Date)
```dax
Revenue MTD = 
CALCULATE(
    [Total Revenue],
    DATESMTD(dim_Date[Date])
)
```
- **Explanation:** Cumulative revenue from the 1st of the current month to current date.
- **Business Purpose:** Intra-month progress tracking.
- **Dependencies:** M01, dim_Date.
- **Format:** ₹#,##0,,"M"

### M30: Revenue QTD (Quarter-to-Date)
```dax
Revenue QTD = 
CALCULATE(
    [Total Revenue],
    DATESQTD(dim_Date[Date])
)
```
- **Explanation:** Cumulative revenue from quarter start to current date.
- **Business Purpose:** Quarterly target tracking.
- **Dependencies:** M01, dim_Date.
- **Format:** ₹#,##0,,"M"

### M31: Revenue YTD (Year-to-Date)
```dax
Revenue YTD = 
CALCULATE(
    [Total Revenue],
    DATESYTD(dim_Date[Date])
)
```
- **Explanation:** Cumulative revenue from Jan 1 to current date.
- **Business Purpose:** Annual progress tracking.
- **Dependencies:** M01, dim_Date.
- **Format:** ₹#,##0,,"M"

### M32: Revenue Fiscal YTD
```dax
Revenue Fiscal YTD = 
CALCULATE(
    [Total Revenue],
    DATESYTD(dim_Date[Date], "3/31")
)
```
- **Explanation:** Cumulative revenue from April 1 (Indian fiscal year start) to current date. The "3/31" parameter tells DATESYTD that year-end is March 31.
- **Business Purpose:** Fiscal year progress tracking (Indian FY convention).
- **Dependencies:** M01, dim_Date.
- **Format:** ₹#,##0,,"M"

### M33: Running Total Revenue
```dax
Running Total Revenue = 
CALCULATE(
    [Total Revenue],
    FILTER(
        ALL(dim_Date[Date]),
        dim_Date[Date] <= MAX(dim_Date[Date])
    )
)
```
- **Explanation:** Cumulative revenue from beginning of data to current point. ALL removes date filters, then re-applies filter for dates up to current maximum.
- **Business Purpose:** Cumulative trend visualization.
- **Dependencies:** M01, dim_Date.
- **Format:** ₹#,##0,,"M"

### M34: Revenue Previous Quarter
```dax
Revenue PQ = 
CALCULATE(
    [Total Revenue],
    DATEADD(dim_Date[Date], -1, QUARTER)
)
```
- **Explanation:** Revenue from the same period shifted back by one quarter.
- **Business Purpose:** Quarter-over-quarter comparison.
- **Dependencies:** M01, dim_Date.
- **Format:** ₹#,##0,,"M"

### M35: QoQ Revenue Growth %
```dax
QoQ Revenue Growth % = 
VAR CurrentQtr = [Total Revenue]
VAR PriorQtr = [Revenue PQ]
RETURN
    DIVIDE(CurrentQtr - PriorQtr, PriorQtr, 0) * 100
```
- **Explanation:** Quarter-over-quarter growth percentage.
- **Business Purpose:** Short-term momentum tracking.
- **Dependencies:** M01, M34.
- **Format:** +0.0%;-0.0%

---

## 5. Comparative & Segmentation Measures

### M36: Member Revenue
```dax
Member Revenue = 
CALCULATE(
    [Total Revenue],
    fct_Bookings[MemberType] = "Member"
)
```
- **Explanation:** Total Revenue filtered to loyalty program members only.
- **Business Purpose:** Measure loyalty program revenue contribution.
- **Dependencies:** M01.
- **Format:** ₹#,##0,,"M"

### M37: Non-Member Revenue
```dax
Non-Member Revenue = 
CALCULATE(
    [Total Revenue],
    fct_Bookings[MemberType] = "Non-Member"
)
```
- **Explanation:** Total Revenue from non-loyalty guests.
- **Business Purpose:** Understand non-member revenue pool (conversion opportunity).
- **Dependencies:** M01.
- **Format:** ₹#,##0,,"M"

### M38: Member Booking %
```dax
Member Booking % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(fct_Bookings),
        fct_Bookings[MemberType] = "Member"
    ),
    COUNTROWS(fct_Bookings),
    0
) * 100
```
- **Explanation:** What percentage of bookings come from loyalty members.
- **Business Purpose:** Loyalty penetration tracking.
- **Dependencies:** None.
- **Format:** 0.0"%"

### M39: Hotel Revenue Rank
```dax
Hotel Revenue Rank = 
RANKX(
    ALL(fct_Bookings[HotelName]),
    [Total Revenue],
    ,
    DESC,
    Dense
)
```
- **Explanation:** Ranks hotels by revenue in descending order (Dense ranking = no gaps).
- **Business Purpose:** Identify top and bottom performing properties.
- **Dependencies:** M01.
- **Format:** #0

### M40: Revenue % of Total
```dax
Revenue % of Total = 
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        ALL(fct_Bookings[HotelName]),
        ALL(fct_Bookings[City]),
        ALL(fct_Bookings[Category])
    ),
    0
) * 100
```
- **Explanation:** Current context revenue as percentage of total across all hotels/cities/categories.
- **Business Purpose:** Market share within portfolio.
- **Dependencies:** M01.
- **Format:** 0.0"%"

### M41: Member Net Profit
```dax
Member Net Profit = 
CALCULATE(
    [Net Profit],
    fct_Bookings[MemberType] = "Member"
)
```
- **Explanation:** Net Profit from member bookings.
- **Business Purpose:** Profitability comparison by membership.
- **Dependencies:** M03.
- **Format:** ₹#,##0,,"M"

### M42: Non-Member Net Profit
```dax
Non-Member Net Profit = 
CALCULATE(
    [Net Profit],
    fct_Bookings[MemberType] = "Non-Member"
)
```
- **Explanation:** Net Profit from non-member bookings.
- **Business Purpose:** Profitability comparison by membership.
- **Dependencies:** M03.
- **Format:** ₹#,##0,,"M"

### M43: Member ADR
```dax
Member ADR = 
CALCULATE(
    [ADR],
    fct_Bookings[MemberType] = "Member"
)
```
- **Explanation:** Average Daily Rate for members.
- **Dependencies:** M05.
- **Format:** ₹#,##0

### M44: Non-Member ADR
```dax
Non-Member ADR = 
CALCULATE(
    [ADR],
    fct_Bookings[MemberType] = "Non-Member"
)
```
- **Explanation:** Average Daily Rate for non-members.
- **Dependencies:** M05.
- **Format:** ₹#,##0

---

## 6. Variance Measures

### M45: Revenue Variance (YoY)
```dax
Revenue Variance YoY = 
[Total Revenue] - [Revenue PY]
```
- **Explanation:** Absolute difference between current and prior year revenue.
- **Business Purpose:** Understand magnitude of growth/decline.
- **Dependencies:** M01, M27.
- **Format:** ₹#,##0,,"M"

### M46: Profit Variance (YoY)
```dax
Profit Variance YoY = 
[Net Profit] - CALCULATE([Net Profit], SAMEPERIODLASTYEAR(dim_Date[Date]))
```
- **Explanation:** Absolute profit change vs same period last year.
- **Business Purpose:** Track profit momentum.
- **Dependencies:** M03, dim_Date.
- **Format:** ₹#,##0,,"M"

### M47: GSS Variance (YoY)
```dax
GSS Variance YoY = 
[GSS] - CALCULATE([GSS], SAMEPERIODLASTYEAR(dim_Date[Date]))
```
- **Explanation:** Change in Guest Satisfaction Score vs prior year.
- **Business Purpose:** Track service quality improvement/decline.
- **Dependencies:** M21, dim_Date.
- **Format:** +0.00;-0.00

---

## 7. Display & Formatting Measures

### M48: Revenue Trend Icon
```dax
Revenue Trend Icon = 
IF(
    [YoY Revenue Growth %] > 0,
    "▲ " & FORMAT([YoY Revenue Growth %], "0.0") & "%",
    IF(
        [YoY Revenue Growth %] < 0,
        "▼ " & FORMAT([YoY Revenue Growth %], "0.0") & "%",
        "● 0.0%"
    )
)
```
- **Explanation:** Returns up/down arrow with growth percentage.
- **Business Purpose:** Visual indicator on KPI cards.
- **Dependencies:** M28.
- **Format:** Text

### M49: GSS Band
```dax
GSS Band = 
SWITCH(
    TRUE(),
    [GSS] >= 4.5, "Excellent",
    [GSS] >= 4.0, "Very Good",
    [GSS] >= 3.5, "Good",
    [GSS] >= 3.0, "Average",
    [GSS] >= 0, "Below Average",
    "No Data"
)
```
- **Explanation:** Categorizes GSS score into named bands.
- **Business Purpose:** Qualitative label for satisfaction dashboards.
- **Dependencies:** M21.
- **Format:** Text

### M50: Manager Title (Dynamic)
```dax
Manager Title = 
VAR MgrName = SELECTEDVALUE(dim_Manager[ManagerName], "All Properties")
VAR HtlName = SELECTEDVALUE(dim_Manager[HotelName], "Portfolio View")
RETURN
    MgrName & " — " & HtlName
```
- **Explanation:** Dynamically shows manager name and hotel based on RLS context.
- **Business Purpose:** Page 3 header personalization.
- **Dependencies:** dim_Manager table.
- **Format:** Text

### M51: Last Refresh Date
```dax
Last Refresh = 
"Data refreshed: " & FORMAT(NOW(), "DD-MMM-YYYY HH:MM")
```
- **Explanation:** Shows current timestamp of last data refresh.
- **Business Purpose:** Data freshness indicator in report footer.
- **Dependencies:** None.
- **Format:** Text

---

## 8. DAX Function Inventory

| # | DAX Function | Measures Using It | Category |
|---|-------------|-------------------|----------|
| 1 | SUM | M01, M02, M07–M10, M17 | Aggregation |
| 2 | CALCULATE | M01–M11, M13–M17, M25–M44 | Filter modification |
| 3 | SUMX | M03 | Iterator |
| 4 | DIVIDE | M04–M06, M11, M14, M15, M19, M25, M26, M28, M35, M38, M40 | Safe division |
| 5 | AVERAGE | M11, M16, M21–M24 | Aggregation |
| 6 | COUNTROWS | M12–M15, M19, M25, M26, M38 | Counting |
| 7 | DISTINCTCOUNT | M18, M19 | Distinct counting |
| 8 | FILTER | M19, M33 | Row filter |
| 9 | ADDCOLUMNS | M19 | Table enrichment |
| 10 | VALUES | M19 | Distinct values table |
| 11 | VAR / RETURN | M19, M28, M35, M50 | Variables |
| 12 | SAMEPERIODLASTYEAR | M27, M46, M47 | Time intelligence |
| 13 | DATESMTD | M29 | Month-to-date |
| 14 | DATESQTD | M30 | Quarter-to-date |
| 15 | DATESYTD | M31, M32 | Year-to-date |
| 16 | DATEADD | M34 | Date shifting |
| 17 | ALL | M33, M39, M40 | Remove filters |
| 18 | MAX | M33 | Maximum value |
| 19 | RANKX | M39 | Ranking |
| 20 | AVERAGEX | M20 | Row-level average |
| 21 | DATEDIFF | M20 | Date difference |
| 22 | RELATED | M20 | Lookup |
| 23 | IF | M48 | Conditional |
| 24 | SWITCH | M49 | Multi-condition |
| 25 | TRUE | M49 | Boolean |
| 26 | SELECTEDVALUE | M50 | Context value |
| 27 | FORMAT | M48, M51 | Text formatting |
| 28 | NOW | M51 | Current datetime |
| 29 | ROW | Measures table | Table creation |
| 30 | BLANK | Measures table | Null value |

**Total: 30 distinct DAX functions used** (exceeds 20 minimum by 50%)
