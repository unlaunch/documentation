---
title: Unlaunch Java SDK - Official Guide
description: This guide provides complete information about the Unlaunch Java SDK.
---

# Unlaunch Java SDK

This guide provides complete information about the Unlaunch Java SDK and how to integrate it in your applications to call feature flags.

The Unlaunch Java SDK provides a Java API to access Unlaunch. Using the SDK, you can easily build Java applications that can evaluate feature flags, access configuration, and more. Unlaunch Java SDK is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/java-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also checkout the Java [example project](https://github.com/unlaunch/hello-java/blob/main/src/main/java/io/unlaunch/sdk/Hello.java).

### Language Support

The Unlaunch Java SDK supports Java version 8 and above.

If you are looking for Unlaunch Java SDK **Javadocs**, please [click here](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/index.html).

## Prerequisites

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and enabled it. You also know the [Server SDK key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [client-side and server-side SDKs](client-vs-server-side-sdks). This SDK is server-side and optimized for applications that run on the cloud such as web servers, backend services, etc.

## Import the SDK Library

The first step is to import the Unlaunch SDK as Maven or Gradle dependency in your application. For the *latest version* of the SDK, please refer to the [Maven Repository](https://mvnrepository.com/artifact/io.unlaunch.sdk/unlaunch-java-sdk).

For Maven, 
```xml
<dependency>
    <groupId>io.unlaunch.sdk</groupId>
    <artifactId>unlaunch-java-sdk</artifactId>
    <version>0.0.4</version>
</dependency>
```

For Gradle,
```
compile group: 'io.unlaunch.sdk', name: 'unlaunch-java-sdk', version: '0.0.4'
```

## Initializing a New Unlaunch Client Instance

### SDK Architecture

In a nutshell, here is how this SDK works:

1. You initialize the client using one of your project's [SDK keys](sdk-keys). The SDK key uniquely identifies the environment within the project and all feature flags in it.
2. When you build the client, it starts a *background task* to download all active feature flags and configuration data and stores them in memory. This process can take a few seconds depending on the size of the data.
3. You can wait for the initialization to complete using the [`awaitUntilReady`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html) method. You must pass a timeout value as an argument so it doesn't block your application forever.
3. After the initialization is complete, you can evaluate feature flags using the public [`getVariation`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html) method.

### Initialization

To initialize the client, you'll need an SDK key. SDK Keys are available on the **[Settings](https://app.unlaunch.io/settings)** page under the **Projects** tab. There are several different types of [SDK keys](sdk-keys) for each environment. Make sure you copy the *Server Key* for this SDK. Once you have the SDK Key, you can initialize a new client as following:

```java
UnlaunchClient ulClient = UnlaunchClient.create("INSERT_YOUR_SDK_KEY");

ulClient.awaitUntilReady(2, TimeUnit.SECONDS); // Wait until all data is downloaded
```

{% include alert-note.html type="danger" content="For performance reasons, we strongly recommend initializing the Unlaunch client as a SINGLETON and reusing it throughout your application. The client is thread-safe." %}

##### Why should an Unlaunch Client be a Singleton?

When you build an Unlaunch client, it starts a background task to download data and store it in an in memory data strucuture (e.g. Map). This process might take some time depending on the size of the data that needs to be transferred. For performance reasons, it is extremely poor practice to initialize a **new** client *per* incoming request. It will increase response times and hurt system performance. You may also get rate-limited and throttled by Unlaunch servers. 

Instead, you should create the Unlaunch Client as a *singleton* and re-use it throughout your application. If you create more than one instance, we'll print warnings in the logs.

## Using the SDK

After the initialization is complete and the client is created, you are ready to start evaluating feature flags using the [`getVariation`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html) method. This method will return a variation (as `String` )that you have defined in the Unlaunch Console. You can check using a simple `if-else` block to execute code depending on the variation that's returned.

The `getVariation` method requires that you pass in the `flag key`and the `user id` of the user that you are evaluating the feature flag for. If the flag is an Operations flag such as a global kill switch and doesn't require users, you can define a String constant, e.g. `userId=System` and pass it instead.

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

### Evaluating Feature Flags & Getting Vari	ations

The Unlaunch Java SDK provides a few different ways to evaluate feature flags and to get variations. 

##### `getVariation(flagKey, Identity)`

This method evaluates and returns the variation (variation key) for this feature flag that you have defined in the [Unlaunch Console](app.unlaunch.io).

This method returns one of the variations according to *targeting or rollout rules* that you may have defined. It will *never throw an exception* nor will it ever return `null`. Instead, it will return `control` if there are any errors such as:

- The flag was not found.
- There was an exception evaluating the feature flag.
- The flag was archived.

```java
String variation = client.getVariation("new-login-ui", userId);
```

This method is to be used when feature flag targeting rules don't depend on user attributes such as `userRegistrationDate` etc. Use the overloaded method below for passing in attributes.

{% include alert-note.html type="info" content="The getVariation method will never throw an exception." %}

##### `getVariation(flagKey, Identity, attributes)`

An overloaded method that requires that you pass in user attributes that can be used to evaluate targeting rules. For example, suppose you have defined targeting rules for a feature flag as such:

```
if country is "USA" AND subscriber is true
    return "on"
otherwise
    return "off"
```

When evaluating this flag, you must pass in `country` and `subscriber` attributes. If the 
user is from the USA *and* is a subscriber, the "on" variation will be returned. Otherwise, "off". For example, 

```java
client.getVariation(
    "show_bonus_pack", 
    userId, 
    UnlaunchAttribute.newString("country", "USA"),
    UnlaunchAttribute.newBoolean("subscriber", true)
);
```

##### `getFeature(String flagKey, String identity)`

This behaves just like the `getVariation` method but instead of returning a string, it returns an [UnlaunchFeature](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchFeature.html) object instead. Use this method when you want to get more than just the variation. This is mostly used for fetching *dynamic configuration* associated with the feature flag or for getting the *evaluation reason*. 

For example, say you want to change the color of a button for some users. You'd define the colors for each variation in the Unlaunch Console as a key-value pair. Then in your application, you can fetch like this:

```java
UnlaunchFeature feature = client.getFeature("new_login_ui", userId);
String colorHexCode = feature.getVariationConfig().getString("login_button_color", "#cd5c5c");

renderButton(colorHexCode);
```

<div class="justify-content-center">
    <img src="/assets/img/feature_flag_config.png" alt="Dynamic Configuration in Unlaunch"/>
</div>

###### Evaluation Reason
When you evaluate a feature flag, the SDK applies various rules to determine which variation should be returned. If you want to know why a certain variation was returned for debugging purposes, you can use the `getEvaluationReason()` method of the `UnlaunchFeature` class.

```java
UnlaunchFeature feature = client.getFeature("new_login_ui", userId);
String reason = feature.getEvaluationReason();

logger.debug("{} variation was returned because: {}", feature.getVariation(), reason);
// This might print
// on variation was returned because: Default Rule match
```

##### `getFeature(flagKey, identity, attributes)`

Just like the method above but uses attributes that are passed in to evaluate targeting rules.

### Attribute syntax

The [attributes and associated operators](https://docs.unlaunch.io/docs/features/attributes-operators) used in [targeting rules](https://docs.unlaunch.io/docs/features/targetingrules), the attributes are passed as Set or List in *getVariation* method of SDK. 

In the example below, the attributes are provided as a Set in *getVariation* method. These attributes are compared and evaluated against the attributes used in the targeting rule defined on the web to decide whether to show the *on* or *off* variation to a user.

The *getVariation* method supports six types of attributes: string, number, boolean, date, DateTime, and set. The [attributes](https://docs.unlaunch.io/docs/features/attributes) and syntax with [operators](https://docs.unlaunch.io/docs/features/attributes-operators) are defined in SDK as: 

        UnlaunchClient client = UnlaunchClient.create(SDK_KEY);

        Set<String> userSet = new HashSet<>();
        userSet.add("1");
        userSet.add("2");
        
        String variation = client.getVariation(
                FEATURE_FLAG_KEY,
                UUID.randomUUID().toString(),
                UnlaunchAttribute.newBoolean("registered", true),
                UnlaunchAttribute.newString("device", "ABCS"),
                UnlaunchAttribute.newNumber("age", 30),
                UnlaunchAttribute.newDate("start_date", System.currentTimeMillis()),
                UnlaunchAttribute.newDateTime("expiry_date", System.currentTimeMillis()),
                UnlaunchAttribute.newSet("user_ids", userSet));

        // Print variation
        LOG.info("[DEMO] getVariation() returned {}", variation);

        // shutdown the client to flush any events or metrics
        client.shutdown();

### awaitUntilReady

After you build a new client, it performs an *initial sync* to download feature flags and store in an in-memory store. Until this initial sync is complete, you shouldn't use the client: if you call `getVariation` or `getFeature` methods, they will return `control` variation since the client is not in a ready state. It is a good practice to wait until the client is ready.

```java
UnlaunchClient client = UnlaunchClient.builder()
                            .sdkKey("your_sdk_key")
                            .build();

try {
    client.awaitUntilReady(2, TimeUnit.SECONDS);
} catch (InterruptedException | TimeoutException e) {
    System.out.println("client wasn't ready " + e.getMessage());
}
```

You can check if the client is ready by calling the [`isReady`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html#isReady()) method.

### Shutdown 

When your application is shutting down, you can shutdown the Unlaunch client using the `shutdown` method. Calling shutdown ensures that any pending metrics are sent to Unlaunch servers.

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
The SDK periodically sends events like metrics and diagnostics data to our servers. This controls how frequently this data will be sent. When you shutdown a client using the [`shutdown()`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html#shutdown()) method, all pending metrics are automatically sent to the server. The default value is 30 seconds for production and 15 seconds for non-production environments. For example, to change the event flush time to 10 minutes:

```java 
UnlaunchClient client = UnlaunchClient.builder()
                            .metricsFlushInterval(2, TimeUnit.MINUTES)
                            .sdkKey("your_environment_sdk_key")
                            .build();
```

##### `metricsQueueSize()`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. The default value is 500.

##### `eventsFlushInterval()`
This controls how frequently tracking events are sent to the server. The default value is 60 seconds for production and 15 seconds for non-production environments.

##### `eventsQueueSize()`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. The default value is 500.

##### `offlineMode()`
When enabled, this starts the SDK in offline mode where no flags are downloaded from the server, nor anything is sent. All calls to [`getVariation()`](https://javadoc.io/doc/io.unlaunch.sdk/unlaunch-java-sdk/latest/io/unlaunch/UnlaunchClient.html#getVariation(java.lang.String,java.lang.String)) will return `control`. Please see [Offline Mode](https://docs.unlaunch.io/docs/sdks/java-sdk#offline-mode) below for more information.

##### `connectionTimeout()`
Sets the default connection timeout for the HTTP client SDK uses to connect to Unlaunch servers. The default value is 10 seconds. The minimum value allowed is 1 second.

##### `readTimeout()`
Sets the default read timeout for the HTTP connections. Specifies the time to wait for data to arrive after establishing the connection. The default value is 10 seconds. The minimum value allowed is 1 second.

##### `offlineModeWithLocalFeatures()`
This is intended for testing, including unit testing. This allows you to pass a YAML file containing feature flags and the variations to return when they are evaluated. You can also control dynamic configuration and specify which values to return. Please see [Offline Mode](https://docs.unlaunch.io/docs/sdks/java-sdk#offline-mode) below for more information and a YAML template.

##### `host()`
Unlaunch server to connect to for downloading feature flags and for submitting events. Only use this if you are running Unlaunch backend service on-premise or are an enterprise customer. The default value is https://api.unlaunch.io

## Advanced Usage

### Concepts

#### 1. In-memory data store
Unlaunch Java SDK (all server-side SDKs) connect to Unlaunch servers upon initialization to download all feature flags, variations and dynamic configurations, and store these in an in-memory data store. All subsequent flag evaluations are done using the in-memory data store and results are available instantly.

#### 2. Events and Metrics Tracking
Unlaunch Java SDK periodically sends events to Unlaunch servers. These events are used for showing metrics and to generate data for the Insights Graph.

### Offline Mode

Feature flags start their journey on a developer's computer. A developer should be able to build and run their code locally even if they don't have network connectivity. To achieve this, Unlaunch Java SDK can be started in **offline mode**. When running in offline mode, the SDK will not connect to Unlaunch servers nor will it send any data to it. 

When activating offline mode, you don't need to pass in the SDK key. When you evaluate a feature flag in offline mode, it will return the `control` variation. 

```java
UnlaunchClient ulClient = UnlaunchClient.builder()
                .offlineMode()
                .build();
```

If you want to control what variations are returned, you can specify feature flags and variations in a YAML file and load it using `offlineModeWithLocalFeatures` method. Unlaunch will return variations that you have specified in the YAML file. Any flag not found from the YAML file will return `control` variation.

### Logging

The Unlaunch Java SDK uses [SLF4J](http://www.slf4j.org/) (slf4j-api). SLF4J is a logging facade which provides abstractions for various logging frameworks. This allows the SDK to use whichever logging framework your application provides for logging events. All loggers log under `io.unlaunch` name.

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

## More Questions?

At Unlaunch, we are obsessed about making it easier for developers all over the world to release features safely and with confidence. If you have *any* questions or something isn't working as expected, please email **unlaunch@gmail.com**.

