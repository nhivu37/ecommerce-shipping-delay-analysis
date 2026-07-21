# ecommerce-shipping-delay-analysis
SQL, Python &amp; statistical analysis of e-commerce shipping delays

## E-Commerce Shipping Delay Analysis
End-to-end data analytics project examining what drives late deliveries in an e-commerce shipping dataset, combining SQL, statistical testing, and predictive modeling.

## Overview
using ~11,000 orders from an e-commerce logistic dataset, this project investigates which operational and customer factors are associated with late deliveries, tests those relationships statistically, and builds a predictive model - while explicitly indentifying and correcting for data leakage rather than reporting an inflated accuracy score.

## Dataset: 
E-Commerce Shipping Data (Kaggle,~11,000 orders, 12 features)

## Tools & Skills
- SQL (SQLite via Python) - aggregations, CASE WHEN bucketing, HAVING, subqueries, RANK() window functions, CTEs
- Python - pandas for data manipulation, scipy.stats for hypothesis testing, scikit-learn for modeling, matlotlib/seaborn for visualization
- Statistics - chi-square tests of independence
- Machine Learning - logistic regression, feature importance analysis, leakage detection

## Project Structure
1. SQL Analysis - 12 queries exploring late delivery rates by warehouse, shipment mode, customer behavior, discount level, and product characteristics, including window functions and subqueries.
2. Statistical Testing - Chi-square tests to confirm which relationships found in SQL are statiistically significant.
3. Predictive Modeling - Two logistic regression models built and compared to test for data leakage

4. ## Key Findings
5. 1. Warehouse and shipment mode have no meaningful effect on delivery delays. Late delivery rates were nearly identical across all warehouses (58-60%) and shipment modes (59-60%), confirmed by chi-square tests (p>0.05 for both). These are common;y assumed risk factors that this analysis rules out.
   2. Discount level is a near-perfect(and likely leaked) predictor of delay. Every single order with a discount above 10% was late (0 exceptions across 2,647 orders), and the chi-square test for this relationship was highly significant (p≈0)/ This pattern most likely reflects discounts being issued after a delay was alreadyknown - e.g.,as compensation - rather than discounts causing delays.
   3. A leakage-aware model retains nearly all predictive power without the leaked feature. Two logistic regression models were compared:
      | Model | Features | Accuracy |
|---|---|---|
| A — with leakage | Includes `Discount_offered` | 63.55% |
| B — realistic | Excludes `Discount_offered` | 63.14% |

The 0.4-point gap shows the leakage effect is diluted because the high-discount segment is only ~ 24% of orders. Model B's strongest predictors - weight_in_gms and Customer_care_calls - indicate the dataset contains genuine, non-leaked predictive signal, making it a more trustworthy basis for a production model.

## Why This Approach Matters
Rather than reporting the highest possible accuracy, this project prioritizes indentifying why a feature is predictive before using it - a data leakage check that is easy to skip but critical in real-world modeling, where a leaked feature (like a discount applied after the fact)would be unavailable at prediction time and would silently break the model in production.

## How to Run
The full analysis is in Ecommerce_Shipping_Analysis.ipynb, desigmed to run in Google Colab:
1. Open the notebook in Colab.
2. Upload Train.csv(from the Kaggle dataset above) when prompted.
3. Run all cells sequentially - SQL queries, statistical tests, and models are all self-contained in the notebook using SQLite and scikit-learn.

4. ## Author
5. Nhi Vu - Business Administrtion (Data Analytics concentration), University of Alabama in Huntsville
