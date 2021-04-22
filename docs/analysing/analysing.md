---
title: Analyzing test runs
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

The test run information sections shows all the meta data for the test. It is possible to add annotations to the test run. If required, it is also possible to manually update the `Version`, `Start`, `End` and `Tags` properties by clicking it.

### Variables

The variables section shows all of the key-value pairs pairs passed by the load test script. Variables can be used in [links](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#links)

### Check results

The check result section shows the Service Level Objectives check results and, if applicable, checks on the delta's for Service Level Indicators between this and earlier test runs.

![Check results](https://docs.perfana.io/docs/images/check-results.png)

Expanding the sections by clicking them will reveal more detailed information on the check results. See [Service Level Indicators configuration](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#service-level-indicators)

### Alerts

If any [alerts](https://docs.perfana.io/docs/alerts/alerts.html) were triggered during the test they will be displayed in this section. The alerts will also be displayed in graphs as vertical lines. In case of alerts originating from Grafana it is possible to view the metric that triggered the alert by clicking the `eye` icon. By clicking the `settings` icon you can edit the alert configuration in Grafana.

![Alerts](https://docs.perfana.io/docs/images/test-run-alerts.png)


### Events

If any events occured during the test (for instance triggered by using the [events-scheduler plugin](https://github.com/stokpop/event-scheduler)) they will be displayed in this section. The events will also be displayed in graphs as vertical lines.

![Events](https://docs.perfana.io/docs/images/test-run-events.png)

### Links

If any [links have been configured](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#links) they will be displayed in this section. 


## Comments

In the comments tab, comments on the test are displayed. Comments can be added for selected graphs from this view, the `Service Level Indicators` view or the `Dashboards` view and can be used to share knowledge among team members and/or other users.

![Comments](https://docs.perfana.io/docs/images/comments.png)

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

In this tab all configured Service Level Indicators graphs can be inspected in one overview. See [Service Level Indicators configuration](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#service-level-indicators).


## Dashboards 

In this tab all [Grafana dashboards](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#grafana-dashboards) linked to this tes can be viewed. The dashboards are organised in tabs based on dashboard tags. Click on the header rows to expand or collapse the dashboards. You can use the filter (accepts regular expressions) to display a subset of the dashboards only. 

![Test run dashboards](https://docs.perfana.io/docs/images/test-run-dashboards.png)

--- 

> Based on the [configured datasource retention times](https://docs.perfana.io/docs/administration/administration.html#data-retention-and-test-run-expiry) either the dashboards with "live" data from the datasource or a [snapshot](https://grafana.com/docs/grafana/next/sharing/share-dashboard/#publish-a-snapshot) of the dashboard will be shown.

---


## Report

If a [reporting template](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#reporting-template) was configured, the `Report` tab will be available. The report can be used to share test results with stakeholders by poviding a selection of relevant graphs with descriptions. The report will be generated automatically based on the configured reporting template. Default descriptions can be configured in the template and can edited for each test run if necessary. The report is automatically persisted by opening the report tab. 

It is also possible to add comparisons (see next paragraph) to one or more test runs to the report. 

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
* **Exclude ramp up time**: if checked, the configured ramp up time will be excluded when comparing the metrics
* **Average all metrics per panel**: When comparing, metrics are matched on the series names. Sometimes the series names contain dynamic parts that vary between test runs and cannot be mapped. In that case this option can be used as workaround so instead an average over all series will be compared.
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

Perfana can be [integrated with Jaeger](https://docs.perfana.io/docs/integrations/integrations.html#jaeger) to include tracing information in your test run results. If tracing has been set up the `Traces` tab will be visible. From this tab you will be able to deeplink into the Jaeger UI based on a number of filters:

* **Only show traces that fail to meet Service Level Objective**: If a Service Level Objective has been specified for response times metrics, the requirement values can be used to filter traces.
* **Only show traces with duration longer than**: Can be used to filter on minimum duration of the traces
* **Only show traces with duration shorter than**: Can be used to filter on maximum duration of the traces
* **Request name**: Filter on request name

To deeplink into the Jager UI based on the filters, click on the request names. 

![Traces](https://docs.perfana.io/docs/images/traces.png)

## Dynatrace (Enterprise feature)

Perfana can be [integrated with Dynatrace](https://docs.perfana.io/docs/integrations/integrations.html#dynatrace-enterprise-feature) to include tracing information in your test run results. If a Dynatrace integration has been setup the `Dynatrace` tab will be visible. 

The Dynatrace view has the following sections:

### Dynatrace entitiy

Use the dropdown to select one of the [configured Dynatrace entities](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#system-under-test-settings) for the `system under test`.
### Problems

The problem section will show if any "Problems" have been detected by the Dynatrace AI that impacted the selected Dynatrace entity during the test.
### Request filter

In this section you can set a number of request filters:

* **Only show traces that fail to meet Service Level Objective**: If a Service Level Objective has been specified for response times metrics, the requirement values can be used to filter traces.
* **Only show traces with duration longer than**: Can be used to filter on minimum duration of the traces
* **Only show traces with duration shorter than**: Can be used to filter on maximum duration of the traces
* **Request name**: Filter on request name

### Multidimensional analysis

This section has deeplinks, based on the selected Dynatrace entity and request filters, into the multidimensional analysis view in Dynatrace

### Deeplinks

This section has deeplinks, based on the selected Dynatrace entity and request filters, into miscellaneous views in Dynatrace that might help you analyse your test run results.

### Compare 

In the compare section you can select another test run as baseline and use the Dynatrace compare feature to compare several metrics for the test run.

## Jira (Enterprise feature)

Perfana can be [integrated with Jira](https://docs.perfana.io/docs/integrations/integrations.html#jira-enterprise-feature) to be able to create issues from Perfana and link them to a test run.

If the [Jira integration has been set up](https://docs.perfana.io/docs/administration/administration.html#jira-configuration-enterprise-feature), the `Jira` tab will be visible. 

### Create issue

To create a Jira issue click `create issue` to bring up the `Create Jira issue` form. The form will show the fields that have been configured "required" for the selected project and issue type. 

If any comments have been made for the  test run, there will be an option to include them in the Jira Issue.

If the user that is logged in Perfana does not exist in Jira, there will be a `reporter` dropdown to pick a reporter for the issue. These reporters can be specified in the [Jira configuration](https://docs.perfana.io/docs/administration/administration.html#jira-configuration-enterprise-feature)

### Link issue

If there are issues already created and linked to other test runs for the `system under test` a `Link to existing issue` button will appear that will start the `Add as comment to exisitng issue` dialog, to link the test run results as comment to an existing Jira issue.

## Manage

The `Manage` is only available for users that have a `team-admin` role for the team reponsible for the system under test and users with `admin` role. It has a number of sections for managing the test run:

* **Status**: displays the status for the different stages the test run will go through after it has finished. It also displays the expiry date for the test run. Read moren on test run expiry [here](https://docs.perfana.io/docs/administration/administration.html#data-retention-and-test-run-expiry)
* **Manage Grafana snapshots**: create, delete, update en view Grafana snapshots for the test run. To keep the snapshots from being deleted after the configured retention period, use `save snapshot`. If a report is generated for the test run (by opening the report tab), the snapshots are automatically saved.
* **Manage test run**:  This section can be used to delete a test run or to set it as `fixed baseline`. Users with the `admin` role are also allowed to edit the test run properties.
* **Manage checks**: This section can be used to manually re-evaluate the configured checks
* **Manage Report**: This section can be used to delete the persisted report

