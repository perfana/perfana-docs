---
title: Settings
layout: default
has_children: false
nav_order: 7
---

# Settings
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Perfana was build using the [Meteor framework](https://www.meteor.com/). Perfana are passed to the application in JSON format via the [METEOR_SETTINGS environment variable](https://docs.meteor.com/environment-variables.html#METEOR-SETTINGS)

The following configuration can be set:

### General settings

#### Organisations

It is possible to add multiple organisations to Perfana to organise your teams by. 

```json
 "organisations": [
    {
      "name": "Org1",
      "description": "Organisation 1"
    },
    {
      "name": "Org2",
      "description": "Organisation 2"
    }
  ],
``` 
If omitted, a default organisation named `Perfana` will be added.   
#### adminEmail
If, at startup of Perfana, no users are found with the `admin` role, a user with the `admin` role is created with this email address. Default value: "admin@perfana.io"

```json
  "adminEmail": "admin@mycomapny.com"
```    
#### adminPassword
If, at startup of Perfana, no users are found with the `admin` role, a user with the `admin` role is created with this password.  Default value: "perfana"

```json
  "adminPassword": "secret"
```    
#### perfanaUrl
Client facing url of Perfana, default value `http://localhost:4000`

```json
  "perfanaUrl": "https://perfana.myhost.com"
```    
#### perfanaCheckUrl
Url used by Perfana to connect to `perfana-check` service, default value `http://localhost:9191`

```json
  "perfanaCheckUrl": "http://perfana-check:9191"
```    
#### influxDbHost
Hostname used by Perfana to connect to InfluxDB, default value `localhost`

```json
  "influxDbHost": "influxDB"
```    

#### allowedFrameSrc
Perfana uses iframes to display the Grafana and Jaeger UI. To allow iframing these locations the client facing url patterns of the Grafana and Jaeger UI should be added here, e.g. if the client facing url would be `https://grafana.myhost.com` and `https://jaeger.myhost.com`:

```json
  "allowedFrameSrc": [
    "https://*.myhost.com"
  ],
```


### Authentication settings

#### authenticationServices
If an external service is used to authenticate Perfana users, add settings here.

```json
 "authenticationServices": {
    "keycloak": {
      "realm": "perfana",
      "auth-server-url": "https://keycloak:8443/auth",
      "loginStyle": "popup",
      "ssl-required": "external",
      "resource": "perfana-fe",
      "public-client": true,
      "use-resource-role-mappings": true,
      "bearer-only": true,
      "realm-public-key": "<keycloak public key>"
    }
  },
  ```
#### loginExpirationInDays
Defines after how many days the user has to sign in again, default value is 1 day. It is possible to use decimal values, e.g. 

```json
  "loginExpirationInDays": 0.5
```    
### Retention settings

Perfana decides based on these datasource retention settings to show "live" Grafana dashboards, connecting to the datasources to fetch data or, if the data is no longer available, show [snapshots](https://grafana.com/docs/grafana/latest/reference/share_dashboard/#dashboard-snapshot) of the dashboards instead. If no retention has been configured for a datasource, Perfana will assume retention is 0 and will always show snapshots.

#### prometheusRetention
Metrics retention in seconds. Default value is `2592000` (30 days). 

```json
  "prometheusRetention": "43200"  // 12 hours
```

#### graphiteRetention
Metrics retention in seconds. Default value is `2592000` (30 days). 

```json
  "graphiteRetention": "86400"  // 24 hours
```

#### influxDbRetention
Metrics retention in seconds. Default value is `2592000` (30 days). 

```json
  "influxDbRetention": "604800"  // 1 week
```

#### elasticSearchRetention
Metrics retention in seconds. Default value is `2592000` (30 days). 

```json
  "elasticSearchRetention": "2592000"  // 30 days
```

#### snapshotExpires
This setting is used to specify how long snapshots will be stored in Grafana in seconds. Default value is `7776000` (90 days). 

```json
  "snapshotExpires": "7776000"  // 90 days
```

If snapshotExpires is set to 0, snapshots will not expire.

--- 

> If no expiry is set for snapshots, the Grafana database will continue to grow!

---

### Grafana settings

Perfana requires at least one Grafana instance to be configured, refer to [here](/docs/administration/administration.html#grafana-configuration) for more details.

#### grafanaInstances

```json
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

### Jira settings
To integrate with Jira, add oauth settings for one or more Jira instances. Refer to [here](/docs/administration/administration.html#jira-configuration-enterprise-feature) for more details

#### jiraInstances
```json
    "jiraInstances": [ 	
        { 		
            "host": "jira.mycompany.com", 		
            "oauth": { 			
                "consumer_key": "perfana", 			
                "private_key": "-----BEGIN PRIVATE <key> -----END PRIVATE KEY-----\n", 			
                "token": "<token>", 			
                "token_secret": "<secret>" 		
            } 	
        }
    ],  
```

### Dynatrace settings

To integrate with Dynatrace, add these properties, refer to [here](/docs/integrations/integrations.html#create-dynatrace-api-token) for more details

#### dynatraceUrl

```json
    "dynatraceUrl": "<Dynatrace host>"
```
#### dynatraceApiToken

```json
    "dynatraceApiToken": "<Dynatrace token>",
```
### Public settings

Settings that need to be exposed to the client are in the `public` section

#### forbidClientAccountCreation
Set this value to true if you want to disable the option to register new users from the login screen. Defaults to `false`

#### overrideLoginButtonText
If you are using Keycloak as authentication service, this property can be used to set the login button text.
#### googleSignIn
If you are using Google as authentication service, setting this property to `true` will show a Google branded login button

#### perfanaUrl
Client facing url of Perfana, default value `http://localhost:4000`

#### jaegerUrl
Client facing url of Jaeger, default value `http://localhost:16686`

#### jaegerLimit
Maximum number of traces to show in Jaeger UI, default value `500`

```json
"public": {
    "forbidClientAccountCreation": true,
    "perfanaUrl": "http://localhost:4000",
    "jaegerUrl": "https://jaeger.myhost.com",
    "jaegerLimit": 500,
    "overrideLoginButtonText": "My company Login",
    "googleSignIn": "true"
  },
 ```

### Alert settings

#### customAlertTags

If alerts do not have `system_under_test` and `test_environment` labels to allow Perfana to map alerts to test runs, you can specify custom tags and (dynamic) values to be used for mapping.

```json
 "customAlertTags": [
    {
        "key": "application",
        "value": "perfana-applications"
    },
    {
        "key": "namespace",
        "value": "perfana-test-environment"
    }
 ],
```    
#### omitAlertTags
By default, Perfana passes on all tags set on incoming alerts. If you want to omit specific tags, add them here.


```json
"omitAlertTags": [
    {
        "alertSource": "alertmanager",
        "tag": "kubernetes_pod_name"
    },
    {
        "alertSource": "alertmanager",
        "tag": "node"
    }
 ],
```