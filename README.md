# Olist E-Commerce Sales Analysis

Sales analysis of the Brazilian e-commerce dataset published by Olist, using Python (Pandas) and SQL.

**Status: in progress.** The category and monthly analysis in Python is done. Slow movers, charts and the SQL version are next.

## Questions this project answers

- Which product categories sell the most by units, and which earn the most revenue?
- Which categories sell poorly (slow movers)?
- Are there categories that sell many units but bring in little revenue?
- Which months are the peaks, and do sales grow over time?

## Dataset

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) on Kaggle: about 100,000 orders placed between September 2016 and October 2018.

| Table | Rows | Columns | Used for |
|---|---|---|---|
| `orders` | 99,441 | 8 | order status and purchase date |
| `order_items` | 112,650 | 7 | one row per item sold, with price |
| `products` | 32,951 | 9 | product category |
| `order_payments` | 103,886 | 5 | loaded, not used (payments are recorded per order, not per item) |

## Tools

Python, Pandas, Google Colab. SQL queries (SQLite) will be added in a later step.

## What has been done

1. **Loaded the data** with `kagglehub` and checked the shape of each table.
2. **Explored columns and missing values.** Some delivery-date columns have missing values, which matches orders that were never delivered.
3. **Checked order statuses.** 96,478 of the 99,441 orders are `delivered`.
4. **Decided what counts as a sale:** only `delivered` orders. Canceled, unavailable and in-progress orders are excluded, so the numbers reflect confirmed sales.
5. **Converted `order_purchase_timestamp` to a real date.** The data covers 2016-09-04 to 2018-10-17.
6. **Built the sales table:** one row per item sold, with its purchase date, category and price. `order_items` was joined to the delivered orders (inner join, which also removes items from undelivered orders) and then to `products` (left join, so no sale is dropped because its category is missing). Result: 110,197 rows.
7. **Translated category names to English** using the translation file. Items with no category are labelled `unknown` (1,537 items), so total revenue stays complete. Some categories kept their Portuguese name because they have no match in the translation file.
8. **Aggregated by category:** units, revenue, number of distinct products and orders, and average price.
9. **Aggregated by month:** units, revenue and orders, and compared the top months on each measure.

Revenue is the sum of item `price` (freight excluded).

## Findings so far

- **Units and revenue tell different stories.** Among the top 10 categories by revenue, `bed_bath_table` sells the most units (10,953) but ranks third in revenue (about 1.02M), with an average price of about 93. `watches_gifts` ranks second in revenue (about 1.17M) with roughly half the units (5,859), because its average price is about 199. The top category by revenue is `health_beauty` (about 1.23M).
- **November 2017 is the top month on all three measures:** revenue (987,765), units (8,475) and orders (7,289). April and May 2018 come very close in revenue (about 974K and 978K) with fewer units, so their revenue per unit is higher.
- **This is a peak, not yet a seasonal pattern.** There are no delivered orders in November 2016, so the data holds only one full November. A link to Black Friday is a hypothesis that has not been tested.

## Data notes

- 1,537 of the 110,197 sold items have no product category.
- The first months of the dataset are almost empty (3 items in September 2016, 1 item in December 2016, none in November 2016), and the data ends on 2018-10-17, so October 2018 is partial. These months should not be read as real drops in sales.

## Next steps

- Find slow-moving categories (low units per product).
- Add charts.
- Write the main queries in SQL.
- Summarize the findings.
- Build a Power BI dashboard.

## How to load the data

```python
import kagglehub

path = kagglehub.dataset_download("olistbr/brazilian-ecommerce")
```
