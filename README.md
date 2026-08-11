# Card Portfolio & Transaction Analytics

## Power BI | SQL | Excel | Power Query | DAX

A portfolio data analytics project designed to analyze card portfolio performance, transaction activity, customer behavior, transaction outcomes, and regional performance.

The project uses synthetic data to simulate a real-world card and payments analytics environment.

It demonstrates how transaction-level data can be cleaned, validated, modeled, analyzed, and converted into business KPIs and actionable insights.

---

## Project Overview

The objective of this project is to provide a business-facing analytics solution for monitoring card performance and transaction activity.

The analysis focuses on:

- Transaction volume
- Transaction value
- Card usage
- Card activation
- Transaction success rate
- Declined transactions
- Reversed transactions
- Customer transaction behavior
- Regional performance
- Card type performance
- Merchant category performance
- Transaction channel performance
- Monthly trends
- Data quality and validation

The project was designed around the type of analytical requirements commonly found in card, payments, banking, and financial services environments.

---

## Business Questions

The analysis answers questions such as:

1. How many card transactions are processed each month?

2. What is the total transaction value?

3. What percentage of transactions are successful?

4. Which regions generate the highest transaction value?

5. Which card types generate the most transaction activity?

6. Which transaction channels are most frequently used?

7. Which merchant categories generate the highest spend?

8. What percentage of transactions are declined or reversed?

9. How does transaction activity change over time?

10. Which customer and transaction patterns require further investigation?

11. How many cards are active versus inactive?

12. Which regions have the strongest card activity?

13. Are there unusual transaction patterns or performance variances?

---

# Dataset

The project contains two synthetic datasets.

## 1. Card Transactions

File:

`card_transactions.csv`

The dataset contains 2,500 synthetic card transactions covering January to June 2026.

### Fields

| Field | Description |
|---|---|
| transaction_id | Unique transaction identifier |
| transaction_date | Date of transaction |
| customer_id | Customer identifier |
| card_id | Card identifier |
| region | Customer/card region |
| card_type | Type of card |
| channel | Transaction channel |
| merchant_category | Merchant category |
| transaction_amount_ngn | Transaction value in NGN |
| status | Successful, Declined or Reversed |
| is_successful | Binary success indicator |
| transaction_month | Transaction month |

---

## 2. Card Portfolio

File:

`card_portfolio.csv`

The dataset contains 600 synthetic cards.

### Fields

| Field | Description |
|---|---|
| card_id | Unique card identifier |
| customer_id | Customer identifier |
| card_type | Type of card |
| region | Card/customer region |
| issue_date | Date card was issued |
| activation_status | Active or Inactive |
| monthly_limit_ngn | Assigned monthly card limit |

---

# Data Preparation

The datasets were prepared for analysis using Power Query.

Key preparation steps included:

- Reviewing column data types
- Standardizing date fields
- Standardizing categorical values
- Checking transaction amounts
- Checking for missing values
- Checking for duplicate transaction IDs
- Creating transaction month fields
- Creating success indicators
- Validating transaction statuses
- Reviewing relationships between customers and cards
- Preparing datasets for Power BI modeling

---

# Data Model

The Power BI model uses the card transaction table and card portfolio table.

### Main relationship

```text
Card Portfolio
     |
     | card_id
     |
     v
Card Transactions

The card portfolio table provides card-level attributes while the transaction table provides transaction-level activity.
This structure allows transaction performance to be analyzed by:
* Card type
* Region
* Activation status
* Issue date
* Customer
* Transaction date
* Channel
* Merchant category

Power BI Dashboard
The dashboard is designed around four main analytical areas.
1. Executive Overview
Key performance indicators:
* Total Transactions
* Transaction Value
* Successful Transactions
* Success Rate
* Average Transaction Value
* Declined Transactions
* Reversed Transactions
* Active Cards
* Activation Rate
Recommended visuals
* Monthly transaction trend
* Transaction value by region
* Transaction value by card type
* Transaction status distribution
* Transaction volume by channel
* Transaction value by merchant category

2. Transaction Performance
This page focuses on transaction activity.
Metrics include:
* Transaction volume
* Transaction value
* Average transaction value
* Success rate
* Decline rate
* Reversal rate
Analysis can be filtered by:
* Date
* Region
* Card type
* Channel
* Merchant category

3. Customer & Transaction Analysis
This page examines customer transaction behavior.
Analysis includes:
* Customer transaction frequency
* Customer transaction value
* Average transaction value
* Merchant category preferences
* Channel usage
* High-value transactions
* Regional customer activity
The objective is to identify patterns that can support customer engagement and product decisions.

4. Card Performance
This page focuses on the card portfolio.
Metrics include:
* Cards issued
* Active cards
* Inactive cards
* Activation rate
* Transactions by card type
* Transaction value by card type
* Regional card activity
* Card usage patterns

Key DAX Measures
The following measures are used to create the main dashboard KPIs.
Total Transactions

Total Transactions =
COUNTROWS(CardTransactions)

Transaction Value

Transaction Value =
SUM(CardTransactions[transaction_amount_ngn])

Successful Transactions

Successful Transactions =
CALCULATE(
    [Total Transactions],
    CardTransactions[status] = "Successful"
)

Successful Transaction Value

Successful Transaction Value =
CALCULATE(
    [Transaction Value],
    CardTransactions[status] = "Successful"
)

Success Rate

Success Rate =
DIVIDE(
    [Successful Transactions],
    [Total Transactions],
    0
)

Format this measure as a percentage.
Declined Transactions

Declined Transactions =
CALCULATE(
    [Total Transactions],
    CardTransactions[status] = "Declined"
)

Decline Rate

Decline Rate =
DIVIDE(
    [Declined Transactions],
    [Total Transactions],
    0
)

Reversed Transactions

Reversed Transactions =
CALCULATE(
    [Total Transactions],
    CardTransactions[status] = "Reversed"
)

Reversal Rate

Reversal Rate =
DIVIDE(
    [Reversed Transactions],
    [Total Transactions],
    0
)

Average Transaction Value

Average Transaction Value =
DIVIDE(
    [Transaction Value],
    [Total Transactions],
    0
)

Total Cards

Total Cards =
DISTINCTCOUNT(CardPortfolio[card_id])

Active Cards

Active Cards =
CALCULATE(
    [Total Cards],
    CardPortfolio[activation_status] = "Active"
)

Inactive Cards

Inactive Cards =
CALCULATE(
    [Total Cards],
    CardPortfolio[activation_status] = "Inactive"
)

Activation Rate

Activation Rate =
DIVIDE(
    [Active Cards],
    [Total Cards],
    0
)


SQL Analysis
SQL was used to perform transaction analysis and generate business-level metrics.
Transaction Performance by Card Type

SELECT
    card_type,
    COUNT(*) AS transaction_count,
    SUM(transaction_amount_ngn) AS transaction_value,
    AVG(transaction_amount_ngn) AS average_transaction_value
FROM card_transactions
WHERE status = 'Successful'
GROUP BY card_type
ORDER BY transaction_value DESC;

This query identifies which card types generate the highest successful transaction value.

Monthly Transaction Performance

SELECT
    transaction_month,
    COUNT(*) AS transaction_count,
    SUM(transaction_amount_ngn) AS transaction_value,
    AVG(transaction_amount_ngn) AS average_transaction_value
FROM card_transactions
GROUP BY transaction_month
ORDER BY transaction_month;

This analysis supports monthly performance monitoring.

Transaction Performance by Region

SELECT
    region,
    COUNT(*) AS transaction_count,
    SUM(transaction_amount_ngn) AS transaction_value,
    AVG(transaction_amount_ngn) AS average_transaction_value
FROM card_transactions
WHERE status = 'Successful'
GROUP BY region
ORDER BY transaction_value DESC;

This identifies the regions generating the highest transaction activity.

Transaction Status Analysis

SELECT
    status,
    COUNT(*) AS transaction_count,
    SUM(transaction_amount_ngn) AS transaction_value
FROM card_transactions
GROUP BY status
ORDER BY transaction_count DESC;

This helps monitor successful, declined, and reversed transactions.

Channel Analysis

SELECT
    channel,
    COUNT(*) AS transaction_count,
    SUM(transaction_amount_ngn) AS transaction_value,
    AVG(transaction_amount_ngn) AS average_transaction_value
FROM card_transactions
GROUP BY channel
ORDER BY transaction_value DESC;

This identifies the transaction channels generating the highest activity.

Merchant Category Analysis

SELECT
    merchant_category,
    COUNT(*) AS transaction_count,
    SUM(transaction_amount_ngn) AS transaction_value,
    AVG(transaction_amount_ngn) AS average_transaction_value
FROM card_transactions
WHERE status = 'Successful'
GROUP BY merchant_category
ORDER BY transaction_value DESC;

This provides insight into customer spending behavior.

Data Quality Checks
Data quality is an important part of the analysis.
The following checks were considered:
Duplicate Transactions

SELECT
    transaction_id,
    COUNT(*) AS record_count
FROM card_transactions
GROUP BY transaction_id
HAVING COUNT(*) > 1;

Missing Transaction Values

SELECT
    COUNT(*) AS missing_amounts
FROM card_transactions
WHERE transaction_amount_ngn IS NULL;

Invalid Transaction Amounts

SELECT
    COUNT(*) AS invalid_amounts
FROM card_transactions
WHERE transaction_amount_ngn <= 0;

Transaction Status Distribution

SELECT
    status,
    COUNT(*) AS transaction_count
FROM card_transactions
GROUP BY status;

These checks help ensure that reporting outputs are based on consistent and reliable data.

Excel Analysis
Excel can also be used to validate and analyze the datasets before loading them into Power BI.
Examples of useful functions include:
XLOOKUP

=XLOOKUP(A2,CardPortfolio[card_id],CardPortfolio[card_type],"Not Found")

Used to retrieve card attributes.
SUMIFS

=SUMIFS(
    CardTransactions[transaction_amount_ngn],
    CardTransactions[status],
    "Successful"
)

Used to calculate successful transaction value.
COUNTIFS

=COUNTIFS(
    CardTransactions[status],
    "Successful"
)

Used to count successful transactions.
AVERAGEIFS

=AVERAGEIFS(
    CardTransactions[transaction_amount_ngn],
    CardTransactions[status],
    "Successful"
)

Used to calculate average successful transaction value.

Key KPIs
KPI	Definition
Total Transactions	Total number of card transactions
Transaction Value	Total transaction amount
Success Rate	Successful transactions divided by total transactions
Decline Rate	Declined transactions divided by total transactions
Reversal Rate	Reversed transactions divided by total transactions
Average Transaction Value	Transaction value divided by transaction count
Active Cards	Number of cards with Active status
Activation Rate	Active cards divided by total cards
Example Business Insights
The dashboard can be used to identify:
* Regions generating the highest transaction value
* Card types with stronger transaction activity
* Periods with unusually high or low transaction volume
* Channels generating the most transactions
* Merchant categories with the highest customer spend
* Transaction decline patterns
* Reversal activity
* Differences between transaction volume and transaction value
* Card activation gaps
* Potential areas requiring operational investigation
The analysis is designed to move beyond reporting by connecting KPI movements to potential business questions.

Business Recommendations
Based on the analytical findings, management could:
1. Investigate regions with high transaction volume but lower transaction value.
2. Review decline patterns by channel and merchant category.
3. Monitor reversal rates for unusual increases.
4. Focus card activation campaigns on regions with high inactive-card levels.
5. Analyze high-value customer transaction patterns to identify opportunities for customer retention.
6. Monitor transaction trends monthly to identify changes in card usage.
7. Review underperforming card types and customer segments.
8. Use transaction and customer behavior data to support product and marketing decisions.

Tools Used
* Power BI
* DAX
* Power Query
* SQL
* Microsoft Excel
* Python
* Git
* GitHub

Skills Demonstrated
This project demonstrates practical experience in:
* Data analysis
* Transaction analysis
* Customer behavior analysis
* KPI development
* Dashboard development
* Power BI
* DAX
* SQL
* Excel
* Power Query
* Data cleaning
* Data validation
* Data modeling
* Trend analysis
* Performance monitoring
* Business reporting
* Stakeholder-focused insights
* Analytical problem solving

Project Structure

Card-Portfolio-Transaction-Analytics/
│
├── README.md
│
├── data/
│   ├── card_transactions.csv
│   └── card_portfolio.csv
│
├── powerbi/
│   └── card_portfolio_analytics.pbix
│
├── sql/
│   ├── transaction_analysis.sql
│   ├── customer_analysis.sql
│   └── data_quality_checks.sql
│
├── dax/
│   └── kpi_measures.dax
│
├── screenshots/
│   └── dashboard_mockup.png
│
└── documentation/
    └── data_dictionary.md


Dashboard Preview
The dashboard provides an executive view of:
* Transaction volume
* Transaction value
* Success rate
* Average transaction value
* Regional performance
* Card type performance
* Transaction status
* Monthly trends

Portfolio Relevance
This project was developed as a portfolio case study for a Data Analyst role involving card portfolio reporting and transaction analytics.
It demonstrates the ability to:
* Work with transaction-level datasets
* Analyze customer and card behavior
* Build KPI-driven dashboards
* Write SQL queries
* Create DAX measures
* Validate data
* Identify trends and anomalies
* Communicate findings through business-focused reporting

Disclaimer
All data used in this project is synthetic.
The datasets do not contain real customer, card, transaction, banking, or financial information.
The project is intended for portfolio and learning purposes only.

