# PROMPT: DATA MODEL DESIGN & RELATIONSHIPS

**Role:** You are a Data Engineer and BI Developer.
**Context:** I have raw data sources and need to design a star schema for efficient querying in Power BI.
**Data Sources:** [LIST YOUR TABLES/COLUMNS HERE, e.g., Sales (Date, ProductID, CustomerID, Quantity, Revenue), Products (ProductID, Name, Category), Customers (CustomerID, Name, Region)]

**Task:**
Design a dimensional model that supports the business requirements identified earlier:
1. **Fact Tables:** Identify the central facts (e.g., Sales Fact with measures like Total Revenue).
2. **Dimension Tables:** Define dimensions for slicing (e.g., Date Dimension, Product Dimension).
3. **Relationships:** Specify the relationships between tables (1:1, 1:Many, Many:Many) and any bridge tables needed.
4. **Calculated Columns:** Suggest any necessary calculated columns (e.g., Month-Year from Date).
5. **Data Types & Keys:** Ensure proper data types and surrogate keys where needed.

**Output Format:**
Provide a diagram description or table schema for the model.