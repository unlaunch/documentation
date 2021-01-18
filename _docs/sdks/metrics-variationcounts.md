---
title: Metrics - Variation Counts
description: Overview of variation count metrics that are sent by Unlaunch SDKs.
---

# Metrics - Variation Counts

Unlaunch SDKs regularly send metrics to Unlaunch servers. These are useful for statistics and debugging purposes. 

Variation count is a type of an aggregate metric. This helps you understand how variations are being assigned and what their distribution is. For example, **Insights Graph** relies on this data to show variation assignments over time.

During the course of its operation, an SDK will evaluate flags for many different users. Instead of sending each event separately, the SDKs perform aggregation by combining events for the same variation and sending the sum. For example, suppose, we evaluate the following variations:

- flag_key: login-ui; variation_key: on
- flag_key: login-ui; variation_key: on
- flag_key: login-ui; variation_key: off
- flag_key: login-ui; variation_key: on
- flag_key: login-ui; variation_key: on
- flag_key: login-ui; variation_key: off

These events will be aggregated as follows before being sent to the server:
- flag_key: login-ui; variation_key: on (count=4)
- flag_key: login-ui; variation_key: off (count=2)


### Adjusting Flush Interval and Queue Size

Server side SDKs send variation count events periodically to Unlaunch servers. You can control how often these are sent to the server by setting the `metricsFlushInterval()` or `metricsQueueSize()` methods when building the client.