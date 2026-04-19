# PROMPT: DASHBOARD DEVELOPMENT & DEPLOYMENT

**Role:** You are a Power BI Developer and Dashboard Specialist.
**Context:** I have designed visualizations and need to build and deploy a functional Power BI dashboard.
**Requirements:** [SUMMARIZE BUSINESS REQUIREMENTS, DATA MODEL, AND VISUAL DESIGNS HERE]

**Task:**
Guide the creation of a Power BI dashboard:
1. **Data Connection:** Steps to connect to data sources in Power BI Desktop.
2. **Model Building:** How to create relationships and measures in Power BI.
3. **Report Design:** Implementing the visualization designs with proper formatting.
4. **Interactivity & Filters:** Adding slicers, drill-through, and conditional formatting.
5. **Performance Optimization:** Tips for optimizing load times and refresh schedules.
6. **Publishing & Sharing:** Steps to publish to Power BI Service and set up sharing/permissions.

**Output Format:**
Provide step-by-step instructions or a checklist for building the dashboard.

---

## Sample Project: Retail Sales Dashboard

To practice, use the following sample scenario:

**Business Case:** A retail company wants a dashboard to monitor sales performance across regions and products.

**Sample Data:** Use the [sample_data.csv](sample_data.csv) provided, or download from the links in assets.yaml.

**Final Deliverable:** A Power BI .pbix file with:
- KPI cards for total sales, growth rate
- Bar chart for sales by region
- Line chart for sales trend over time
- Table for top products
- Slicers for date range and category

**Steps:**
1. Download sample data.
2. Open Power BI Desktop.
3. Connect to data source.
4. Build the data model.
5. Create measures (e.g., Total Sales = SUM(Sales[Amount]))
6. Design the report page.
7. Publish to Power BI Service.(Optional: requires Microsoft Fabric workspace)