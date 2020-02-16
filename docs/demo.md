---
layout: default
title: Demo
nav_order: 2
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
  git clone https://gitlab.com/perfana/perfana-demo.git
  ```
  or download [here](https://gitlab.com/perfana/perfana-demo/-/archive/master/perfana-demo-master.zip)
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

To start stop or remove containers individually use `docker-compose` and the service name as used in the `docker-compose.yml` file, e.g. when a newer version of the perfana image is available

```shell script
docker-compose stop perfana && docker-compose rm -f perfana && docker-compose up -d perfana
```


## Exploring the demo environment

### Perfana
{: .no_toc }

To log into Perfana, open http://localhost:4000 and use `admin@perfana.io` as user with password `admin` 

### Jenkins

To log into Jenkins, open http://localhost:8090 and use `perfana` as user with password `perfana` 

### Grafana

To log into Grafana, open http://localhost:3000 and use `perfana` as user with password `perfana` 
