---
title: Alerting
has_children: false
nav_order: 5
---

# Alerting
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

Perfana integrates with a number of tools that can emit alerts based on rules. If an integrated alerting tool triggers an alert, Perfana will try to map alert tags to properties of a running test. If it finds a match, it will create an [annotation](https://grafana.com/docs/grafana/latest/reference/annotations/) for all linked Grafana dashboards in each graph for that test run. This can help you track down root causes for bottlenecks in your test.

## Alertmanager

In order to integrate Alertmanager alerts with Perfana, Perfana has to be set up as a [receiver](https://prometheus.io/docs/alerting/configuration/#receiver)

```
receivers:
- name: default-receiver
  webhook_configs:
  - url: 'http://<perfana_host>/prometheus-alerts'
    send_resolved: false

``` 

Then create one ore more [alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/) like in [prometheus.rules.yml](https://github.com/perfana/perfana-demo/blob/master/prometheus-config/prometheus.rules.yml

## Grafana alerts

It is possible to create [alerts in Grafana](https://grafana.com/docs/grafana/latest/alerting/rules/). In order to integrate these alerts with Perfana a [notifications channel](https://grafana.com/docs/grafana/latest/alerting/notifications/) has to be configured.

The channel type should be `webhook`, the url `http://<perfana-host>/grafana-alerts` and the httpMethod `POST`

![Notications channel](https://docs.perfana.io/docs/images/notifications-channel.png)

Then create an alert on a panel in one of your dashboards. To do so, choose `edit` from the panel menu an go to the `Alert` section.

The follwoing properties should be set:

No Data & Error Handling:
* **If no data or all values are null** SET STATE TO `Ok`
* **If execution error or timeout** SET STATE TO `Keep Last State`

Notications:

* **Send to**: Select the `Perfana` notification channel you have created earlier. 
* **Message**: Add a descriptive message, this will be displayed in Perfana.
* **Tags**: 
  * **system_under_test**: *Required* should match the `system_under_test` for your test  
  * **test_environment**: *Required*  should match the `test_environment` for your test
  * **test run abort tag**: *Optional* [tag to use to abort a test run](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#abort-alert-tags)  
  

![Alert](https://docs.perfana.io/docs/images/alert.png)
