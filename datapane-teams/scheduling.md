---
description: Generate reports on a scheduled cadence to create live dashboards
---

# Scheduled Report Runs

If you deploy your script or Jupyter Notebook to Datapane, you may want to run it on a schedule to automatically create reports for your team -- for instance, to pull down fresh data from your warehouse each day, or poll an internal API for changes. 

Scheduling is controlled through the Datapane CLI, where you can use crontab to configure when you would like your report to run. 

To create a new schedule, use `create`:

```text
$ datapane schedule create <script name> <crontab> [-p <parameters>]
```

`create` takes three parameters:

* `script`: the name of the script to run
* `cron`: crontab representing the schedule interval
* `parameters` \(optional\): if your script requires parameters, a key/value list of parameters to use when running the script on schedule

{% hint style="info" %}
If you need help generating a crontab, please use a website such as [http://corntab.com/](http://corntab.com/).
{% endhint %}

If we wanted to run our COVID script every day at 9am, we could use the following command:

```text
$ datapane schedule [your-username]/covid_script "0 9 * * MON" 
Created schedule: 3 (https://acme.datapane.net/api/schedules/3/)
```

Optionally, we could also include any input parameters using `-p`

```text
$ datapane schedule [your-username]/covid_script "0 9 * * MON" -p '{"continents": ["Europe"], "field_to_plot": "gdp_per_capita"}' 
Created schedule: 4 (https://acme.datapane.net/api/schedules/4/)
```

If we wanted to find out what active schedules we have, we can use the `list` command:

```text
$ datapane schedule list
Available Schedules:
  id  script                                          cron         parameter_vals
----  ----------------------------------------------  -----------  -------------------------------------------------------------
   3  https://acme.datapane.net/api/scripts/X0AEQAd/  0 9 * * MON  {}
   4  https://acme.datapane.net/api/scripts/X0AEQAd/  0 9 * * MON  {"continents": ["Europe"], "field_to_plot": "gdp_per_capita"}

```

 If we would like to delete our schedule, we can use `delete`

```text
$ datapane schedule delete 3
Deleted schedule 3
```

## Using a scheduled report

As you can update a report, scheduling allows you to create a live report which is updated automatically on a cadence. If you browse to the report which your script publishes, it will automatically be updated and show you the latest version. Similarly, if you are [embedding your report](../reports/embedding-reports-in-social-platforms/#business-tooling) \(or an element of your report\) into a third-party platform such as Confluence or Salesforce, your embed will be updated automatically.

