Flatten multiindex and rename with underscore

```python
df.columns = ["_".join(a) for a in df.columns.to_flat_index()]
```

Show more rows 

```python
pd.set_option('display.max_rows', 500)
```

Subtotals 

```python
df = pd.pivot_table(
	df, 
	values=["values_to_agg"], 
	index=["index_col"], 
	columns=["columns"], 
	aggfunc=np.mean, 
	margins=True
).stack("columns")
```