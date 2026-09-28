# Jcars_logistic_analysis_project
## Introduction

JCars Logistics is a company that imports, sells, and delivers vehicles to customers across different regions in Kenya. The company generates data from various business activities, including sales transactions, vehicles, customers, branches, sales representatives, payments, deliveries, logistics costs, returns, cancellations, and customer experiences. This project focused on transforming the company’s raw flat dataset into a reliable and interactive Power BI business intelligence solution. The data was first examined to identify quality issues such as missing information, inconsistencies, unusual values, and formatting problems. The cleaned data was then prepared and modeled to support meaningful analysis, calculations, visualizations, and management reporting. The final solution provided insights into business performance and areas requiring attention, supporting evidence-based decision-making by JCars Logistics management.
## Business objectives
1. To analyze JCars Logistics overall business performance by evaluating sales, revenue, costs, profitability, and performance trends over time.
2. To evaluate operational and customer performance by analyzing vehicle, customer, branch, geographic, sales representative, payment, delivery, and logistics performance.
3. To identify business opportunities and areas requiring attention by analyzing sales channels, lead sources, returns, cancellations, customer experience, and unusual transactions.
# Dataset and grain
1. Source file: Jcars_data.csv is a single flat table 
2. Grain: Each row represents one vehicle sales order line, a single order for one or more units of one vehicle configuration, sold by one sales rep to one customer through one branch.
3. Row count: 255 orders , representing 415 total units sold.
4. Date coverage: Order Date spans 1 Jan 2025 – 1 Dec 2026.
5. Business entities represented in the columns: customer (name, type, age, rating), location (region, county, city, branch), , sales rep, lead source, vehicle (make, model, type, year, fuel, transmission, color), transactions made (units, price, cost, discount, delivery fee, revenue recorded), payment (method, status), logistics (delivery status, delivery date, logistics cost), and customer experience (rating, review count, returned flag).


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
# Currency standardization
  The following DAX function was used to convert other currencies like USD, ZAR and EUR to Kenyan shillings
  
  `Unit Selling Price KES =SWITCH(TRUE(),'Jcars'[Currency] = "KES", 'Jcars'[Unit Selling Price],'Jcars'[Currency] = "USD", 'Jcars'[Unit Selling Price] * 130,'Jcars'[Currency] = "EUR", 'Jcars'[Unit Selling Price] * 150,'Jcars'[Currency] = "ZAR", 'Jcars'[Unit Selling Price] * 7.5,BLANK())`

Another DAX function was used to convert numerics with suffix   `M` to KES million shillings

`Amount Converted =VAR Amount = 'Jcars'[Logistic cost]
RETURN IF(RIGHT(Amount, 1) = "M", VALUE(LEFT(Amount, LEN(Amount) - 1)) * 1000000,VALUE(Amount))`
# Data cleaning and preparation using power query
The data cleaning process was carried out using **Power Query Editor** to improve the quality, consistency, and reliability of the dataset. All columns were trimmed to remove unnecessary leading and trailing spaces, and duplicate records were identified and removed to avoid repeated entries. The **Order ID** column was converted to **uppercase** to maintain consistency. Invalid entries such as **N/A**, **NA**, **null**, **missing**, **unknown**, and blank values were replaced with null values. **Find and Replace** was used to correct inconsistent spellings and abbreviations such as **cental** to **Central**, **Cost** to **Coast**, **Nbi** and **Nrb** to **Nairobi**, and **Westen** to **Western**. Similar standardization was applied to customer types, counties, cities, branches, sales representatives, lead sources, vehicle makes and models, payment methods, payment statuses, delivery statuses, vehicle types, fuel types, transmission types, and colours. Missing **County** values were filled using the corresponding **Branch**, while missing **Region** values were filled using the corresponding **County**. For example, **Kisumu Yard** was mapped to **Kisumu County**, and **Kisumu County** was mapped to the **Nyanza Region**. Other mappings included **Eldoret Yard to Uasin Gishu**, **Kakamega Yard to Kakamega**, **Mombasa Port Yard to Mombasa**, **Nairobi HQ to Nairobi**, **Nakuru Yard to Nakuru**, **Thika Yard to Kiambu**, and **Athi River Yard to Machakos**, with the respective counties then mapped to their regions. Numerical fields were converted to appropriate data types and checked against valid ranges, while discounts and customer ratings were standardized, Find and replace was used to remove out of 5. Order and delivery dates were converted into proper date formats. Monetary fields were cleaned by identifying **KES, USD, EUR, and ZAR**, removing currency symbols and unnecessary text, converting values with an **M** suffix into full numerical amounts, such as 2M to 2,000,000, and converting foreign currencies to **Kenyan Shillings (KES)** using the applicable exchange rates.
# Data modelling
This step was done in power query to establish a one to many relationships between the facts and dimensional tables. The four dimensional tables that used to form a star schema model view were;

1. **dim_location** that cointained the following columns Branch, city ,county ,region and Location ID

2. **dim_customers** that contained the columns Customer age, customer type, customer name, Customer ID

3. **dim_salesrep** that contained the columns Sales rep ,sales rep ID

4. **dim_vehicle** that contained the columns Car make, car model, colour, transmission ,fuel type, vehicle ID, Vehicle type ,vehicle year

   The facts table contained all the  other remaining columns that were not in the dimensional tables and the primary keys in relation to every dimeensional table.The columns in the facts table were:**Order ID, order date ,delivery date, Lead source, units sold, unit selling price, unit cost, discount, delivery fee, logistics cost, payment method ,payment status, delivery status, customer rating, review count, returned, revenue recorded , customer ID, Sales rep ID, Location ID, Gross profit and gross profit margin**

The relationships were created prior to creating a dashboard in order to obtain an interactive dashboard in the future step of the project. Instead of using the date functions to generate months ,week and day of the delivery or order date another option was found in visualization to find  month revenue. The step taken to  month revenue is justified by the image below, by clicking out the year and day to only remain with the month 

<img width="557" height="384" alt="image" src="https://github.com/user-attachments/assets/68fa0371-fd78-49a7-abbb-4df24983576b" />

   
The IDs were generated by adding an index column from 1 after removing duplicates in order to obtain primary keys that were necessary for the facts table to create the required relationship. To create dimensional tables the jcars data table was referenced four times and renamed to a relatable name, Unnecessary columns were removed to obtain the required columns in each dim table. The original table was replaced to be the facts table. The facts tables was merged to the dimensional tables in order to obtain primary keys that were the IDs from the dimensional table. The facts table contained all the transactional columns ,primary keys and every other column that was not in the dimensional table
# DAX functions that were applied 
The following DAX functions were applied to help build visualizations and KPIs in my dashboard:
1. Total units sold = sum('Jcars_data facts table'[Units Sold])
2. Total gross profit = sum('Jcars_data facts table'[Grosss profit])
3. Total net profit = [Total gross profit]-sum('Jcars_data facts table'[Logistics Cost])
4. total revenue recorded = sum('Jcars_data facts table'[Revenue Recorded])
5. total orders = CALCULATE(COUNTROWS('Jcars_data facts table'))
6. Gross profit margin = 'Jcars_data facts table'[Grosss profit]/(('Jcars_data facts table'[Units Sold]* 'Jcars_data facts table'[Unit Selling Price])* (1-'Jcars_data facts table'[Discount]))
7. Grosss profit = (('Jcars_data facts table'[Units Sold]* 'Jcars_data facts table'[Unit Selling Price]* (1-'Jcars_data facts table'[Discount]))-('Jcars_data facts table'[Units Sold]*'Jcars_data facts table'[Unit Cost]))
8. Total Returns = CALCULATE([Total Orders],'Jcars_data facts table'[Returned] = "Yes"
9. Return Rate = DIVIDE([Total Returns], [total orders])
10. Cancelled Revenue = CALCULATE([total revenue recorded],'Jcars_data facts table'[Delivery Status] = "Cancelled")
    # Executive dashboard
    An executive dashboard was built to anwser business questions by simply visualizing it. The dashboard contained slicers for interactivity, KPIs and a title that would make it easy to understand.The following diagrams shows the dashboard built for Jcars data
    <img width="903" height="537" alt="image" src="https://github.com/user-attachments/assets/a392ae9f-8b56-4cef-ad63-7432f67a4de8" />

    <img width="906" height="516" alt="image" src="https://github.com/user-attachments/assets/364d47ff-68f0-46bc-ba18-8cb7dff84df5" />

    <img width="912" height="586" alt="image" src="https://github.com/user-attachments/assets/2984f170-2e83-49ea-b638-dfa689ae2db8" />
## Business terms used in jcars data anlysis
    
- Gross Profit – The money left after subtracting the cost of the products from the money earned from selling them.
- Gross Profit Margin – The percentage of sales that remains as profit after the cost of the products has been deducted.
- Units Sold – The total number of products or vehicles that were sold.
- Net Profit – The money remaining after all business expenses have been paid.
- Revenue – The total amount of money earned from selling products or services before expenses are deducted.
- Total – The overall or combined amount of something.
- Rturned Rate – The percentage of products that were returned by customers compared with the total products sold.
- Cancelled Revenue – The amount of money associated with sales that were cancelled and therefore not completed
# Business insights and reccommendations
## Business insights
The dashboards above shows a business with high revenue and positive profitability, with performance largely driven by Toyota vehicles, SUVs, and strong-performing branches and sales representatives. However, the 79 returns, 3.57 average customer rating, and units associated with pending, partially paid, cancelled, and refunded transactions indicate areas that require attention.The business recorded approximately KSh 1.39 billion in revenue from 415 units sold, generating KSh 387.63 million in gross profit and approximately KSh 362 million in net profit, indicating strong profitability. Toyota generated the highest revenue and gross profit among the car makes, while SUVs recorded the highest number of units sold, followed by crossovers and hatchbacks, showing stronger demand for these vehicle types. Sales performance varied among sales representatives, with some recording higher unit sales than others. Although most transactions were paid, there were also pending, partially paid, cancelled, and refunded transactions that may affect completed sales and cash flow. The average customer rating was 3.57 out of 5, indicating moderate customer satisfaction, while 79 returns highlight the need to monitor vehicle quality, customer expectations, and after-sales service. Revenue also varied across months and branches, with some periods and branches contributing more than others. Petrol vehicles contributed the largest share of revenue, while manual transmission vehicles generated more revenue than automatic vehicles.
  ## Business reccommendations
The business should focus on maintaining the strong performance of high-selling brands such as Toyota and popular vehicle types such as SUVs, while ensuring that sufficient stock is available to meet customer demand. Sales representatives with lower sales volumes should be supported through additional training, performance monitoring, and effective sales strategies. The business should also work on reducing the 79 returns by improving vehicle quality checks, providing accurate vehicle information, and strengthening after-sales services. Since the average customer rating was 3.57 out of 5, customer feedback should be regularly collected and used to address common complaints and improve customer satisfaction. Management should also follow up on pending and partially paid transactions to improve payment completion and reduce cancelled or refunded sales. Finally, the business should monitor monthly and branch-level performance to identify periods and locations with lower sales and develop targeted promotions and strategies to improve their performance.
# World business assumptions
Monetary values with no currency marker are assumed to be KES.
Fixed exchange rates (USD 129.54, EUR 147.84, ZAR 7.93) are applied consistently project-wide rather than varying by transaction date; cite your source/date in the final write-up.
Revenue is defined and calculated as Units Sold × Unit Selling Price × (1 − Discount) + Delivery Fee, not taken from the raw "Revenue Recorded" field.
Gross Profit = Revenue − (Unit Cost × Units Sold); Gross Profit Margin = Gross Profit ÷ Revenue.
Discount values above 50% are treated as invalid rather than trusted.
Customer Rating is normalized to a 0–5 scale; values outside that range are treated as invalid.
Customer Age outside 18–100 is treated as invalid.
Logistics Cost of 0 on a "Delivered" order, and Revenue Recorded of 0 on a non-cancelled/non-refunded order, are both treated as suspicious rather than genuine zeros
## My questions as an analyst
1. What percentage of total order volume is tied up in pending, partially paid, or refunded statuses, and which branches account for the highest uncollected revenue? points specific branch yards where credit controls or payment collection workflows need stricter enforcement to prevent bad debts.
2. How do vehicle preferences, payment methods, and discount sensitivity differ between corporate and individual customers across age groups? It Enables targeted inventory sourcing and custom financing packages based on demographic patterns (e.g., corporate buyers favoring fleets of petrol crossovers vs. younger individual buyers).
3. How do delivery fulfillment times and logistics costs vary by county and branch yard, and how does this impact customer satisfaction ratings? Identifies whether regional logistics bottlenecks (e.g., long delivery times or high freight costs to specific yards) directly cause lower customer ratings or order cancellations.
# Challenges faced
Data cleaning- The individual collected the data in a very raw format that slowed down my computer while working on the project

# Conclusion
The data transformation and business intelligence solution developed for JCars Logistics successfully converted a raw, multi-currency flat dataset into an interactive star-schema Power BI model. By standardizing categorical variables, resolving currency conversions into KES, and auditing data quality issues, the project established a reliable baseline for executive decision-making. The analysis showed that the business recorded KSh 1.39 billion in revenue, 255 orders made, 415 units sold, and approximately KSh 362 million in net profit, with Toyota, Nakuru, petrol vehicles, and manual transmission contributing significantly to the recorded performance. Overall, the dashboard provided useful insights that can support better inventory planning, branch performance improvement, and profitability management. By monitoring high-performing and lower-performing areas, JCars can make informed decisions aimed at improving sales and maintaining sustainable business performance.


