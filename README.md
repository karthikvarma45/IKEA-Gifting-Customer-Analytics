IKEA Gifting Customer Analytics
Turning retail transactions into customer insights with MySQL, Python,
and Power BI.
Project overview
Imagine running an online gifting and home-decor business. Every day,
customers place orders, return items, and make purchases of different
sizes. The business has plenty of transaction data, but the data alone
does not explain who its most valuable customers are, who may be
becoming inactive, or how revenue changes over time.

That is the problem this project explores.

I built an end-to-end analytics workflow using the Online Retail II
dataset. I used MySQL to clean and prepare the transaction data, Python
to study customer behaviour, and Power BI to present the findings in
interactive dashboards.
Project context: This is an IKEA-themed retail analytics case
study using the public Online Retail II dataset. It is not actual IKEA
customer or sales data.

Business questions
The analysis was designed to answer a few practical questions:
- Which customer groups contribute the most revenue?
- How can customers be grouped based on their purchasing behaviour?
- Which customers have not purchased for 90 days or more?
- How does revenue change across months, quarters, and years?
- What actions could the business consider for different customer
  segments?
Tools used
  Tool                                How I used it
  MySQL                               Loaded, explored, cleaned, and
summarised transaction data

  Python                              Prepared customer data and
performed RFM and inactivity
analysis

  Pandas                              Worked with tabular data, dates,
grouping, and calculations

  NumPy                               Applied customer segmentation and
rule-based conditions
Power BI                            Built interactive dashboards to
communicate the results
Dataset
The project uses the Online Retail II dataset, which contains online
retail transactions from December 2009 to December 2011.

The original dataset contains approximately 1,067,371 transaction
rows and fields such as invoice number, product code, description,
quantity, invoice date, unit price, customer ID, and country.
Project workflow
Online Retail II Dataset
          |
          v
     MySQL - Raw Data
          |
          v
  Data Quality Checks
          |
          v
 SQL Cleaning and Preparation
          |
          v
 Customer-Level Summary
          |
          v
 Python - RFM Analysis
          |
          +------> Customer Segmentation
          |
          +------> 90-Day Inactivity Analysis
          |
          +------> Monthly and Quarterly Churn Snapshots
          |
          v
     Prepared CSV Files
          |
          v
       Power BI
          |
          v
 Interactive Dashboards and Business Insights
What I did
1. Loaded and explored the data in MySQL
I first loaded the two source files into a raw MySQL table. Before
cleaning anything, I checked the row count, column values, missing
customer IDs, duplicate records, cancelled invoices, and invalid
quantities and prices.

This helped me understand the quality of the source data before using it
for analysis.
2. Cleaned the transaction data with SQL
I created a cleaned transaction table and applied rules to remove
cancelled invoices, non-positive quantities, non-positive prices, and
duplicate rows.

I also converted the quantity, price, and invoice date fields into
appropriate data types and calculated revenue:

Revenue = Quantity × Price

Valid transactions with missing customer IDs were retained in the
transaction-level table. However, they could not be included in
customer-level RFM analysis because they could not be linked to a known
customer.
3. Created a customer summary
Next, I grouped the cleaned transactions by customer ID and calculated:
- Total Revenue: Total recorded revenue for the customer.
- Purchase Frequency: Number of distinct invoices.
- Last Purchase Date: Most recent transaction date.
- Total Quantity: Total quantity purchased.
This produced one summary row per identifiable customer and made the
next stage easier to manage.
4. Performed RFM analysis in Python
I exported the customer summary and used Pandas and NumPy to calculate
the three RFM measures:
- Recency: How many days had passed since the customer's last
  purchase, measured against the latest transaction date in the
  dataset.
- Frequency: How many distinct invoices the customer had.
- Monetary: How much revenue the customer generated.
I assigned scores from 1 to 5 to each RFM measure and used the scores to
group customers into six segments.
  Segment                             What it represents
  Champions                           Recent, frequent, and high-spending
customers

  Loyal                               Customers who purchase regularly

  At Risk                             Previously frequent customers who
have not purchased recently

  New                                 Customers with recent but limited
purchase activity

  Needs Attention                     Customers who may need further
engagement
Lost                                Customers with older and limited
purchase activity
5. Applied a 90-day inactivity rule
For the customer-level RFM snapshot, I marked a customer as churn-risk
when their recency was 90 days or more.

I also prepared monthly and quarterly historical snapshots to examine
how the share of inactive customers changed over time.

This is a rule-based inactivity measure, not a machine-learning
churn prediction. It identifies customers who meet the chosen inactivity
condition; it does not predict which customers will leave in the future.
6. Built Power BI dashboards
I loaded the prepared data into Power BI and created two analytical
pages.
Business Overview
This page gives a high-level view of customer and revenue performance.
It includes:
- Total customers
- Customer-attributed revenue
- Churned percentage
- Average customer value
- Revenue by customer segment
- Revenue trends by month, quarter, and year
- Filters for time period, country, segment, and churn status where
  supported by the model
RFM Analysis
This page focuses on customer behaviour and segment performance. It
includes:
- Segment performance summary
- Average customer value by segment
- Churn/inactivity trends by month
- Churn/inactivity trends by quarter
- Filters to explore customer groups and reporting periods
Key results
The following figures are from the completed analysis:

  Measure                                                  Result
  Original transaction rows                             1,067,371
Cleaned transaction rows                              1,007,895
Identifiable customers used for RFM                       5,878
Customer-attributed revenue                      £17,374,804.25
Total revenue in cleaned transactions            £20,476,260.43
Customers meeting the 90-day inactivity rule              2,989
90-day inactivity rate                                   50.85%

A note about revenue: The cleaned transaction revenue includes valid
transactions where the customer ID is missing. Customer-attributed
revenue includes only transactions linked to identifiable customers.
That is why the two revenue totals are different.
Customer segment results
  Segment             Customers   Customer-attributed revenue
  Champions               1,289                £11,819,626.87
Loyal                   1,413                 £2,713,749.34
At Risk                   825                 £1,590,384.58
Lost                    1,526                   £655,444.68
New                       441                   £391,838.26
Needs Attention           384                   £203,760.52
Total           5,878            £17,374,804.25

One interesting finding was that Champions generated approximately 68%
of customer-attributed revenue. This suggests that retaining
high-value customers deserves particular attention.
Business recommendations
Based on the analysis, the business could consider:
- Champions: Focus on loyalty, recognition, and relevant early
  access to offers.
- Loyal: Encourage repeat purchases with relevant product bundles
  or loyalty benefits.
- At Risk: Consider targeted re-engagement and investigate why
  purchase activity has slowed.
- New: Help first-time customers discover relevant products and
  encourage a second purchase.
- Needs Attention: Test suitable reminders or offers and monitor
  whether engagement improves.
- Lost: Use carefully targeted win-back activity and compare the
  cost with the likely customer value.
These are recommendations from the analysis; campaigns were not executed
as part of this project.
Repository structure
A suggested way to organise the project files:
IKEA-Gifting-Customer-Analytics/
|
+-- Data/
|   +-- Raw/
|   |   +-- online_retail_II_page_1.csv
|   |   +-- online_retail_II_page_2.csv
|   |
|   +-- Processed/
|       +-- customer_summary.csv
|       +-- customer_rfm_final.csv
|       +-- monthly_churn.csv
|       +-- quarterly_churn.csv
|
+-- SQL/
|   +-- data_quality_checks.sql
|   +-- data_cleaning.sql
|   +-- customer_summary.sql
|
+-- Notebooks/
|   +-- 02_customer_rfm_analysis.ipynb
|   +-- 03_historical_churn_analysis.ipynb
|
+-- PowerBI/
|   +-- IKEA_Customer_Analytics.pbix
|
+-- README.md
The names above are a suggested structure. Adjust them to match the
files actually present in your repository.
How to reproduce the analysis
1. Download the Online Retail II dataset and place the source CSV files
   in the raw-data folder.
2. Create a MySQL database and load the source data into a raw table.
3. Run the SQL checks and cleaning steps to create the cleaned
   transaction table.
4. Run the customer-summary query and export the result to CSV.
5. Open the Python notebooks, update the input and output paths if
   needed, and run the RFM and historical inactivity analysis.
6. Load the processed CSV files into Power BI.
7. Refresh the model and review the dashboard visuals.
You may need to adjust file paths, MySQL connection settings, and column
names for your own environment.
What I learned
This project gave me practical experience with the complete analytics
process. I learned how to inspect and clean a large transaction dataset,
use SQL to prepare customer-level data, apply RFM analysis in Python,
define and examine inactivity, and communicate findings through Power
BI.

More importantly, it helped me connect technical work with business
questions: not just calculating numbers, but understanding what those
numbers could mean for a business.
Scope and limitations
- The project is a learning and portfolio case study based on the
  Online Retail II dataset.
- The IKEA name is used to frame the retail business scenario; the
  source data is not IKEA data.
- The 90-day inactivity flag is a business rule, not a predictive
  churn model.
- Historical churn snapshots are available at monthly and quarterly
  levels in this workflow.
- Recommendations are proposed actions; their real-world impact has
  not been measured.
Thanks for taking a look at my project! Feedback and suggestions are
welcome.
