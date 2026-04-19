# Power Query Recipes & Transformations

Ready-to-use Power Query M code snippets for common data transformation tasks. Copy and adapt to your needs.

**Reference:** [Microsoft Learn - Power Query Editor](https://learn.microsoft.com/en-us/training/modules/work-with-power-query-in-power-bi-desktop/)

---

## 🚀 How to Use This Guide

1. **Open Power Query Editor:** Home tab → Transform data
2. **Create new step:** Right-click in query steps (left pane) → Copy step
3. **Paste recipe:** Replace with code from this guide
4. **Customize:** Update table names, column names
5. **Test:** Apply → See results

---

## 📋 Quick Navigation

- **Cleaning:** Remove nulls, trim spaces, standardize case
- **Filtering:** Remove rows, keep specific values
- **Transformations:** Split columns, extract dates, create new columns
- **Aggregations:** Group, sum, count data
- **Joining:** Combine tables

---

## 🧹 CLEANING RECIPES

### 1.1 Remove Null/Empty Rows

**Problem:** Data has empty rows causing errors  
**Solution:**

```sql
= Table.SelectRows(Source, each [Column_Name] <> null and [Column_Name] <> "")
```

**How to use:**
1. Replace `Source` with previous step name (check left pane)
2. Replace `Column_Name` with your column name
3. Apply → Empty rows removed

**Example:**
```sql
= Table.SelectRows(TransactionData, each [Amount] <> null and [Amount] <> "")
```

---

### 1.2 Remove Duplicate Rows

**Problem:** Data has duplicate transactions  
**Solution:**

```sql
= Table.Distinct(Source, {"Column1", "Column2"})
```

**How to use:**
- Replace `{"Column1", "Column2"}` with columns that define uniqueness
- If you want all columns unique: `Table.Distinct(Source)`

**Example:**
```sql
= Table.Distinct(Transactions, {"TransactionID", "Date"})
```

---

### 1.3 Trim Whitespace (Leading/Trailing Spaces)

**Problem:** Customer names have extra spaces: "  John  "  
**Solution:**

```sql
= Table.TransformColumns(Source, {{"Column_Name", Text.Trim}})
```

**Example:**
```sql
= Table.TransformColumns(Customers, {{"CustomerName", Text.Trim}})
```

**Result:** "  John  " → "John"

---

### 1.4 Standardize Text Case

**Problem:** Names inconsistent: "JOHN", "john", "John"  
**Solution (Proper Case):**

```sql
= Table.TransformColumns(Source, {{"Column_Name", Text.Proper}})
```

**Other Options:**
```sql
Text.Upper([Column])    -- ALL CAPS
Text.Lower([Column])    -- all lowercase
Text.Proper([Column])   -- Proper Case
```

**Example:**
```sql
= Table.TransformColumns(Customers, {{"CustomerName", Text.Proper}})
```

**Result:** "JOHN", "john", "John" → "John"

---

### 1.5 Replace Values

**Problem:** Null values should be "Unknown"  
**Solution:**

```sql
= Table.ReplaceValue(Source, null, "Unknown", Replacer.ReplaceValue, {"Column_Name"})
```

**How to use:**
- First parameter: What to replace (null, "", "N/A", etc.)
- Second parameter: What to replace with
- Last parameter: Column names

**Example:**
```sql
= Table.ReplaceValue(Accounts, null, "Active", Replacer.ReplaceValue, {"Status"})
```

**Result:** NULL Status → "Active"

---

## 🔍 FILTERING RECIPES

### 2.1 Remove Rows with Specific Value

**Problem:** Want to exclude test transactions  
**Solution:**

```sql
= Table.SelectRows(Source, each [Column_Name] <> "Test")
```

**Example:**
```sql
= Table.SelectRows(Transactions, each [TransactionType] <> "Test")
```

**Result:** Test transactions removed

---

### 2.2 Keep Only Specific Values

**Problem:** Only want Active accounts  
**Solution:**

```sql
= Table.SelectRows(Source, each [Column_Name] = "Active")
```

**Example:**
```sql
= Table.SelectRows(Accounts, each [Status] = "Active")
```

**Result:** Only Active rows remain

---

### 2.3 Filter by Number Range

**Problem:** Only want transactions over €1000  
**Solution:**

```sql
= Table.SelectRows(Source, each [Amount] >= 1000 and [Amount] <= 10000)
```

**Example:**
```sql
= Table.SelectRows(Transactions, each [Amount] >= 1000)
```

**Result:** Only transactions €1000+ shown

---

### 2.4 Filter by Date Range

**Problem:** Only want last 12 months  
**Solution:**

```sql
= Table.SelectRows(Source, each [Date] >= Date.AddYears(Date.Today(), -1))
```

**Example:**
```sql
= Table.SelectRows(Transactions, each [Date] >= Date.AddMonths(Date.Today(), -12))
```

**Result:** Only transactions from last 12 months

---

### 2.5 Filter Text Containing Value

**Problem:** Only want emails from domain  
**Solution:**

```sql
= Table.SelectRows(Source, each Text.Contains([Email], "@domain.com"))
```

**Example:**
```sql
= Table.SelectRows(Customers, each Text.Contains([Email], "@company.com"))
```

**Result:** Only @company.com emails

---

## 🔄 TRANSFORMATION RECIPES

### 3.1 Split Column by Delimiter

**Problem:** Full name in one column, need first/last separate  
**Solution:**

```sql
= Table.ExpandListColumn(
    Table.TransformColumns(
        Source,
        {{"FullName", Splitter.SplitTextByDelimiter(" ", QuoteStyle.Csv), let itemType = (type nullable text) meta [Serialized.Text = true] in type {itemType}}}
    ),
    "FullName"
)
```

**Simpler alternative (UI approach):**
1. Select FullName column
2. Home tab → Split Column → By Delimiter
3. Choose " " (space)

**Result:** "John Smith" → FirstName = "John", LastName = "Smith"

---

### 3.2 Extract Text Before Character

**Problem:** Extract account number from "ACC-12345"  
**Solution:**

```sql
= Table.AddColumn(Source, "AccountNumber", each Text.BetweenDelimiters([OriginalText], "", "-"))
```

**Or using Text.Before:**
```sql
= Table.AddColumn(Source, "Prefix", each Text.Before([Code], "-"))
```

**Example:**
```sql
= Table.AddColumn(Accounts, "Prefix", each Text.Before([AccountCode], "-"))
```

**Result:** "ACC-12345" → Prefix = "ACC"

---

### 3.3 Extract Text After Character

**Problem:** Extract number from "ACC-12345"  
**Solution:**

```sql
= Table.AddColumn(Source, "Number", each Text.After([Code], "-"))
```

**Example:**
```sql
= Table.AddColumn(Accounts, "Number", each Text.After([AccountCode], "-"))
```

**Result:** "ACC-12345" → Number = "12345"

---

### 3.4 Create Date from Text

**Problem:** Date stored as "20240101" (YYYYMMDD), need Date type  
**Solution:**

```sql
= Table.TransformColumns(Source, {{"DateText", each Date.FromText(Text.Range(_, 0, 4) & "-" & Text.Range(_, 4, 2) & "-" & Text.Range(_, 6, 2))}})
```

**Simpler:** Use UI
1. Select column
2. Home tab → Data Type → Date

**Result:** "20240101" (text) → 1/1/2024 (date)

---

### 3.5 Extract Year/Month from Date

**Problem:** Need Year and Month as separate columns  
**Solution:**

```sql
= Table.AddColumn(
    Table.AddColumn(Source, "Year", each Date.Year([Date])),
    "Month", each Date.Month([Date])
)
```

**Result:** New columns "Year" and "Month" added

---

### 3.6 Create Age Bucket

**Problem:** Convert age (25, 35, 45) to bucket ("25-34", "35-44")  
**Solution:**

```sql
= Table.AddColumn(Source, "AgeBucket", each 
    if [Age] < 25 then "Under 25"
    else if [Age] < 35 then "25-34"
    else if [Age] < 45 then "35-44"
    else "45+"
)
```

**Result:** Age 28 → "25-34", Age 50 → "45+"

---

## 📊 AGGREGATION RECIPES

### 4.1 Group and Sum

**Problem:** Sum sales by customer  
**Solution:**

```sql
= Table.Group(Source, {"CustomerID"}, {{"TotalSales", each List.Sum([Amount]), type number}})
```

**Example:**
```sql
= Table.Group(Transactions, {"CustomerID"}, {{"TotalSales", each List.Sum([Amount]), type number}})
```

**Result:** 
| CustomerID | TotalSales |
|-----------|-----------|
| 1 | €5000 |
| 2 | €3500 |

---

### 4.2 Group and Count

**Problem:** Count transactions by customer  
**Solution:**

```sql
= Table.Group(Source, {"CustomerID"}, {{"TransactionCount", each List.Count([ID]), type number}})
```

**Result:** 
| CustomerID | TransactionCount |
|-----------|-----------------|
| 1 | 25 |
| 2 | 18 |

---

### 4.3 Group and Average

**Problem:** Average transaction amount by product  
**Solution:**

```sql
= Table.Group(Source, {"Product"}, {{"AvgAmount", each List.Average([Amount]), type number}})
```

**Result:** Avg transaction per product

---

## 🔗 JOINING RECIPES

### 5.1 Inner Join (Only Matching Rows)

**Problem:** Combine customers with their transactions  
**Solution:**

```sql
= Table.Join(Transactions, {"CustomerID"}, Customers, {"ID"}, JoinKind.Inner)
```

**Result:** Only customers with transactions appear

---

### 5.2 Left Outer Join (Keep All Left Table Rows)

**Problem:** All customers, even if no transactions  
**Solution:**

```sql
= Table.Join(Customers, {"ID"}, Transactions, {"CustomerID"}, JoinKind.LeftOuter)
```

**Result:** All customers shown; transactions NULL if none

---

### 5.3 Lookup Value from Another Table

**Problem:** Add customer name to transaction  
**Solution:**

```sql
= Table.AddColumn(Transactions, "CustomerName", each 
    Table.SelectRows(Customers, each [ID] = [CustomerID])[CustomerName]{0},
    type text)
```

**Result:** Transaction now has customer name

---

## 📅 DATE TABLE RECIPE

### 6.1 Create Complete Date Table (1 Year)

**Problem:** No date dimension exists; need one for date slicers  
**Solution:**

```m
let
    Start = #date(2024, 1, 1),
    End = #date(2024, 12, 31),
    Count = Duration.Days(End - Start) + 1,
    Dates = List.Dates(Start, Count, #duration(1, 0, 0, 0)),
    DateTable = Table.FromList(Dates, Splitter.SplitByNothing(), {"Date"}),
    #"Added Year" = Table.AddColumn(DateTable, "Year", each Date.Year([Date]), Int64.Type),
    #"Added Month" = Table.AddColumn(#"Added Year", "Month", each Date.Month([Date]), Int64.Type),
    #"Added Quarter" = Table.AddColumn(#"Added Month", "Quarter", each "Q" & Text.From(Date.QuarterOfYear([Date])), type text),
    #"Added Day of Week" = Table.AddColumn(#"Added Quarter", "DayOfWeek", each Date.DayOfWeek([Date]), Int64.Type),
    #"Added Month Name" = Table.AddColumn(#"Added Day of Week", "MonthName", each Text.Proper(Date.ToText([Date], "mmmm")), type text)
in
    #"Added Month Name"
```

**How to use:**
1. **Home → New Source → Blank Query**
2. **Paste code above**
3. Adjust dates (Start, End) to your range
4. **Close & Apply**

**Result:** Date table with Year, Month, Quarter, DayOfWeek, MonthName

---

## ✅ QUALITY CHECKS

After each transformation:

- [ ] No error messages (red X)
- [ ] Data types correct (icons show correct types)
- [ ] Row count reasonable (didn't lose too many rows)
- [ ] Sample data looks right (preview tab)
- [ ] Applied steps make sense (left pane readable)

---

## 🐛 Troubleshooting

### "Query Error: Token Parsing Error"
- **Cause:** Syntax error (missing comma, bracket, etc.)
- **Fix:** Check brackets and commas match exactly
- **Reference:** Microsoft Learn - Power Query M syntax

### "Column Not Found"
- **Cause:** Column name doesn't exist or misspelled
- **Fix:** Check exact column name (case-sensitive)
- **Verification:** Right-click table → View data → see column names

### "Type Error"
- **Cause:** Operation expects one type (e.g., Text), got another (e.g., Number)
- **Fix:** Convert type first using `Value()`, `Text.From()`, etc.

---

## 🔗 Resources

- **Power Query M Reference:** https://learn.microsoft.com/en-us/powerquery-m/
- **Text Functions:** https://learn.microsoft.com/en-us/powerquery-m/text-functions

---

**Questions? See [Troubleshooting.md](Troubleshooting.md) or Microsoft Learn references above**
