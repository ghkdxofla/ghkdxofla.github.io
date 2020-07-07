---
layout: post
title: "Bokeh chart 및 legend 설정"
comments: true
categories: [Data Science, Bokeh]
shortinfo: "Bokeh chart 중 line chart와 line stacked chart에 대해서 설명한다. 추가로, legend 설정을 통해 특정 series를 show/hide 하는 방법을 기술한다."
tags: [Data Science, Data Analysis, Visualization, Python]
---

### Chart 예시

**Line chart:**
```python
from bokeh.plotting import figure, show

TOOLS="hover,crosshair,wheel_zoom,zoom_in,zoom_out,box_zoom,undo,redo,reset,tap,save,box_select,poly_select"

p = figure(x_axis_type="datetime", plot_width=1600, plot_height=800, title="Line example", tools=TOOLS)
color_list = ['red', 'green', 'blue', 'orange', 'black', 'purple', 'pink', 'gold', 'yellowgreen', 'cyan', 'bisque', 'brown', 'coral', 'crimson', 'teal', 'olive']
p.y_range.start = 0.0
p.y_range.end = 1.2
x_data=[1, 2, 3, 4, 5]
y_data=[1, 4, 2, 2, 3]

p.line(x_data, y_data, legend_label='Line example series', color='green')

show(p)
```

**Line stacked chart:**
```python
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure, show

source = ColumnDataSource(data=dict(
    x=[1, 2, 3, 4, 5],
    y=[1, 4, 2, 2, 3]
))
p = figure(plot_width=400, plot_height=400)

p.varea_stack(['y'], x='x', color="red", source=source, name='gg', alpha=0.1)

show(p)
```

### Legend click을 통한 show/hide

```python
import pandas as pd
from bokeh.palettes import Spectral4
from bokeh.plotting import figure, show
from bokeh.sampledata.stocks import AAPL, IBM, MSFT, GOOG

p = figure(plot_width = 800, plot_height = 250, x_axis_type = "datetime", title = 'Glyphs hidden by default')

lines = {}
for data, name, color in zip([AAPL, IBM, MSFT, GOOG], ["AAPL", "IBM", "MSFT", "GOOG"], Spectral4):
    df = pd.DataFrame(data)
    df['date'] = pd.to_datetime(df['date'])
    lines[name] = p.line(df['date'], df['close'], line_width = 2, color = color, alpha = 0.8, legend = name)
    lines[name].visible = True if name == "IBM" else False

p.legend.location = "top_left"
p.legend.click_policy = "hide"

show(p)
```

### 참조 링크

[Bokeh Reference](https://docs.bokeh.org/en/latest/docs/reference.html)

[Legend click policy hide or mute](https://stackoverflow.com/questions/55709766/python-bokeh-define-preselected-ones-in-legend-click-policy-hide-or-mute)