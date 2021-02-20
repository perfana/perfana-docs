---
title: Test run details
has_children: false
nav_order: 4
---

# Analyzing test runs
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

## Summary

The test run summary view has a number of sections

### Test run information

The test run information sections shows all the meta data for the test. It is possible to add annotations to the test run. If required, it is also possible to manually update the `Version` property by clicking it.

### Test properties

The test properties section shows all of the key-value pairs pairs passed by the performance test. This feature can for instance be used to describe a test setup used for the test.

### Check results

The check result section shows the Service Level Objectives check results and, if applicable, checks on the delta's for Service Level Indicators between this and earlier test runs.

![Check results](https://docs.perfana.io/docs/images/check-results.png)

Exapnding the sections by clicking them will reveal more detailed information on the check results. See [Service Level Indicators configuration](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#key-metrics)

### Alerts

If any [alerts](https://docs.perfana.io/docs/alerts/alerts.html) were triggered during the test they will be displayed in this section.

## Comments

In the comments tab, comments on the test are displayed. Comments can be added for selected graphs from this view, the `Service Level Indicators` view or the `Dashboards` view and can be used to share knowledge among team members and/or other users.

![Comments](https://docs.perfana.io/docs/images/comments.png)

### Adding comments

Adding comments to a test run can be done from different places:
* From the `Comments` tab, by clicking `Add comment`
* From the `Service Level Indicators` tab, by cliking the `comment` icon in the top right of the metric panels
* From the `Dashboards` tab, by cliking the `comment` icon in the top right of the dashboard panels

The `Add comment` dialog has the following fields:
* **Comment**: Use this field to add your comment. You can mentions users by entering a `@` and selecting the user from the drop down list
* **Select notification channels**: If one or more notification channels have been configured for the `system under test` you can can select them here. This will send a notification of the comment to the selected channels
* **Select dashboard**: [Optional, based on from where dialog has been triggered] Select dashboard to comment on 
* **Select panel**: [Optional, based on from where dialog has been triggered] Select panel to comment on. when selected the panel will be shown in the dialog 

## Service Level Indicators

In this tab all configured Service Level Indicators graphs can be inspected in one overview. See [Service Level Indicators configuration](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#key-metrics)


## Dashboards

In this tab all [Grafana dashboards](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#grafana-dashboards) linked to this tes can be viewed. Click on the header rows to expand or collapse the dashboards. Use the filter (accepts regular expressions) to display a subset of the dashboards only. 

## Report

If a [reporting template](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#reporting-template) was configured, the `Report` tab will be available. The report can be used to share test results with stakeholders by poviding a selection of relevant graphs with descriptions. The report will be generated automatically based on the configured reporting template. Default descriptions can be configured in the template and can edited for each test run if necessary. The report is automatically persisted by opening the report tab.

## Compare

When there are multiple test runs available with the samen `environment` and `workload` properties, the `Compare` tab will be available. The compare feature can be used to select another test run as baseline and compare all, or a selection of dashboards and metrics. The compare results are stored in the database for future reference.

To do a new comparison, click on `Compare`, this will launch the `Comparison wizard` that will guide you through the configuration steps:

### Select baseline test run
{: .no_toc }
Select test run to use as baseline. This can be any test run, executed prior to or after the current test run
### Select comparison type
{: .no_toc }
* Service Level Indicators only: Compare configured Service Level Indicators only and use comparison thresholds as specified
* Custom: Select what dashboards and metrics to compare 

### Settings
{: .no_toc } 
* **Exclude ramp up time**: if checked, the configured ramp up time will be excluded when comapring the metrics
* **Average all metrics per panel**: When comparing, metrics are matched on the series names. Sometimes the series names contain dynamic parts that vary between test runs and cannot be mapped. In that case this option can be used as workaround. In that case an average over all series will be compared.
* **Metric aggregation**: configure what aggregation to use when comparing the metrics
* **Evaluate comparison results**: This option can be used to flag comparison results if they do need meet the specified condition. 

### Select dashboards
{: .no_toc } 
Select which Grafana dashboards to compare.
### Select panels
{: .no_toc }  
Select per dashboard what panels to compare.
### Save comparison results
{: .no_toc }  
Provide a description for the comparison result and click `Compare`


![Compare results](https://docs.perfana.io/docs/images/compare-results.png)

## Tracing

Perfana can be [integrated with Jaeger](https://docs.perfana.io/docs/integrations/integrations.html#jaeger) to include tracing information in your test run results. If tracing has been setup the `Traces` tab will be visible. From this tab you will be able to deeplink into the Jaeger UI based on a number of filters:

* **Only show traces that fail to meet Service Level Objective**: If a Service Level Objective has been specified for response times metrics, the reqirement values can be used to filter traces.
* **Only show traces with duration longer than**: Can be used to filter on minimum duration of the traces
* **Only show traces with duration shorte than**: Can be used to filter on maximum duration of the traces
* **Request name**: Filter on request name

To deeplink into the Jager UI based on the filters, click on the request names. 

![Traces](https://docs.perfana.io/docs/images/traces.png)

## Manage

The `Manage` is only available for users that have a `team-admin` role for the team reponsible for the system under test and users with `admin` role. It has a number of sections for managing the test run:

* **Status**: displays the status for the different stages the test run will go through after it has finished. It also displays the expiry date for the test run. Read moren on test run expiry [here](https://docs.perfana.io/docs/administration/administration.html#data-retention-and-test-run-expiry)
* **Manage Grafana snapshots**: create, delete, update en view Grafana snapshots for the test run. To keep the snapshots from being deleted after the configured retention period, use `save snapshot`. If a report is generated for the test run (by opening the report tab), the snapshots are automatically saved.
* **Manage test run**:  This section can be used to delete a test run or to set it as `fixed baseline`. Users with the `admin` role are also allowed to edit the test run properties.
* **Manage checks**: This section can be used to manually re-evaluate the configured checks
* **Manage Report**: This section can be used to delete the persisted report