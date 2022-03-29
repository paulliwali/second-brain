# Plot


```python
def legend_without_duplicate_labels(ax):
    handles, labels = ax.get_legend_handles_labels()
    unique = [(h, l) for i, (h, l) in enumerate(zip(handles, labels)) if l not in labels[:i]]
    ax.legend(*zip(*unique))
```	

```python
color_map = plt.get_smap("viridis")
norm = plt.Normalize(0, 20)
plt.plot(df, color=color_map(norm(value)))
scalarmappaple = plt.cm.ScalarMappable(norm=norm, cmap=plt.cm.viridis)
plt.colorbar(scalarmappaple, oreientation="horizontal")
```

```python
ax.set_xticklabels(ax.get_xticklabels(), rotation=30)
```

```python
Try using the legend='full' parameter for seaborn.lineplot()
```