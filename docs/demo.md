---
layout: default
title: Demo
nav_order: 8
---

# Perfana demo
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Demo environment

Perfana provides a demo environment that can be used to try out all the features. It is based on Docker compose and has a all components to emulate live-like environment for Perfana, including an application to performance test.

## Prerequisites

* [Docker](https://docs.docker.com/get-started/)
* [Docker compose](https://docs.docker.com/compose/gettingstarted/)
* 8Gb of RAM allocated to docker daemon

## Getting started

* Clone perfana-demo repository 
  ```
  git clone https://github.com/perfana/perfana-demo.git
  ```
  or download [here](https://github.com/perfana/perfana-demo/archive/master.zip)
* Inside the repository root run
  ```
  ./start.sh
  ```

This will spin up a number of containers

| Container       | Description          | local port |
|:-------------   |:------------------|:------|
| perfana         | Perfana front end  | 4000  |
| perfana-grafana | Perfana - Grafana integration service   | n/a  |
| perfana-snapshot| Perfana snapshot service | n/a   |
| perfana-check   | Perfana results check service | n/a  |
| perfana-fixture | Loads fixture data | n/a  |
| perfana-jenkins | Jenkins with two performance test jobs pre-configured | 8090  |
| mongoDb         | MongoDb replicaset to store Perfana configuration | 27011 / 27012 /27013 |
| grafana         | Grafana graphing dashboard | 3000  |
| influxdb        | InfluxDb metrics datastore | 8086 / 2003  |
| telegraf        | Telegraf metrics agent | n/a  |
| prometheus      | Prometheus metrics datastore| 9090  |
| alertmanager    | Aletmanager handles alerts from Prometheus | 9093  |
| afterburner     | Springboot test application  | 8080  |


To stop all containers, run

```
./stop.sh
```

To remove all containers, use

```
./clean.sh
```
> When you use the `clean.sh` script, all of the data inside the containers will be lost!

To start, stop or remove containers individually use `docker-compose` and the service name as used in the `docker-compose.yml` file, e.g. when a newer version of the perfana image is available

```
docker-compose stop perfana && docker-compose rm -f perfana && docker-compose up -d perfana
```


## Exploring the demo environment

### Perfana
{: .no_toc }

To log into Perfana, open [http://localhost:4000](http://localhost:4000) and use `admin@perfana.io` as user with password `admin`. If you want to log in as a non-admin user, you can try users `daniel@perfana.io` or `dylan@perfana.io`, both with password `perfana`

### Jenkins
{: .no_toc }

To log into Jenkins, open [http://localhost:8090](http://localhost:8090) and use `perfana` as user with password `perfana` 

### Grafana
{: .no_toc }

To log into Grafana, open [http://localhost:3000](http://localhost:3000) and use `perfana` as user with password `perfana` 

## Start a test

The Perfana demo environment comes with a Jenkins instance preconfigured with two jobs that will trigger a Gatling script to execute a load test on [Afterburner](https://github.com/stokpop/afterburner), a springboot test application. The two job are configured to checkout the [perfana-gatling-afterburner](https://github.com/perfana/perfana-gatling-afterburner) repository and trigger the test via the [perfana-gatling-maven-plugin](https://github.com/perfana/perfana-gatling-maven-plugin). When the job is started, this plugin will start sending meta data for the test, configured in the `pom.xml`, to Perfana.

To start a test, go to [http://localhost:8090](http://localhost:8090), log in, click on the `perfana-gatling-afterburner` job and click `Build Now`. When you open the console log, you can see the [perfana-gatling-maven-plugin](https://github.com/perfana/perfana-gatling-maven-plugin) is building and executing the test.

## View test runs in Perfana

To have a look at the test results in Perfana open [http://localhost:4000](http://localhost:4000)  and use `admin@perfana.io` as user with password `admin` to log in.

In the home page click on `Afterburner` under `Your systems under test`. In the `Running tests` section you will see the test you have just triggered.

![Running test](images/running-test.png)



The Key metrics and Grafana dashboards can be viewed and configured via the items in the `settings` section in the sidebar.

When the test has finished the test run will be displayed in the `recent test runs` section and a numbers of event will be triggered:
* Perfana will create [snapshots](https://grafana.com/docs/grafana/latest/reference/share_dashboard/#dashboard-snapshot) for all of the Grafana dashboards configured for the test run.
* Perfana will evaluate all requirements and comparison thresholds set for key metrics for the test run. When finished evaluating the consolidated results will be displayed in the `Results` column.

View test run details

To view more details for the test run you can click on the test run row, The test run details view has a number of tabs:

[Summary](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#summary)

[Comments](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#comments)

[Key metrics](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#key-metrics)

[Dashboards](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#dashboards)

[Report](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#report)

[Compare](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#compare)

[Trends](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#trends)

[Manage](https://perfana.github.io/perfana-docs/docs/analysing/analysing.html#manage)

