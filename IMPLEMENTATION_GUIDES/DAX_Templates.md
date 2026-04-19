# DAX Templates & Code Snippets

Copy-paste DAX formulas for common financial dashboard scenarios. Organized by complexity level with explanations.

**Reference:** [Microsoft Learn - DAX Fundamentals](https://learn.microsoft.com/en-us/training/modules/understand-dax-fundamentals/)

---

## 📋 Quick Navigation

- **Level 1:** Basic aggregations (start here)
- **Level 2:** Filters and conditions
- **Level 3:** Time intelligence (YTD, YoY)
- **Level 4:** Advanced analytics
- **Level 5:** Complex scenarios

---

## 🔴 Level 1: Basic Aggregations (Start Here)

These formulas are simple and safe. Copy directly into your Power BI model.

### 1.1 Total Sum
**Use when:** You need total of a numeric column
```dax
Total_Sales = SUM(Transactions[Amount])
```
**How to use:** Drag into KPI card, bar chart, or table  
**Example:** Total revenue = €5.2M

---

### 1.2 Average
**Use when:** You need average value per row
```dax
Average_Transaction = AVERAGE(Transactions[Amount])
```
**How to use:** Drag into KPI card  
**Example:** Average transaction = €250

---

### 1.3 Count
**Use when:** You need count of items/rows
```dax
Transaction_Count = COUNT(Transactions[ID])
```
**How to use:** Drag into KPI card  
**Example:** Total transactions = 1.2M

---

### 1.4 Distinct Count
**Use when:** You need count of unique items (no duplicates)
```dax
Unique_Accounts = DISTINCTCOUNT(Transactions[AccountID])
```
**How to use:** Compare against total count  
**Example:** 5,000 unique accounts out of 1.2M transactions

---

### 1.5 Minimum / Maximum
**Use when:** You need lowest or highest value
```dax
Min_Loan_Amount = MIN(Loans[Amount])
Max_Loan_Amount = MAX(Loans[Amount])
```
**How to use:** Drag into cards side-by-side  
**Example:** Min = €500, Max = €500k

---

## 🟡 Level 2: Filtered Aggregations

Add conditions to aggregate specific rows only.

### 2.1 Sum with Filter
**Use when:** You need sum of rows matching a condition
```dax
At_Risk_Loans = CALCULATE(
    SUM(Loans[Amount]),
    Loans[Status] = "At-Risk"
)
```

**How to use:**
1. Right-click table → New measure
2. Paste formula above
3. Replace `Loans` with your table name
4. Replace `[Amount]` with your column
5. Replace `[Status]` and `"At-Risk"` with your column/value

**Example result:** €2.1M at risk

---

### 2.2 Count with Filter
**Use when:** You need count of items meeting a condition
```dax
Active_Accounts = CALCULATE(
    COUNTA(Accounts[AccountID]),
    Accounts[Status] = "Active"
)
```

**How to use:** Same as above, replace table/column names  
**Example:** 3,500 active accounts

---

### 2.3 Multiple Filters (AND)
**Use when:** You need to filter by two conditions simultaneously
```dax
Active_High_Value = CALCULATE(
    COUNTA(Loans[LoanID]),
    Loans[Status] = "Active",
    Loans[Amount] > 10000
)
```

**How it works:**
- First condition: Status = "Active"
- Second condition: Amount > 10,000
- Both must be TRUE (AND logic)

**Example:** 1,200 active loans over €10k

---

### 2.4 Multiple Filters (OR)
**Use when:** Either condition can be true
```dax
At_Risk_Or_Closed = CALCULATE(
    COUNTA(Loans[LoanID]),
    OR(
        Loans[Status] = "At-Risk",
        Loans[Status] = "Closed"
    )
)
```

**How it works:** Counts loans that are EITHER "At-Risk" OR "Closed"

---

### 2.5 Percentage Calculation
**Use when:** You need ratio as percentage
```dax
Percent_At_Risk = DIVIDE(
    [At_Risk_Loans],
    [Total_Loans]
) 
```

**How to use:** 
- First create `[At_Risk_Loans]` and `[Total_Loans]` measures
- Create new measure with formula above
- Format as percentage (right-click measure → Format: 0.00%)

**Example:** 15% at risk

**Why DIVIDE instead of /:**
- `DIVIDE` handles zero division gracefully (shows 0 instead of #ERROR)
- Safer for production dashboards

---

## 🟠 Level 3: Time Intelligence

For trends, comparisons, and period-over-period calculations.

### 3.1 Year-to-Date (YTD)
**Use when:** You need sum from start of year to current date
```dax
YTD_Sales = TOTALYTD(
    SUM(Transactions[Amount]),
    Dates[Date]
)
```

**Prerequisite:** Must have Dates table with continuous daily dates  
**How to use:** Drag into card or line chart  
**Example:** YTD sales = €4.2M (Jan 1 to today)

---

### 3.2 Prior Year Sum
**Use when:** You need comparison to same period last year
```dax
Prior_Year_Sales = CALCULATE(
    SUM(Transactions[Amount]),
    PREVIOUSYEAR(Dates[Date])
)
```

**Example:** Last year same period = €3.8M (compare to €4.2M YTD this year)

---

### 3.3 Year-over-Year Growth
**Use when:** You need % growth vs. prior year
```dax
YoY_Growth_Percent = DIVIDE(
    [YTD_Sales] - [Prior_Year_Sales],
    [Prior_Year_Sales]
)
```

**How to use:** Format as percentage  
**Example:** YoY growth = +10.5%

---

### 3.4 Month-to-Date (MTD)
**Use when:** You need sum for current month only
```dax
MTD_Sales = TOTALMTD(
    SUM(Transactions[Amount]),
    Dates[Date]
)
```

**Example:** MTD (month to date) = €400k

---

### 3.5 Moving Average (Last 30 Days)
**Use when:** You need smoothed trend (removes spikes)
```dax
Moving_Avg_30_Days = CALCULATE(
    AVERAGE(Transactions[Amount]),
    DATESBETWEEN(
        Dates[Date],
        TODAY() - 30,
        TODAY()
    )
)
```

**Example:** 30-day average = €245 (smooths daily fluctuations)

---

## 🔵 Level 4: Advanced Analytics

More complex scenarios requiring multiple steps.

### 4.1 Running Total
**Use when:** You need cumulative sum (grows as you move through time)
```dax
Running_Total = CALCULATE(
    SUM(Transactions[Amount]),
    FILTER(
        ALL(Dates[Date]),
        Dates[Date] <= MAX(Dates[Date])
    )
)
```

**Use in:** Line chart (shows cumulative growth)  
**Example:** Day 1: €100k, Day 2: €250k, Day 3: €400k (cumulative)

---

### 4.2 Rank
**Use when:** You want to rank items (1st, 2nd, 3rd best performer)
```dax
Sales_Rank = RANK(
    [Total_Sales],
    [Total_Sales]
)
```

**Use in:** Table (shows rank per category)

---

### 4.3 Conditional Text
**Use when:** You need status badge based on value (e.g., "Healthy" vs. "At Risk")
```dax
Portfolio_Status = 
    IF(
        [Percent_At_Risk] < 0.1,
        "✓ Healthy",
        IF(
            [Percent_At_Risk] < 0.2,
            "⚠ Monitor",
            "✗ At Risk"
        )
    )
```

**Use in:** Card or table  
**Example:** Shows "✓ Healthy" (if < 10% at risk), "⚠ Monitor" (10-20%), or "✗ At Risk" (> 20%)

---

### 4.4 Variance Analysis
**Use when:** You need difference from target
```dax
Variance_vs_Target = [Total_Sales] - [Target_Sales]

Variance_Percent = DIVIDE(
    [Variance_vs_Target],
    [Target_Sales]
)
```

**Example:** 
- Target: €5M
- Actual: €5.2M  
- Variance: +€200k (+4%)

---

### 4.5 Correlation (Two Measures)
**Use when:** You want to show relationship between two metrics
```dax
Sales_Loan_Count_Corr = CORRELATIONX(
    VALUES(Dates[Date]),
    [Total_Sales],
    [Total_Loans]
)
```

**Use in:** KPI card or visual note  
**Example:** Correlation = 0.85 (strong: more sales → more loans)

---

## 🟣 Level 5: Complex Financial Scenarios

Real-world examples from financial dashboards.

### 5.1 Portfolio Health Score (Multi-Factor)
**Use when:** You need composite health metric combining multiple factors
```dax
Portfolio_Health_Score = 
    VAR RiskScore = IF([Percent_At_Risk] > 0.25, 50, 100)
    VAR DelinquencyScore = IF([Delinquency_Rate] > 0.1, 50, 100)
    VAR GrowthScore = IF([YoY_Growth_Percent] > 0.05, 100, 50)
    RETURN
    (RiskScore + DelinquencyScore + GrowthScore) / 3
```

**How to read:**
- Each factor scored 0-100
- Final score = average of three factors
- Score < 60: Poor health
- Score 60-80: Acceptable
- Score > 80: Good health

**Example:** Overall portfolio health = 78 (acceptable)

---

### 5.2 Customer Lifetime Value (CLV) - Simple
**Use when:** You need total value per customer
```dax
Customer_Lifetime_Value = CALCULATE(
    SUM(Transactions[Amount]),
    ALLEXCEPT(Transactions, Transactions[CustomerID])
)
```

**Use in:** Table grouped by Customer  
**Example:** Customer 123 CLV = €50k (total spent)

---

### 5.3 Customer Lifetime Value (CLV) - Advanced (with Recency)
**Use when:** You need CLV weighted by recent activity
```dax
CLV_Weighted = 
    VAR RecentWeight = IF(MAX(Transactions[Date]) >= TODAY() - 90, 1.2, 0.8)
    RETURN
    CALCULATE(
        SUM(Transactions[Amount]),
        ALLEXCEPT(Transactions, Transactions[CustomerID])
    ) * RecentWeight
```

**How it works:**
- Recent customers (≤90 days) = 1.2x multiplier
- Inactive customers (>90 days) = 0.8x multiplier
- Incentivizes recency

**Example:** 
- Recent active customer: €50k × 1.2 = €60k (weighted)
- Dormant customer: €50k × 0.8 = €40k (weighted)

---

### 5.4 Loan Portfolio Risk Analysis
**Use when:** You need risk-adjusted metrics
```dax
Weighted_Risk_Score = 
    SUMPRODUCT(
        Loans[Amount],
        Loans[Risk_Score]
    ) / SUM(Loans[Amount])
```

**How to read:** Weighted average risk across portfolio  
**Example:** 
- Loan A: €1M at risk 2 = €2M weighted
- Loan B: €1M at risk 8 = €8M weighted
- Total: €2M at risk score = 5 (medium risk)

---

### 5.5 Cohort Analysis (Customer Retention)
**Use when:** You want to track customer retention by sign-up cohort
```dax
Cohort_Retention = 
    VAR CurrentMonth = MONTH(MAX(Transactions[Date]))
    VAR CohortMonth = MONTH(MAX(Transactions[SignupDate]))
    VAR MonthsActive = CurrentMonth - CohortMonth
    RETURN
    IF(
        MonthsActive = 0, "Month 0",
        IF(MonthsActive <= 3, "1-3 Months",
        IF(MonthsActive <= 12, "4-12 Months", "12+ Months"))
    )
```

---

## 🔴 Common Errors & Fixes

### #ERROR in Formula

**Problem 1: Column name misspelled**
```dax
WRONG: SUM(Transaction[Amount])  ✗
RIGHT: SUM(Transactions[Amount])  ✓
```
**Fix:** Use exact table/column names (check Data tab)

---

**Problem 2: Using text as number**
```dax
WRONG: Loans[Amount] + 100  (if Amount is text) ✗
RIGHT: VALUE(Loans[Amount]) + 100  ✓
```
**Fix:** Convert to number first using VALUE()

---

### #DIV/0! (Division by Zero)

**Problem:** Denominator is zero
```dax
WRONG: [Sales] / [Transactions]  (if Transactions = 0) ✗
RIGHT: DIVIDE([Sales], [Transactions])  ✓
```
**Fix:** Always use DIVIDE function (handles zero gracefully)

---

### Blank Result

**Problem 1: No matching data**
```dax
At_Risk = CALCULATE(COUNTA(Loans[ID]), Loans[Status] = "At-Risk")
→ Shows BLANK if no at-risk loans
```
**Fix:** Use IF to show "0" instead:
```dax
At_Risk = IF(ISBLANK([At_Risk_Base]), 0, [At_Risk_Base])
```

**Problem 2: Date filter not working**
- Verify dates table is marked as "Date" type (not Text)
- Ensure relationship exists between fact and dates table

---

## 📊 Testing Your Formulas

### Method 1: Create Test Card
1. Drag measure into blank area
2. See calculated value
3. If error, review formula

### Method 2: Verify Manually
1. Use Excel: Open data file
2. Calculate expected result (e.g., SUM of column)
3. Compare to Power BI result
4. Match? ✓ Good. Different? ✗ Review formula

### Method 3: Use Measure in Table
1. Create table visual
2. Drag measure into table
3. See values broken down by category
4. Manually spot-check calculations

---

## 🎯 Formula Design Patterns

### Pattern 1: Safe Division
```dax
Result = DIVIDE([Numerator], [Denominator], 0)
```
**Benefit:** Always works; shows 0 if denominator = 0

### Pattern 2: Conditional with Multiple Factors
```dax
Status = 
    IF([Factor1] > Threshold1 AND [Factor2] > Threshold2, "Good", "Bad")
```

### Pattern 3: Year-over-Year Comparison
```dax
YoY = DIVIDE([This_Year], [Last_Year]) - 1
```
**Note:** Multiply by 100 to show percentage

### Pattern 4: Aggregate with Filter
```dax
Filtered_Sum = CALCULATE(SUM([Column]), [Filter_Field] = "Value")
```
**Benefits:** Works with slicer interactions

---

## 🔗 Learning Path

1. **Start:** Levels 1-2 (basic aggregations + filters)
2. **Practice:** Create 3-4 measures for your dashboard
3. **Expand:** Level 3 (time intelligence) once comfortable
4. **Advanced:** Levels 4-5 only if needed for your dashboard

**Avoid:** Jumping to Level 5 without mastering 1-3 first

---

## 📚 Additional Resources
- **DAX Function Reference:** https://learn.microsoft.com/en-us/dax/dax-function-reference
- **Power BI Formula Reference:** Built into Power BI (fx → Insert formula)

---

## ✅ Quick Checklist

After writing each formula:
- [ ] No #ERROR in card/visual
- [ ] Result matches manual calculation
- [ ] Formula uses DIVIDE (not /)
- [ ] Measures referenced correctly ([Measure_Name])
- [ ] Column names exactly match data
- [ ] Formula includes comments (optional but helpful)

---

**Questions? See [Troubleshooting.md](Troubleshooting.md)