# Card Portfolio & Transaction Analytics

Power BI | SQL | Excel | Power Query | DAX | Python

A synthetic card and transaction analytics project designed to demonstrate how transaction-level data can be cleaned, validated, modeled, analyzed, and converted into business-focused KPIs and dashboards.

## Project Overview

This project simulates a card portfolio analytics environment covering transaction performance, card usage, customer behavior, regional performance, merchant categories, transaction channels, transaction outcomes, and card activation.

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
- Monthly transaction trends
- Data quality and validation

## Business Questions

- How many card transactions are processed each month?
- What is the total transaction value?
- What percentage of transactions are successful?
- Which regions generate the highest transaction value?
- Which card types generate the most transaction activity?
- Which transaction channels are most frequently used?
- Which merchant categories generate the highest transaction value?
- What percentage of transactions are declined or reversed?
- How does transaction activity change over time?
- Which transaction patterns require further investigation?
- How many cards are active versus inactive?
- Which regions have the strongest card activity?
- Which channels or merchant categories have higher decline rates?
- Where are potential card activation opportunities?

## Dataset

The project uses two synthetic datasets.

### Card Transactions

`data/card_transactions.csv`

Contains 2,500 synthetic card transactions covering January to June 2026.

| Field | Description |
|---|---|
| transaction_id | Unique transaction identifier |
| transaction_date | Date of transaction |
| customer_id | Customer identifier |
| card_id | Card identifier |
| region | Customer or card region |
| card_type | Type of card |
| channel | Transaction channel |
| merchant_category | Merchant category |
| transaction_amount_ngn | Transaction value in NGN |
| status | Successful, Declined, or Reversed |
| is_successful | Binary success indicator |
| transaction_month | Transaction month |

### Card Portfolio

`data/card_portfolio.csv`

Contains 600 synthetic cards.

| Field | Description |
|---|---|
| card_id | Unique card identifier |
| customer_id | Customer identifier |
| card_type | Type of card |
| region | Card or customer region |
| issue_date | Date the card was issued |
| activation_status | Active or Inactive |
| monthly_limit_ngn | Assigned monthly card limit |

## Data Preparation

Power Query was used to prepare the datasets for analysis.

Key preparation activities included:

- Reviewing column data types
- Standardizing date fields
- Standardizing categorical values
- Checking transaction amounts
- Checking missing values
- Checking duplicate transaction IDs
- Creating transaction month fields
- Creating transaction success indicators
- Validating transaction statuses
- Reviewing customer and card relationships
- Preparing datasets for Power BI modeling

## Data Quality Checks

Data quality checks include:

- Duplicate transaction IDs
- Missing transaction values
- Invalid transaction amounts
- Missing card IDs
- Missing customer IDs
- Invalid transaction statuses
- Duplicate card IDs
- Invalid activation statuses
- Date validation
- Referential integrity between cards and transactions

## Power BI Data Model

The model uses two main tables:

    Card Portfolio
          |
          | card_id
          |
          v
    Card Transactions

The Card Portfolio table provides card-level attributes.

The Card Transactions table provides transaction-level activity.

This allows transaction performance to be analyzed by:

- Card type
- Region
- Activation status
- Issue date
- Customer
- Transaction date
- Channel
- Merchant category
- Transaction status

# Dashboard

## Executive Overview

The executive dashboard provides a high-level view of card and transaction performance.

### KPIs

- Total Transactions
- Transaction Value
- Successful Transactions
- Success Rate
- Average Transaction Value
- Declined Transactions
- Reversed Transactions
- Total Cards
- Active Cards
- Activation Rate

### Visuals

- Monthly transaction trend
- Transaction value by region
- Transaction value by card type
- Transaction status distribution
- Transaction volume by channel
- Transaction value by merchant category
- Active versus inactive cards

## Transaction Performance

This page focuses on transaction activity and transaction outcomes.

Metrics include:

- Transaction volume
- Transaction value
- Average transaction value
- Successful transactions
- Success rate
- Declined transactions
- Decline rate
- Reversed transactions
- Reversal rate

Filters include:

- Transaction date
- Region
- Card type
- Channel
- Merchant category
- Transaction status

## Customer & Transaction Analysis

This page examines customer transaction behavior.

Analysis includes:

- Customer transaction frequency
- Customer transaction value
- Average transaction value
- Merchant category preferences
- Channel usage
- High-value transactions
- Regional customer activity
- Transaction frequency by customer
- Transaction value by customer

## Card Performance

This page focuses on card portfolio performance.

Metrics include:

- Cards issued
- Active cards
- Inactive cards
- Activation rate
- Transactions by card type
- Transaction value by card type
- Regional card activity
- Card usage patterns
- Average transaction value by card type

# DAX Measures

## Total Transactions

    Total Transactions =
    COUNTROWS(CardTransactions)

## Transaction Value

    Transaction Value =
    SUM(CardTransactions[transaction_amount_ngn])

## Successful Transactions

    Successful Transactions =
    CALCULATE(
        [Total Transactions],
        CardTransactions[status] = "Successful"
    )

## Successful Transaction Value

    Successful Transaction Value =
    CALCULATE(
        [Transaction Value],
        CardTransactions[status] = "Successful"
    )

## Success Rate

    Success Rate =
    DIVIDE(
        [Successful Transactions],
        [Total Transactions],
        0
    )

Format as Percentage.

## Declined Transactions

    Declined Transactions =
    CALCULATE(
        [Total Transactions],
        CardTransactions[status] = "Declined"
    )

## Decline Rate

    Decline Rate =
    DIVIDE(
        [Declined Transactions],
        [Total Transactions],
        0
    )

Format as Percentage.

## Reversed Transactions

    Reversed Transactions =
    CALCULATE(
        [Total Transactions],
        CardTransactions[status] = "Reversed"
    )

## Reversal Rate

    Reversal Rate =
    DIVIDE(
        [Reversed Transactions],
        [Total Transactions],
        0
    )

Format as Percentage.

## Average Transaction Value

    Average Transaction Value =
    DIVIDE(
        [Transaction Value],
        [Total Transactions],
        0
    )

## Total Cards

    Total Cards =
    DISTINCTCOUNT(CardPortfolio[card_id])

## Active Cards

    Active Cards =
    CALCULATE(
        [Total Cards],
        CardPortfolio[activation_status] = "Active"
    )

## Inactive Cards

    Inactive Cards =
    CALCULATE(
        [Total Cards],
        CardPortfolio[activation_status] = "Inactive"
    )

## Activation Rate

    Activation Rate =
    DIVIDE(
        [Active Cards],
        [Total Cards],
        0
    )

Format as Percentage.

# SQL Analysis

## Transaction Performance by Card Type

    SELECT
        card_type,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value,
        AVG(transaction_amount_ngn) AS average_transaction_value
    FROM card_transactions
    WHERE status = 'Successful'
    GROUP BY card_type
    ORDER BY transaction_value DESC;

## Monthly Transaction Performance

    SELECT
        transaction_month,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value,
        AVG(transaction_amount_ngn) AS average_transaction_value
    FROM card_transactions
    GROUP BY transaction_month
    ORDER BY transaction_month;

## Transaction Performance by Region

    SELECT
        region,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value,
        AVG(transaction_amount_ngn) AS average_transaction_value
    FROM card_transactions
    WHERE status = 'Successful'
    GROUP BY region
    ORDER BY transaction_value DESC;

## Transaction Status Analysis

    SELECT
        status,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value
    FROM card_transactions
    GROUP BY status
    ORDER BY transaction_count DESC;

## Channel Analysis

    SELECT
        channel,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value,
        AVG(transaction_amount_ngn) AS average_transaction_value
    FROM card_transactions
    GROUP BY channel
    ORDER BY transaction_value DESC;

## Merchant Category Analysis

    SELECT
        merchant_category,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value,
        AVG(transaction_amount_ngn) AS average_transaction_value
    FROM card_transactions
    WHERE status = 'Successful'
    GROUP BY merchant_category
    ORDER BY transaction_value DESC;

## Customer Analysis

    SELECT
        customer_id,
        COUNT(*) AS transaction_count,
        SUM(transaction_amount_ngn) AS transaction_value,
        AVG(transaction_amount_ngn) AS average_transaction_value
    FROM card_transactions
    WHERE status = 'Successful'
    GROUP BY customer_id
    ORDER BY transaction_value DESC;

## High-Value Transaction Analysis

    SELECT
        transaction_id,
        transaction_date,
        customer_id,
        card_id,
        region,
        card_type,
        channel,
        merchant_category,
        transaction_amount_ngn,
        status
    FROM card_transactions
    WHERE transaction_amount_ngn >= 500000
    ORDER BY transaction_amount_ngn DESC;

## Transaction Decline Analysis

    SELECT
        region,
        channel,
        merchant_category,
        COUNT(*) AS declined_transactions,
        SUM(transaction_amount_ngn) AS declined_value
    FROM card_transactions
    WHERE status = 'Declined'
    GROUP BY
        region,
        channel,
        merchant_category
    ORDER BY declined_transactions DESC;

# Data Quality SQL

## Duplicate Transactions

    SELECT
        transaction_id,
        COUNT(*) AS record_count
    FROM card_transactions
    GROUP BY transaction_id
    HAVING COUNT(*) > 1;

## Missing Transaction Values

    SELECT
        COUNT(*) AS missing_amounts
    FROM card_transactions
    WHERE transaction_amount_ngn IS NULL;

## Invalid Transaction Amounts

    SELECT
        COUNT(*) AS invalid_amounts
    FROM card_transactions
    WHERE transaction_amount_ngn <= 0;

## Transaction Status Distribution

    SELECT
        status,
        COUNT(*) AS transaction_count
    FROM card_transactions
    GROUP BY status
    ORDER BY transaction_count DESC;

## Duplicate Cards

    SELECT
        card_id,
        COUNT(*) AS record_count
    FROM card_portfolio
    GROUP BY card_id
    HAVING COUNT(*) > 1;

# Excel Analysis

Excel can be used to validate and analyze the datasets before loading them into Power BI.

## XLOOKUP

    =XLOOKUP(
        A2,
        CardPortfolio[card_id],
        CardPortfolio[card_type],
        "Not Found"
    )

Retrieves card attributes.

## SUMIFS

    =SUMIFS(
        CardTransactions[transaction_amount_ngn],
        CardTransactions[status],
        "Successful"
    )

Calculates successful transaction value.

## COUNTIFS

    =COUNTIFS(
        CardTransactions[status],
        "Successful"
    )

Counts successful transactions.

## AVERAGEIFS

    =AVERAGEIFS(
        CardTransactions[transaction_amount_ngn],
        CardTransactions[status],
        "Successful"
    )

Calculates average successful transaction value.

## COUNTIFS by Region

    =COUNTIFS(
        CardTransactions[region],
        A2,
        CardTransactions[status],
        "Successful"
    )

Calculates successful transaction volume by region.

## SUMIFS by Region

    =SUMIFS(
        CardTransactions[transaction_amount_ngn],
        CardTransactions[region],
        A2,
        CardTransactions[status],
        "Successful"
    )

Calculates successful transaction value by region.

# Key KPIs

| KPI | Definition |
|---|---|
| Total Transactions | Total number of card transactions |
| Transaction Value | Total transaction amount |
| Successful Transactions | Number of successful transactions |
| Success Rate | Successful transactions divided by total transactions |
| Decline Rate | Declined transactions divided by total transactions |
| Reversal Rate | Reversed transactions divided by total transactions |
| Average Transaction Value | Transaction value divided by transaction count |
| Total Cards | Distinct number of cards |
| Active Cards | Number of cards with Active status |
| Inactive Cards | Number of cards with Inactive status |
| Activation Rate | Active cards divided by total cards |

# Business Insights

The dashboard is designed to identify:

- Regions generating the highest transaction value
- Card types with stronger transaction activity
- Periods with unusually high or low transaction volume
- Channels generating the most transactions
- Merchant categories with the highest customer spend
- Transaction decline patterns
- Reversal activity
- Differences between transaction volume and transaction value
- Card activation gaps
- High-value transaction patterns
- Customers with high transaction activity
- Areas requiring further investigation

The analysis connects KPI movements to business questions rather than presenting metrics in isolation.

# Business Recommendations

1. Investigate regions with high transaction volume but lower transaction value.

2. Review transaction decline patterns by channel and merchant category.

3. Monitor reversal rates for unusual increases.

4. Focus card activation activities on regions with high inactive-card levels.

5. Analyze high-value customer transaction patterns to identify customer engagement opportunities.

6. Monitor monthly transaction trends to identify changes in card usage.

7. Review underperforming card types and customer segments.

8. Investigate transaction channels with unusual decline or reversal patterns.

9. Compare transaction volume with transaction value to identify differences in customer behavior.

10. Use transaction and customer behavior data to support product and operational decisions.

# Analytical Approach

    Raw Data
       |
       v
    Data Validation
       |
       v
    Data Cleaning
       |
       v
    Data Transformation
       |
       v
    Data Modeling
       |
       v
    SQL Analysis
       |
       v
    DAX KPI Development
       |
       v
    Power BI Dashboard
       |
       v
    Business Insights
       |
       v
    Recommendations

# Dashboard Preview

The dashboard provides an executive view of:

- Transaction volume
- Transaction value
- Success rate
- Average transaction value
- Active cards
- Activation rate
- Regional performance
- Card type performance
- Transaction status
- Monthly trends
- Channel performance
- Merchant category performance

![Card Portfolio Analytics Dashboard](screenshots/dashboard_mockup.png)

# Project Structure

    Card-Portfolio-Transaction-Analytics/
    |
    ├── README.md
    |
    ├── data/
    │   ├── card_transactions.csv
    │   └── card_portfolio.csv
    |
    ├── powerbi/
    │   └── card_portfolio_analytics.pbix
    |
    ├── sql/
    │   ├── transaction_analysis.sql
    │   ├── customer_analysis.sql
    │   └── data_quality_checks.sql
    |
    ├── dax/
    │   └── kpi_measures.dax
    |
    ├── screenshots/
    │   └── dashboard_mockup.png
    |
    └── documentation/
        └── data_dictionary.md

# Tools Used

- Microsoft Power BI
- DAX
- Power Query
- SQL
- Microsoft Excel
- Python
- Git
- GitHub

# Skills Demonstrated

- Card transaction analysis
- Customer behavior analysis
- Transaction-level data analysis
- KPI development
- Dashboard development
- Power BI
- DAX
- SQL
- Advanced Excel
- Power Query
- Data cleaning
- Data validation
- Data modeling
- Trend analysis
- Performance monitoring
- Business reporting
- Customer analytics
- Transaction monitoring
- Root cause analysis
- Business-focused insight generation
- Analytical problem solving

# Portfolio Relevance

This project was developed as a portfolio case study for Data Analyst roles involving card portfolio reporting, transaction analytics, customer analytics, and business performance monitoring.

It demonstrates the ability to:

- Work with transaction-level datasets
- Analyze customer and card behavior
- Build KPI-driven dashboards
- Write SQL queries
- Develop DAX measures
- Use Excel for data validation and analysis
- Clean and validate datasets
- Build analytical data models
- Identify trends and anomalies
- Investigate transaction performance
- Communicate findings through business-focused reporting

The project is particularly relevant to roles involving:

- Card portfolio analytics
- Transaction monitoring
- Customer analytics
- Payments analytics
- Business intelligence
- Performance reporting
- Data quality
- Operational analytics

# Disclaimer

All data used in this project is synthetic.

The datasets do not contain real customer, card, transaction, banking, payment, or financial information.

The project is intended for portfolio demonstration and learning purposes only.
