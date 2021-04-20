---
title: Administration
has_children: false
nav_order: 6
---

# Administration
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## Grafana configuration

Perfana requires at least one Grafana instance to be configured. One or more Grafana instances can be configured via the `METEOR-SETTINGS` in the Perfana environment variables:

```
"grafanaInstances": [
    {
      "label": "Demo",
      "clientUrl": "http://localhost:3000",
      "serverUrl": "http://grafana:3000",
      "orgId": "1",
      "apiKey": "eyJrIjoiM3lqdHE2UzB4OTRJbnZBRVpncnZnaFBwb3lkbEloYWciLCJuIjoiYmxhIiwiaWQiOjF9",
      "username": "perfana",
      "password": "perfana",
      "snapshotInstance": true,
      "trendsInstance": true,
      
    }
  ],
  ```

* **label**: Descriptive name for this Grafana instance
* **clientUrl**: The grafana url that your browser uses to connect to the Grafana instances
* **serverUrl**: *Optional* Url to connect to Grafana instance from Perfana server side. For instance the service name when both Perfana and Grafana are running in Kubernetes.
* **orgId**: Grafana Organisation ID
* **apiKey**: API key with `admin` role, see [Grafana documentation](https://grafana.com/docs/grafana/latest/http_api/auth/#create-api-token)
* **username**: Username for Grafana admin user
* **password**
* **snapshotInstance**: if set to true this Grafana instance will be used to store snapshots.
* **trendsInstance**: if set to true Perfana will use this instance for hosting Perfana dashboards for trends and profile check results.

As an `admin` user you can maintain the Grafana dashboards that have been synced with Perfana. To do so open the `Grafana configuration` from the `Admin` section of the sidebar.


The `Grafana configuration` view will show you one or more Grafana instances and for each dashboard the dashboards that have been registered in Perfana. To register a Grafana dashboard is Perfana is simple: just add a `perfana` tag to the dashboard:

![Grafana dashboard tags](https://docs.perfana.io/docs/images/grafana-dashboard-tags.png)

The `grafana-perfana` service will now automatically register the dashboard in Perfana and will update it when changes are made. If the dashboard is deleted from Grafana by mistake, the `grafana-perfana` service will restore it. 

--- 

> If you use the `delete` icon in the `Linked dashboards` section, the dashboard will be deleted in both Perfana and Grafana!

---
## Teams configuration

Perfana allows admin users to create `Teams` to efficiently organise the test run data. `Teams` consist of one or more `team members` and a `system under test` is linked to one `team`.

To mantain the configured `Teams` an `admin` user can open the `Teams` item from the `Admin` section of the sidebar.

A new `Team` can be added by clicking the `Add team` button, this will open the `Add team` from with these fields:

* **Organisation**: select Organisation
* **Name**
* **Description**: *optional* Add description for the team

Click the `team` row to view the team members and the systems under test linked to this team.

A team has one or more `team members` and `users` can be member of one or more `teams`. To add `team members` click on the select box to select one or more users.

A `team` can be responsible for one or more `systems under test`, but a `system under test` is linked to one `team` only. To link `system under tests` to the selected team, select one or more via the select box.

--- 

> When you link a `system under test` to a `team` that was already linked to another team, it will no longer be linked to the other team!

---
## Users configuration 

The `users` view show all the users known in Perfana, if they have the `admin` role or for what teams they have the `team-admin` role. Admin users can assign roles to users or delete users. 

Adding users can be done only currently by:
* Using an external IDP via Keycloak
* By registering a new user via the login screen. To enable this set the following in the `METEOR-SETTINGS` in the `public` section:

```
  "public": {
    "forbidClientAccountCreation": "false",
    ...
  },
```
--- 
> New users will have a `viewer` role by default, allowing them to view all test results
---

## Profiles configuration 

Perfana profiles can be used to automatically configure test runs. By passing `tags` via the Gatling script [pom.xml](https://github.com/perfana/perfana-gatling-afterburner/blob/master/pom.xml#L252) profiles can be activated for test runs with the provide properties and the following configuration can automatically be added:

### Grafana dashboards 

Profiles can be used to automatically link Grafana dashboards to test runs. To use a Grafana dashboard for autoconfiguration it has to be tagged in Grafana as `perfana-template`

![Perfana template tag](https://docs.perfana.io/docs/images/perfana-template-tag.png)

To add a template dashboard to a `profile`, click on `Add dashboard` to open the form with these fields:

* **Profile**: The selected profile
* **Grafana**: Select the Grafana instance that hosts the template dashboard
* **Dashboard name**: select the template dashboard
* **Remove templating variables and replace them with values**: if set to true, templating variables in the created instance of the template dashboard will be replaced by hardcoded values. This is useful if you want to integrate [Grafana alerts](https://docs.perfana.io/docs/alerts/alerts.html#grafana-alerts) with Perfana. Unfortunately Grafana alerts cannot be created on templated dashboards.
* **Create separate dashboard for variable**: *optional* if a templating variable is selected, a separate Grafana dashboard will be created for each distinct value that is found for that templating variable.
* **Set hardcoded value for variables**: *optional* use this to set hardcoded values for the available templating variables
* **Match regex for variables**: *optional* use this to only add the dashboard when a value for the selected templating value matches the provided regular expression.

--- 

> The easiest way to use this feature is to label metric data with tags `system_under_test` and `test_environment` and in your template Grafana dashboard use these properties as templating variables. Perfana will automatically match these to the test run properties. If for some reason this is not possible, use the `Match regex for variables` to determine what metrics to add to your test. 
---

You can use dynamic variables passed by the load test script variables. The variable name should have the format `perfana-.*` e.g.

```
 <variables>
    <perfana-services>^optimus-prime-.*</perfana-services>
 </variables>
```
![Template dashboard configuration](https://docs.perfana.io/docs/images/template-dashboard.png)

### Service Level Indicators

For the any of the Grafana dashboards added to the `profile` it is possible to automatically add `Service Level Indicators` to test runs that are tagged with the `profile` name.

--- 

> This feature can be used to set ***enterprise-wide*** Service Level Objectives for specific metrics. The check results are recorded and the results can be viewed over time aggregated by `team` and `system under test`

--- 

To add a `service level indicator` to a `profile` click `Add metric`. Learn [here](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#add-service-level-indicator) how to add a service level indicator.

### Reporting panel

A `profile` can also be used to specify a reporting template for test runs with matching tags.

--- 

> This feature can be used to create a ***enterprise-wide uniform reporting standard***, for instance for formal release tests reports.

--- 
To add a `report panel` to the `reporting template` fo a `profile` click `Add panel`. Learn [here](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#reporting-template) how to add a `report panel`.


## License

In the `license` view you can add and view your license. There are two ways to add a license:

* Add license key: add your license key (requires Perfana to be able to connect to the internet)
* Add offline license key: add offline license key when no internet connection is available


## Notifications channels

Perfana can use [Slack](https://docs.perfana.io/docs/integrations/integrations.html#slack), [Teams](https://docs.perfana.io/docs/integrations/integrations.html#teams) or [Google Chat](https://docs.perfana.io/docs/integrations/integrations.html#google-chat) channels to notify your team of specified events. 

### Admin notification channel
As an `admin` user, you can setup notification channels that are not linked to a `system under test`. A typical use case for this would be a "performance engineering support channel" that can be included in comments by users to ask the community for help. To set up a such a channel, click on the `Admin notifications channel` item in the `Admin` section in the sidebar.

This will open the `Add notification channel` dialog with the following fields:
* **Channel type**: Select `Slack`, `Teams` or `Google Chat`
* **Channel name**: Provide channel name
* **Webhook url**: Provide Webhook url
* **Include user mentions** Select additional (admin) users,that will trigger a notification when mentioned.

## Jira configuration (Enterprise feature)

Perfana can integrate with one or more Jira instances to create issues and link them to test run results. The `Jira configuration` view can be used to setup the integration.

### Setting up an AppLink

To use Jira from Perfana an [AppLink](https://confluence.atlassian.com/adminjiraserver073/using-applinks-to-link-to-other-applications-861253079.html) is used. Perfana has a wizard that will guide you through the process. You will require a Jira user that has sufficient rights to set up AppLinks.

The first step is to click `Add Jira instance`, provide a host and description and determine if `GDPR strict mode` is enabled on the instance.

![Add Jira Instance](https://docs.perfana.io/docs/images/add-jira-instance.png)

The next step is to click the `configure` link in the `Oauth configured` column:

![Configure Jira oauth](https://docs.perfana.io/docs/images/jira-configure-oauth.png

Follow the wizard steps and finish by adding the Jira oauth configuration to the `METEOR-SETTINGS` and restart Perfana.

### Configure allowed issue types and priorities 

By default Perfana will show issue types and priorities that are configured for the selected Jira project. Is possible to limit the issue type and priorities options that can be used by adding the allowed options in the configuration section.

--- 
> Be aware by adding options here that do not exist for a specific project this could result in empty option lists in the `Add Jira issue` form!
---
### Configure Jira reporters

If the user that is used to login to Perfana does not exist in Jira, it is not possible to create issues. In the `Jira reporters` section one or more fallback users can be configured to be used as `reporter` in Jira issues. The configured users should ofcourse exist in the Jira instance!