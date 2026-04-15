This mapping implements a complete ETL pipeline using IICS to process sales data from multiple sources, clean it, validate it, enrich it, and generate analytical insights such as Top-N products and aggregated revenue.

🔹 Step 1: Source Data Ingestion
Multiple source tables/files are used (e.g., sales data, product data, store data)
These sources may contain:
Inconsistent formats
Missing values
Duplicate records

👉 Goal: Bring all raw data into the pipeline

🔹 Step 2: Union Transformation (Data Integration)
Combine multiple source datasets into a single unified stream
Ensures all sources follow the same structure

👉 Example:

➡️ Combined using Union
🔹 Step 3: Expression Transformation (Data Cleansing)
Perform data standardization:
Convert string to date
Handle null values using IIF / NVL

👉 Example logic:

TO_DATE(eo_date)
revenue = quantity * price
🔹 Step 4: Joiner Transformation (Data Enrichment)
Join main data with lookup-like datasets:
Product table → Product details
Store table → Location details

👉 Join Condition:

Product_ID
Store_ID
🔹 Step 5: Router Transformation (Data Validation)
Split data into multiple groups:
✅ Valid Data
❌ Invalid Data (missing values, incorrect format)

👉 Invalid records:

Stored in reject table/log

👉 Benefit:
Ensures high data quality in target

🔹 Step 6: Rank Transformation (Top-N Logic)
Identify top-performing records
Ranking based on:
Revenue / Sales

👉 Configuration:

Rank Type → Top
Rank Number → 3 (Top 3)
Group By → Year / Category
🔹 Step 7: Expression (Post-Rank Processing)
Additional calculations after ranking
Prepare fields for aggregation
🔹 Step 8: Aggregator Transformation (Summarization)
Perform business-level calculations:
Monthly Average Revenue
Category-wise total sales

👉 Group By:

Month
Category

👉 Output:

Aggregated dataset for reporting
🔹 Step 9: Expression (Post-Aggregation)
Format aggregated data
Add calculated KPIs if required
🔹 Step 10: Lookup Transformation (Data Enrichment)
Fetch additional attributes:
Product Name
Store Details

👉 Used for:

Enhancing reporting output
🔹 Step 11: Filter Transformation
Apply business rules:
Remove unwanted records
Keep only relevant data

👉 Example:

Revenue > 0
Valid category only
🔹 Step 12: Final Expression
Final formatting of output
Prepare columns for target load
🔹 Step 13: Target Load
Load processed data into target table
Final output contains:
Clean data
Aggregated metrics
Ranked results




🏗️ Complete Flow Summary
Sources 
→ Union 
→ Expression (Clean) 
→ Joiner 
→ Router (Valid/Invalid) 
→ Rank 
→ Expression 
→ Aggregator 
→ Expression 
→ Lookup 
→ Filter 
→ Expression 
→ Target
