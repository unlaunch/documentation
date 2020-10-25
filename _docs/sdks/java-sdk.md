---
title: Java SDK
description: This guide provides complete information about the Unlaunch Java SDK.
---

# Unlaunch Java SDK

This guide provides complete information about the Unlaunch Java SDK and how to integrate it in your applications to call feature flags.

The Unlaunch Java SDK provides a Java API to access Unlaunch. Using the SDK, you can easily build Java applications that can evaluate feature flags, access configuration, and more. Unlaunch Java SDK is *open source* and its full source code is available on <a href="https://github.com/unlaunch/java-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a>

### Language Support

The Unlaunch Java SDK support Java version 8 and above.

## Prerequisite

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and Enabled it. You also know the [Server SDK key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [cient-side and server-side SDKs](client-vs-server-side-sdks). This SDK is server-side and optimized for applications that run on the cloud such as web servers, backend services, etc.

## Import Maven/Gradle Dependency

The first step is to import the Unlaunch SDK as Maven or Gradle dependency in your application. 

```xml
<dependency>
    <groupId>io.unlaunch.sdk</groupId>
    <artifactId>unlaunch-java-sdk</artifactId>
    <version>1.0.0</version>
</dependency>
```

## Initializing a New Unlaunch Client Instance

In a nutshell, here is how this SDK works:

1. You initialize the client using a [Server SDK key](sdk-keys). The SDK key identifies the environment within the project you have defined feature flags.
2. When you build the clients, it starts a background task to download all active feature flags and configuration data and store them in an in-memory cache. This process can take some time depending on the size of the data. We'll discuss this process in detail later.
3. You can wait for the initialization to complete using the `awaitUntilReady` method. You must pass a timeout value as an argument so it doesn't block your application forever.
3. After the initialization is complete, you can evaluate feature flags using the public `getVariation` method.

You initialize the client, you'll need your Server SDK key. SDK Keys are available on the **[Settings](https://app.unlaunch.io/settings)** page under **Projects** tab. Once you have it ready, you can initialize a new client as following:

```java
UnlaunchClient ulClient = UnlaunchClient.create("INSERT_YOUR_SDK_KEY");

ulClient.awaitUntilReady(2, TimeUnit.SECONDS); // Wait until all data is downloaded
```

{% include alert-note.html type="danger" content="For performance reasons, we strongly recommend initialization the Unlaunch client as a SINGLETON and reusing it throughout your application. The client is thread-safe." %}

##### Why Unlaunch Client Should be a Singleton?

When you build an Unlaunch client, it starts a background task to download data and store it in an in-memory cache. This process might take some time depending on the size of the data that needs to be transferred. It is extremely BAD practice is you initialize a new client for each client request such is a web request handler method or in Spring's `@Controller`. It will add latency to the request and all downloaded data will be discarded once the request is complete.

It is important that you create Unlaunch client as a singleton and re-use it throughout your application. If you create more than one instance, we'll print warnings in the logs.

## Using the SDK

After the initialization is complete, you are ready to start evaluating feature flags using the `getVariation` method. This method will return you a variation that you have defined in the Unlaunch Console. You can check using simple if-else block to execute code depending on the variation that's returned.

The `getVariation` method requires that you pass in user id of the user that you are evaluating for the feature flag. If the flag is an operational flag such as a global kill switch and doesn't require users, you can define a String constant and pass it instead.

```java
String userId = "123";
String variation = ulClient.getVariation("log-levels", userId);

if (variation.equals("on")) {
    // code to show the feature
} else if (variation.equals("off")) {
    // code to hide the feature
} else {
    // default code when feature isn't found or evaluated
}
```

{% include alert-note.html type="info" content="The getVariation method will never throw an exception." %}

### Shutdown 

When your application is shutting down, you can shutdown Unlaunch client using the `shutdown` method. Calling shutdown ensures that any pending metrics are sent you Unlaunch servers.

```java
client.shutdown();
```

## Advanced Usage

### Concepts

in-memory store, blocking, etc.

## Offline Mode

Feature flags start their journey on a developer's computer. A developer should be able to build and run their code locally even if they don't have network connectivity. To achieve this, Unlaunch Java SDK can be started in **offline mode**. When running in offline mode, the SDK will not connect to Unlaunch servers nor it will send any data to it. 

When activating offline mode, you can pass in any value for the SDK key. We recommend `offline`. When you evaluate a feature flag in offline mode, it will return the `control` variation. 

```java
UnlaunchClient ulClient = UnlaunchClient.builder()
                .sdkKey("offline")
                .offlineMode()
                .build();
```

If you want to control what variations are returned, you can specify feature flags and variations in a YAML file and load it using `offlineModeWithLocalFeatures` method. Unlaunch will return variations that you have specified in the YAML file. Any flag not found from the YAML file will return `control` variation.

### Configuration Options

### Unlaunch Users

### Event Tracking

### Offline Mode

