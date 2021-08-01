---
title: Frequently Asked Questions
description: This page answers frequently asked questions about Unlaunch
---

# FAQs

## How to call feature flag from my application?

To evaluate and use feature flags in your application, you'll need to integrate and use [Unlaunch SDK](../sdks) for your programming language. To initialize the SDK, you'll need to pass it the [SDK keys](../sdks/sdk-keys) for the current environment. 

## Is it possible to create a new feature flag only in one environment? 

When you create a new feature flag, it's available for all environments of the project. So it is not possible to create a feature flag, for example, in Test but not in Production. Because flags are created in disabled state and any targeting rules you apply are specific to the current environment only, you can **safely** add feature flags, test on non-production environments.

For example, say you are adding a new feature flag. Because you are still actively developing, you want to work in Test environment and don’t want to bring your work to the Production environment. You can safely create flag in the Test environment. While the flag will be visible in Production (because we need to reserve flag key across all environments), it will be in disabled state. Any changes you make to flag in Test environment will not be reflected in the Production environment.

## Is it possible to unarchive a feature flag?

No. At this point, a feature flag that that has been archived cannot be unarchived.

## How do I get help?

Just send us an email at help@unlaunch.io and we'll be happy to help you out.