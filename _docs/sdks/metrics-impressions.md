---
title: Metrics - Impressions
description: Overview of impression metrics that are sent by Unlaunch SDKs.
---

# Metrics - Impressions

Unlaunch SDKs regularly send metrics to Unlaunch servers. These are useful for statistics and debugging purposes. 

Metrics are a type of an [event](event). Metric events are automatically generated when feature flags are evaluated on SDKs. Unlaunch SDKs keep a separate queue for metrics and allows fine-grained control over how frequently metrics are submitted to the server, separate from other types of events. 

When a feature flag is evaluated on the SDK, an **impression** capturing the details of the evaluation is generated and recorded by the SDK in a queue. This queue is periodically flushed and events are sent to Unlaunch servers. This information includes the flag details, evaluation results, user information, timestamp etc. These events are then stored in a database. You can view these events in real-time using [**Live Tail**](../managingflags/livetail). 

{% include alert-note.html type="info" content="We normally keep events for 15-30 days and support custom retention policies. Please email unlaunch@gmail.com for more information." %}

### Impression Event fields

Every impression event contains the following fields:


| Key                | Description                                                                  |
|--------------------|------------------------------------------------------------------------------|
| Key                | The feature flag key that was evaluated                                      |
| User Id            | User Id for which the event was evaluated                                    |
| Variation Key      | The variation that was returned                                              |
| Evaluation Reason  | Why the particular variation was evaluated e.g. Targeting Rule matched, etc. |
| Flag Status        | Whether the flag was enabled or disabled                                     |
| Machine name       | Hostname of the machine where feature flag was evaluated                     |
| SDK Name & Version | The name and version of the SDK                                              |

### Adjusting Flush Interval and Queue Size

Server side SDKs send impression events periodically to Unlaunch servers. You can control how often these are sent to the server by setting the `metricsFlushInterval()` or `metricsQueueSize()` methods when building the client.