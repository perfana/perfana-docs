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

  > If you use the download option, make sure to extract the zip to a directory named `perfana-demo`!

* Add this to your hosts file
  ```
  127.0.1.1  perfana
  127.0.1.1  jenkins  
  127.0.1.1  grafana
  ```

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
| alertmanager    | Alertmanager handles alerts from Prometheus | 9093  |
| optimus-prime-fe     | Springboot test application  | 8080  |
| optimus-prime-be     | Springboot test application  | 8080  | 


To stop all containers, run

```
./stop.sh
```

To remove all containers, use

```
./clean.sh
``` 

--- 

> When you use the `clean.sh` script, all of the data inside the containers will be lost!

---


To start, stop or remove containers individually use `docker-compose` and the service name as used in the `docker-compose.yml` file, e.g. when a newer version of the perfana image is available

```
docker-compose stop perfana && docker-compose rm -f perfana && docker-compose up -d perfana
```

> The `perfana-demo` repository is updated frequently, so to get the latest and greatest pull repo and images.


```
git pull && docker-compose pull
```

## Exploring the demo environment

### Perfana
{: .no_toc }

To log into Perfana, open [http://localhost:4000](http://localhost:4000) and use `admin@perfana.io` as user with password `perfana`. If you want to log in as a non-admin user, you can try users `daniel@perfana.io` or `dylan@perfana.io`, both with password `perfana`

### Jenkins
{: .no_toc }

To log into Jenkins, open [http://localhost:8090](http://localhost:8090) and use `perfana` as user with password `perfana` 

### Grafana
{: .no_toc }

To log into Grafana, open [http://localhost:3000](http://localhost:3000) and use `perfana` as user with password `perfana` 

## Start a test

The Perfana demo environment comes with a Jenkins instance preconfigured with a job that will trigger a Gatling script to execute a load test on two instances of [Afterburner](https://github.com/stokpop/afterburner), a springboot test application, deployed as `optimus-prime-fe` and `optimus-prime-be`. The job is configured to checkout the [perfana-gatling-afterburner](https://github.com/perfana/perfana-gatling-afterburner) repository and trigger the test via the [events-gatling-maven-plugin](https://github.com/stokpop/events-gatling-maven-plugin). When the job is started, this plugin will start sending meta data for the test, configured in the `pom.xml`, to Perfana.

To start a test, go to [http://localhost:8090](http://localhost:8090), log in, click on the `OptimusPrime` job and click `Build with parameters`. The following parameters can be set:

* **system_under_test**: Use this paramater to se the System Under Test property in Perfana. Default is set to `OptimusPrime`
* **gatlingRepo**: Repository containing the Gatling script
* **gatlingBranch**: Branch to use
* **workload**: workload to use in the test, corresponds to the [workload profiles in the pom.xml](https://github.com/perfana/perfana-gatling-afterburner/blob/master/pom.xml#L252)
* **annotation**: can be used to add some annotation to the test run.

The demo contains three workload examples to demonstrate some use cases for Perfana:

* **load test**: a steady state load on OptimusPrime. Run this workload a few times to view the automatic analysis feature in action, that can be used as quality gate in a CI/CD setup. 
* **stress test**: an increasing load on OptimusPrime, until the test is automatically terminated when the system under test reaches a specified condition. Demonstrates the use of [abort alert tags](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#abort-alert-tags)
* **slow back end test**: a steady state load on OptimusPrime. A [wiremock](http://wiremock.org/) stub is configured to emulate a back end service for OptimusPrime. The [test-events-wiremock plugin](https://github.com/stokpop/test-events-wiremock) is then used to increase the response times of the stub during the test. This test can be used to test resilience of a system under test.

 When the job has started, open the console log to see the [events-gatling-maven-plugin](https://github.com/stokpop/events-gatling-maven-plugin) is building and executing the test. 


--- 

> If the job is running for the first time it will take some time to start up because Maven has to download all dependencies. Check the build log via `Console Output` to see the progress.

--- 

## View test runs in Perfana

To have a look at the test results in Perfana open [http://localhost:4000](http://localhost:4000)  and use `admin@perfana.io` as user with password `admin` to log in.

In the home page click on `Afterburner` under `Your systems under test`. In the `Running tests` section you will see the test you have just triggered.

![Running test](images/running-test.png)

Click [here](https://docs.perfana.io/docs/navigating/navigating.html#running-tests) to learn more about the `Running tests` section.
 

When the test has finished the test run will be displayed in the `Recent runs` section and a numbers of event will be triggered:
* Perfana will create [snapshots](https://grafana.com/docs/grafana/latest/reference/share_dashboard/#dashboard-snapshot) for all of the Grafana dashboards configured for the test run.
* Perfana will evaluate all requirements and comparison thresholds set for key metrics for the test run. When finished evaluating the consolidated results will be displayed in the `Results` column.

Click [here](https://docs.perfana.io/docs/navigating/navigating.html#recent-runs) to learn more about the `Recent runs` section.

If more than one test run is available for a specific set of test run properties, there will be a `Trends` tab visible. In this tab you can view trends over time for the specified `key metrics`. [Read more on trends](https://docs.perfana.io/docs/navigating/navigating.html#trends)

### View test run details
{: .no_toc }


To view more details for the test run you can click on the test run row, The test run details view has a number of tabs:

[Summary](https://docs.perfana.io/docs/analysing/analysing.html#summary)

[Comments](https://docs.perfana.io/docs/analysing/analysing.html#comments)

[Key metrics](https://docs.perfana.io/docs/analysing/analysing.html#key-metrics)

[Dashboards](https://docs.perfana.io/docs/analysing/analysing.html#dashboards)

[Report](https://docs.perfana.io/docs/analysing/analysing.html#report)

[Compare](https://docs.perfana.io/docs/analysing/analysing.html#compare)

[Manage](https://docs.perfana.io/docs/analysing/analysing.html#manage)

## Alerts

In the demo environment two Alertmanager alerts have been specified, as you can see in the [Prometheus UI](http://localhost:9090/alerts). 

These alert rules can be found in the [prometheus.rules.yml](https://github.com/perfana/perfana-demo/blob/master/prometheus-config/prometheus.rules.yml) file in the [perfana-demo repo](https://github.com/perfana/perfana-demo)

In [alertmanager.yml](https://github.com/perfana/perfana-demo/blob/master/prometheus-config/alertmanager.yml) Perfana has been set up as [receiver](https://prometheus.io/docs/alerting/configuration/#receiver)

The [Afterburner test application](https://github.com/stokpop/afterburner) has been [set up to expose JVM metrics via actuator](https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/html/production-ready-metrics.html#production-ready-metrics-export-prometheus) and two [metric tags have been added](https://github.com/perfana/perfana-demo/blob/master/docker-compose.yml#L113) `system_under_test` and `test_environment`. 

If one of the alerts triggers, Perfana will try to map alert metric tags to properties of any running test. If it finds a match, it will create an [annotation](https://grafana.com/docs/grafana/latest/reference/annotations/) for all linked Grafana dashboards in each graph. This can help you track down root causes for bottleneck in your test runs.

Learn more on alerting [here](https://docs.perfana.io/docs/alerts/alerts.html)