---
description: All the blocks you can use to develop and layout your reports
---

# Building Reports

## Overview

Reports are comprised of multiple Blocks, which wrap up Python objects, such as Pandas DataFrames, Visualisations, and Markdown. Datapane also includes layout blocks to add tabs, pages, and interactive selects to your reports. 

We are always adding new components, and if you have some ideas on what you would like to use in your reports, please [start a discussion on GitHub](https://github.com/datapane/datapane/discussions).

In this section we describe the Block types and provide examples. More detailed API usage can be found in our [API docs](https://datapane.github.io/datapane/report.html). 

{% hint style="info" %}
If you pass your Python object into your without wrapping it in a specific block component, Datapane will try and automatically choose the best block type. 
{% endhint %}

## Report Types

Datapane has three types of reports, which dictate sizing and layout:

* **Report:** medium width and margins, optimized for mixed content
* **Article:** small width and large margins, optimized for long-form text
* **Dashboard:** full-width with no margins, optimized for grid layout and visualizations

The default type is report, and you can choose other types when you create your report as follows:

```python
import datapane as dp

# Create a report (default)
dp.Report(..., type=dp.ReportType.REPORT)

# Create a dashboard
dp.Report(..., type=dp.ReportType.DASHBOARD)

# Create an article
dp.Report(..., type=dp.ReportType.ARTICLE)
```

## Block Types

{% page-ref page="tables-and-data.md" %}

{% page-ref page="plots-and-visualizations.md" %}

{% page-ref page="text-code-and-html.md" %}

{% page-ref page="layout-pages-and-selects.md" %}

{% page-ref page="files-and-images.md" %}

## Default Block Handling

As well as explicitly specifying your block type \(for instance, by using `dp.Plot`\), Datapane will try and choose the best block for your object if you pass it in directly, for instance as follows:

```python
import datapane as dp
import pandas as pd

d = {'col1': [1, 2], 'col2': [3, 4]}
df = pd.DataFrame(data=d)

dp.Report(
  df,
  "This is text"
)
```

 The defaults are as follows:

| Object Type | Datapane Block |
| :--- | :--- |
| pandas DataFrame | `dp.Table` |
| string | `dp.Text` |
| Altair  | `dp.Plot` |
| Bokeh  | `dp.Plot` |
| Folium  | `dp.Plot` |
| Matplotlib / Seaborn  | `dp.Plot` |
| Plotly | `dp.Plot` |

