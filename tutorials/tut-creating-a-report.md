---
description: >-
  Reports wrap the results of your Python analyses, such as datasets and plots,
  into interactive documents which you can share.
---

# Creating a Report

## Introduction

For many Python data analyses, you only want to share a specific user-facing part of it rather than the whole code or notebook. This often is of the form of a standalone product that non-technical people can view directly from their existing tools - e.g. browsers, email, Slack, etc., and without the overhead and requirements of Python and Jupyter.

Datapane allows you to programmatically create reports from the objects in your Python analyses, such as pandas DataFrames, plots from visualization libraries, and Markdown text. 

## Creating a report

Datapane provides a Python API that allows you to create, save, and publish reports comprised of a collection of data-centric components.

For instance, Datapane provides a Table component that takes a pandas DataFrame. We can create a Table component by passing a DataFrame into it, and create a Report with that single component in it as follows:

{% code title="simple\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

dataset = pd.read_csv('https://covid.ourworldindata.org/data/owid-covid-data.csv')
df = dataset.groupby(['continent', 'date'])['new_cases_smoothed_per_million'].mean().reset_index()

dp.Report(
    dp.Table(df)
).save(path='report.html', open=True)
```
{% endcode %}

Copying this code into a new script and running it will generate the report. 

```bash
$ python3 simple_report.py
```

![](../.gitbook/assets/image%20%28108%29.png)

If you send this HTML file to somebody \(or [publish it on _Datapane Public_](publishing-and-sharing.md#publish-your-report)\), they will be able to view your dataset, sort and filter it, and download it as a CSV.

### A richer report

That report was pretty basic, but we can jazz it up by adding some plots and text. Unlike a traditional BI tool, Datapane does not rely on a proprietary visualization engine; instead, it natively supports Python visualization libraries such as Altair, Plotly, Bokeh, and Folium. 

Let's take the example above, and plot some data using the Python library Altair.

{% hint style="info" %}
We'll use the below sample code snippet for the rest of the tutorials, so feel free to copy and paste it into a new Python script or Jupyter notebook and follow along
{% endhint %}

{% code title="simple\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

dataset = pd.read_csv('https://covid.ourworldindata.org/data/owid-covid-data.csv')
df = dataset.groupby(['continent', 'date'])['new_cases_smoothed_per_million'].mean().reset_index()

plot = alt.Chart(df).mark_area(opacity=0.4, stroke='black').encode(
    x='date:T',
    y=alt.Y('new_cases_smoothed_per_million:Q', stack=None),
    color=alt.Color('continent:N', scale=alt.Scale(scheme='set1')),
    tooltip='continent:N'
).interactive().properties(width='container')

dp.Report(
    dp.Plot(plot), 
    dp.Table(df)
).save(path='report.html', open=True)
```
{% endcode %}

When this python script is run, using the same command as earlier, we get the following report.

![](../.gitbook/assets/image%20%28101%29.png)

#### Viewing your report

As described above, we can easily view our report in a browser. However, there are other ways to view and share our report whilst developing it.

{% hint style="info" %}
If you are using Datapane on a private instance, any report with ORG visibility will be accessible to people inside your organization, but not the public.
{% endhint %}

Datapane has specific integration into Jupyter Notebooks: if you're iterating a report, instead of having to open a new window to view it, you can preview a report directly from inside your notebook by calling `report.preview()`, embedding it live into your notebook.

![](../.gitbook/assets/image%20%2885%29.png)

Next, we will explore how to publish and share reports online, either on _Datapane Public_, or on your own _Datapane for Teams_ instance.

## 

