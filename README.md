# Olist E-Commerce Sales Analysis

Sales analysis of the Brazilian e-commerce dataset published by Olist, using Python (Pandas) and SQL.

**Status: in progress.** Data loading, exploration and the first cleaning steps are done. The analysis itself is next.

## Questions this project will answer

- Which product categories sell the most by units, and which earn the most revenue?
- Which categories sell poorly (slow movers)?
- Are there categories that sell many units but bring in little revenue?
- Which months are the peaks, and do sales grow over time?

## Dataset

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) on Kaggle: about 100,000 orders placed between September 2016 and October 2018.

Tables used so far:

| Table | Rows | Columns | Used for |
|---|---|---|---|
| `orders` | 99,441 | 8 | order status and purchase date |
| `order_items` | 112,650 | 7 | one row per item sold, with price |
| `products` | 32,951 | 9 | product category |
| `order_payments` | 103,886 | 5 | loaded, not used yet (payments are recorded per order, not per item) |

## Tools

Python, Pandas, Google Colab. SQL queries (SQLite) will be added in a later step.

## What has been done so far

1. **Loaded the data** with `kagglehub` and checked the shape of each table.
2. **Explored the columns and missing values.** Some delivery-date columns have missing values, which matches orders that were never delivered.
3. **Checked order statuses.** 96,478 of the 99,441 orders are `delivered`. The rest are shipped, canceled, unavailable, invoiced, processing, created or approved.
4. **Decided what counts as a sale:** only `delivered` orders. Canceled, unavailable and still-in-progress orders are excluded, so the numbers reflect confirmed sales.
5. **Converted `order_purchase_timestamp` to a real date.** The data covers 2016-09-04 to 2018-10-17.
6. **Built the sales table:** one row per item sold, with its purchase date, category and price. `order_items` was joined to the delivered orders (inner join, which also removes items from undelivered orders) and then to `products` (left join, so no sale is dropped because its category is missing). Result: 110,197 rows.

## Things noticed in the data

- 1,537 of the 110,197 sold items have no product category.
- The first and last months of the dataset are probably incomplete. To be checked when plotting monthly sales.
- Revenue will be calculated from item `price` in `order_items`, since payments are recorded per order.

## Next steps

- Decide how to handle the items with no category.
- Add English category names using the category translation file.
- Compare units sold and revenue by category.
- Look at monthly trends and peaks.
- Write the same analysis as SQL queries.
- Add charts and a short summary of findings.

## How to load the data

```python
import kagglehub

path = kagglehub.dataset_download("olistbr/brazilian-ecommerce")
```
