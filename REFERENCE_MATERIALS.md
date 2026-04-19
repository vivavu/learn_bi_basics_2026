# Reference Materials & Decision Matrices

Quick reference glossary, decision matrices, and external resources. Use this when you need to quickly clarify a term or make an architectural decision.

---

## 📚 BI Glossary (Quick Reference)

### A-B
**Attribute:** Descriptive characteristic of an entity (e.g., customer's age, region)

**Benchmark:** Target goal to compare performance against (e.g., "Sales should reach €100k")

**BI (Business Intelligence):** Process of turning data into actionable insights

**Booking:** Recording a transaction/event in the system

### C-D
**Cardinal:** Number of items in a set (e.g., "200 customers")

**Cardinality:** Relationship strength between tables (1-to-1, 1-to-many, many-to-many)
- **1-to-many:** One dimension row → many fact rows (most common)
- **Many-to-many:** Complex (avoid if possible)

**Coercion:** Forcing one data type to another (e.g., text "123" → number 123)

**Cube:** Multi-dimensional data structure (older BI term; mostly replaced by modern data models)

**DAX:** Data Analysis Expression; language for Power BI calculations

**Delinquency:** Failure to pay on schedule (e.g., loan payment 30 days overdue)

**Dimension:** Lookup table with descriptive data
- Example: DIM_Customer (customer name, region, segment)
- Typically: Smaller table, connected to fact table

### E-F
**ETL:** Extract, Transform, Load (process of moving data)
- Extract: Pull data from source
- Transform: Clean and reshape
- Load: Move to destination

**Fact Table:** Central table containing transactions/events and numeric measurements
- Example: FACT_Transactions (transaction amount, date, customer ID)
- Typically: Larger table, contains foreign keys

**Foreign Key:** Column in fact table linking to dimension table's primary key
- Example: FACT_Transactions.customer_id → DIM_Customer.id

### G-H
**Grain:** Level of detail in data (e.g., "one row per transaction", "one row per day")

**Hierarchy:** Logical grouping of categories (e.g., Year → Month → Day or Region → City → Store)

### I-J-K
**KPI (Key Performance Indicator):** Metric showing business progress toward goals
- Example: "Total revenue", "% of at-risk loans"

**Latency:** Delay before data appears in dashboard (e.g., "data refreshes daily at 2 AM")

### L-M
**Measure:** Numeric calculation aggregating fact table data
- Example: SUM(sales), AVERAGE(loan_amount)
- Usually DAX formula

**Metadata:** Data about data (e.g., column name, data type, description)

**Metric:** Quantifiable measure of something (synonym for "measure")

### N-O-P
**Null:** Missing/empty value (no data recorded)

**OLAP:** Online Analytical Processing (querying multi-dimensional data)

**OLTP:** Online Transaction Processing (recording transactions in real-time)

**Partition:** Dividing data into smaller chunks for performance
- Example: Store 2023 data separately from 2024

**Primary Key:** Unique identifier for each row in a table
- Example: customer_id uniquely identifies each customer
- No duplicates allowed

### Q-R
**Query:** Request for data from database (SQL command)

**Relationship:** Connection between tables (via foreign key)

**Refresh:** Updating data in dashboard (pulling latest data from source)

**Row-Level Security (RLS):** Restricting data so users see only their assigned rows

### S
**SCD (Slowly Changing Dimension):**
- **Type 0:** Never changes (e.g., birth date)
- **Type 1:** Overwrite old value (e.g., current address; history lost)
- **Type 2:** Keep history (e.g., effective dates; track all versions)

**Schema:** Structure of database (tables, columns, relationships)
- **Star schema:** Fact table with dimension tables around it (common)
- **Snowflake schema:** Dimensions related to other dimensions (more complex)

**Slicer:** Interactive filter on dashboard (user clicks to filter data)

**Snowflake Schema:** Extended schema with normalized dimensions (dimensions related to other dimensions)

**Spanning:** Extent or range (e.g., "data spans 5 years")

**Star Schema:** Simple schema with 1 fact table and multiple dimension tables around it

### T-U-V
**Transformation:** Changing data format/structure (cleaning, aggregating, joining)

**Tuple:** Single row of data

**Unique Constraint:** Ensures no duplicate values (similar to primary key but allows nulls)

**Validation Rule:** Business rule checking data quality (e.g., "loan amount must be > 0")

**Visualization:** Chart, map, or graphic showing data

### W-X-Y-Z
**Warehouse:** Centralized data storage optimized for reporting (data warehouse)

**Workbook:** Collection of sheets/queries (Excel term; in Power BI, it's "report")

**Yield:** Return or output (e.g., "investment yield = 5%")

---

## 🎯 Decision Matrices

Use these when deciding where to do something or how to structure your dashboard.

### Matrix 1: Where to Calculate?

**Question:** Should I calculate this in Power Query, DAX, or leave in source?

| Calculation | Power Query | DAX | SQL/Source |
|------------|------------|-----|-----------|
| **Simple aggregation (SUM, COUNT)** | ⭐⭐⭐ Better | ⭐⭐⭐ Good | ⭐⭐ OK |
| **Complex filtering (multiple conditions)** | ⭐⭐⭐ Better | ⭐⭐⭐ Good | ⭐⭐⭐ Best |
| **Row-level transformation (split, extract text)** | ⭐⭐⭐ Better | ⭐⭐ OK | ⭐⭐ OK |
| **Date calculations (year, month)** | ⭐⭐⭐ Better | ⭐⭐⭐ Good | ⭐⭐⭐ Best |
| **Time intelligence (YTD, YoY)** | ⭐ OK | ⭐⭐⭐ Better | ⭐⭐ OK |
| **Interactivity-dependent (filters)** | ⭐ No | ⭐⭐⭐ Better | ⭐⭐ OK |
| **Performance-critical (millions of rows)** | ⭐⭐ OK | ⭐⭐ OK | ⭐⭐⭐ Better |

**Decision Rule:**
- **Power Query:** For row-level transformations and data quality (done once, applies to all measures)
- **DAX:** For aggregations and interactive calculations (changes with filters)
- **SQL:** For heavy lifting on massive datasets (push computation to database)

---

### Matrix 2: Which Chart Type?

**Question:** Which visualization best shows my data?

| Data Story | Chart Type | When to Use | Example |
|-----------|-----------|-----------|---------|
| **Show total value** | Card / KPI | Headline metric | Total Sales: €5.2M |
| **Trend over time** | Line | See pattern, growth | Sales trend Jan-Dec |
| **Compare categories** | Bar/Column | Rank or compare | Sales by region |
| **Part of whole** | Pie/Donut | Show percentages | Market share |
| **Distribution** | Histogram/Box | See data spread | Loan amount distribution |
| **Relationship** | Scatter | Correlation | Risk vs. Return |
| **Geographic** | Map | Location-based | Sales by country |
| **Detailed data** | Table | Explore row-by-row | Top 100 customers |
| **Composition over time** | Stacked area | Show stacking | Product sales mix |
| **Detailed hierarchy** | Tree map | Show hierarchy | Product category sales |

**Decision Rule:**
- Choose chart that **requires fewest label explanations**
- If asking "What is this showing?", pick different chart
- Test with non-technical user: Can they understand in 5 seconds?

---

### Matrix 3: Dimension vs. Measure

**Question:** Should this be a dimension or measure?

| Characteristic | Dimension | Measure |
|---|---|---|
| **Granularity** | One row per unique value | Aggregates many rows |
| **Size** | Typically small (< 1M rows) | Calculated from fact rows |
| **Contains** | Text, dates, IDs, categories | Numbers, aggregations |
| **Example** | Customer name, Date, Region | Total sales, Count, Average |
| **In chart** | Axis, Legend, Slicer | Values, Size, Color intensity |
| **Changes with filter?** | No (static lookup) | Yes (responds to slicer) |
| **DAX needed?** | No | Yes (formula required) |

**Decision Rule:**
- If it's a **lookup/reference value** → Dimension
- If it's a **calculation/aggregate** → Measure

---

### Matrix 4: Relationship Cardinality

**Question:** What relationship type do I have?

| Situation | Cardinality | Example | Risk |
|----------|-----------|---------|------|
| **One customer → many orders** | 1-to-Many | DIM_Customer (1) ← FACT_Orders (Many) | Ambiguous if flipped |
| **One date → many transactions** | 1-to-Many | DIM_Date (1) ← FACT_Transactions (Many) | Ambiguous if flipped |
| **One order → one shipment** | 1-to-1 | FACT_Orders (1) ← FACT_Shipments (1) | Rare; consider merging |
| **One student → many courses, One course → many students** | Many-to-Many | DIM_Students (Many) ← Bridge Table ← DIM_Courses (Many) | Complex; use bridge table |

**Decision Rule:**
- **Always:** Dimension (1) → Fact (Many)
- **Avoid:** Many-to-Many relationships (use bridge table instead)

---

### Matrix 5: Data Type Selection

**Question:** What data type should this column be?

| Content | Recommended Type | Why | Example |
|---------|------------------|-----|---------|
| **Dates** | Date | Enables time hierarchy, filtering | 2024-01-15 |
| **Currency amounts** | Decimal | Precise (cents matter) | €1,234.56 |
| **Quantities** | Whole Number | Can't have 0.5 units | 100 units |
| **Percentages** | Decimal | Precise (e.g., 12.5%) | 0.125 |
| **Customer ID** | Text | Alphanumeric, leading zeros | ACC-00123 |
| **Category** | Text | Lookup/classification | "Active", "Inactive" |
| **True/False** | Boolean | Yes/No/On/Off | True |
| **Description** | Text | Long text | "Best customer in region" |

**Decision Rule:**
- **Financial data:** Decimal (never text)
- **IDs:** Text (preserves leading zeros and special chars)
- **Calculations:** Match source data type

---

## 🔗 External Resources by Topic

### Foundational BI Concepts (Review if needed)
- **Microsoft Learn - BI Fundamentals:** https://learn.microsoft.com/en-us/training/paths/bi-analyst/
- **Codecademy - BI Fundamentals:** https://www.codecademy.com/learn/bi-fundamentals (already completed)

### Power BI Learning
- **Getting Started:** https://learn.microsoft.com/en-us/training/modules/get-started-with-power-bi/
- **Power BI Desktop Overview:** https://learn.microsoft.com/en-us/training/modules/power-bi-desktop-overview/
- **Model Data in Power BI:** https://learn.microsoft.com/en-us/training/modules/model-data-power-bi/

### DAX & Measures
- **DAX Fundamentals:** https://learn.microsoft.com/en-us/training/modules/understand-dax-fundamentals/
- **Create Measures in DAX:** https://learn.microsoft.com/en-us/training/modules/create-measures-dax-power-bi/
- **DAX Function Reference:** https://learn.microsoft.com/en-us/dax/dax-function-reference

### Power Query & Data Transformation
- **Power Query Editor:** https://learn.microsoft.com/en-us/training/modules/work-with-power-query-in-power-bi-desktop/
- **Power Query M Reference:** https://learn.microsoft.com/en-us/powerquery-m/

### Visualization & Design
- **Effective Visualization:** https://learn.microsoft.com/en-us/training/modules/visuals-power-bi/
- **Accessibility in Power BI:** https://learn.microsoft.com/en-us/training/modules/accessibility-power-bi/

### Data Modeling
- **Star Schema Basics:** https://learn.microsoft.com/en-us/power-bi/guidance/star-schema
- **Relationships:** https://learn.microsoft.com/en-us/training/modules/model-data-power-bi/

### Publishing & Collaboration
- **Power BI Service:** https://learn.microsoft.com/en-us/training/modules/power-bi-service-overview/
- **Sharing & Collaboration:** https://learn.microsoft.com/en-us/training/modules/power-bi-shared-workspaces/

### Performance & Optimization
- **Optimize Power BI Performance:** https://learn.microsoft.com/en-us/power-bi/guidance/performance-benchmarks
- **DirectQuery Best Practices:** https://learn.microsoft.com/en-us/power-bi/guidance/directquery-guidance

---

## 🎓 Learning Progression Recommendation

If you need to **deepen knowledge** beyond this course:

**Week 1-2: Foundations (Review)**
- Power BI Desktop Overview
- Data Modeling Basics
- Visualization Principles

**Week 3-4: DAX Mastery**
- DAX Fundamentals
- Create Measures in DAX
- Time Intelligence

**Week 5-6: Performance**
- Optimization Techniques
- Query Folding in Power Query
- Troubleshooting Slow Dashboards

**Week 7-8: Advanced Topics**
- Row-Level Security (RLS)
- Advanced Data Modeling (bridges, multiple relationships)
- Premium Capacity Features

---

## 🆘 When to Reference What

| Situation | Reference |
|-----------|-----------|
| "What does this term mean?" | ← BI Glossary (this page) |
| "Should I calculate in DAX or Power Query?" | ← Decision Matrix 1 |
| "Which chart shows trends best?" | ← Decision Matrix 2 |
| "I need a DAX formula" | → [DAX_Templates.md](IMPLEMENTATION_GUIDES/DAX_Templates.md) |
| "I need a Power Query formula" | → [Power_Query_Recipes.md](IMPLEMENTATION_GUIDES/Power_Query_Recipes.md) |
| "Error message appeared" | → [Troubleshooting.md](IMPLEMENTATION_GUIDES/Troubleshooting.md) |
| "Step-by-step Power BI guide" | → [Phase_4_Power_BI_Walkthrough.md](IMPLEMENTATION_GUIDES/Phase_4_Power_BI_Walkthrough.md) |
| "I need Microsoft Learn course" | → [External Resources](#-external-resources-by-topic) (above) |

---

## 🔑 Key Takeaways

1. **BI is about communication:** Data → Insight → Action
2. **Design for 5-second comprehension:** KPIs first, details second
3. **Calculations belong in the right place:** Power Query (once) vs. DAX (dynamic)
4. **Chart type matters:** Pick visualization that tells story clearly
5. **Test with business user:** If they don't understand, redesign

---

**Need more help? See [IMPLEMENTATION_GUIDES/](IMPLEMENTATION_GUIDES/) for detailed walkthroughs and code examples.**
