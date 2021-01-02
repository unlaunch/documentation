---
title: Attributes
description: This page will help you understand what attributes are, their types and use cases.
---

## Overview 
The attributes are used to [**target**](targetingrules) users. They allow you to control which variations are shown to users based on user properties.

## Example Walkthrough
To understand how attributes are used, let's walk through a complete example. Let's say you have launched a feature flag that has two variations: _on_ and _off_. You want to show the _on_ variation to all "registered" users of your application. To achieve this, you can define a new (boolean) attribute called "registered". Then in feature flag targeting rules, you'd define a new rule to show the _on_ variation when a user has "registered = true" property.

#### 1. Create New Attribute
<div class="justify-content-center">
    <img src="/assets/img/attributes/create.png" alt="create a new attribute"/>
</div>

#### 2. Target Users by Attribute
<div class="justify-content-center">
    <img src="/assets/img/attributes/target.png" alt="target users by registered attribute"/>
</div>

#### 3. Pass Attributes in the SDK
You can now start passing attributes in your application through Unlaunch SDKs and they'll be assigned appropriate variation based on the value of the attribute. For example, in JavaScript, you'd do something like this:

##### 3a. JavaScript Example
```javascript
// Set registered attribute to true to get the "on" variation.
let attributes = {
    "registered": true
}

const ulclient = ULClient.initialize(
    'prod-public-c1967ee6-f9f3-43c5-ae40-7f8ab6d725cd',
    ['mfa-feature'], // feature name
    'anonymous',     // user id
    attributes,      // Pass the attributes
    options
);

ulclient.on('ready', function () {
    let variation = ulclient.variation(flag);

    if (variation === 'on') {
        // Show feature
    } else {
        // Don't show featyre
    }
})
```

##### 3b. Java Example

```java
public static void main(String[] args) {
    // Create UnlaunchClient
    UnlaunchClient client = UnlaunchClient.create(
        "prod-public-c1967ee6-f9f3-43c5-ae40-7f8ab6d725cd");

    // Wait for the client to be ready
    try {
        client.awaitUntilReady(2, TimeUnit.SECONDS);
    } catch (InterruptedException | TimeoutException e) {
        LOG.error("[DEMO] client wasn't ready " + e.getMessage());
    }

    // Get variation for random identity and attributes
    String variation = client.getVariation(
            "mfa-feature",
            UUID.randomUUID().toString(),
            // Pass the registered attribute as true which returns "true".
            // Set this to false to get the "off" variation
            UnlaunchAttribute.newBoolean("registered", true));

    // Print variation
    LOG.info("getVariation() returned {}", variation);

    // shutdown the client to flush any events or metrics
    client.shutdown();
}
```

This will print:

```
12:03:39.796 INFO  io.unlaunch.sdk.HelloAttributes - getVariation() returned on
```

## Attribute Types

Unlaunch supports the following attribute types:

- Boolean
- String
- Number
- Set
- Date
- DateTime

## Summary

In this article, we explored attributes and looked at an example of how to use them in Unlaunch. Attributes are used to target users in [**target rules**](targetingrules) to control variations that are assigned to users.