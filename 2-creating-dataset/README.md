# Creating New BigQuery Datasets and Visualizing Insights

Course 2: [Creating New BigQuery Datasets and Visualizing Insights](https://www.coursera.org/learn/gcp-creating-bigquery-datasets-visualizing-insights)

## Content

### Creating and Managing Table with Partitions

https://cloud.google.com/bigquery/docs/partitioned-tables

This page provides an overview of partitioned table support in BigQuery.

A partitioned table is a special table that is divided into segments, called partitions, that make it easier to manage and query your data. By dividing a large table into smaller partitions, you can improve query performance, and you can control costs by reducing the number of bytes read by a query.

There are two types of table partitioning in BigQuery:

Tables partitioned by ingestion time: Tables partitioned based on the data's ingestion (load) date or arrival date.
Partitioned tables: Tables that are partitioned based on a TIMESTAMP or DATE column.
You will practice creating the second type (Partitioned Tables) in your next lab.

### Advanced Visualization

Read the below article on how to connect Stackdriver + BigQuery + DataStudio to monitor your BigQuery usage:

https://medium.com/google-cloud/visualize-gcp-billing-using-bigquery-and-data-studio-d3e695f90c08

## Useful SQL

### Deduplicating Rows with ARRAY_AGG()

We will be using this StackOverflow post by Felipe Hoffa as inspiration.

```sql
#standardSQL
# take the one name associated with a SKU
WITH product_query AS (
  SELECT
  DISTINCT
  v2ProductName,
  productSKU
  FROM `data-to-insights.ecommerce.all_sessions_raw`
  WHERE v2ProductName IS NOT NULL
)

SELECT k.* FROM (

  # aggregate the products into an array and
  # only take 1 result
  SELECT ARRAY_AGG(x LIMIT 1)[OFFSET(0)] k
  FROM product_query x
  GROUP BY productSKU # this is the field we want deduplicated
);
```
