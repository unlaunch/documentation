---
title: Live Tail
description: Use Live Tail to see impression events coming in from SDKs in real-time. This page describes how to use Live Tail for real-time insights into feature flag evaluations and to debug errors.
---

# Live Tail

Live Tail is a developer-friendly feature that we have added to make it easy for you to get real-time insights into feature flag evaluations that are happening on SDKs to identify and debug any issues. 

When you are testing a new feature flag, you might run into issues such as *"our QA Engineer was supposed to get OFF variation, but getting ON variation instead. Why?"* or *"is my application even processing the login feature flag*? Use Live Tail to investigate these further. Whenever your application using one of SDKs process the feature flag you are troubleshooting, it will show up here. 

While Live Tail is most useful in pre-production environments, you can also run it in your production environments. Please know that doing so might generate a lot of data on the page as it will capture all events.

## What is Live Tail?

As the word 'live' in its name hints, Live Tail is a feature that allows you to see all [Impression](../sdks/metrics-impressions) events in real-time that (Unlaunch SDKs) your applications are sending. 

## How to Use Live Tail

Live Tail is very easy to use. You don't need to initialize your SDKs with any special configuration options. All SDKs that are not running in [**Offline Mode**](https://docs.unlaunch.io/docs/sdks/java-sdk#offline-mode) submit events periodically to Unlaunch servers. 

To use Live Tail: 

1. Navigate to the feature flag that you want to troubleshoot by clicking on its name on the feature flags list page in the [Unlaunch Console](https://app.unlaunch.io). 
2. On feature flag details page, click on the "Live Tail" tab. 
3. Click on the **Start Capture** button.

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img img-fluid rounded" src="/assets/img/live_tail.gif" alt="Unlaunch Live Tail - Showing SDK events in real-time" width="600"/>
        <figcaption class="figure-caption text-center">Live Tail tab showing real-time events for a feature flag</figcaption>
    </figure>
</div>

That's all, you should start seeing events within seconds. Please note that it might take up to 60 seconds to see any events. On non-production environments, the SDKs default to 15 seconds for submitting these events. You can usually control this setting by configuring the `metricsFlush()` interval when initializing the SDK client. 

When you are done, don't forget click on **Stop Capture** button. The captures will automatically timeout in 5 minutes or more.

## Performance Concerns

Using Live Tail doesn't force your SDKs to send extra traffic. It uses [Impression](../sdks/metrics-impressions) events to populate the data on this page.

## Data Privacy

[Impression](../sdks/metrics-impressions) events are kept in our AWS RDS database and logs for up to 30 days before being permanently deleted. If you are interested in custom data retention policies, please contact us at unlaunch@gmail.com. 

For privacy reasons, our client-side SDKs do not send **Machine Name** with the events. This option is only enabled on server-side SDKs that run on your cloud. You can disable this when initializing the SDK. Please refer to your SDK's official guide for more information on how to do this.

