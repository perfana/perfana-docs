---
title: Integrations
has_children: false
nav_order: 7
---

# Integrations
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .fs-6 }

## events-gatling-maven-plugin

[View it on GitHub](https://github.com/stokpop/events-gatling-maven-plugin){: .btn .fs-5 .mb-4 .mb-md-0 }

Gatling Maven Test Events Extension

## event-scheduler

[View it on GitHub](https://github.com/stokpop/event-scheduler){: .btn .fs-5 .mb-4 .mb-md-0 }

library to generate timed events

## test-events-wiremock

[View it on GitHub](https://github.com/stokpop/test-events-wiremock){: .btn .fs-5 .mb-4 .mb-md-0 }

Events to load and change wiremock stubs during load tests.

## perfana-java-client

[View it on GitHub](https://github.com/perfana/perfana-java-client){: .btn .fs-5 .mb-4 .mb-md-0 }

Java library to integrate with Perfana

## perfana-gatling-maven-plugin

[View it on GitHub](https://github.com/perfana/perfana-gatling-maven-plugin){: .btn .fs-5 .mb-4 .mb-md-0 }

Maven plugin to integrate Gatling with Perfana.

## perfana-jmeter-maven-plugin

[View it on GitHub](https://github.com/perfana/perfana-jmeter-maven-plugin){: .btn .fs-5 .mb-4 .mb-md-0 }

Maven plugin to integrate Jmeter with Perfana.

## Jaeger

Perfana can be integrated with Jaeger to enable deeplinking from test results into the Jaeger UI to view application traces. To enable this integration, a number things have to be set up:

### Add headers to (Gatling) script
In order to be able to filter traces in the Jaeger UI based on test run id and request names in your (Gatling) script, two headers have to be added to each request in your script, that will passed by Spring as “baggage” with the traces:

* `perfana-test-run-id`
* `perfana-request-name`

In Gatling you can the  `perfana-test-run-id` header at HttpProtocol level by adding a line to the HttpProtocolBuilder

```
 val httpProtocol: HttpProtocolBuilder = {
    val protocol = http
      .baseUrl(ApplicationConfiguration.host)
      .inferHtmlResources()
      .acceptHeader("*/*")
      .acceptEncodingHeader("gzip, deflate, sdch")
      .acceptLanguageHeader("en-US,en;q=0.8")
      .contentTypeHeader("application/json")
      .userAgentHeader("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36")
      .shareConnections
      .header("perfana-test-run-id",System.getProperty("testRunId"))
```

The `perfana-request-name` header has to be added to each request separately, e.g.

```
 val call = exec(http("remote_call_delayed")
    .get("/remote/call?path=delay")
    .header("perfana-request-name", "remote_call_delayed")
    )
```
> When using a Gatling script, if your request names contains spaces, replace those with underscores in the `perfana-request-name` header value.

### Configure baggage

In the Perfana demo setup [Spring Cloud Sleuth](https://docs.spring.io/spring-cloud-sleuth/docs/current-SNAPSHOT/reference/html/) is used to create traces and send them to the Jaeger collector in the Zipkin format. To pass the headers added in the script to Jaeger, the headers have to configured as [baggage](https://docs.spring.io/spring-cloud-sleuth/docs/current-SNAPSHOT/reference/html/#baggage):

```
"spring.sleuth.keys.http.headers": "perfana-test-run-id,perfana-request-name"
"spring.sleuth.propagation.tag.enabled": "true"
"spring.sleuth.propagation.tag.whitelisted-keys": "perfana-test-run-id,perfana-request-name"
"spring.sleuth.propagation-keys": "perfana-test-run-id,perfana-request-name"
"spring.sleuth.baggage-keys": "perfana-test-run-id,perfana-request-name"```
```

### Tag metrics 

Perfana will create a `Only show traces that fail to meet Service Level Objective` filter when it finds a key metric with a Service Level Objective specified based a Grafana `panel` that has `perfana-response-times` in the description.

![Panel description perfana-response-times](https://docs.perfana.io/docs/images/perfana-response-times.png)

## Slack

To configure your Slack channel as a Perfana notification channel, create a `Incoming Webhook` for your channel using [these instructions](https://api.slack.com/messaging/webhooks). Use the `Webhook URL` when setting up your [notifications channel](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#notifications-channels-enterprise-feature)

## Teams

To configure your Teams channel as a Perfana notification channel, create a `Incoming Webhook` for your channel using [these instructions](https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook). Use the `Webhook URL` when setting up your [notifications channel](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#notifications-channels-enterprise-feature)

## Google Chat

To configure your Google Chats Room as a Perfana notification channel, create a `Incoming Webhook` for your room using [these instructions](https://developers.google.com/hangouts/chat/how-tos/webhooks). Use the `Webhook URL` when setting up your [notifications channel](https://docs.perfana.io/docs/testconfiguration/testconfiguration.html#notifications-channels-enterprise-feature)

## Jira (Enterprise feature)

Perfana can integrate with one or more Jira instances to create issues and link them to test run results. The [Jira configuration view](https://docs.perfana.io/docs/administration/administration.html#jira-configuration) can be used to setup the integration.

## Dynatrace (Enterprise feature)

Perfana can be integrated with Dynatrace to include tracing information in your test run results. To enable this integration, take the following steps:

### Create Dynatrace API token

In Dynatrace, go to `Settings -> Integration -> Dynatrace API` and click `Generate Token`. Name the token `perfana` and add the the following scopes:

API v1:
* Access problem and event feed, metrics, and topology
* Read Configuration

API v2:
* Read entities
* Read problems
* Read settings

Copy the generated API token. Next, click the `Dynatrace API explorer` link and copy the host from the url from the adress bar.

Add API token and host to the `METEOR-SETTINGS` and restart Perfana:

```
  "dynatraceApiToken": "<Dynatrace API token>",
  "dynatraceHost": "https://somehost.live.dynatrace.com",
```


### Add headers to (Gatling) script
In order to be able to filter traces in Dynatrace based on test run id and request names in your (Gatling) script, two headers have to be added to each request in your script

* `perfana-test-run-id`
* `perfana-request-name`

In Gatling you can the  `perfana-test-run-id` header at HttpProtocol level by adding a line to the HttpProtocolBuilder

```
 val httpProtocol: HttpProtocolBuilder = {
    val protocol = http
      .baseUrl(ApplicationConfiguration.host)
      .inferHtmlResources()
      .acceptHeader("*/*")
      .acceptEncodingHeader("gzip, deflate, sdch")
      .acceptLanguageHeader("en-US,en;q=0.8")
      .contentTypeHeader("application/json")
      .userAgentHeader("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36")
      .shareConnections
      .header("perfana-test-run-id",System.getProperty("testRunId"))
```

The `perfana-request-name` header has to be added to each request separately, e.g.

```
 val call = exec(http("remote_call_delayed")
    .get("/remote/call?path=delay")
    .header("perfana-request-name", "remote_call_delayed")
    )
```
> When using a Gatling script, if your request names contains spaces, replace those with underscores in the `perfana-request-name` header value.

### Adding request attributes

In Dynatrace `request attributes` have to be set up to parse the Perfana headers from the incoming requests. Go to `Settings -> Server-side service monitoring -> Request attributes` and click `Define a new request attribute`

* Add `perfana-test-run-id` as `Request attribute name`
* Click on `Add new data source`
* In the `Request attribute source` dropdown, select `HTTP request header`
* Add `perfana-test-run-id` as `Parameter name` and click `Save`
* Click `Save` in the top right to save the `request attribute`

Repeat the same steps for `perfana-request-name`
