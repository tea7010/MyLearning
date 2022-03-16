https://plotly.com/python/plotly-fundamentals/

```python
import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])
fig.show()
```

## メモ
* hist系はデータ点数が多いと固まるくらい重い