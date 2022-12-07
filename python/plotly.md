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

## Multiple plot(subplot)
https://plotly.com/python/subplots/

```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go

fig = make_subplots(rows=1, cols=2)

fig.add_trace(
    go.Scatter(x=[1, 2, 3], y=[4, 5, 6]),
    row=1, col=1
)

fig.add_trace(
    go.Scatter(x=[20, 30, 40], y=[50, 60, 70]),
    row=1, col=2
)

fig.update_layout(height=600, width=800, title_text="Side By Side Subplots")
fig.show()
```

### Text Plot
```python
fig = go.Figure()

# Lines
fig.add_trace(
    go.Scatter(x=x, y=y,
                mode='lines',
                name='lines',
                line=dict(width=1, color="#0000ff"),
                showlegend=False,
                )
)

# Polygons
fig.add_trace(
    go.Scatter(x=x, y=y,
                fill="toself",
                fillcolor='#AFE1AF',
                opacity=0.7,
                line=dict(color="#AFE1AF"),
                showlegend=False,
                )
)
    
# Text
fig.add_trace(
    go.Scatter(
        x=d['x'],
        y=d['y'],
        mode="markers+text",
        text=df_point['text'],
        textfont=dict(size=8),
        showlegend=False,
    )
)

fig.update_layout(
    autosize=False,
    width=800,
    height=800,
)   
fig.update_yaxes(scaleanchor = "x", scaleratio = 1)
    
fig.show()
```


# Seaborn

### figsizeをmatplotのplt使わずに変更する方法
```python
sns.set(rc={'figure.figsize':(10, 10)})
```
