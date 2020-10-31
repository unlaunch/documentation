---
title: Java SDK
description: This guide provides complete information about the Unlaunch Java SDK.
---

# Unlaunch Java SDK

This guide provides complete information about the Unlaunch Java SDK and how to integrate it in your applications to call feature flags.

The Unlaunch Java SDK provides a Java API to access Unlaunch. Using the SDK, you can easily build Java applications that can evaluate feature flags, access configuration, and more. Unlaunch Java SDK is *open source* and its full source code is available on <a href="https://github.com/unlaunch/java-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a>

### Language Support

The Unlaunch Java SDK support Java version 8 and above.

If you are looking for Unlaunch Java SDK **Javadocs**, please [click here](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/index.html).

## Prerequisite

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and Enabled it. You also know the [Server SDK key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [cient-side and server-side SDKs](client-vs-server-side-sdks). This SDK is server-side and optimized for applications that run on the cloud such as web servers, backend services, etc.

## Import Dependency

The first step is to import the Unlaunch SDK as Maven or Gradle dependency in your application. 

For Maven, 
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
3. You can wait for the initialization to complete using the [`awaitUntilReady`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html) method. You must pass a timeout value as an argument so it doesn't block your application forever.
3. After the initialization is complete, you can evaluate feature flags using the public [`getVariation`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html) method.

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

### Configuration

When initializing the client, you have several configuration options to fine-tune the performance and adjust to your needs. These options are:

##### `pollingInterval()`
The Unlaunch Java SDK periodically downloads flags and other data from the servers and stores it in-memory so feature flags can be evaluated with no added latency. The `pollingInterval` controls how often the SDK download flags from the servers if the data has changed. The default value is 60 seconds for production environments and 15 seconds for non-production. For example, to change the polling interval to 5 minutes: 

```java 
UnlaunchClient client = UnlaunchClient.builder()
                            .pollingInterval(5, TimeUnit.MINUTES)
                            .sdkKey("<your environment sdk key>")
                            .build();
```

##### `metricsFlushInterval()`
The SDK periodically sends events like metrics and diagnostics data to our servers. This controls how frequently this data will be sent. When you shutdown a client using the [`shutdown()`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html#shutdown()) method, all metrics are automatically sent to the server. The default value is 30 seconds for production and 15 seconds for non-production environments. For example, to change the event flush time to 10 minutes:

```java 
UnlaunchClient client = UnlaunchClient.builder()
                            .metricsFlushInterval(2, TimeUnit.MINUTES)
                            .sdkKey("<your environment sdk key>")
                            .build();
```

##### `eventsFlushInterval()`
This controls how frequently tracking events are sent to the server. The default value is 60 seconds for production and 15 seconds for non-production environments.

##### `eventsQueueSize()`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. 

##### `offlineMode()`
When enabled, this starts the SDK in offline mode where no flags are downloaded from the server, nor anything is sent. All calls to [`getVariation()`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html#getVariation(java.lang.String,java.lang.String)) will return `control`. Please see **Offline Mode** below for more information.

##### `offlineModeWithLocalFeatures()`
This is intended for testing, including unit testing. This allows you to pass a YAML file containing feature flags and the variations to return when they are evaluated. You can also control dynamic configuration and specify which values to return. lease see **Offline Mode** below for more information and a YAML template.

##### `host()`
Unlaunch server to connect to for downloading feature flags and for submitting events. Only use this if you are running Unlaunch backend service on-premise or are enterprise customer. The default value is https://api.unlaunch.io

## Advanced Usage

### Concepts

#### 1. In-memory data store
Unlaunch Java SDK (all server-side SDKs) connect to Unlaunch servers upon initialization to download all feature flags, variations and dynamic configurations, and store these in an in-memory data store. All subsequent flag evaluations are done using the in-memory data store and give results very quickly. 

#### 2. Events and Metrics Tracking
Unlaunch Java SDK periodically sends events to Unlaunch servers. These events are used for showing metrics and to generate data for the Insights Graph.

#### 3. Offline Mode

Feature flags start their journey on a developer's computer. A developer should be able to build and run their code locally even if they don't have network connectivity. To achieve this, Unlaunch Java SDK can be started in **offline mode**. When running in offline mode, the SDK will not connect to Unlaunch servers nor it will send any data to it. 

When activating offline mode, you don't need to pass in the SDK key. When you evaluate a feature flag in offline mode, it will return the `control` variation. 

```java
UnlaunchClient ulClient = UnlaunchClient.builder()
                .offlineMode()
                .build();
```

If you want to control what variations are returned, you can specify feature flags and variations in a YAML file and load it using `offlineModeWithLocalFeatures` method. Unlaunch will return variations that you have specified in the YAML file. Any flag not found from the YAML file will return `control` variation.

#### 4. Logging

The Unlaunch Java SDK uses [SLF4J](http://www.slf4j.org/) (slf4j-api). SLF4J is a logging facade which provides abstractions for various logging frameworks. This allows the SDK to use whichever logging framework your application uses and use it for logging events. All loggers log under `io.unlaunch`. 

If you see the following error:

```sh
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

This means you need to provide a concrete implementation. [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) is a popular logging framework that you can use. If you are using Maven, here's how you can add it:

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.7</version>
</dependency>

<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.7</version>
</dependency>

<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>2.7</version>
</dependency>
```

## More Question

At Unlaunch, we are obsessed about making it easier for developers all over the world to release features safely and with confidence. If you have *any* questions or something isn't working as, please email unlaunch@gmail.com.

