---
title: Test run configuration
has_children: false
nav_order: 3
---

# Test configuration
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

When a user has the `team-admin` role for a `team` that is responsible for  a `system under test`, the side bar in the `test runs view` will have a `settings` section.

In Perfana, test run configuration is set for a combination of test run properties. 

The `Grafana dashboards` linked to a test run are configured for a combination  of `System under test` and `Test environment` properties.

The `Service Level Indicators` and `Reporting template` are configured for a combination  of `System under test`, `Test environment` and `Workload` properties.

Test run configuration can be set via [profiles](https://docs.perfana.io/docs/administration/administration.html#profiles-configuration) or manually.

## Grafana dashboards

It is possible to link any Grafana dashboard that is [registered in Perfana](https://docs.perfana.io/docs/administration/administration.html#grafana-configuration) to a test run. In order to add a dashboard click `Add dashboard` and the `Add Grafana dashboard` dialog will be showed.

The form has the following fields:
* **Grafana**: select Grafana instance 
* **Dashboard name**: Select Grafana dashboard
* **Dashboard label**: Add descriptive label, e.g. "Host metrics webserver 1"
* **Variables**: If the Grafana dashboard has [templating values](https://grafana.com/docs/grafana/latest/reference/templating/), set variables and one or more values.

## Service Level Indicators  

When one or more Grafana dashboard have been linked to the test runs, it is possible to select metrics from these dashboards as `Service Level Indicators`. 

--- 

> Service Level Indicators are set on `workload` level, so this item in the sidebar will be active only when the `workload` property in the `Test run selector` has been set.

--- 
Service Level Indicators can be optionally configured to be automatically assessed after a test run has finished. The results can then be used to act as a `quality gate` when running tests from a CI/CD pipeline. See [Setting up Perfana as quality gate](https://docs.perfana.io/docs/administration/ci-cd.html#quality-gate)

Two types of checks can be configured for a `key metric`:
* **Service Level Objective**: Check if the `key metric` value is `greater than` or `smaller than` a specified value. An example check would be "Average CPU usage should be smaller than 70%" or "Maximum throughput should be higher than 1000 (rps)"
* **Comparison**: Check if the `delta` between test runs is within allowed thresholds. Example: "Allow a positve deviation of 20% between test runs for the 99th percentile response times"

### Add Service Level Indicator
{: .no_toc }

To add a Service Level Indicator, click `Add metric`. This will open the `Add Service Level Indicator` form.

The form has the following fields:
* **Grafana**: select Grafana instance
* **Dashboard label**: Select dashboard to select metric from
* **Panel**: Select [panel](https://grafana.com/docs/grafana/latest/features/panels/panels/). Currently only [graph panels](https://grafana.com/docs/grafana/latest/features/panels/graph/) can be used as key metric.
* **Exclude ramp up time when evaluating test run**: When this option is checked the configured `rampUpTime` will be excluded from evaluation data.
* **Average all panel series when comparing test runs**: if this option is checked all series produced by the panel will be averaged before evaluating the data.
* **Evaluate**: select the aggregation to be used on the data when evaluating: `Average` (default), `Maximum`, `Minimum` or `Last`
* **Only apply to metrics matching regex pattern**: a panel could produce multiple series. A regular expression can be provided to filter the series that the configured checks apply to, e.g.

![Match regex](https://docs.perfana.io/docs/images/match-regex.png)

* **Service Level Objective**: the Service Level Objective to check
* **Comparison**: the allowed deviation between test runs 
* **Update existing test runs**: if checked, the checks will be evaluated for all existing test runs with matching `System under test`, `Test environment` and `Workload` properties.

### Edit Service Level Indicator
{: .no_toc }


A Service Level Indicator can be edited by clicking the `pencil` icon in the row. 

If `Service Level Indicators` have been configured via [profiles](https://docs.perfana.io/docs/administration/administration.html#profiles-configuration) they can't be edited or removed. These `Service Level Indicators` will display the originating `profile` in the last column of the table. The only property that can be edited for these Service Level Indicators is the `Only apply to metrics matching regex pattern` property

### Regular expression helper
{: .no_toc }
In the Service Level Indicators view it is possible to use the `regular expression helper` to update the `Only apply to metrics matching regex pattern` property for a key metric by clicking the `settings` icon in the end of a key metric row.. If there is metric data available for a key metric the `regular expression helper` can be used to preview the results of a regular expression applied to a set of metrics.

![Regex helper](https://docs.perfana.io/docs/images/regex-helper.png)


## Reporting template 

--- 

> The `reporting template` is set on `workload` level, so this item in the sidebar will be active only when the `workload` property in the `Test run selector` has been set. 

--- 

Perfana can produce a test run report can be used to share test results with stakeholders by providing a selection of relevant graphs with descriptions. The report will be generated automatically based on the configured reporting template. 

To add a `panel` to the report template click the `Add panel` button. This will open the `Add report panel` form, with the following fields:

* **System under test**
* **Workload**
* **Test environment**
* **Grafana**: select Grafana instance
* **Dashboard label**: Select dashboard to select metric from
* **Panel**: Select [panel](https://grafana.com/docs/grafana/latest/features/panels/panels/). 
* **Annotation**: add a default annotation to be displayed next to the graph

## Abort alert tags 

--- 

> The `abort alert tags` are set on `workload` level, so this item in the sidebar will be active only when the `workload` property in the `Test run selector` has been set. 

--- 

Perfana can automatically abort a test run based on specified tag of [alerts](https://docs.perfana.io/docs/alerts/alerts.html) received during the test.

To add an `abort alert tag` click `Add alert tag`, this will open the `Add abort alert tag` form with the following fields:

* **System under test**
* **Workload**
* **Test environment**
* **Alert source**: select the alert source, AlertManager, Kapacitor or Grafana
* **Tag key**: provide the tag key you want to trigger a test run abort
* **Tag value**: provide the tag value you want to trigger a test run abort