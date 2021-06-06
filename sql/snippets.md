# SQL Snippets

```SQL
-- Convert datetime
FROM_UNIXTIME(timestamp_epoch_ms / 1000, 'UTC') as utc_timestamp

-- Note, that dBeaver might not print it with the timezone conversion, to show it you need to
CAST(utc_timestamp as varchar)
```
