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

## Kapacitor

## Grafana alerts