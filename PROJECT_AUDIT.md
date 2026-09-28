# E-commerce Data — Project Audit

## 1. Executive Summary

This project is an e-commerce sales analytics model built from instructor-provided CSV files and stored as a Power BI Project (PBIP) with a semantic model and report definitions. The files show a conventional sales-data architecture: one large fact table (`fact_sales`) and several dimensions (`dim_customer`, `dim_product`, `dim_region`, `dim_channel`, `dim_payment`, `dim_campaign`, `dim_date`).

Observed facts:
- The source data is a CSV-based e-commerce dataset with orders, customers, products, dates, payments, channels, campaigns, and regions.
- The model uses a star-schema-like structure with a central fact table and multiple lookup dimensions.
- There are multiple DAX measures for revenue, orders, AOV, gross margin, return rate, customer segmentation, product performance, and time-based comparisons.
- The report contains 4 pages and 72 visuals in total.
- The project is best described conservatively as an RBI cohort learning project using instructor-provided e-commerce data, not as a production or client delivery.

## 2. Project Identity and Cohort Context

This project is in the folder `D:\Power BI\Ecommerece Data` and is named `Ecomm Analysis`.

Observed project files:
- `Ecomm Analysis.pbip`
- `Ecomm Analysis.pbix`
- `Ecomm Analysis.Report/`
- `Ecomm Analysis.SemanticModel/`
- CSV dimension files and one fact file

The creator-provided project context states this was built during the RBI learning cohort and that the dataset was supplied by the instructor. That is treated as creator-provided history and not as a verified fact about a real production business context.

## 3. Creator-Provided History

Creator-provided context:
- The project was built during the RBI learning cohort.
- The instructor provided the e-commerce dataset.
- The dataset was shared/explored with AI and AI assisted in building the project.
- The project is a cohort learning project and should not be represented as a client delivery, production deployment, paid engagement, or verified business impact.

Classification:
- CREATOR-PROVIDED HISTORY: the statements above.
- OBSERVED IN PROJECT: the project is a structured e-commerce analytics model with source CSVs and a report.
- UNKNOWN: which specific implementation pieces were AI-generated, and whether any implementation was not AI-assisted.

## 4. Source Data Inventory

The source files are all CSV-based and are present in the root project folder.

Source inventory:
- `fact_sales.csv` — main transactional fact table
- `dim_customer.csv` — customer dimension
- `dim_product.csv` — product dimension
- `dim_region.csv` — region dimension
- `dim_channel.csv` — channel dimension
- `dim_payment.csv` — payment dimension
- `dim_campaign.csv` — campaign dimension
- `dim_date.csv` — date dimension

Row counts (verified from file content):
- `fact_sales.csv`: 40,000 rows
- `dim_customer.csv`: 5,000 rows
- `dim_product.csv`: 200 rows
- `dim_region.csv`: 6 rows
- `dim_channel.csv`: 3 rows
- `dim_payment.csv`: 4 rows
- `dim_campaign.csv`: 20 rows
- `dim_date.csv`: 1,366 rows

### Fact table: `fact_sales.csv`

Observed columns:
- `order_id`
- `order_line_id`
- `order_date`
- `date_key`
- `product_id`
- `customer_id`
- `region_id`
- `channel_id`
- `payment_id`
- `campaign_id`
- `quantity`
- `unit_price`
- `discount_pct`
- `shipping_amount`
- `tax_pct`
- `line_revenue`
- `line_cost`
- `gross_profit`
- `is_return`
- `refund_amount`
- `delivery_days`
- `order_status`

This is line-item sales data, not a summary table. Each row appears to represent an order line.

Observed grain:
- transaction line / order-line grain (the `order_line_id` and `order_id` combination indicates item-level transactional rows)

Primary meaningful keys:
- `order_id` — order identity
- `order_line_id` — order line identity
- `product_id`, `customer_id`, `region_id`, `channel_id`, `payment_id`, `campaign_id` — foreign keys to dimensions
- `date_key` and `order_date` — temporal references

### Dimension tables

`dim_customer.csv` contains:
- `customer_id`
- `customer_name`
- `email`
- `join_date`
- `loyalty_tier`
- `gender`

`dim_product.csv` contains:
- `product_id`
- `sku`
- `product_name`
- `category`
- `brand`
- `list_price`
- `cost`
- `is_active`

`dim_region.csv` contains:
- `region_id`
- `country`
- `state`
- `city`

`dim_channel.csv` contains:
- `channel_id`
- `channel`

`dim_payment.csv` contains:
- `payment_id`
- `payment_method`

`dim_campaign.csv` contains:
- `campaign_id`
- `campaign_name`
- `channel`
- `start_date`
- `end_date`

`dim_date.csv` contains:
- `date_key`
- `date`
- `year`
- `quarter`
- `month`
- `month_name`
- `day`
- `day_of_week`
- `is_weekend`

## 5. Power Query Architecture

The data model shows direct CSV imports to Power Query and minimal but meaningful transformations.

Observed ingestion pattern:
- All source tables are imported with `Csv.Document(File.Contents(...))`.
- The model uses `Table.PromoteHeaders` after each CSV import.
- Columns are typed using `Table.TransformColumnTypes`.

Examples from the model:
- `fact_sales` loads from `D:\Power BI\ecom\fact_sales.csv`
- `dim_customer` loads from `D:\Power BI\ecom\dim_customer.csv`
- `dim_product` loads from `D:\Power BI\ecom\dim_product.csv`
- `dim_channel` loads from `D:\Power BI\ecom\dim_channel.csv`
- `dim_payment` loads from `D:\Power BI\ecom\dim_payment.csv`
- `dim_campaign` loads from `D:\Power BI\ecom\dim_campaign.csv`
- `dim_region` loads from `D:\Power BI\ecom\dim_region.csv`
- `dim_date` loads from `D:\Power BI\ecom\dim_date.csv`

This is a direct CSV-to-model ingestion pattern with no database or Excel stage observed.

## 6. Data Cleaning and Transformation

Observed transformations are present in the model and are factual.

### Actual transformations observed
- CSV ingestion using `File.Contents` and `Csv.Document`
- Header promotion via `Table.PromoteHeaders`
- Type conversion for IDs, dates, decimals, and booleans
- Date parsing for `order_date`, `join_date`, `start_date`, `end_date`, and `date`
- Column typing including `Int64.Type`, `type date`, `type text`, and `type logical`
- Retention of raw IDs as integer keys for dimension relationships
- No complex data deduplication step is visible in the main source definitions

### Appended/merged/lookup style operations
- The model does not show a multi-step append pipeline or heavy Power Query join logic in the source tables.
- The star schema is implemented primarily via relationship keys rather than extensive query merges.
- `dim_campaign` has date columns that are used to create date relationships to Power BI-generated local date tables.

### Cleaning behavior that is visible
- The data is imported in a clean, columnar form with explicit type conversion.
- `dim_date` includes calculated date attributes such as `year`, `quarter`, `month`, `day_of_week`, and `is_weekend`.
- The model does not reveal a broad null-removal or custom function layer beyond basic type conversion and header promotion.

## 7. Data Model Architecture

The semantic model is a star-schema-style e-commerce model centered on `fact_sales`.

Observed tables in the model:
- `fact_sales` — transactional fact table
- `dim_customer` — customer master
- `dim_product` — product master
- `dim_region` — region master
- `dim_channel` — marketing channel master
- `dim_payment` — payment method master
- `dim_campaign` — campaign master
- `dim_date` — calendar/date dimension
- `_Measures` — DAX measure table
- `Waterfall Category` — present in model references but not inspected in the same depth as the main source tables

### Relationships observed
From `Ecomm Analysis.SemanticModel/definition/relationships.tmdl`:
- `fact_sales.channel_id` → `dim_channel.channel_id`
- `fact_sales.customer_id` → `dim_customer.customer_id`
- `fact_sales.date_key` → `dim_date.date_key`
- `fact_sales.payment_id` → `dim_payment.payment_id`
- `fact_sales.product_id` → `dim_product.product_id`
- `fact_sales.region_id` → `dim_region.region_id`
- `fact_sales.campaign_id` → `dim_campaign.campaign_id`
- `fact_sales.order_date` → local date table
- `dim_customer.join_date` → local date table
- `dim_campaign.start_date` and `dim_campaign.end_date` → local date tables
- `dim_date.date` → local date table

This is a standard model pattern for an e-commerce fact table with multiple dimensions and date references.

### Grain of the model
- `fact_sales` is the main grain: one row per order line / transaction line.
- `dim_customer` is customer-level.
- `dim_product` is product-level.
- `dim_region` is geographic location-level.
- `dim_campaign` is campaign-level.
- `dim_channel` is channel-level.
- `dim_payment` is payment-type-level.
- `dim_date` is daily calendar-level.

## 8. DAX Architecture

The model defines a measure table named `_Measures` containing a broad range of business measures and segmentation logic.

### Measure families observed
Revenue / sales measures:
- `Total Revenue`
- `Gross Profit`
- `MTD Revenue`
- `YTD Revenue`
- `Previous MTD`
- `Vs Last Month`
- `Revenue with Discount`
- `Revenue without Discount`
- `Discount Dependency %`
- `Gross Margin %`

Order / fulfillment measures:
- `Total Orders`
- `AOV`
- `Avg Items per Order`
- `Return Rate %`
- `Returned Order Count`

Customer / loyalty measures:
- `Customer Count`
- `Active Customers 30d`
- `Inactive Customers 90d`
- `Inactive Customers 180d`
- `Customer LTV`
- `Avg Customer Value`
- `Reactivation Revenue Opportunity`
- `Inactive %`
- `Cohort Size`
- `Cohort Retention %`
- `Champions Count`
- `Champions Revenue`
- `Champions % of Total`
- `One-Time Buyers`
- `Repeat Customers`
- `Repeat Rate %`
- `One-Time Buyer %`

Product measures:
- `Total Products`
- `Active Products`
- `ABC Classification`

## 9. Important DAX Patterns

The DAX is not trivial and contains several meaningful patterns.

### Observed patterns
- `SUM` and `DISTINCTCOUNT` for revenue and order counting
- `DIVIDE` for percentages and ratios
- `CALCULATE` for filter context changes and segment comparisons
- `TOTALMTD` and `TOTALYTD` for month-to-date and year-to-date logic
- `DATEADD` for month-over-month comparisons
- `FILTER`, `ALL`, `VALUES`, `ALLEXCEPT`, and `RELATEDTABLE` for segmentation and context adjustments
- `RANKX` used for product ABC classification
- `SWITCH` used for formatting and segment assignment
- `AVERAGEX` used in customer-value analysis
- `DATEDIFF` used in customer recency logic

### Notable examples

`Total Revenue`:
- `SUM(fact_sales[line_revenue])`
- Simple and direct revenue aggregation

`AOV`:
- `CALCULATE(DIVIDE([Total Revenue],[Total Orders],0))`
- Revenue divided by order count

`Return Rate %`:
- `DIVIDE(CALCULATE(COUNTROWS(fact_sales), fact_sales[is_return] = 1), COUNTROWS(fact_sales), 0)`
- Measures return proportion of rows flagged as returns

`MTD Revenue`:
- `TOTALMTD(SUM(fact_sales[line_revenue]), dim_date[date])`
- Time-intelligence measure using order date context

`YoY Growth %`:
- Compares `YTD Revenue` vs `LY YTD` using `DIVIDE` and `SWITCH`
- This is a modelled growth story with color-coded results in text format

Customer segmentation measures:
- `RFM Segment`, `Recency Days`, `Frequency Orders`, `Monetary Value`, `Cohort Month`, and `LTV Bucket` are built on the customer dimension and related fact table.
- This indicates the project attempted to model customer lifecycle and retention/loyalty analysis.

Product ABC classification:
- `ABC Classification` uses `RANKX` on product revenue and assigns A/B/C groups.
- This is a real analytical feature present in the DAX model.

## 10. Business Questions Supported

The report supports a number of genuine e-commerce analytics questions.

### Directly supported by model and measures
- What is total revenue and gross profit?
- How many orders were placed and what is average order value?
- What is the month-to-date and year-to-date revenue trend?
- What is month-over-month or year-over-year growth?
- Which products contribute the most revenue?
- Which product categories are top performers or lower performers?
- What is the return rate?
- Which customers are active, inactive, champions, one-time buyers, or repeat customers?
- What is revenue by channel, payment method, region, or campaign?
- How do customer loyalty and RFM segments behave?
- How does discounting affect revenue?
- What is the revenue impact of customer reactivation opportunity?

### Evidence-based mapping
The model confirms a broad set of business questions, but the report should not be described as a fully mature e-commerce KPI suite unless the actual pages and visuals support it. The evidence supports a general but reasonably complete sales analytics dashboard.

## 11. Report / PBIR Architecture

The report is stored in PBIR and contains 4 pages.

Page names (verified from page metadata):
1. `Executive Overview`
2. `Sales Deep Dive`
3. `Customer Intelligence `
4. `Product Performance`

Visual count per page (verified from report definition):
- Executive Overview: 19 visuals
- Sales Deep Dive: 16 visuals
- Customer Intelligence: 21 visuals
- Product Performance: 16 visuals

Total verified visuals:
- 72 visuals across 4 pages

This is a multi-page, dashboard-style report rather than a single-page executive scorecard.

### Observed report characteristics
- The project includes a custom theme in addition to the standard Power BI base theme.
- There is at least one bookmark object listed in the report metadata (`bookmarks.json` includes a bookmark item).
- The report uses standard Power BI visual JSON structure with cards, tables, matrices, and charts implied by the definitions.
- No runtime visual inspection was performed; the assessment is based on PBIR metadata rather than rendered output.

This means the report architecture is confirmed at the metadata level, but the exact rendered appearance cannot be fully reconstructed without opening the report in a renderer.

## 12. End-to-End Lineage

The project lineage is straightforward and consistent:

1. Source files (`*.csv`) are read by Power Query.
2. Power Query promotes headers and types the columns.
3. Tables are loaded into the semantic model as `fact_sales` and dimension tables.
4. Relationships are created between fact and dimension tables.
5. Model columns and measures are built in DAX.
6. Report visuals use those tables and measures.

### Important lineage chains
- `fact_sales` → `customer_id` → `dim_customer` → `Customer Count`, `RFM Segment`, `Champions Revenue`
- `fact_sales` → `product_id` → `dim_product` → `ABC Classification`, `Total Products`, product performance visuals
- `fact_sales` → `region_id` → `dim_region` → region-level analysis
- `fact_sales` → `channel_id` → `dim_channel` → channel performance analysis
- `fact_sales` → `payment_id` → `dim_payment` → payment-method analysis
- `fact_sales` → `campaign_id` → `dim_campaign` → campaign analysis
- `fact_sales` → `order_date` / `date_key` → `dim_date` → time intelligence and MTD/YTD measures

## 13. AI Assistance Boundary

The creator states that the dataset was shared/explored with AI and AI assisted in building the project.

Important classification:
- CREATOR-PROVIDED HISTORY: AI was used.
- OBSERVED IN PROJECT: the project files show a conventional Power BI implementation pattern but do not prove which exact steps were AI-assisted.
- UNKNOWN: whether specific visuals, DAX formulas, or transformations were authored by AI versus manually written.

The project evidence supports the existence of AI-assisted development history, but it does not support claims that the entire implementation was AI-generated.

## 14. Implemented vs Partially Implemented vs Planned vs Unknown

### IMPLEMENTED
- E-commerce source data imported from CSV files
- Fact and dimension tables created in the semantic model
- Date table and related time logic
- Standard star-schema relationships between fact and dimensions
- Revenue, orders, gross margin, return rate, and AOV measures
- Customer segmentation measures and product ABC logic
- Multi-page report with four pages and 72 visuals
- Custom theme and bookmark metadata

### PARTIALLY IMPLEMENTED
- The project includes substantial analytical logic but not all source tables are equally deep in the report metadata reviewed.
- Some logical measures exist in the model but the report architecture may only surface selected ones in visuals.
- The model contains more analytical intent than is clearly displayed in the metadata.

### PLANNED / DOCUMENTED ONLY
- No clear sign of an explicit project plan or roadmap file was found in the project artifacts reviewed.
- The existence of a bookmark and custom theme indicates polish, but not necessarily a formal project charter.

### UNKNOWN
- Whether all measures were actually used in report visuals
- Whether there are hidden pages or non-obvious report logic not visible in the definition metadata
- Whether there were additional data quality or transformation steps outside the reviewed files
- Exact authoring provenance of each measure and visual

## 15. Technical Capabilities Demonstrated

This project demonstrates several relevant Power BI skills:
- Data ingestion from CSV sources
- Semantic model design with a fact table and several dimensions
- Relationship building and star-schema modeling
- Date dimensioning and time intelligence measures
- DAX for revenue, margin, return-rate, growth, and segmentation logic
- Customer behavior analysis using recency/frequency/monetary logic
- Product classification using ranking logic
- Report-building across multiple pages
- KPI-oriented dashboard design
- Use of a custom theme and bookmark metadata

This is a credible demonstration of Power BI analytics capability, especially in sales and marketing-type reporting.

## 16. Portfolio Relevance

This project is relevant for a portfolio as an e-commerce analytics case because it demonstrates:
- sales analytics
- transaction-level data modeling
- customer segmentation logic
- product performance analysis
- time-based business metrics
- dashboard storytelling
- DAX and semantic model design

### Strongest demonstrable skills
- e-commerce analytics
- customer segmentation and RFM-style analysis
- DAX measure design
- relationships across a star schema
- multi-page reporting

### Claims that should not be made
- This should not be presented as a client project.
- It should not be described as a production implementation.
- It should not claim verified business impact beyond the dataset and model evidence.
- It should not be stated as a real-world business deployment without direct evidence.

## 17. Claims That Should NOT Be Made

The following claims are unsupported by the project evidence:
- “This is my client solution.”
- “This was deployed to a live business.”
- “This produced measurable business impact.”
- “The entire dashboard was AI-generated.”
- “This represents a production-grade enterprise system.”
- “This is a real business implementation from an external customer.”

The conservative, accurate characterization is:
- “RBI cohort project built using instructor-provided e-commerce data.”

## 18. Technical Limitations and Unknowns

The following are important limitations:
- No runtime visual verification was performed; the assessment relies on metadata and model definitions.
- The file names and source paths indicate local/staged data ingestion, not a formal deployment environment.
- Some fields and measures imply analytical intent, but the exact report-level usage of every metric is not fully visible from the metadata alone.
- The actual business context is not recovered beyond the project’s e-commerce data structure.
- The project does not provide enough evidence to claim original business requirements beyond the available source schema and display logic.

## 19. Audit Method and Evidence Boundaries

This audit was conducted using read-only forensic inspection of the project files, including:
- PBIP metadata
- semantic model TMDL files
- relationship definitions
- DAX measure definitions
- source CSV files
- report page metadata and visual counts

This audit intentionally avoids assumptions about:
- client context
- production deployment
- business impact
- AI generation for specific objects unless directly evidenced

The written conclusions are therefore constrained to what the project files actually show.

## 20. Final Factual Project Summary

The actual project truth is as follows:

- The project is a Power BI e-commerce sales analytics project built from instructor-provided CSV data.
- The source data contains fact sales plus customer, product, payment, channel, campaign, region, and date dimensions.
- The semantic model is a star-schema-like design with `fact_sales` at the center and multiple dimension tables connected by integer keys.
- The model implements revenue, margin, order, return, customer lifecycles, and product performance measures.
- The report contains four pages: Executive Overview, Sales Deep Dive, Customer Intelligence, and Product Performance.
- The project demonstrates strong Power BI analytics capability in data modeling, DAX, and dashboard design using a synthetic or curated e-commerce dataset.
- The project should be positioned honestly as an RBI cohort learning project using provided e-commerce data, not as a real client implementation or production business deployment.

This project is credible as a learning and portfolio artifact, but its strongest honest description is: a structured e-commerce analytics dashboard built during a cohort using instructor-provided data.
