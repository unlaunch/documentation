---
title: Unlaunch Go SDK - Official Guide
description: This guide provides complete information about the Unlaunch Go SDK.
---

# Unlaunch Go SDK

This guide provides complete information about the Unlaunch Go SDK and how to integrate it in your applications to use feature flags to safely and confidently release new features to your users. 

The Unlaunch Go SDK provides a Go API to access Unlaunch. Using the SDK, you can easily build Go applications that can evaluate feature flags, access configuration, and more. Unlaunch Go SDK is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/go-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also checkout the [example project](https://github.com/unlaunch/hello-go).

### Language Support

This supports Go language version 1.13 and above.

## Prerequisites

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and enabled it. You also know the [Server SDK key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [client-side and server-side SDKs](client-vs-server-side-sdks). This SDK is server-side and optimized for applications that run on the cloud such as web servers, backend services, etc.

## Import the SDK Library
There are several ways to add the Unlaunch Go SDK to your project. It depends on what dependency management system you're using.

- If you are using [Go Modules](https://blog.golang.org/using-go-modules,) you can simply import the Unlaunch SDK in your code and `go build` will automatically download it.
- If you are using `dep`, import the SDK packages in your code and run `dep ensure`.
- Or you can simply use `go get` command to:

```
go get github.com/unlaunch/go-sdk
```

## Initializing a New Unlaunch Client Instance

### SDK Architecture

Before we dive into the details, let's us understand at a high-level how the Unlaunch Go SDK works.

1. You initialize the client using one of your project's [SDK keys](sdk-keys). The *SDK key* uniquely identifies the environment within the project and all feature flags in it.
2. When you build the client, it starts a *background task* to download all active feature flags and configuration data and stores them in memory. This process can take a few seconds depending on the size of the data. We'll discuss this process in detail later.
3. You can wait for the initialization to complete using the [`AwaitUntilReady`](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClient.AwaitUntilReady) method. You must pass a timeout value as an argument so it doesn't block your application forever.
3. After the initialization is complete, you can evaluate feature flags using the [`Variation`](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClient.Variation) method.

### Initialization

To initialize the client, you'll need an SDK key. SDK Keys are available on the **[Settings](https://app.unlaunch.io/settings)** page under the **Projects** tab. There are several different types of [SDK keys](sdk-keys) for each environment. Make sure you copy the *Server Key* for this SDK. Once you have the SDK Key, you can initialize a new client as following:

```go
config := client.DefaultConfig()
factory, _ := client.NewUnlaunchClientFactory("YOUR_SERVER_KEY", config)

unlaunchClient := factory.Client() // create a new Unlaunch Client

// Wait until all data is downloaded

if err = unlaunchClient.AwaitUntilReady(3 * time.Second); err != nil {
    fmt.Printf("Unlaunch Client wasn't ready %s\n", err)
}
```

{% include alert-note.html type="danger" content="For performance reasons, we strongly recommend initializing the Unlaunch client as a SINGLETON and reusing it throughout your application and go routines." %}

##### Why should an Unlaunch Client be a Singleton?

When you build an Unlaunch client, it starts a background task to download data and store it in an in memory data structure (e.g. Map). This process might take some time depending on the size of the data that needs to be transferred. For performance reasons, it is extremely poor practice to initialize a **new** client *per* incoming request. It will increase response times and hurt system performance. You may also get rate-limited and throttled by Unlaunch servers. 

Instead, you should create the Unlaunch Client as a *singleton* and re-use it throughout your application. If you create more than one instance, we'll print warnings in the logs.

## Using the SDK

After the initialization is complete and the client is created, you are ready to start evaluating feature flags using the [`Variation`](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClient.Variation) method. This method will return a variation (as `string`) that you have defined in the Unlaunch Console. You can check using a simple `if-else` block to execute code depending on the variation that's returned.

```go
variation := unlaunchClient.Variation("PASTE_FLAG_KEY", "UNIQUE_USER_ID_123", nil)

if variation == "on" {
    // show feature
} else if variation == "off" {
    // don't show feature
} else {
    // default code when feature isn't found or evaluated
}
```
### AwaitUntilReady

After you build a new client, it performs an *initial sync* to download feature flags and store in an in-memory store. Until this initial sync is complete, you shouldn't use the client: if you call `Variation` or `Feature` methods, they will return `control` variation since the client is not in a ready state. It is a good practice to wait until the client is ready.

```go
factory, _ := client.NewUnlaunchClientFactory(apiKey, nil)
	
unlaunchClient := factory.Client()

if err := unlaunchClient.AwaitUntilReady(5 * time.Second); err != nil {
    fmt.Printf("Unlaunch Client isn't ready %s\n", err)
}
```

### Evaluating Feature Flags & Getting Variations

The Unlaunch Go SDK provides a few different ways to evaluate feature flags and to get variations, associated configuration and evaluation details. 

##### `Variation(flagKey, identity, attributes)`

This method evaluates and returns the variation (variation key) for this feature flag that you have defined in the [Unlaunch Console](app.unlaunch.io).

This method returns one of the variations according to *targeting or rollout rules* that you may have defined. It will *never panic* nor will it ever return empty string. Instead, it will return `control` if there are any errors such as:

- The flag was not found.
- There was an error evaluating the feature flag.
- The flag was archived.

```go
variation := unlaunchClient.Variation("new-login-ui", "user123@gmail.com", nil)
```

Here, we pass `nil` for the last argument, which represents `attributes`. When your feature flag depends on targeting rules based on user attributes such as `userRegistrationDate`, you must pass in the attributes, as we'll see later.

{% include alert-note.html type="info" content="The getVariation method will never throw an exception." %}

##### `Feature(flagKey, identity, attributes)`

The [Feature()](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClient.Feature) method is similar `Variation()` in the sense that it evaluates feature flag and returns its result. However, instead of only returning the variation as a `string`, it returns a `struct` that contains more information, such as:

- Configuration attached to variation
- Evaluation reason

This is mostly used for fetching *dynamic configuration* associated with the feature flag or for getting the *evaluation reason*. 

For example, say you want to change the color of a button for some users. You'd define the colors for each variation in the Unlaunch Console as a key-value pair. Then in your application, you can fetch the configuration using this method as shown below. The configuration is returned as `map[string]string`.

```go
feature := unlaunchClient.Feature("new_login_ui", "user123@gmail.com", nil);
fmt.Println(fmt.Sprintf("%v", feature.VariationConfiguration)) // print configuration
```

<div class="justify-content-center">
    <img src="/assets/img/feature_flag_config.png" alt="Dynamic Configuration in Unlaunch"/>
</div>

###### Evaluation Reason
When you evaluate a feature flag, the SDK applies various rules to determine which variation should be returned. If you want to know why a certain variation was returned for debugging purposes, you can use the `EvaluationReason` field of the [UnlaunchFeature](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/dtos#UnlaunchFeature) struct.

```go
feature := unlaunchClient.Feature(flagKey, "user123", attr)
fmt.Printf("The variation for feature is: %s. Evaluation reason is: %s\n",
		feature.Variation, feature.EvaluationReason)
// prints
// The variation for feature is: on. evaluation reason is: Targeting Rule Match
```

### Passing Attributes 

The [attributes and associated operators](https://docs.unlaunch.io/docs/features/attributes-operators) are used in [targeting rules](https://docs.unlaunch.io/docs/features/targetingrules). These attributes can be passed to the SDK so it can use them when evaluating rules. 

The SDK method supports six types of attributes: String, Number, Boolean, Date, DateTime, and Set. Here's an example showing how to pass attributes to `GetVariation()` method.

```go
// Let's pass some attributes
attr := make(map[string]interface{}) // map to store all attributes

attr["registered"] = true
attr["device"] = "iphone"
attr["age"] = 30
attr["startDate"] = time.Now().UTC().Unix()

// let's make a "Set" to store some user Id
// We use map because "Go" doesn't have a set type. Only keys are used. The value could be anything and is ignored
userIDs := make(map[string]bool)
userIDs["user123@gmail.com"] = true
userIDs["userabc@gmail.com"] = true

feature := unlaunchClient.Feature(flagKey, "user123", attr)
fmt.Printf("The variation for feature is: %s\n", feature.Variation)
```

### Shutdown 

When your application is shutting down, you can shutdown the Unlaunch client using the `Shutdown()` method. Calling shutdown ensures that any pending metrics are sent to Unlaunch servers.

```go
unlaunchClient.Shutdown()
```

### Configuration

When initializing the client, you have several configuration options to fine-tune the performance and adjust to your needs. The [config](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClientConfig) struct is passed to the factory.

```go
cfg := client.DefaultConfig()
// Customize default client
cfg.PollingInterval = 15 * time.Second // How often flags are fetched
cfg.HTTPTimeout = 3 * time.Second
cfg.MetricsFlushInterval = 30 * time.Second // How often metrics are sent
cfg.MetricsQueueSize = 500

factory, _ := client.NewUnlaunchClientFactory(apiKey, cfg)

unlaunchClient := factory.Client()
```

These options are:

##### `PollingInterval`
The Unlaunch Go SDK periodically downloads flags and other data from the servers and stores it in memory so feature flags can be evaluated with no added latency. The `PollingInterval` controls how often the SDK download flags from the servers if the data has changed. The default value is 60 seconds for production environments and 15 seconds for non-production. 


##### `MetricsFlushInterval`
The SDK periodically sends events like metrics and diagnostics data to our servers. This controls how frequently this data will be sent. When you shutdown a client using the [`Shutdown()`](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClient.Shutdown) method, all pending metrics are automatically sent to the server. The default value is 45 seconds for production and 15 seconds for non-production environments.

##### `MetricsQueueSize`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. The default value is 500 for production and 20 for non-production environments.

##### `OfflineMode`
Feature flags start their journey on a developer's computer. A developer should be able to build and run their code locally even if they don't have network connectivity. To achieve this, Unlaunch Go SDK can be started in **offline mode**. When running in offline mode, the SDK will not connect to Unlaunch servers nor will it send any data to it. 

When activating offline mode, you don't need to pass in the SDK key. When you evaluate a feature flag in offline mode, it will return the `control` variation. 

##### `HTTPTimeout`
Sets the default connection timeout for the HTTPClient SDK uses to connect to Unlaunch servers. The default value is 10 seconds. The minimum value allowed is 1 second.

##### `Host`
Unlaunch server to connect to for downloading feature flags and for submitting events. Only use this if you are running Unlaunch backend service on-premise or are an enterprise customer. The default value is https://api.unlaunch.io

## Advanced Usage

### Concepts

#### 1. In memory data store
Unlaunch Go SDK (like all server-side SDKs) connect to Unlaunch servers upon initialization to download all feature flags, variations and dynamic configurations, and store these in an in-memory data store (e.g. Map). All subsequent flag evaluations are done using the in memory data store and results are available instantly.

#### 2. Events and Metrics Tracking

Unlaunch Go SDK periodically sends events to Unlaunch servers. These events are used for showing metrics and to generate data for the Insights Graph.

### Offline Mode

Unlaunch Go SDK can be started in **offline mode**. In offline mode, SDK will not connect to Unlaunch Server and will not send data to it. 

When you evaluate a feature flag in offline mode, it will return the `control` variation.

To set offline mode, set `Offline` to `true` in configs. By default, it is `false`.
```
    config := client.DefaultConfig()
    config.OfflineMode = true
    
    factory, err := client.NewUnlaunchClientFactory("YOUR_SERVER_KEY", config)
```

### Logging

The Unlaunch Go SDK uses custom logging and allows you to supply your own logger (by implementing the [logger interface](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/util/logger#Interface).) By default, the logger logs ERROR logs only and the output is sent to `os.Stdout`.

```go
type Interface interface {
	Trace(msg ...interface{})
	Debug(msg ...interface{})
	Info(msg ...interface{})
	Warn(msg ...interface{})
	Error(msg ...interface{})
}
```

### How to Change Log Level

```go
cfg := client.DefaultConfig()
cfg.LoggerConfig = &logger.LogOptions{
    Level:    "INFO",
    Colorful: true,
}
factory, err := client.NewUnlaunchClientFactory(apiKey, cfg)
```

## More Questions?

At Unlaunch, we are obsessed about making it easier for developers all over the world to release features safely and with confidence. If you have *any* questions or something isn't working as expected, please email **help@unlaunch.io**.


