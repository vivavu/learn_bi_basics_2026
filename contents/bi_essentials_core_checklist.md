# BI Essentials: Core checklist

Welcome. We are skipping the fluff, the history of data warehousing, and the technical jargon. This is about how you turn raw numbers into business decisions.

---

### 1. The BI Core Cycle
Business Intelligence is not about "reports"; it is about a specific loop. If you break the loop, the data is useless.

1.  **Ingest:** Collecting data from source (Sales logs, CRM, Excel).
2.  **Transform:** Cleaning the "trash" (Fixing dates, removing duplicates).
3.  **Model:** Connecting tables (e.g., Linking "Customer ID" in Sales to "Customer Name" in the Directory).
4.  **Visualize:** Choosing the right chart to tell a story.

---

### 2. The Only Two Numbers That Matter
In every BI tool, you are dealing with only two types of data.

| Feature | **Dimensions** | **Measures** |
| :--- | :--- | :--- |
| **What is it?** | Qualitative (The "Context") | Quantitative (The "Math") |
| **Examples** | Date, Region, Product Category | Revenue, Unit Count, Profit Margin |
| **Function** | Used to **slice** or **group** data. | Used to **calculate** or **aggregate**. |

---

### 3. Effective Visualization: The "5-Second Rule"
A dashboard is successful only if a stakeholder can understand the "health" of the business in 5 seconds.

* **KPI Cards:** Use for single, vital numbers (e.g., **Total Sales: $1.2M**).
* **Line Charts:** Use **only** for trends over time. Never use them to compare categories.
* **Bar Charts:** The "Workhorse." Best for comparing categories (e.g., Sales by Region).
* **Avoid Pie Charts:** The human eye struggles to compare angles. Use a Bar Chart instead.

---

### 4. Data Hygiene (The "Garbage In, Garbage Out" Rule)
The most expensive dashboard in the world is worthless if the data is wrong.

* **Nulls:** Decide if a blank should be a "0" or "Unknown."
* **Granularity:** Are you looking at sales by *day* or sales by *transaction*? Do not mix them in the same calculation without intent.
* **Consistency:** "USA," "U.S.A," and "United States" must be cleaned into a single value before you hit "Refresh."

---

### 5. The "So What?" Test
Before publishing any visual, ask: **"If this number drops by 20%, what action does the user take?"**

* If there is an action (e.g., "Call the supplier"), the visual stays.
* If there is no action (e.g., "That's interesting to know"), **delete the visual.**

---

### Summary Checklist for Participants
* [ ] Did I identify my **Dimensions** and **Measures**?
* [ ] Is my data **cleaned** and **standardized**?
* [ ] Does my dashboard pass the **3-second rule**?
* [ ] Is every chart tied to a **specific business action**?