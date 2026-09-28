# Jcars_logistic_analysis_project
## Introduction

JCars Logistics is a company that imports, sells, and delivers vehicles to customers across different regions in Kenya. The company generates data from various business activities, including sales transactions, vehicles, customers, branches, sales representatives, payments, deliveries, logistics costs, returns, cancellations, and customer experiences. This project focused on transforming the company’s raw flat dataset into a reliable and interactive Power BI business intelligence solution. The data was first examined to identify quality issues such as missing information, inconsistencies, unusual values, and formatting problems. The cleaned data was then prepared and modeled to support meaningful analysis, calculations, visualizations, and management reporting. The final solution provided insights into business performance and areas requiring attention, supporting evidence-based decision-making by JCars Logistics management.
## Business objectives
1. To analyze JCars Logistics overall business performance by evaluating sales, revenue, costs, profitability, and performance trends over time.
2. To evaluate operational and customer performance by analyzing vehicle, customer, branch, geographic, sales representative, payment, delivery, and logistics performance.
3. To identify business opportunities and areas requiring attention by analyzing sales channels, lead sources, returns, cancellations, customer experience, and unusual transactions.
# Dataset and grain
1. Source file: Jcars_data.csv is a single flat table with 32 columns.
2. Grain: Each row represents one vehicle sales order line: a single order for one or more units of one vehicle configuration, sold by one sales rep to one customer through one branch.
3. Row count: 276 order lines, representing 466 total units sold.
4. Date coverage: Order Date spans 1 Jan 2025 – 1 Dec 2026.
5. Business entities represented in the columns: customer (name, type, age), location (region, county, city), branch, sales rep, lead source/channel, vehicle (make, model, type, year, fuel, transmission, color), transaction economics (units, price, cost, discount, delivery fee, revenue recorded), payment (method, status), logistics (delivery status, delivery date, logistics cost), and customer experience (rating, review count, returned flag).


## Data Quality Audit
| # | Data Quality Issue | Impact on Analysis | Action Taken |
|---|---|---|---|
| 1 | Inconsistent or misspelled categories such as `toyota`, `toyta`, `totoya`, `harier`, and `hondaa` | The same category could appear as different values, affecting totals and visualizations. | Mapping tables were created to standardize different variations into consistent labels. |
| 2 | Multiple currencies such as KES, USD, EUR, and ZAR | Monetary values in different currencies could not be safely compared or added together. | Foreign currencies were detected and converted to KES using year-specific exchange rates. |
| 3 | Monetary values ending in `M`, such as `5M` | Values could be interpreted incorrectly and lead to inaccurate financial calculations. | The `M` suffix was detected and converted into millions before currency conversion. |
| 4 | Missing-value and error placeholders such as `N/A`, `NULL`, `TBD`, `-`, and `#VALUE!` | These values could cause calculation errors and affect data analysis. | Recognized placeholders were converted into null values. |
| 5 | Inconsistent date formats and Excel serial dates | Incorrect date interpretation could place transactions in the wrong time period. | Different date formats and Excel serial dates were converted into a consistent date format. |
| 6 | Inconsistent text formatting and spacing | Extra spaces and non-printing characters could create duplicate categories. | Text was cleaned, unnecessary spaces were removed, and non-printing characters were eliminated. |
| 7 | Inconsistent numeric values and formats | Invalid values could distort calculations and summary statistics. | Numeric fields were converted to numbers and checked against reasonable ranges. |
| 8 | Inconsistent customer, branch, region, and city names | Geographic and customer-level analysis could produce fragmented results. | Standardized mapping tables were used to create consistent names. |
| 9 | Inconsistent payment and delivery status values | Different labels for the same status could affect payment and delivery analysis. | Status values were standardized into consistent categories. |
| 10 | Inconsistent vehicle makes, models, fuel types, and transmission values | Similar vehicles could be treated as different categories in analysis. | Vehicle and related categorical values were standardized using mapping tables. |
| 11 | Incomplete geographic information | Missing county or region values could affect geographic performance analysis. | Missing county values were derived from the branch, while missing regions were derived from the county. |

