---
title: Test configuration
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

The `System under test` settings and `Notifications channels`  are specified on selected `System under test` level.

The `Grafana dashboards` linked to a test run are configured for a combination  of `System under test` and `Test environment` properties.

The `Service Level Indicators`, `Reporting template`, `Abort alert tags` and `Links` are configured for a combination  of `System under test`, `Test environment` and `Workload` properties.

--- 

> Please refer to [profiles](https://docs.perfana.io/docs/administration/administration.html#profiles-configuration) for automated test run configuration.
--- 

## System under test settings 

To open the `System under tests` settings click on the menu item in the `settings` section of the sidebar. 

The following properties can be configured:

* **Jaeger service name**: if a [Jaeger integration](https://docs.perfana.io/docs/integrations/integrations.html#jaeger) has been set up, the service name to use as entrypoint can be set here
* **Dynatrace entities**: if a [Dynatrace integration](https://docs.perfana.io/docs/integrations/integrations.html#dynatrace-enterprise-feature) has been set up, one or more Dynatrace entities can be linked to the system under test
* **Baseline test run id**: For each `test environment` - `workload` combination a baseline test run can be set that is used in the automated analysis of test runs.

--- 

> The Jaeger and Dynatrace properties can be set on **global** level, **test environment level** or **workload** level. Values specified on **workload** level override those on **test environment** level and values specified on **test environment** level override **global** values.
--- 

![System under test settings](https://docs.perfana.io/docs/images/system-under-test-settings.png)

## Notifications channels (Enterprise feature)

Perfana can use [Slack](https://docs.perfana.io/docs/integrations/integrations.html#slack-enterprise-feature), [Teams](https://docs.perfana.io/docs/integrations/integrations.html#teams-enterprise-feature) or [Google Chat](https://docs.perfana.io/docs/integrations/integrations.html#google-chat-enterprise-feature) channels to notify your team of specified events. 

In order to setup a nofication channel for your `sytem under test` click on the `Notifications channels` item in the `Settings` section of the side bar.

This will open the `Add notification channel` dialog with the following fields:
* **System under test**
* **Channel type**: Select `Slack`, `Teams` or `Google Chat`
* **Channel name**: Provide channel name
* **Webhook url**: Provide Webhook url
* **Send notification when a test run has finished** When this option is checked a notification will be sent to the channel when a test runs has finished
* **Send notification for failed test runs only** When this option is checked a notification will be sent to the channel only when a test run has failed
* **Send notification to this channel if any of the team members are mentioned in comments** When this option is checked a notification will be sent to the channel when any of your team members is mentioned in comment
* **Include user mentions** Select additional users,that will trigger a notification when mentioned.
* **Use as team notifications channel (for all systems under test for this team)**: If set to true, this notification channel will be used for all `systems under test` that are linked to this `team`

## Grafana dashboards

It is possible to link any Grafana dashboard that is [registered in Perfana](https://docs.perfana.io/docs/administration/administration.html#grafana-configuration) to a test run. In order to add a dashboard click `Add dashboard` and the `Add Grafana dashboard` dialog will be showed.

The form has the following fields:
* **Grafana**: select Grafana instance 
* **Dashboard name**: Select Grafana dashboard
* **Dashboard label**: Add descriptive label, e.g. "Host metrics webserver 1"
* **Variables**: If the Grafana dashboard has [templating variables](https://grafana.com/docs/grafana/next/variables/#templates-and-variables), select variables and set one or more values.

## Service Level Indicators  

When one or more Grafana dashboards have been linked to the test runs, it is possible to select metrics from these dashboards as `Service Level Indicators`. 

--- 

> Service Level Indicators are set on `workload` level, so this item in the sidebar will be active only when the `workload` property in the `Test run selector` has been set.

--- 
Service Level Indicators can be optionally configured to be automatically assessed after a test run has finished. The results can then be used to act as a `quality gate` when running tests from a CI/CD pipeline. See [Setting up Perfana as quality gate](https://docs.perfana.io/docs/administration/ci-cd.html#quality-gate)

Two types of checks can be configured for a `servive level indicator`:
* **Service Level Objective**: Check if the `servive level indicator` value is `greater than` or `smaller than` a specified value. An example check would be "Average CPU usage should be smaller than 70%" or "Maximum throughput should be higher than 1000 (rps)"
* **Comparison**: Check if the `delta` between test runs is within allowed thresholds. Example: "Allow a positve deviation of 20% between test runs for the 99th percentile response times"

### Add Service Level Indicator
{: .no_toc }

To add a Service Level Indicator, click `Add metric`. This will open the `Add Service Level Indicator` form.

The form has the following fields:
* **Grafana**: select Grafana instance
* **Dashboard label**: Select dashboard to select metric from
* **Panel**: Select [panel](https://grafana.com/docs/grafana/latest/features/panels/panels/). Currently only [graph panels](https://grafana.com/docs/grafana/latest/features/panels/graph/) and [table panels] (https://grafana.com/docs/grafana/next/panels/visualizations/table/) can be used as `service level indicator`.
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

If `Service Level Indicators` have been configured via [profiles](https://docs.perfana.io/docs/administration/administration.html#profiles-configuration) they can't be edited or removed (only by an user with the `admin` role). These `Service Level Indicators` will display the originating `profile` in the last column of the table. The only property that can be edited for these Service Level Indicators is the `Only apply to metrics matching regex pattern` property

### Regular expression helper
{: .no_toc }
In the `Service Level Indicators` and `Check results` views it is possible to use the `Filter request helper` to set the `Only apply to metrics matching regex pattern` property for a SLI by clicking the `filter` icon in the end the row. 
If there is check results data available the `Filter request helper` can be used to preview the results is a regular expression is used to filter the requests.

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

> The `Abort alert tags` are set on `workload` level, so this item in the sidebar will be active only when the `workload` property in the `Test run selector` has been set. 

--- 

Perfana can automatically abort a test run based on specified tag of [alerts](https://docs.perfana.io/docs/alerts/alerts.html) received during the test.

To add an `Abort alert tag` click `Add alert tag`, this will open the `Add abort alert tag` form with the following fields:

* **System under test**
* **Workload**
* **Test environment**
* **Alert source**: select the alert source, AlertManager, Kapacitor or Grafana
* **Tag key**: provide the tag key you want to trigger a test run abort
* **Tag value**: provide the tag value you want to trigger a test run abort## Abort alert tags 


## Links 
--- 

> The `Links` are set on `workload` level, so this item in the sidebar will be active only when the `workload` property in the `Test run selector` has been set. 

--- 

With the `Links` feature you can create deep links from your test run results to any relevant url. In the url you can use dynamic values passed via the `variables` property in your test script. Also, for your convenience, start and end timestamps are available in several formats.

To add a `link` click `Add link`, this will open the `Add link` form with the following fields:
* **Name**: Name for the link, this will be displayed in the `test run summary` view
* **Url**: Url for the link. To add one of the available variables to the url, type the "{" (open curly brace) character, this will display a dropdown menu of all the available variables. 


