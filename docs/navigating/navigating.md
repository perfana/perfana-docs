---
title: Navigating
has_children: true
nav_order: 2
---

# Navigating test runs
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


> This page is work in progress!

{: .fs-6 }

## Landing page

After logging in the user is redirected to his or her personal landig page. It has deeplinks to test runs view with preset filters on either the team(s) the user is member of or the systems under test that those teams are responsible for.

It also has a section called `Test runs that may require your attention` that will show test runs with failed checks not visited yet by the user.

In the top right of the screens there are notifications for the user that are created on several events, like test runs with failed checks and new comments on test runs.

## Test runs 

### Test run selector
{: .no_toc }

The test run selector can be used to filter test runs based on test run properties `System under test`, `Test environment` and `Workload`. When the selector is switched to `team mode` a `Team` filter option is added.

### Running tests
{: .no_toc }

The `Running tests` section will show any currenty running tests matching the properties set in the `Test run selector`. 

Clicking on the test run will open the running test view. The view has two tabs:
* Key metrics: shows the configured key metrics for this test. See [Key metrics configuration](https://perfana.github.io/perfana-docs/docs/testconfiguration/testconfiguration.html#key-metrics)
* Dashboard: show the configured dashboards for this test. See [Grafana dashboards configuration](https://perfana.github.io/perfana-docs/docs/testconfiguration/testconfiguration.html#grafana-dashboards)
  
  
### Recent tests
{: .no_toc }
