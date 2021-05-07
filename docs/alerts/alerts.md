---
title: Alerts
layout: default
has_children: false
nav_order: 5
---

# Alerts
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

Perfana integrates with a number of tools that emit alerts based on rules. If an integrated alerting tool triggers an alert, Perfana tries to map alert tags to properties of a running test. If it finds a match, it creates an [annotation](https://grafana.com/docs/grafana/latest/reference/annotations/) for all linked Grafana dashboards in each graph for that test run. This helps you to track down root causes for bottlenecks in your test.

## Alertmanager

To integrate Alertmanager alerts with Perfana, Perfana has to be set up as a Prometheus [receiver](https://prometheus.io/docs/alerting/configuration/#receiver).

```yaml
receivers:
- name: default-receiver
  webhook_configs:
  - url: 'http://<perfana_host>/prometheus-alerts'
    send_resolved: false

``` 

Next, create one ore more [alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/) like the ones in [prometheus.rules.yml](https://github.com/perfana/perfana-demo/blob/master/prometheus-config/prometheus.rules.yml).

## Grafana alerts

You can also create [alerts in Grafana](https://grafana.com/docs/grafana/latest/alerting/rules/). To integrate these alerts with Perfana create a [notifications channel](https://grafana.com/docs/grafana/latest/alerting/notifications/) in Grafana.

The channel type should be `webhook`, the url `http://<perfana-host>/grafana-alerts` and the httpMethod `POST`

![Notications channel](/docs/images/notifications-channel.png)

Then create an alert on a panel in one of your dashboards. To do so, choose `edit` from the panel menu and find the `Alert` section.

Set the following properties:

No Data & Error Handling:
* **If no data or all values are null** SET STATE TO `Ok`
* **If execution error or timeout** SET STATE TO `Keep Last State`

--- 

> Make sure interval used in the query `query(A, 10s, now)` is larger than the write interval for the metric, to prevent the alerting engine to set state to `Ok` unintentionally when `null` values are returned!

---

Notications:

* **Send to**: Select the `Perfana` notification channel you have created earlier. 
* **Message**: Add a descriptive message, this will be displayed in Perfana.
* **Tags**: 
  * **system_under_test**: *Required* should match the `system_under_test` for your test  
  * **test_environment**: *Required*  should match the `test_environment` for your test
  * **test run abort tag**: *Optional* See [tag to use to abort a test run](/docs/testconfiguration/testconfiguration.html#abort-alert-tags). In the example screenshot below this is `maximum_response_time_abort`. 
  
![Alert](/docs/images/alert.png)
