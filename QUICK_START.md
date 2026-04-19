# Quick Start: Build Your First Dashboard in 60 Minutes

Get a working Power BI dashboard up and running quickly. This guide proves your environment works and gives you immediate confidence.

---

## 📊 What You'll Build

**Dashboard:** Simple Sales Performance Dashboard
- **KPI Card:** Total Sales  
- **Line Chart:** Sales Trend Over Time
- **Bar Chart:** Sales by Region
- **Slicer:** Filter by Date Range

**Time Estimate:** 60 minutes  
**Prerequisites:** Power BI Desktop installed ([see PREREQUISITES.md](PREREQUISITES.md))

---

## 📥 Step 1: Get Sample Data (5 minutes)

We'll use a simple CSV file for this quick start.

### Option A: Create Sample Data File
Save this as `sample_sales.csv` in your project folder:

```csv
Date,Region,Product,Sales,Quantity
2024-01-01,North,Product A,5000,50
2024-01-01,South,Product B,3500,35
2024-01-01,East,Product A,4200,42
2024-01-01,West,Product C,2800,28
2024-01-02,North,Product B,4500,45
2024-01-02,South,Product C,3200,32
2024-01-02,East,Product B,5100,51
2024-01-02,West,Product A,3800,38
2024-01-03,North,Product C,4800,48
2024-01-03,South,Product A,3900,39
2024-01-03,East,Product C,5200,52
2024-01-03,West,Product B,3100,31
2024-01-04,North,Product A,5500,55
2024-01-04,South,Product B,4200,42
2024-01-04,East,Product A,4800,48
2024-01-04,West,Product C,3500,35
2024-01-05,North,Product B,5200,52
2024-01-05,South,Product C,3800,38
2024-01-05,East,Product B,5400,54
2024-01-05,West,Product A,4100,41
```

### Option B: Use Your Own Data
- Any CSV with Date, categorical field (Region), and numeric field (Sales)
- Minimum 20 rows for meaningful visualization

---

## 🚀 Step 2: Load Data into Power BI (10 minutes)

### 2.1 Open Power BI Desktop
- Launch **Power BI Desktop**
- Click **Get data** (home screen) or **File → New**

### 2.2 Import CSV File
1. **Home tab → Get data → Text/CSV**
2. **Browse** to `sample_sales.csv`
3. **Click Open**
4. **Preview** shows your data - verify it looks correct
5. **Load** (bottom-right button)

**Expected Result:** Your data appears in Power Query Editor

### 2.3 Create Date Table (Power Query)
Power BI needs a proper date dimension. We'll create it:

1. **Home tab → New source → Enter data**
2. **Create table** (leave blank for now)
3. **Name it:** `DIM_Date`
4. **Load** (we'll populate it next)

**Alternative (Simpler):** Skip this for now. We'll create it in Power BI Desktop after data loads.

---

## 📊 Step 3: Build Data Model (15 minutes)

### 3.1 Create Relationships
1. **After loading CSV**, Power BI shows the data as a table
2. **Power BI automatically creates date hierarchy** (if it recognizes the Date column)
3. **Verify:** Go to **Model** tab (left panel)
   - You should see your table with columns listed
   - Date column should have calendar icon 📅

### 3.2 Create Key Measures
We'll add simple DAX measures for aggregations:

1. **In Model tab, right-click on your data table → New measure**
2. **Add first measure:**
   ```dax
   Total Sales = SUM(sales_table[Sales])
   ```
   - Replace `sales_table` with your actual table name
   - Replace `[Sales]` with your sales column name
   - **Press Enter**

3. **Add second measure (optional):**
   ```dax
   Average Sales = AVERAGE(sales_table[Sales])
   ```

**Verify:** Measures appear in the Model pane ✓

---

## 🎨 Step 4: Create Visualizations (25 minutes)

### 4.1 KPI Card
1. **Go to Report tab** (leave Model tab)
2. **Visualizations pane (right) → Card visual**
3. **Drag `Total Sales` measure into the Card**
4. **Resize card** to top-left corner
5. **Right-click card → Format** and adjust color/font if desired

**Result:** Card shows total sales value (e.g., "78,000")

### 4.2 Line Chart (Sales Trend)
1. **Blank area → Visualizations → Line chart**
2. **Axis:** Drag `Date` field
3. **Values:** Drag `Total Sales` measure
4. **Result:** Line shows sales trend over time

**Position:** Below KPI card, spanning 60% of width

### 4.3 Bar Chart (Sales by Region)
1. **Blank area → Visualizations → Bar chart**
2. **Axis:** Drag `Region` field
3. **Values:** Drag `Total Sales` measure
4. **Result:** Bar chart shows sales by region

**Position:** Right side, aligned with line chart height

### 4.4 Slicer (Filter by Date)
1. **Blank area → Visualizations → Slicer**
2. **Field:** Drag `Date` field
3. **Format:** Change to "List" or "Between" style
4. **Result:** Interactive date filter appears

**Position:** Top-left corner (above KPI card)

**Test it:** Click dates on slicer → all charts should filter ✓

---

## 💾 Step 5: Save & Validate (5 minutes)

### 5.1 Save Your File
1. **File → Save** (Ctrl + S)
2. **Name:** `financial_dashboard_quick_start.pbix`
3. **Location:** Your project folder
4. **Saved:** ✓ (check title bar)

### 5.2 Test Interactivity
- **Click on slicer dates** → Charts update? ✓
- **Click on bar chart bars** → Other visuals filter? (if cross-filter enabled)
- **Hover over line chart** → Tooltip shows values? ✓
- **All data visible** (no errors)? ✓

---

## 🎯 Success Criteria

✅ **Your dashboard is working if:**
- [ ] Power BI opens without errors
- [ ] CSV data loads successfully (shows all rows)
- [ ] KPI card displays a number (total sales)
- [ ] Line chart shows trend over time
- [ ] Bar chart shows sales by region
- [ ] Slicer filters data when clicked
- [ ] File saves without errors
- [ ] Dashboard doesn't crash on interaction

**If all boxes checked:** ✅ Environment validated. Ready to proceed!

---

## 🐛 Troubleshooting

### "Date column not recognized"
- **Problem:** Power BI treating dates as text
- **Solution:** 
  1. Go to **Home → Transform data**
  2. **Select Date column → Data type (top) → Date**
  3. **Close & Apply**

### "Measure formula error (#ERROR)"
- **Problem:** Column names don't match
- **Solution:**
  1. In DAX formula, use exact column name
  2. Right-click table → View data to see exact names
  3. Rebuild formula with corrected names

### "Charts won't filter when I click slicer"
- **Solution:** 
  1. Go to **Report tab**
  2. **Home → Edit interactions** (top ribbon)
  3. **Click slicer → Click each chart → Select Filter**

### "Performance is slow"
- **Reason:** Too much data or complex filters
- **Solution:** For now, ignore (course uses small sample data)

---

## 📈 Next Steps

✅ **First dashboard complete!** You've learned:
- Loading data into Power BI
- Creating measures with DAX
- Building basic visualizations
- Creating interactive slicers

### Ready for Full Course?
1. **Review [PREREQUISITES.md](PREREQUISITES.md)** (if not done)
2. **Start Phase 1:** [Business Requirements](contents/1_business_requirements/)
3. **Reference:** [IMPLEMENTATION_GUIDES/](IMPLEMENTATION_GUIDES/) for Power BI help

### Want to Go Deeper?
- **DAX Learning:** See [IMPLEMENTATION_GUIDES/DAX_Templates.md](IMPLEMENTATION_GUIDES/DAX_Templates.md)
- **Power Query:** See [IMPLEMENTATION_GUIDES/Power_Query_Recipes.md](IMPLEMENTATION_GUIDES/Power_Query_Recipes.md)
- **Troubleshooting:** See [IMPLEMENTATION_GUIDES/Troubleshooting.md](IMPLEMENTATION_GUIDES/Troubleshooting.md)

---

**Congratulations! You're ready for the full learning path.** 🎉

Next: [Start Phase 1 - Business Requirements](contents/1_business_requirements/)
