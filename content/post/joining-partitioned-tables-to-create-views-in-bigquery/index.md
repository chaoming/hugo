+++
author = "Chaoming Li"
title = "Joining Partitioned Tables to Create Views in BigQuery"
description = "How to join partitioned tables in a single SQL query that still can query the tables using partitions to save time and money."
date = "2018-03-30"
categories = [
    "Big Data"
]
tags = [
    "bigquery",
    "partitioned tables",
    "sql",
    "database",
    "google cloud platform",
    "gcp"
]
image = "bigquery-500x250.png"
+++

BigQuery date partitioned tables can limit the data scan by partitions to help keep the query cost low and improve query performance. However, Google’s documents do not give much clue about how to use partitioned tables to create views that support partition queries. I did some research and found the way to do it, even with views that are created from joining partitioned tables.

To expose the partitioning pseudo-column, create a query similar to this one:

```sql
SELECT *, EXTRACT(DATE FROM _PARTITIONTIME) AS date
FROM partitioned-table;
```

If you save the query as a view, you can limit the query partitions by using the date column in your `WHERE` clause.

The above example is probably too simple for any actual query. We often create views because we have complex queries that join multiple tables. In that case, to make the views support partitions, it is just as simple as creating multiple date columns as the above example and making sure your query of the view contains a `WHERE` clause that limits the search of these date columns. Here is an example of joining two partition tables:

```sql
SELECT * FROM (
    (SELECT *, EXTRACT(DATE FROM _PARTITIONTIME) AS date1
    FROM partitioned-table1) t1
    LEFT JOIN
    (SELECT *, EXTRACT(DATE FROM _PARTITIONTIME) AS date2
    FROM partitioned-table1) t2
    ON t1.key = t2.key
);
```

When you query this view, and you want to limit the query to the data of `2018-03-20`, you can do this:

```sql
SELECT * FROM partitioned-view
WHERE date1 = '2018-03-20' AND date2 = '2018-03-20';
```

I wish I could combine the different date columns as one, so I tried to join the date columns as keys in the ON statement, but that doesn’t help. It seems you always have to have one date column for each partitioned table in the query, that’s a little bit annoying but it will help lower the costs.