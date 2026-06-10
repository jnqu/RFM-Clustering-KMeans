# Customer Segmentation using RFM Analysis and K-Means Clustering

## Overview

This project segments the customers of a UK-based online retailer into distinct, actionable groups using **RFM analysis** and the **K-Means clustering** algorithm.

Recency, Frequency, and Monetary value (RFM) is an analytical model that groups customers according to how recently they last purchased (Recency), how often they purchase (Frequency), and how much they spend (Monetary). By scoring customers across these three dimensions, businesses can identify their most valuable customers and tailor marketing strategy to each group [1].

The data is drawn from the **Online Retail II** dataset [2], a transactional record from a UK-based, registered non-store online retailer that primarily sells unique all-occasion gifts, many of whose customers are wholesalers. The analysis uses the transactions covering **December 2009 – December 2010**. The combination of RFM features with K-Means clustering follows the customer-segmentation methodology established by Chen, Sain, and Guo [3].

## Objectives

1. Clean and prepare raw transactional data into a reliable, customer-level dataset suitable for analysis.
2. Engineer Recency, Frequency, and Monetary (RFM) features for each customer.
3. Apply K-Means clustering to group customers into meaningful segments based on their RFM profiles.
4. Interpret and label the resulting segments, and translate them into concrete business recommendations.

## Methodology

### Data
- **Source:** Online Retail II dataset (UCI Machine Learning Repository) [2].
- **Scope:** 525,461 transactions spanning December 2009 – December 2010.
- **Granularity:** Aggregated from individual transaction lines to one row per customer (4,285 customers after cleaning).

### Data Cleaning
- Removed transactions with missing `Customer ID` (records that cannot be attributed to a customer).
- Retained only genuine sales invoices, excluding cancellations (prefix `C`) and accounting adjustments (prefix `A`).
- Filtered stock codes to valid product codes, removing non-product entries such as postage, bank charges, gift-card top-ups, manual adjustments, and test transactions.
- Removed non-positive quantities and prices, and dropped duplicate records.
- Approximately 76% of the original records were retained.

### Feature Engineering
- **Recency:** days between a customer's most recent purchase and the dataset's reference date.
- **Frequency:** number of distinct invoices (orders) per customer.
- **Monetary:** total spend per customer.

### Preprocessing
- High-value customers were **retained** rather than removed as outliers, as they represent the business's most valuable segment.
- RFM features, which are heavily right-skewed, were **log-transformed** to reduce skew, then **standardised** (zero mean, unit variance) so that each feature contributes equally to the distance metric used by K-Means.

### Clustering and Evaluation
- The optimal number of clusters was selected using the **Elbow method** (inertia) and the **Silhouette score**.
- A value of **k = 4** was chosen, balancing statistical separation with business interpretability.
- Segments were profiled on the original (un-transformed) RFM values and compared against the overall customer average.

## Key Findings

Four customer segments were identified:

| Segment | Share | Avg. Recency | Avg. Frequency | Avg. Monetary | Description |
|---|---|---|---|---|---|
| **MVPs** | 16.1% | 11 days | 14.2 | £7,828 | Recent, frequent, high-spending customers; the most valuable segment. |
| **Regulars** | 29.1% | 73 days | 4.4 | £1,848 | Average-everything customers forming the core base. |
| **Newcomers** | 20.7% | 22 days | 2.0 | £530 | Recently acquired customers with low frequency and spend so far. |
| **Inactive** | 34.0% | 182 days | 1.3 | £320 | Lapsed, low-value customers who have not purchased in roughly six months. |

1. **Revenue is highly concentrated.** The MVP segment comprises only 16.1% of customers yet records by far the highest average spend and frequency, underlining the importance of retention for a small group of high-value customers.
2. **The largest segment is the least profitable.** Inactive customers make up 34.0% of the base but contribute the lowest average value, representing a substantial pool of lapsed or one-off buyers.
3. **Recency separates comparable-value segments.** Newcomers and Inactive customers have similar frequency and monetary profiles but differ sharply in recency, making recency the decisive variable for distinguishing customers worth re-investing in from those likely already lost.

## Limitations

- The analysis covers a single 12-month period; segment membership is a snapshot and would shift over time.
- High-value customers were retained and handled through log-transformation rather than removal; an alternative approach would model these customers separately.
- The segmentation uses only RFM features. Incorporating additional signals such as product-category preferences or customer tenure could yield more refined segments.

## References

[1] Fernando, J. *Recency, Frequency, Monetary Value (RFM).* Investopedia. https://www.investopedia.com/terms/r/rfm-recency-frequency-monetary-value.asp

[2] Chen, D. *Online Retail II Data Set.* UCI Machine Learning Repository, 2019. https://archive.ics.uci.edu/dataset/352/online+retail

[3] Chen, D., Sain, S. L., & Guo, K. *Data mining for the online retail industry: A case study of RFM model-based customer segmentation using data mining.* Journal of Database Marketing & Customer Strategy Management, 19(3), 197–208, 2012. https://link.springer.com/article/10.1057/dbm.2012.17
