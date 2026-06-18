# Power Query (M) Transformations
## Complete Implementation-Ready Code

---

## 1. fct_Bookings — Central Fact Table

**Source:** Hospitality_Data_Global_fct.csv  
**Encoding:** UTF-8  
**Records:** 100,000  

```powerquery-m
let
    // Step 1: Load CSV file
    Source = Csv.Document(
        File.Contents("C:\Data\Hospitality_Datasets\Hospitality_Data_Global_fct.csv"),
        [Delimiter = ",", Columns = 21, Encoding = 65001, QuoteStyle = QuoteStyle.None]
    ),

    // Step 2: Promote first row as headers
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars = true]),

    // Step 3: Set initial data types (text for dates, proper types for others)
    InitialTypes = Table.TransformColumnTypes(PromotedHeaders, {
        {"BookingID", Int64.Type},
        {"CustomerID", type text},
        {"FirstName", type text},
        {"LastName", type text},
        {"GuestName", type text},
        {"Gender", type text},
        {"Age", Int64.Type},
        {"Country", type text},
        {"RoomType", type text},
        {"CheckInDate", type text},
        {"CheckOutDate", type text},
        {"NightsStayed", Int64.Type},
        {"RatePerNight", type number},
        {"MemberType", type text},
        {"HotelName", type text},
        {"City", type text},
        {"Category", type text},
        {"PaymentMethod", type text},
        {"Channel", type text},
        {"DeviceType", type text},
        {"PaymentStatus", type text}
    }),

    // Step 4: Parse CheckInDate from DD-MM-YYYY text to Date type
    ParseCheckIn = Table.TransformColumns(InitialTypes, {
        {"CheckInDate", each
            let
                parts = Text.Split(_, "-"),
                day = Number.From(parts{0}),
                month = Number.From(parts{1}),
                year = Number.From(parts{2})
            in
                #date(year, month, day),
        type date}
    }),

    // Step 5: Parse CheckOutDate from DD-MM-YYYY text to Date type
    ParseCheckOut = Table.TransformColumns(ParseCheckIn, {
        {"CheckOutDate", each
            let
                parts = Text.Split(_, "-"),
                day = Number.From(parts{0}),
                month = Number.From(parts{1}),
                year = Number.From(parts{2})
            in
                #date(year, month, day),
        type date}
    }),

    // Step 6: Handle potential nulls (replace nulls with defaults)
    HandleNulls = Table.ReplaceValue(ParseCheckOut, null, "Unknown",
        Replacer.ReplaceValue, {"Gender", "Country", "RoomType", "MemberType",
        "PaymentMethod", "Channel", "DeviceType", "PaymentStatus"}),

    // Step 7: Add calculated column - RoomRevenue
    AddRoomRevenue = Table.AddColumn(HandleNulls, "RoomRevenue",
        each [NightsStayed] * [RatePerNight], type number),

    // Step 8: Add calculated column - IsPaid (helper for efficient filtering)
    AddIsPaid = Table.AddColumn(AddRoomRevenue, "IsPaid",
        each if [PaymentStatus] = "Paid" then 1 else 0, Int64.Type),

    // Step 9: Trim whitespace from text columns
    TrimmedText = Table.TransformColumns(AddIsPaid, {
        {"HotelName", Text.Trim, type text},
        {"City", Text.Trim, type text},
        {"Category", Text.Trim, type text},
        {"Channel", Text.Trim, type text},
        {"PaymentStatus", Text.Trim, type text},
        {"MemberType", Text.Trim, type text},
        {"RoomType", Text.Trim, type text}
    }),

    // Step 10: Remove exact duplicate rows (if any)
    RemovedDuplicates = Table.Distinct(TrimmedText, {"BookingID"}),

    // Step 11: Final column order
    ReorderedColumns = Table.ReorderColumns(RemovedDuplicates, {
        "BookingID", "CustomerID", "GuestName", "FirstName", "LastName",
        "Gender", "Age", "Country", "RoomType", "CheckInDate", "CheckOutDate",
        "NightsStayed", "RatePerNight", "RoomRevenue", "MemberType",
        "HotelName", "City", "Category", "PaymentMethod", "Channel",
        "DeviceType", "PaymentStatus", "IsPaid"
    })

in
    ReorderedColumns
```

---

## 2. dim_Finance

**Source:** Finance_dim.csv  
**Encoding:** UTF-8  
**Records:** 100,000  

```powerquery-m
let
    // Step 1: Load CSV
    Source = Csv.Document(
        File.Contents("C:\Data\Hospitality_Datasets\Finance_dim.csv"),
        [Delimiter = ",", Columns = 9, Encoding = 65001, QuoteStyle = QuoteStyle.None]
    ),

    // Step 2: Promote headers
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars = true]),

    // Step 3: Set data types
    TypedColumns = Table.TransformColumnTypes(PromotedHeaders, {
        {"BookingID", Int64.Type},
        {"CustomerID", type text},
        {"DiscountPercent", Int64.Type},
        {"TaxAmount", type number},
        {"TotalRevenue", type number},
        {"Cost", type number},
        {"AdditionalServiceCost", type number},
        {"AdditionalServiceRevenue", type number},
        {"DiscountsGiven", type number}
    }),

    // Step 4: Handle nulls - replace with 0 for numeric columns
    HandleNulls = Table.ReplaceValue(TypedColumns, null, 0,
        Replacer.ReplaceValue, {"DiscountPercent", "TaxAmount", "TotalRevenue",
        "Cost", "AdditionalServiceCost", "AdditionalServiceRevenue", "DiscountsGiven"}),

    // Step 5: Add NetProfit calculated column
    AddNetProfit = Table.AddColumn(HandleNulls, "NetProfit",
        each [TotalRevenue] - [Cost] - [AdditionalServiceCost]
             + [AdditionalServiceRevenue] - [DiscountsGiven],
        type number),

    // Step 6: Add ProfitMarginPct calculated column
    AddProfitMargin = Table.AddColumn(AddNetProfit, "ProfitMarginPct",
        each if [TotalRevenue] = 0 or [TotalRevenue] = null then 0
             else ([NetProfit] / [TotalRevenue]) * 100,
        type number),

    // Step 7: Remove duplicate BookingIDs (keep first occurrence)
    RemovedDuplicates = Table.Distinct(AddProfitMargin, {"BookingID"})

in
    RemovedDuplicates
```

---

## 3. dim_Feedback

**Source:** Feedback_dim.csv  
**Encoding:** UTF-8  
**Records:** 100,000  

```powerquery-m
let
    // Step 1: Load CSV
    Source = Csv.Document(
        File.Contents("C:\Data\Hospitality_Datasets\Feedback_dim.csv"),
        [Delimiter = ",", Columns = 7, Encoding = 65001, QuoteStyle = QuoteStyle.None]
    ),

    // Step 2: Promote headers
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars = true]),

    // Step 3: Set data types
    TypedColumns = Table.TransformColumnTypes(PromotedHeaders, {
        {"BookingID", Int64.Type},
        {"CustomerID", type text},
        {"Rating", type number},
        {"Feedback", type text},
        {"SubmittedDate", type text},
        {"StaffRating", type number},
        {"CleanlinessRating", type number}
    }),

    // Step 4: Parse SubmittedDate from DD-MM-YYYY
    ParsedDate = Table.TransformColumns(TypedColumns, {
        {"SubmittedDate", each
            let
                parts = Text.Split(_, "-"),
                day = Number.From(parts{0}),
                month = Number.From(parts{1}),
                year = Number.From(parts{2})
            in
                #date(year, month, day),
        type date}
    }),

    // Step 5: Handle null ratings (replace with 0)
    HandleNulls = Table.ReplaceValue(ParsedDate, null, 0,
        Replacer.ReplaceValue, {"Rating", "StaffRating", "CleanlinessRating"}),

    // Step 6: Add GuestSatisfactionScore (simple average of 3 ratings)
    AddGSS = Table.AddColumn(HandleNulls, "GuestSatisfactionScore",
        each ([Rating] + [StaffRating] + [CleanlinessRating]) / 3,
        type number),

    // Step 7: Add FeedbackSentiment classification
    AddSentiment = Table.AddColumn(AddGSS, "FeedbackSentiment",
        each if List.Contains({"Average", "Could be better", "Check-in was slow"}, [Feedback])
             then "Negative"
             else "Positive",
        type text),

    // Step 8: Trim feedback text
    TrimFeedback = Table.TransformColumns(AddSentiment, {
        {"Feedback", Text.Trim, type text}
    }),

    // Step 9: Remove duplicates
    RemovedDuplicates = Table.Distinct(TrimFeedback, {"BookingID"})

in
    RemovedDuplicates
```

---

## 4. dim_OnlineBooking

**Source:** OnlineBooking_dim.csv  
**Encoding:** Windows-1252 (Latin-1) — contains special characters  
**Records:** 100,000  

```powerquery-m
let
    // Step 1: Load CSV with Latin-1 encoding to handle special chars (é, ô, etc.)
    Source = Csv.Document(
        File.Contents("C:\Data\Hospitality_Datasets\OnlineBooking_dim.csv"),
        [Delimiter = ",", Columns = 7, Encoding = 1252, QuoteStyle = QuoteStyle.None]
    ),

    // Step 2: Promote headers
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars = true]),

    // Step 3: Set data types
    TypedColumns = Table.TransformColumnTypes(PromotedHeaders, {
        {"BookingID", Int64.Type},
        {"CustomerID", type text},
        {"Channel", type text},
        {"GuestEmail", type text},
        {"BookingDate", type text},
        {"Location", type text},
        {"DeviceType", type text}
    }),

    // Step 4: Parse BookingDate from DD-MM-YYYY
    ParsedDate = Table.TransformColumns(TypedColumns, {
        {"BookingDate", each
            let
                parts = Text.Split(_, "-"),
                day = Number.From(parts{0}),
                month = Number.From(parts{1}),
                year = Number.From(parts{2})
            in
                #date(year, month, day),
        type date}
    }),

    // Step 5: Mask PII - GuestEmail
    // Replace with first 3 chars + "***@" + domain
    MaskedEmail = Table.TransformColumns(ParsedDate, {
        {"GuestEmail", each
            let
                atPos = Text.PositionOf(_, "@"),
                prefix = if atPos > 3 then Text.Start(_, 3) else Text.Start(_, atPos),
                domain = Text.AfterDelimiter(_, "@")
            in
                prefix & "***@" & domain,
        type text}
    }),

    // Step 6: Trim text columns
    TrimmedText = Table.TransformColumns(MaskedEmail, {
        {"Channel", Text.Trim, type text},
        {"Location", Text.Trim, type text},
        {"DeviceType", Text.Trim, type text}
    }),

    // Step 7: Remove duplicates
    RemovedDuplicates = Table.Distinct(TrimmedText, {"BookingID"})

in
    RemovedDuplicates
```

---

## 5. dim_Hotel

**Source:** Hotel_List_dim.xlsx  
**Records:** 7  

```powerquery-m
let
    // Step 1: Load Excel workbook
    Source = Excel.Workbook(
        File.Contents("C:\Data\Hospitality_Datasets\Hotel_List_dim.xlsx"),
        null, true
    ),

    // Step 2: Navigate to Sheet1
    Sheet1 = Source{[Item = "Sheet1", Kind = "Sheet"]}[Data],

    // Step 3: Promote headers
    PromotedHeaders = Table.PromoteHeaders(Sheet1, [PromoteAllScalars = true]),

    // Step 4: Set data types
    TypedColumns = Table.TransformColumnTypes(PromotedHeaders, {
        {"Hotel_Name", type text},
        {"City", type text},
        {"Category", type text}
    }),

    // Step 5: Trim whitespace
    TrimmedText = Table.TransformColumns(TypedColumns, {
        {"Hotel_Name", Text.Trim, type text},
        {"City", Text.Trim, type text},
        {"Category", Text.Trim, type text}
    })

in
    TrimmedText
```

---

## 6. dim_Manager

**Source:** Manager_Details_dim.xlsx  
**Records:** 7  

```powerquery-m
let
    // Step 1: Load Excel workbook
    Source = Excel.Workbook(
        File.Contents("C:\Data\Hospitality_Datasets\Manager_Details_dim.xlsx"),
        null, true
    ),

    // Step 2: Navigate to Sheet1
    Sheet1 = Source{[Item = "Sheet1", Kind = "Sheet"]}[Data],

    // Step 3: Promote headers
    PromotedHeaders = Table.PromoteHeaders(Sheet1, [PromoteAllScalars = true]),

    // Step 4: Set data types
    TypedColumns = Table.TransformColumnTypes(PromotedHeaders, {
        {"HotelName", type text},
        {"ManagerID", type text},
        {"ManagerName", type text},
        {"ManagerEmail", type text}
    }),

    // Step 5: Trim whitespace
    TrimmedText = Table.TransformColumns(TypedColumns, {
        {"HotelName", Text.Trim, type text},
        {"ManagerName", Text.Trim, type text},
        {"ManagerEmail", each Text.Lower(Text.Trim(_)), type text}
    })

in
    TrimmedText
```

---

## 7. dim_Date — Generated Calendar Table

**Source:** Power Query generated  
**Range:** 01-Jan-2021 to 31-Dec-2025  
**Records:** 1,826  

```powerquery-m
let
    // Step 1: Define date range
    StartDate = #date(2021, 1, 1),
    EndDate = #date(2025, 12, 31),
    DayCount = Duration.Days(EndDate - StartDate) + 1,

    // Step 2: Generate list of dates
    DateList = List.Dates(StartDate, DayCount, #duration(1, 0, 0, 0)),

    // Step 3: Convert to table
    DateTable = Table.FromList(DateList, Splitter.SplitByNothing(),
        {"Date"}, null, ExtraValues.Error),

    // Step 4: Set Date column type
    TypedDate = Table.TransformColumnTypes(DateTable, {{"Date", type date}}),

    // Step 5: Add Year
    AddYear = Table.AddColumn(TypedDate, "Year",
        each Date.Year([Date]), Int64.Type),

    // Step 6: Add Quarter
    AddQuarter = Table.AddColumn(AddYear, "Quarter",
        each Date.QuarterOfYear([Date]), Int64.Type),

    // Step 7: Add QuarterLabel
    AddQuarterLabel = Table.AddColumn(AddQuarter, "QuarterLabel",
        each "Q" & Text.From(Date.QuarterOfYear([Date])), type text),

    // Step 8: Add Month number
    AddMonth = Table.AddColumn(AddQuarterLabel, "Month",
        each Date.Month([Date]), Int64.Type),

    // Step 9: Add MonthName
    AddMonthName = Table.AddColumn(AddMonth, "MonthName",
        each Date.MonthName([Date]), type text),

    // Step 10: Add MonthShort (3-letter)
    AddMonthShort = Table.AddColumn(AddMonthName, "MonthShort",
        each Text.Start(Date.MonthName([Date]), 3), type text),

    // Step 11: Add DayOfMonth
    AddDayOfMonth = Table.AddColumn(AddMonthShort, "DayOfMonth",
        each Date.Day([Date]), Int64.Type),

    // Step 12: Add WeekDay (Monday = 1)
    AddWeekDay = Table.AddColumn(AddDayOfMonth, "WeekDay",
        each Date.DayOfWeek([Date], Day.Monday) + 1, Int64.Type),

    // Step 13: Add WeekDayName
    AddWeekDayName = Table.AddColumn(AddWeekDay, "WeekDayName",
        each Date.DayOfWeekName([Date]), type text),

    // Step 14: Add IsWeekend flag
    AddIsWeekend = Table.AddColumn(AddWeekDayName, "IsWeekend",
        each Date.DayOfWeek([Date], Day.Monday) >= 5, type logical),

    // Step 15: Add YearMonth (sortable: "2021-01")
    AddYearMonth = Table.AddColumn(AddIsWeekend, "YearMonth",
        each Text.From(Date.Year([Date])) & "-" &
             Text.PadStart(Text.From(Date.Month([Date])), 2, "0"),
        type text),

    // Step 16: Add YearQuarter ("2021-Q1")
    AddYearQuarter = Table.AddColumn(AddYearMonth, "YearQuarter",
        each Text.From(Date.Year([Date])) & "-Q" &
             Text.From(Date.QuarterOfYear([Date])),
        type text),

    // Step 17: Add FiscalYear (Indian: Apr-Mar, FY label = end year)
    AddFiscalYear = Table.AddColumn(AddYearQuarter, "FiscalYear",
        each if Date.Month([Date]) >= 4
             then Date.Year([Date]) + 1
             else Date.Year([Date]),
        Int64.Type),

    // Step 18: Add FiscalQuarter
    AddFiscalQuarter = Table.AddColumn(AddFiscalYear, "FiscalQuarter",
        each let m = Date.Month([Date]) in
            if m >= 4 and m <= 6 then 1
            else if m >= 7 and m <= 9 then 2
            else if m >= 10 and m <= 12 then 3
            else 4,
        Int64.Type),

    // Step 19: Add FiscalYearLabel ("FY22")
    AddFYLabel = Table.AddColumn(AddFiscalQuarter, "FiscalYearLabel",
        each "FY" & Text.End(Text.From([FiscalYear]), 2), type text),

    // Step 20: Add MonthYear display ("Jan 2021")
    AddMonthYear = Table.AddColumn(AddFYLabel, "MonthYear",
        each Text.Start(Date.MonthName([Date]), 3) & " " &
             Text.From(Date.Year([Date])),
        type text)

in
    AddMonthYear
```

---

## 8. Power Query Transformation Summary

| Table | Steps | Key Transformations |
|-------|-------|---------------------|
| fct_Bookings | 11 | Date parsing, null handling, RoomRevenue calc, IsPaid flag, dedup |
| dim_Finance | 7 | Type casting, null→0, NetProfit calc, ProfitMargin calc, dedup |
| dim_Feedback | 9 | Date parsing, null→0, GSS calc, Sentiment classification, dedup |
| dim_OnlineBooking | 7 | Latin-1 encoding, date parsing, PII masking, dedup |
| dim_Hotel | 5 | Type casting, text trimming |
| dim_Manager | 5 | Type casting, email lowercase, text trimming |
| dim_Date | 20 | Full calendar generation with fiscal year support |
