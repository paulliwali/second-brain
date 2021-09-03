# SQL Snippets

## Advanced Queries

```SQL
-- CTE
WITH sub_table_name AS (
	SELECT *
	FROM db.table
), sub_table_name_2 as (
	SELECT *
	FROM db.table
)
SELECT *
FROM sub_table_name
LEFT JOIN sub_table_name_2
ON sub_table_name.col_name = sub_table_name_2.col_name
```

```SQL
-- Anti join
SELECT *
FROM table_left
LEFT JOIN table_right
ON table_left.col_name = table_right.col_name
WHERE table_right.site_id IS NULL 
```

## Time related

```SQL
-- Convert datetime
FROM_UNIXTIME(timestamp_epoch_ms / 1000, 'UTC') as utc_timestamp

-- Note, that dBeaver might not print it with the timezone conversion, to show it you need to
CAST(utc_timestamp as varchar)

-- Convert to epoch ms
CAST(to_unixtime(resampled_timestamp_s) AS bigint) as epoch_ms 

-- Get hour
HOUR(CAST(datetime_str as timestamp))
```

## Partitions

```SQL
-- Window functions
SELECT orderkey, clerk, totalprice,
	rank() OVER (PARTITION BY clerk
				ORDER BY totalprice DESC) as rnk
FROM orders
ORDER BY clerk, rnk
```