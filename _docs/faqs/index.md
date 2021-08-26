---
title: Frequently Asked Questions
description: This page answers frequently asked questions about Unlaunch
---

# FAQs

## How to evaluate feature flag from my application?

To evaluate and use feature flags in your application, you'll need to integrate and use [Unlaunch SDK](../sdks) for your programming language. To initialize the SDK, you'll need to pass it the [SDK keys](../sdks/sdk-keys) for the current environment. 
<hr/>

## Why am I getting the "control" variation and the flag is not available on my SDK?

If you have just created a feature flag and are not seeing it in your application. Follow these steps:

- If you're using a [client-side SDK](../sdks/client-side-sdks/), such as React, JavaScript (Library) or Angular, check to make sure that flag is enabled for **client-side** access. You can do this by clicking on the "Settings" tab under the feature flag and making sure that the checkbox "*Make this flag available to client-side SDKs*" is checked. 
- If the above doesn't work or you're using a server-side SDK, make sure that the flag is not archived.

If the problem persists, email us at help@unlaunch.io
<hr/>

## Is it possible to create a new feature flag only in one environment?

When you create a new feature flag, it's available for all environments of the project. So it is not possible to create a feature flag, for example, in Test but not in Production. Because flags are created in disabled state and any targeting rules you apply are specific to the current environment only, you can **safely** add feature flags, test on non-production environments.

For example, say you are adding a new feature flag. Because you are still actively developing, you want to work in Test environment and don’t want to bring your work to the Production environment. You can safely create flag in the Test environment. While the flag will be visible in Production (because we need to reserve flag key across all environments), it will be in disabled state. Any changes you make to flag in Test environment will not be reflected in the Production environment.
<hr/>

## Is it possible to unarchive a feature flag?

No. At this point, a feature flag that that has been archived cannot be unarchived.
<hr/>

## How do I get help?

Just send us an email at help@unlaunch.io and we'll be happy to help you out.