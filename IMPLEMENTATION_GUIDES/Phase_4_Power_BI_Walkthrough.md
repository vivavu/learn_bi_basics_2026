# Phase 4: Power BI Implementation Walkthrough

Detailed, step-by-step guide for building the financial dashboard in Power BI Desktop. This guide complements the [dashboard_development_prompt.md](../contents/4_dashboarding/dashboard_development_prompt.md).

---

## 🎯 Overview

**What You'll Build:** Production-ready financial dashboard with:
- KPI cards (Portfolio Health, Total Loans, etc.)
- Interactive slicers (Date range, Account Type, Status)
- Drill-through capabilities
- Performance-optimized refresh

**Time Estimate:** 3-4 hours  
**Audience:** Entry-level developers who have completed Phase 1-3  
**Reference:** [Microsoft Learn - Power BI Desktop Fundamentals](https://learn.microsoft.com/en-us/training/modules/get-started-with-power-bi/)

---

## 📋 Pre-Requisites

Before starting, ensure you have:
- [ ] Power BI Desktop (latest version) installed
- [ ] CSV/database connection configured (see Phase 4 prompt for data sources)
- [ ] Phase 1-3 deliverables completed (requirements, data model, visualization spec)
- [ ] Sample financial data downloaded (Czech dataset or equivalent)

---

## Section 1: Data Connection & Preparation (30 minutes)

### 1.1 Open Power BI and Connect to Data

**Step 1: Launch Power BI Desktop**
- Double-click Power BI Desktop icon
- **File → New** (or Ctrl + N)

**Step 2: Get Data**
- **Home ribbon → Get data dropdown (∨) → Text/CSV** (or your data source)
  
  ![Data types: CSV, Excel, SQL Server, etc.]

**Step 3: Browse to Data File**
- Navigate to your data location
- Select the data file (e.g., `transactions.csv`)
- Click **Open**

**Step 4: Preview and Load**
- Power Query shows data preview
- Verify columns are correct (no corrupted rows visible)
- Click **Load** (bottom-right)
  
  **Result:** Power BI imports data into a table

**Timing:** 5 minutes

---

### 1.2 Repeat for Multiple Tables (if needed)

If your financial data is in multiple CSVs (transactions, accounts, dimensions):

1. **Home → New source → Text/CSV** (repeat for each file)
2. **Load each file** as separate table
3. **Verify in Model view** (you should see multiple tables listed)

**Timing:** 10 minutes

---

### 1.3 Set Data Types

Ensure Power BI recognizes data types correctly:

1. **Home tab → Transform data** (opens Power Query Editor)
2. **Select each column and verify data type:**
   - Dates: Should show calendar icon 📅 (type: "Date")
   - Currency: Should show "$" icon (type: "Decimal Number")
   - Categories: Should show "abc" icon (type: "Text")
   
3. **If type is wrong:** Click column header → Data Type dropdown (top ribbon)
   - Change to correct type (e.g., Date, Whole Number, Decimal, Text)

4. **Home tab → Close & Apply** (saves Power Query changes)

**Result:** Data types are correctly interpreted for calculations  
**Timing:** 10 minutes

---

### 1.4 Quick Validation

Before proceeding to model building:
- [ ] All tables loaded (visible in Model tab)
- [ ] Date columns formatted as Date (not Text)
- [ ] Currency columns formatted as Decimal (not Text)
- [ ] No obviously corrupted data visible
- [ ] Record count matches expectations

---

## Section 2: Data Modeling (45 minutes)

### 2.1 View Model Structure

1. **Left panel → Model tab** (or View menu → Model view)
2. **You see:** Tables with columns, relationships, and data types

**Expected Layout:**
- Fact table (transactions, events) in center
- Dimension tables (accounts, dates, categories) around edges
- Relationship lines connecting tables

![Model view shows connected tables]

---

### 2.2 Create Relationships (if not automatic)

Power BI often auto-detects relationships. If not, create manually:

**Step 1: Identify Foreign Keys**
- Fact table: Column like `account_id`, `client_id`, `date_id`
- Dimension table: Column like `id`, `account_key`

**Step 2: Drag to Create Relationship**
1. **In Model view, drag** fact table's `account_id` **to** dimension table's `id`
2. Power BI creates relationship (shows as line between tables)

**Step 3: Verify Relationship Properties**
1. **Click relationship line → Edit relationship**
2. **Check:**
   - "From" table matches fact table
   - "To" table matches dimension
   - Cardinality is correct (usually 1-to-many: dim → fact)
   - Cross-filter direction: "Both" (typically)

**Repeat** for each foreign key

**Timing:** 15 minutes

---

### 2.3 Create Calculated Columns (if needed)

Some calculations belong in the model (reused across reports):

**Example: Extract Year from Date**

1. **Right-click Fact table → New Column**
2. **Enter formula:**
   ```dax
   Year = YEAR(Transactions[Date])
   ```
3. **Press Enter** (new column added)
4. **Verify:** Column appears in table

**Common Calculated Columns:**
- `Year = YEAR([Date])`
- `Month = MONTH([Date])`
- `Quarter = QUARTER([Date])`
- `Day_of_Week = WEEKDAY([Date])`
- `Account_Age_Days = TODAY() - [Account_Open_Date]`

**Note:** Use calculated columns sparingly (performance cost). Prefer measures.

**Timing:** 10 minutes

---

### 2.4 Set Data Categories (for visuals)

Help Power BI automatically suggest correct visualization types:

1. **Select column (e.g., "Country" or "Region")**
2. **Column tools → Data Category dropdown**
3. **Select category:**
   - "City" → Power BI suggests maps
   - "Country/Region" → Suggests geographic visuals
   - "Image URL" → Displays images
   
**Timing:** 5 minutes

---

### 2.5 Hide Unnecessary Columns

Reduce clutter and improve performance:

1. **Right-click column (in model or field list) → Hide**
   - Technical columns like `id`, `foreign_key` should be hidden
   - Keep user-friendly column names visible

**Timing:** 5 minutes

---

## Section 3: Create Measures (DAX) (45 minutes)

Measures are calculations that aggregate data dynamically. This is where DAX comes in.

**Reference:** [IMPLEMENTATION_GUIDES/DAX_Templates.md](DAX_Templates.md)

### 3.1 Simple Aggregations

**Right-click table → New measure** and enter formula:

**Measure 1: Total Sales (or Loans)**
```dax
Total_Sales = SUM(Transactions[Amount])
```

**Measure 2: Average Transaction**
```dax
Average_Transaction = AVERAGE(Transactions[Amount])
```

**Measure 3: Transaction Count**
```dax
Transaction_Count = COUNT(Transactions[ID])
```

**Timing:** 5 minutes

---

### 3.2 Measures with Filters

**Measure 4: At-Risk Loans** (only loans with status = "At-Risk")
```dax
At_Risk_Loans = CALCULATE(
    COUNTA(Loans[LoanID]),
    Loans[Status] = "At-Risk"
)
```

**Measure 5: Average Loan Amount (Active Only)**
```dax
Average_Active_Loan = CALCULATE(
    AVERAGE(Loans[Amount]),
    Loans[Status] = "Active"
)
```

**Key Concept:** `CALCULATE` allows you to apply filters to a measure

**Timing:** 10 minutes

---

### 3.3 Time Intelligence

**Measure 6: Year-to-Date (YTD) Sum**
```dax
YTD_Sales = TOTALYTD(
    SUM(Transactions[Amount]),
    Dates[Date]
)
```

**Measure 7: Prior Year Comparison**
```dax
Prior_Year_Sales = CALCULATE(
    SUM(Transactions[Amount]),
    PREVIOUSYEAR(Dates[Date])
)
```

**Measure 8: Year-over-Year Growth**
```dax
YoY_Growth = 
    DIVIDE(
        [Total_Sales] - [Prior_Year_Sales],
        [Prior_Year_Sales]
    )
```

**Note:** Requires date table (DIM_Dates) with continuous dates

**Timing:** 15 minutes

---

### 3.4 Conditional Logic

**Measure 9: Portfolio Health Score** (based on business rules)
```dax
Portfolio_Health = 
    IF(
        [At_Risk_Loans] / [Transaction_Count] < 0.1,
        "Healthy",
        IF(
            [At_Risk_Loans] / [Transaction_Count] < 0.2,
            "Monitor",
            "At Risk"
        )
    )
```

**Timing:** 10 minutes

---

### 3.5 Verify Your Measures

1. **In Model tab, scroll down** to see all measures
2. **Each measure should show** correct formula in formula bar
3. **Test a measure:** Drag to blank area → should show calculated value

**Common Errors:**
- `#ERROR` → Column name misspelled (use exact name)
- `#DIV/0!` → Division by zero (use DIVIDE function with default)
- `Blank` → No matching data (verify filter logic)

See [Troubleshooting.md](Troubleshooting.md) for solutions

**Timing:** 5 minutes

---

## Section 4: Build Report Visuals (60 minutes)

### 4.1 Create Report Page

1. **Go to Report tab** (left panel, blank canvas)
2. **You should see:** Blank white canvas, Visualizations pane (right)

---

### 4.2 Add KPI Cards (Top Section)

**Goal:** 4 key metrics visible at a glance (5-second rule)

**Step 1: Insert First KPI Card**
1. **Visualizations → Card** (or KPI visual for trend)
2. **Blank canvas → Drag card to top-left**
3. **Drag measure** (e.g., `Total_Sales`) **into card**

**Result:** Card displays value (e.g., "€5.2M")

**Step 2: Format Card**
1. **Right-click card → Format**
2. **Data label → Adjust font size** (make prominent)
3. **Card title → Edit** (change from "Total Sales" to friendly name)
4. **Color → Choose accent color** (e.g., blue)

**Step 3: Repeat for 3 More Cards**
- Add `Transaction_Count`
- Add `Average_Transaction`
- Add `At_Risk_Loans`

**Arrange:** Horizontally across top, same height

**Timing:** 15 minutes

---

### 4.3 Add Trend Chart (Center)

**Visual Type:** Line Chart (shows trends over time)

**Step 1: Insert Line Chart**
1. **Visualizations → Line chart**
2. **Drag to canvas (center-left)**

**Step 2: Configure**
1. **Axis:** Drag `Date` (from Dates table)
2. **Values:** Drag `Total_Sales` measure
3. **Legend:** Leave empty (single line)

**Result:** Line shows sales trend over date range

**Step 3: Add Second Measure (Optional)**
1. **Drag second measure** (e.g., `Transaction_Count`) **to Values**
2. Power BI creates dual-axis chart (two Y-axes)
3. Useful for comparing trends

**Timing:** 10 minutes

---

### 4.4 Add Category Breakdown (Right)

**Visual Type:** Bar Chart

**Step 1: Insert Bar Chart**
1. **Visualizations → Bar chart**
2. **Position:** Right side, aligned with trend chart

**Step 2: Configure**
1. **Axis:** Drag `Account_Type` (or Region, Category)
2. **Values:** Drag `Total_Sales` measure

**Result:** Bars show sales by account type

**Step 3: Sort**
1. **Right-click bar chart → Sort by → Values (descending)**
2. Bars now ordered from largest to smallest

**Timing:** 10 minutes

---

### 4.5 Add Table (Bottom)

**Visual Type:** Table (detailed data view)

**Step 1: Insert Table**
1. **Visualizations → Table**
2. **Position:** Bottom, spanning full width

**Step 2: Configure**
1. **Drag columns:** `Account_ID`, `Date`, `Amount`, `Status`
2. Table shows raw data (first 1000 rows by default)

**Step 3: Sort and Filter**
1. **Click column header** → Sort ascending/descending
2. **Top visuals filter this table** via slicers

**Timing:** 5 minutes

---

### 4.6 Add Slicers (Left Side)

**Goal:** Interactive filters for users to explore data

**Step 1: Date Range Slicer**
1. **Visualizations → Slicer**
2. **Drag `Date`** from Dates table
3. **Position:** Left side, top

**Result:** Date picker appears

2. **Format → Slicer style → Between** (for date ranges)

**Step 2: Account Type Slicer**
1. **New Slicer → Drag `Account_Type`**
2. **Style:** Dropdown (if many values) or List (if few)

**Step 3: Status Slicer**
1. **New Slicer → Drag `Status`**
2. **Enable multi-select:** Right-click → Format → Selection controls → Enable multi-select

**Result:** Users can select one or multiple statuses

**Timing:** 15 minutes

---

### 4.7 Enable Cross-Filtering

Make slicers filter all visuals:

1. **Home tab → Edit interactions** (top ribbon)
2. **Click slicer**
3. **Click each visual (cards, charts, table) → Select Filter icon**

**Result:** When user selects date, all visuals update

**Timing:** 10 minutes

---

### 4.8 Set Visual Hierarchy (5-Second Rule)

Ensure executive sees key metrics in 5 seconds:

1. **KPI cards:** Prominent, top-left (most important)
2. **Trend chart:** Center (shows context)
3. **Category chart:** Right (additional insight)
4. **Table:** Bottom (details for explorers)
5. **Slicers:** Left (supporting, not distracting)

**Test:** Close eyes 5 seconds, open, what do you see? Should be KPI cards.

**Timing:** Included in above steps

---

## Section 5: Add Interactivity (30 minutes)

### 5.1 Drill-Through (Advanced)

Allows users to click a bar → see details in new page

**Step 1: Create Details Page**
1. **Right-click blank area in Pages pane → New page**
2. **Rename:** "Transaction Details"

**Step 2: Add Visual to Details Page**
1. **Table visual with:** Account ID, Date, Amount, Status, Notes
2. **Position:** Full canvas

**Step 3: Enable Drill-Through from Main Page**
1. **Back to main report page**
2. **Click bar chart → Right-click → Edit interactions**
3. **Set drill-through fields** (drag Account_Type to details page)

**Result:** Users right-click bar → "Drill through" → see details

**Note:** Requires Power BI Pro/Premium for some features

**Timing:** 10 minutes

---

### 5.2 Bookmarks (Optional)

Save specific filter combinations for quick access:

1. **View tab → Bookmarks pane**
2. **Set filters** (e.g., Status = "At-Risk", Date = Last 30 days)
3. **Add Bookmark** (bottom, "Add") → Name it "At-Risk Portfolio"
4. **Users click bookmark** to instantly apply filters

**Timing:** 5 minutes (optional)

---

### 5.3 Conditional Formatting

Color-code data based on values:

1. **Select table column** (e.g., "Amount")
2. **Home tab → Conditional formatting → Data bars** (or color scales)
3. **Choose palette** (red-yellow-green)

**Result:** High values = green, low = red (automatically)

**Timing:** 5 minutes

---

## Section 6: Optimization & Validation (30 minutes)

### 6.1 Check Performance

**Step 1: File Size**
- **File → Save** (check file size)
- **Optimal:** < 50 MB for dashboard with 1M+ rows
- **If > 50 MB:** Archive old data or use aggregations

**Step 2: Load Time**
- **Refresh data:** Ctrl + R
- **Time it:** Should be < 30 seconds for initial load
- **If slower:** Review measure complexity or reduce data volume

**Step 3: Visual Load Time**
- **Click through slicers** and visuals
- **Should respond in < 2 seconds**
- **If slower:** Reduce number of visuals per page

---

### 6.2 Verify Calculations

**Test Each Measure:**
1. **Create new card with measure**
2. **Manually verify** (use Excel if needed)
3. **Compare card value with manual calculation**

**Example:**
- Measure: `Total_Sales` = SUM(Amount)
- Manual: Open CSV, sum Amount column manually
- Match? ✓ Good. Not match? ✗ Review formula.

**Timing:** 15 minutes

---

### 6.3 Accessibility Check

Before publishing:

- [ ] Color blind friendly (avoid red-green only)
- [ ] High contrast (dark text on light background)
- [ ] Font size ≥ 11pt (readable)
- [ ] Alt text for charts (optional, but helpful)

**Test:** View on different monitor (colors may differ)

**Timing:** 5 minutes

---

### 6.4 Final Checklist

Before publishing to Power BI Service:

- [ ] All KPI cards display correct values
- [ ] Slicers filter all visuals
- [ ] Drill-through (if enabled) works
- [ ] Charts load in < 5 seconds
- [ ] No #ERROR or Blank fields visible
- [ ] Tooltips show helpful info (hover over chart)
- [ ] Report title and page names are clear
- [ ] Data last refreshed date is visible (if important)

**Timing:** 5 minutes

---

## Section 7: Publish to Power BI Service (15 minutes)

### 7.1 Prerequisites

- [ ] Power BI Desktop file saved (.pbix)
- [ ] Power BI Service account (requires Pro license)
- [ ] Workspace or "My workspace" available

---

### 7.2 Publish Steps

**Step 1: Home tab → Publish**
- Power BI prompts for workspace (select "My workspace" if unsure)
- **Publish** (bottom-right)

**Step 2: Wait for Upload**
- Power BI uploads .pbix file
- Progress bar appears (usually 30-60 seconds)
- Success message: "Dashboards published successfully"

**Step 3: Open in Service**
- Click link to open in Power BI Service
- Dashboard is now live and shareable

---

### 7.3 Set Up Refresh Schedule

For updated data, configure refresh:

1. **Power BI Service → Dataset (your data source)**
2. **Settings ⚙️ → Schedule refresh**
3. **Frequency:** Daily (or per requirement)
4. **Time:** Off-peak hours (e.g., 2 AM)

**Note:** Requires Power BI Pro license

---

### 7.4 Share Dashboard

1. **Service → Share** (button at top)
2. **Enter email addresses** of viewers
3. **Set permissions:** View, Edit, or Admin
4. **Send** (recipients get link)

---

## 🐛 Troubleshooting Reference

For common errors, see [Troubleshooting.md](Troubleshooting.md):
- DAX formula errors (#ERROR, #DIV/0!)
- Data not loading or showing as Blank
- Performance issues
- Refresh failures
- And more...

---

## ✅ Success Criteria

Your dashboard is production-ready if:
- [ ] All visuals load without errors
- [ ] Slicers filter data correctly
- [ ] KPI cards display values within 5 seconds
- [ ] Drill-through (if enabled) works smoothly
- [ ] File size < 50 MB
- [ ] Dashboard passes accessibility check
- [ ] Team can view in Power BI Service (if published)

---

## 📚 Next Steps

**After completing walkthrough:**

1. **Test with user:** Show dashboard to business stakeholder → gather feedback
2. **Refine visuals:** Adjust based on feedback
3. **Document:** Record final dashboard spec (for maintenance)
4. **Publish:** Share with team via Power BI Service
5. **Monitor:** Check refresh schedules, user adoption, performance

---

## 🔗 Additional Resources

- **DAX Learning:** [DAX_Templates.md](DAX_Templates.md)
- **Power Query:** [Power_Query_Recipes.md](Power_Query_Recipes.md)
- **Troubleshooting:** [Troubleshooting.md](Troubleshooting.md)
- **Microsoft Learn:** https://learn.microsoft.com/en-us/training/modules/model-data-power-bi/

---

**Questions? See [Troubleshooting.md](Troubleshooting.md) or consult [REFERENCE_MATERIALS.md](../REFERENCE_MATERIALS.md)**
