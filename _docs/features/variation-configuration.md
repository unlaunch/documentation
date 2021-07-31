---
title: Dynamic Configuration
description: This page will explain variation configuration 
---

# Dynamic Configuration

Using Unlaunch, you can attach configuration (Key-Value pairs) to each variation of your feature flag and send it back to SDKs. This is sometimes also known as the **Dynamic Configuration**. This is useful when you want to things dynamically for each variation without changing the text. For example, you could define a key to represent text that's shown for each variation instead of defining it in your code. Then, whenever you want to change the text or its copy, you could just update it in the Unlaunch Console and your users will start seeing it automatically. 

<div class="justify-content-center border">
    <img src="/assets/img/dynamic-config.png" alt="Dynamic Configuration"/>
</div> 

To add dynamic configuration to your feature flags:

1. Click on the feature flag from Dashboard to open flag details page.
2. Click on **Configuration** tab.
3. Click on **Add Configuration** button present for each variation.
4. For each variation, add *key*s and *value*s.
5. Save your changes.

To fetch Dynamic Configuration in your app, please refer to SDK guides. For example, to fetch dynamic configuration in the [Java SDK](../sdks/java-sdk#get-variation-dynamic-configuration-and-evaluation-reason-getfeatureflagkey-identity), you could do the following:

```java
String ctaText = feature.getVariationConfig().getString("cta-text", "default value");
```

Dynamic Configuration is only applied to the current environment. To copy configuration from the current environment to another, click on the Copy button in the top right corder.
