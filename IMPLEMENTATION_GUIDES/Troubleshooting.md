# Troubleshooting Guide - Power BI Phase 4

Common errors encountered when building dashboards in Power BI and how to fix them. Search for your error message below.

---

## 🔍 How to Use This Guide

1. **Find your error** (exact message or keyword from error bar)
2. **Read "Why it happens"** to understand root cause
3. **Follow "Solution"** step-by-step
4. **Verify "How to confirm fixed"** to validate fix

---

## 📋 Error Categories

- **Data Loading Errors:** Can't import data
- **Model Errors:** Relationships, columns, data types
- **DAX Errors:** Formula problems (#ERROR, #DIV/0!)
- **Visual Errors:** Charts won't display or filter
- **Performance Errors:** Dashboard slow or freezes
- **Publish/Refresh Errors:** Can't save or update data

---

## 🔴 DATA LOADING ERRORS

### ❌ "File is not found" or "Access Denied"

**Error Message:**
```
Failed to connect to CSV file: File not found or access denied
```

**Why it happens:**
- File moved/deleted
- File path no longer valid
- File is locked (open in Excel)
- Permission denied (read-only file)

**Solution:**

1. **Verify file exists:**
   - Open File Explorer
   - Navigate to folder
   - Check file is present
   - If moved, update connection path

2. **Close file if open:**
   - Close Excel (if file opened there)
   - Wait 2 seconds
   - Refresh Power BI: Ctrl + R

3. **Check file permissions:**
   - Right-click file → Properties
   - Check "Read-only" checkbox is NOT selected
   - If selected, uncheck and click OK

4. **Reconnect in Power BI:**
   - Home → Transform data
   - Select your data source
   - Click "Source" step (first step)
   - Update file path if needed
   - Click Refresh

**How to confirm fixed:**
- Power BI shows data (no error bar)
- Row count appears (e.g., "1,000 rows")
- Preview tab shows data

---

### ❌ "Connection timeout"

**Error Message:**
```
Timeout expired. Timeout period elapsed during the operation or the server is not responding.
```

**Why it happens:**
- Database/server is slow or offline
- Network connection lost
- Query is too complex/large

**Solution:**

1. **Verify network connection:**
   - Ping server: Open CMD → `ping [server_name]`
   - If no response: Internet/network down

2. **Check server status:**
   - Try connecting from different tool (SQL Server Management Studio)
   - If fails: Server is down, wait for it to come back online

3. **Increase timeout setting:**
   - File → Options → Query
   - Find timeout setting
   - Increase from 30 to 60 or 120 seconds
   - Click OK

4. **Retry connection:**
   - Home → Transform data
   - Click Refresh (icon)
   - Wait for data to load

**How to confirm fixed:**
- Data loads successfully
- No timeout error appears

---

### ❌ "No columns detected"

**Error Message:**
```
The CSV file has no columns or is corrupted
```

**Why it happens:**
- CSV file is corrupted/damaged
- Wrong file format (binary instead of text)
- Encoding issue (characters don't render)

**Solution:**

1. **Open CSV in text editor (not Excel):**
   - Right-click CSV → Open with → Notepad
   - Check: First row should have column headers
   - Check: Data below headers looks normal (not gibberish)

2. **If corrupted:**
   - Go back to data source
   - Re-export CSV from source system
   - Delete old file, use new one

3. **If encoding issue:**
   - Right-click CSV → Open with → Notepad
   - File → Encoding → UTF-8
   - Save
   - Try importing again in Power BI

4. **Re-import in Power BI:**
   - Home → Get data → Text/CSV
   - Browse to new file
   - Load

**How to confirm fixed:**
- Columns appear correctly
- Data displays (no special characters)

---

## 🟠 MODEL ERRORS

### ❌ "Relationship cannot be created"

**Error Message:**
```
The relationship cannot be created because of a conflict between foreign key properties and uniqueness constraints
```

**Why it happens:**
- Foreign key column not found in dimension table
- Column names don't match exactly
- Data types different (text vs. number)

**Solution:**

1. **Verify foreign key exists:**
   - Go to Model view
   - Check both tables visible
   - Identify key columns (usually `ID` in dimension, `[Dimension]_ID` in fact)

2. **Check data types match:**
   - Fact table key: Should be Number
   - Dimension table key: Should be Number
   - If one is Text: Mismatch → convert to same type

3. **Create relationship manually:**
   - In Model view, drag fact table key → dimension table key
   - Power BI creates relationship
   - Verify relationship line appears

4. **If still fails:**
   - Try reversing (drag dimension → fact instead)
   - Check for duplicate values in dimension key (should be unique)

**How to confirm fixed:**
- Relationship line appears between tables
- No error dialog
- Measures can now aggregate across tables

---

### ❌ Date column shows as "Text" instead of "Date"

**Error Message:**
- No direct error, but date column has "abc" icon instead of calendar "📅"

**Why it happens:**
- Power BI misidentified data type
- Date stored as text ("20240101" instead of 1/1/2024)
- Locale mismatch (date format differs by country)

**Solution:**

1. **Check data format:**
   - Home → Transform data (Power Query)
   - Select date column
   - Note format (e.g., "2024-01-01" vs "01/01/2024" vs "20240101")

2. **If text format (like "20240101"):**
   - Create new column to convert:
   ```dax
   = Table.AddColumn(Source, "DateConverted", 
        each Date.FromText(Text.Range(_, 0, 4) & "-" & Text.Range(_, 4, 2) & "-" & Text.Range(_, 6, 2)))
   ```
   - Alternatively: Use UI → Data type → Date

3. **Set data type:**
   - Select date column
   - Home → Data type (top ribbon) → Date
   - Choose format (e.g., "m/d/yyyy")
   - Click OK

4. **Close & Apply**

**How to confirm fixed:**
- Column shows calendar icon 📅
- When added to chart, Power BI offers date hierarchy (Year, Month, Day)

---

### ❌ "No matching records found" when joining tables

**Error Message:**
- Relationship appears correct, but filters don't work
- Measure shows blank instead of value

**Why it happens:**
- Foreign key values don't exist in dimension table
- Case mismatch (e.g., "Customer" vs "customer")
- Spaces/special characters in keys

**Solution:**

1. **Verify data exists:**
   - Create table visual with both tables
   - Add fact table dimension key column
   - Add dimension table key column
   - Compare: Do values match?

2. **Example problem:**
   ```
   Fact table: Account_ID = "ACC-001"
   Dimension table: ID = "ACC-001"
   → They match ✓
   
   Fact table: Account_ID = "ACC-001"
   Dimension table: ID = "acc-001" (lowercase!)
   → They DON'T match ✗ (case-sensitive)
   ```

3. **Fix case mismatch:**
   - Home → Transform data
   - Create new column with standardized case:
   ```dax
   = Table.AddColumn(Source, "StandardID", each Text.Upper([ID]))
   ```
   - Use StandardID for relationship

4. **Fix spaces:**
   - Create new column with trimmed text:
   ```dax
   = Table.AddColumn(Source, "CleanID", each Text.Trim([ID]))
   ```

**How to confirm fixed:**
- Join query shows correct number of matching rows
- Measures display values (not blank)

---

## 🔵 DAX ERRORS

### ❌ #ERROR in measure

**Error Message:**
- Card shows "#ERROR" instead of value

**Why it happens:**
- Column name misspelled
- Wrong syntax (missing bracket, comma, etc.)
- Column doesn't exist in table

**Solution:**

1. **Find the measure:**
   - Model view → Scroll down to Measures
   - Right-click measure → Edit formula

2. **Check column names (MOST COMMON):**
   ```dax
   WRONG: SUM(Transactions[Amount])  ✗ (if Amount doesn't exist)
   RIGHT: SUM(Transactions[AmountValue])  ✓
   ```
   - Fix: Use exact column name (case-sensitive)
   - Verify: Home → Transform data → Check actual column name

3. **Check syntax:**
   ```dax
   WRONG: SUM Transactions[Amount]  ✗ (missing bracket)
   RIGHT: SUM(Transactions[Amount])  ✓
   ```

4. **Check function exists:**
   - fx button (formula bar) → Shows available functions
   - Type function name to autocomplete

5. **Save and test:**
   - Ctrl + Enter or click checkmark
   - Drag measure to card
   - Should show value (not #ERROR)

**How to confirm fixed:**
- Measure shows numeric value or text
- No #ERROR in any visual using this measure

---

### ❌ #DIV/0! (Division by Zero)

**Error Message:**
- Card shows "#DIV/0!" or "#Div/0!"

**Why it happens:**
- Denominator = 0
- Example: `[Sales] / [Transactions]` when Transactions = 0

**Solution:**

**Step 1: Use DIVIDE function (always safer):**
```dax
WRONG: [Sales] / [Transactions]  ✗
RIGHT: DIVIDE([Sales], [Transactions])  ✓
```

**Step 2: Add default value:**
```dax
DIVIDE([Sales], [Transactions], 0)
```
- If denominator = 0, returns 0 (not error)

**Step 3: Or use IF to handle:**
```dax
Result = IF([Transactions] = 0, 0, [Sales] / [Transactions])
```

**Step 4: Save and test**

**How to confirm fixed:**
- Measure shows 0 (or default value) instead of #DIV/0!
- Works even when denominator is 0

---

### ❌ Measure shows BLANK instead of value

**Error Message:**
- Card shows nothing (blank) instead of number

**Why it happens:**
- No matching data (filter removes all rows)
- Formula returns BLANK
- Date range filter excludes all transactions

**Solution:**

1. **Check date filter:**
   - Is slicer set to date range?
   - Do transactions exist in that range?
   - Try: Remove slicer (set to "All") → Does measure show value?

2. **Check other filters:**
   - Are slicers filtering out all data?
   - Test: Clear all slicer selections → Does measure show?

3. **Replace BLANK with default:**
   ```dax
   WRONG: [Total_Sales]  (returns BLANK if no data)
   RIGHT: IF(ISBLANK([Total_Sales]), 0, [Total_Sales])
   ```

4. **Verify data exists:**
   - Create table visual with all transactions
   - Apply same filters as measure
   - Are any rows shown?
   - If no: Data doesn't exist in filtered range

**How to confirm fixed:**
- Measure shows 0 (or default) instead of blank
- Works regardless of filter selections

---

## 🟢 VISUAL ERRORS

### ❌ Chart shows "No data available"

**Error Message:**
- Chart visual displays: "No data available"

**Why it happens:**
- Wrong fields dragged (measure in axis, category in values)
- No matching data for selected filters
- Data type mismatch

**Solution:**

1. **Check visual configuration:**
   - Click chart → Visualizations pane (right)
   - Axis section: Should contain category (e.g., Date, Region)
   - Values section: Should contain measure (e.g., Total Sales)

   ```
   WRONG Configuration:
   - Axis: Total Sales (measure) ✗
   - Values: Region (category) ✗
   
   RIGHT Configuration:
   - Axis: Region (category) ✓
   - Values: Total Sales (measure) ✓
   ```

2. **Fix:**
   - Drag wrong fields out of sections
   - Drag correct fields in

3. **Test with no filters:**
   - Clear all slicers
   - Does chart now show data?
   - If yes: Data exists but filtered out by current slicer selections

**How to confirm fixed:**
- Chart displays data (bars, lines, etc.)
- Values visible in legend/axis

---

### ❌ Slicer doesn't filter other visuals

**Error Message:**
- Click slicer → Other charts don't change

**Why it happens:**
- Cross-filter not enabled
- Slicer not connected to visuals
- Visuals filtering disabled

**Solution:**

1. **Enable cross-filter:**
   - Home tab → Edit interactions (top ribbon)
   - Click slicer
   - Click chart/card you want filtered
   - In interaction menu: Select Filter icon ✓

2. **Repeat for all visuals:**
   - Slicer → Card (Filter)
   - Slicer → Chart 1 (Filter)
   - Slicer → Chart 2 (Filter)
   - Etc.

3. **Test:**
   - Click slicer option (e.g., select date range)
   - Other visuals should update
   - If still not: Check relationship between tables

**How to confirm fixed:**
- Click slicer → all connected visuals update
- Filtering works in both directions (if needed)

---

### ❌ Color legend shows wrong colors

**Error Message:**
- Chart shows wrong colors (planned blue but shows red)

**Why it happens:**
- Default color palette applied
- Conditional formatting conflicts
- Theme overrides color

**Solution:**

1. **Check visual settings:**
   - Click chart → Format (right pane)
   - Look for Colors or Conditional formatting section
   - Verify expected palette selected

2. **Reset colors:**
   - Format → Colors → Reset to default

3. **Apply custom colors:**
   - Format → Colors → Choose color picker
   - Set colors manually for each category

4. **Test:**
   - Colors should now match spec

**How to confirm fixed:**
- Chart displays correct colors
- Matches design specification from Phase 3

---

## 🔴 PERFORMANCE ERRORS

### ❌ Dashboard loads slowly (> 10 seconds)

**Error Message:**
- No error, but dashboard takes too long to load/refresh

**Why it happens:**
- Too many visuals on page
- Complex DAX measures (nested calculations)
- Large dataset (millions of rows)
- Data stored inefficiently

**Solution:**

1. **Identify slow step:**
   - View → Performance analyzer (Power BI Desktop)
   - See timing for each visual
   - Note which visuals are slowest

2. **Reduce visual complexity:**
   - Remove unnecessary visuals
   - Limit rows in tables (Top 100 instead of all)
   - Remove fancy effects

3. **Simplify DAX:**
   - Review measures in [DAX_Templates.md](DAX_Templates.md)
   - Avoid nested functions (CALCULATE inside CALCULATE)
   - Pre-calculate in Power Query if possible

4. **Archive old data:**
   - Remove historical data (keep last 2 years)
   - Filter in Power Query: Only recent transactions

5. **Test:**
   - Close Power BI
   - Reopen
   - Time load: Should now be < 5 seconds

**How to confirm fixed:**
- Dashboard loads in < 5 seconds
- Performance analyzer shows all visuals < 1 second each

---

### ❌ Excel file becomes massive (> 100 MB)

**Error Message:**
- File save takes forever
- Power BI Desktop crashes
- File won't open on other computers

**Why it happens:**
- Too much data loaded
- Unused columns included
- .pbix file is inherently large (includes data)

**Solution:**

1. **Remove unused columns:**
   - Home → Transform data
   - Right-click column → Remove
   - Keep only columns used in model/visuals

2. **Filter historical data:**
   - Keep last 2 years only (not 10 years)
   - Power Query: Add filter step

3. **Remove unused queries:**
   - If you loaded tables but don't use them → Delete
   - Right-click query → Delete

4. **Close Power BI, save, check size:**
   - File → Save
   - Check file size (right-click .pbix → Properties)

**How to confirm fixed:**
- File size < 50 MB
- Saves quickly
- Opens reliably

---

## 🟣 PUBLISH/REFRESH ERRORS

### ❌ "You don't have permission to publish"

**Error Message:**
```
You don't have permission to publish to this workspace
```

**Why it happens:**
- Power BI Service account not recognized
- No Pro license (Desktop only, can't publish)
- Workspace permissions too restricted

**Solution:**

1. **Verify Pro license:**
   - Power BI Service (app.powerbi.com)
   - Settings (gear) → Account
   - Check license: Should say "Power BI Pro" or "Premium"
   - If says "Free": Can't publish (license problem)

2. **If no Pro license:**
   - Option A: Purchase Power BI Pro ($10/month)
   - Option B: Save .pbix locally, share via OneDrive/email

3. **If license exists:**
   - Sign out → Sign in again
   - Try publish again
   - File → Publish

4. **Check workspace permissions:**
   - Power BI Service → Workspace
   - Settings → Access
   - Verify your name has "Editor" or "Admin" role

**How to confirm fixed:**
- Dashboard publishes successfully
- Appears in Power BI Service

---

### ❌ "Refresh failed"

**Error Message:**
```
Refresh failed: Unable to connect to data source or query timed out
```

**Why it happens:**
- Data source is offline
- Credentials expired
- File moved/deleted
- Network connectivity lost

**Solution:**

1. **Check data source:**
   - If CSV: Is file accessible? (open in Explorer)
   - If SQL: Is database online? (try connecting with other tool)
   - If Excel: Is file in same location?

2. **Update credentials:**
   - Power BI Service → Settings → Data source settings
   - Click data source
   - Edit → Re-enter username/password
   - Test connection → Refresh

3. **Extend refresh timeout:**
   - Power BI Service → Dataset settings
   - Refresh → Timeout (increase to 120 min)

4. **Retry refresh:**
   - Power BI Service → Dataset
   - Refresh now (icon)
   - Monitor refresh attempt

**How to confirm fixed:**
- Refresh completes successfully
- "Last refresh" timestamp updates
- Data appears up-to-date

---

### ❌ "This visual doesn't support row level security (RLS)"

**Error Message:**
- When trying to apply RLS filters to specific visual

**Why it happens:**
- Visual type doesn't support RLS (rare)
- RLS not properly configured
- User role not assigned

**Solution:**

1. **Check if RLS needed:**
   - RLS = restricting data by user
   - If not needed: Ignore this error
   - If needed: Continue below

2. **Configure RLS:**
   - Power BI Service → Dataset → Security (⚙️)
   - Create role (e.g., "Region A Sales")
   - Define DAX filter: `Sales[Region] = "Region A"`
   - Assign users to role

3. **Test:**
   - Sign in as test user
   - See only their data

**How to confirm fixed:**
- RLS filters work correctly
- Users see only their assigned data

---

## 🎯 GENERAL DEBUGGING CHECKLIST

When stuck, work through this:

- [ ] **Error message exact?** Search this guide for exact text
- [ ] **Recent changes?** What changed? Undo and see if error persists
- [ ] **Data fresh?** Refresh data (Ctrl + R) and retest
- [ ] **Clear cache?** Close Power BI, reopen
- [ ] **Test query separately?** Run measure/filter in simple visual
- [ ] **Check basics:**
   - [ ] Correct table/column names (exact spelling)
   - [ ] Correct data types
   - [ ] Correct relationships
   - [ ] No nulls where not expected

---

## 📚 Additional Resources

- **Power BI Troubleshooting:** https://learn.microsoft.com/en-us/power-bi/support/
- **DAX Function Reference:** https://learn.microsoft.com/en-us/dax/dax-function-reference
- **Power BI Community Forum:** https://community.powerbi.com/

---

## ❓ Error Not Listed?

1. **Search Microsoft Learn:** https://learn.microsoft.com/en-us/power-bi/
2. **Post in Power BI Community:** https://community.powerbi.com/
3. **Check Power BI Blog:** https://powerbi.microsoft.com/en-us/blog/

---

**Still stuck? Email error message + screenshot to instructor or post in Power BI community forums.**
