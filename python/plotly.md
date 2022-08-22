# Plotly

https://plotly.com/python/plotly-fundamentals/

```python
import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])
fig.show()
```

## メモ
* hist系はデータ点数が多いと固まるくらい重い


### Multiple line plot
When the dataframe structure is not suitable for "color" option.

```python
fig = px.line()
fig.add_scatter(x=df['X'], y=df['A'], mode='lines', name='A')
fig.add_scatter(x=df['X'], y=df['B'], mode='lines', name='B')
fig.show()
```

# Seaborn

### figsizeをmatplotのplt使わずに変更する方法
```python
sns.set(rc={'figure.figsize':(10, 10)})
```