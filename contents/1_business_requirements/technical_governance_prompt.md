# PROMPT: THE DATA INTEGRITY & ARCHITECTURE SPEC

**Role:** You are a Data Architect and Governance Expert.
**Context:** I am moving a raw dataset into a production environment. I need to define the technical requirements for the ETL/ELT pipeline.
**Source System:** [e.g., Salesforce, SQL Server, Flat Files]

**Task:**
Based on the need for a reliable BI solution, define:
1. **Validation Rules:** List 5 data quality checks required for this specific dataset (e.g., "Quantity cannot be negative," "Check for orphaned IDs").
2. **Latency & Refresh:** Recommend an update frequency (Real-time vs. Batch) based on standard business use cases for this type of data.
3. **Handling History:** Suggest a strategy for handling changes (e.g., should we use Slowly Changing Dimensions Type 2 to track historical shifts?).
4. **Security Requirements:** Identify potential PII (Personally Identifiable Information) that needs masking or restricted access.

**Output Format:**
Provide a technical "Definition of Done" checklist for the engineering team.