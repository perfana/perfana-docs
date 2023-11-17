---
title: Quick start
layout: default
has_children: false
nav_order: 1
---

# Quick start
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .fs-6 }

## High over Perfana concepts

Quick introduction of Perfana concepts for first time users.

Let us go over the concepts, how they relate and how they are connected 
on a - high over - technical level.

First, we have the __system under test__, which is a deployable piece of software that can be tested.

The __load script__ sends requests to the system under test, as if it is used by many users 
at the same time.

__Metrics__ from both the load script and the system under test are sent to Perfana Cloud. 
The metrics we use to analyse the result of the test.

Metrics are queried and displayed in __dashboards__. Each dashboard contains one or more panels 
for distinguished metrics.

We want to run __many tests__ to see how the behaviour of the SUT changes over time: is it 
getting faster or is it getting slower and is action needed to make it faster.
One test is called a __test run__. Each run has a unique __test run id__.

__Checks__ are used to verify the performance requirements. A __Service Level Indicator (SLI)__ 
defines what to measure, while the __Service Level Objective (SLO)__ sets the objective for 
the specific metric. For each test run Perfana automatically checks is the objective is achieved.

## Tying it all together

By now you probably ask yourself how Perfana ties this all together. How are load scripts, 
SUT’s, dashboards and checks automatically linked to each other? Keywords to this answer are __tags__, 
__("Golden Path") profiles__ and __auto configuration__.

__Profiles__ define a set of dashboards, SLI/SLO’s and more. Perfana contains several Golden Path profiles
that come out of the box for common stacks and apply best practices.

A test run has tags. For instance: `gatling`, `spring-boot`, `kubernetes`. These tags are used by the
__auto configuration__ feature. It matches the tags with the available profiles to automatically create
dashboards and SLI/SLO’s that are defined in these matched profiles.

When dashboards and SLI/SLO’s are created automatically based on these tags, how does it know 
which metrics are relevant to the SUT? Each test run also comes with a __test environment__ and __SUT__.
Now the relevant metrics from this test environment can be shown and used in the dashboard templates in the profiles.

Are all SLI/SLO validations applicable to each type of test? No, certainly not. A stress test usually
hits limits like high cpu usage or failing transactions. The __workload__ defines which set of SLI’s are used for automatically 
validating the test result.

Last but not least, each test run has a __start time__ and an __end time__. Perfana dashboards show the
metrics for the duration of the test run. SLI/SLO validations optionally leave out ramp up time, 
which is what you expect and want most of the time, unless you have a stress test that is essentially
one big ramp up.

The tags of the test run are specified via the test configuration. For most setups you can specify the
tags in the Maven configuration that "wraps" the load script. 

## Snapshots and retention

A lot of metrics are collected all of the time. To limit the amount of storage needed to keep all
this data, most of the data is compressed over time and/or deleted. Sometimes even after just hours
or days after a test run.

Perfana uses __snapshots__ to keep hold of the uncompressed data of a test run. After each test run,
snapshots are created and stored to be kept for several months. You can choose to store snapshots
for designated test runs for ever, for instance for a baseline runs you want to keep.

During the test run dashboards show the live data as it is being generated. After a test run the
snapshot data is shown in a dashboard.

As soon as snapshots are created after the test run, the automatic analysis is done and the checks
appear. There are __check results__ and __compare results__. Check results are for the current test, 
for instance, the 90 percentile response time must be below 1 second. Compare results show and 
check the difference between the current test run and the __previous__ test run, or a test run that
is marked as __baseline__. An example of a compare requirement is: the 90 percentile response time
must not be higher that 10 percent compared to the previous or baseline test run.
