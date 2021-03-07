---
title: Unlaunch .NET SDK - Official Guide
description: This guide provides complete information about the Unlaunch .NET SDK.
---

# Unlaunch .NET SDK

This guide provides complete information about the Unlaunch .NET SDK and how to integrate it in your applications to use Unlaunch feature flags.

The Unlaunch .NET SDK provides a .NET API to access Unlaunch. Using the SDK, you can easily build .NET applications that can evaluate feature flags, access configuration, and more. Unlaunch .NET SDK is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/dotnet-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also checkout the .NET [example project](https://github.com/unlaunch/hello-csharp/blob/master/hello-csharp/Program.cs).

### Language Support

The Unlaunch .NET SDK supports .NET Framework 4.5+ and NetStandard 2.0+

## Prerequisites

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and enabled it. You also know the [Server SDK key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [client-side and server-side SDKs](client-vs-server-side-sdks). This SDK is server-side and optimized for applications that run on the cloud such as web servers, backend services, etc.

## Import the SDK Library
You can import the SDK using **Nuget**. For more information, [click here](https://www.nuget.org/packages/unlaunch).

```
Install-Package unlaunch -Version 0.0.7
```

## Initializing a New Unlaunch Client Instance

In a nutshell, here is how this SDK works:

1. You initialize the client using one of your project's [SDK keys](sdk-keys). The SDK key uniquely identifies the environment within the project and all feature flags in it.
2. When you build the client, it starts a *background task* to download all active feature flags and configuration data and stores them in memory. This process can take a few seconds depending on the size of the data.
3. You can wait for the initialization to complete using the [`AwaitUntilReady`](https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchClient.cs) method. You must pass a timeout/TimeSpan value as an argument so it doesn't block your application forever.
3. After the initialization is complete, you can evaluate feature flags using the [`GetVariation`](https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchClient.cs) method.

### Initialization

To initialize the client, you'll need an SDK key. SDK Keys are available on the **[Settings](https://app.unlaunch.io/settings)** page under the **Projects** tab. There are several different types of [SDK keys](sdk-keys) for each environment. Make sure you copy the *Server Key* for this SDK. Once you have the SDK Key, you can initialize a new client as following:

```csharp
var ulClient = UnlaunchClient.Create("INSERT_YOUR_SDK_KEY");

ulClient.AwaitUntilReady(TimeSpan.FromSeconds(2)); // Wait until all data is downloaded
```

{% include alert-note.html type="danger" content="For performance reasons, we strongly recommend initializing the Unlaunch client as a SINGLETON and reusing it throughout your application. The client is thread-safe." %}

##### Why should an Unlaunch Client be a Singleton?

When you build an Unlaunch client, it starts a background task to download data and store it in an in memory data structure (e.g. Map). This process might take some time depending on the size of the data that needs to be transferred. For performance reasons, it is extremely poor practice to initialize a **new** client *per* incoming request. It will increase response times and hurt system performance. You may also get rate-limited and throttled by Unlaunch servers. 

Instead, you should create the Unlaunch Client as a *singleton* and re-use it throughout your application. If you create more than one instance, we'll print warnings in the logs.

## Using the SDK

After the initialization is complete and the client is created, you are ready to start evaluating feature flags using the [`GetVariation`](https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchClient.cs) method. This method will return a variation that you have defined in the Unlaunch Console. You can check using a simple `if-else` block to execute code depending on the variation that's returned.

The `GetVariation` method requires that you pass in the `flag key`and the `user id` of the user that you are evaluating the feature flag for. If the flag is an Operations flag such as a global kill switch and doesn't require users, you can define a string constant, e.g. `userId=System` and pass it instead.

```csharp
string userId = "123";
string variation = ulClient.GetVariation("log-levels", userId);

if (variation == "on") 
{
    // code to show the feature
} 
else if (variation == "off") 
{
    // code to hide the feature
} 
else 
{
    // default code when feature isn't found or evaluated
}
```

### AwaitUntilReady

After you build a new client, it performs an *initial sync* to download feature flags and store in an in-memory store. Until this initial sync is complete, you shouldn't use the client: if you call `GetVariation` or `GetFeature` methods, they will return `control` variation since the client is not in a ready state. It is a good practice to wait until the client is ready. 

```csharp
var client = UnlaunchClient.Builder()
                            .SdkKey("your_sdk_key")
                            .Build();

try 
{
    client.AwaitUntilReady(TimeSpan.FromSeconds(2));
} 
catch (TimeoutException e) 
{
    Console.WriteLine($"Client wasn't ready, error: {e.Message}");
}
```

You can check if the client is ready by calling the [`IsReady`](https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchClient.cs) method.
#### Note
If client is not ready, sdk will try to pull feature flags again on next polling interval.

### Evaluating Feature Flags & Getting Variations

The Unlaunch .NET SDK provides a few different ways to evaluate feature flags and to get variations. 

##### `GetVariation(flagKey, identity)`

This method evaluates and returns the variation (variation key) for this feature flag that you have defined in the [Unlaunch Console](app.unlaunch.io).

This method returns one of the variations according to *targeting or rollout rules* that you may have defined. It will *never throw an exception* nor will it ever return `null`. Instead, it will return `control` if there are any errors such as:

- The flag was not found.
- There was an exception evaluating the feature flag.
- The flag was archived.

```csharp
string variation = client.GetVariation("new-login-ui", userId);
```

This method is to be used when feature flag targeting rules don't depend on user attributes such as `userRegistrationDate` etc. Use the overloaded method below for passing in attributes.

{% include alert-note.html type="info" content="The getVariation method will never throw an exception." %}

##### `GetVariation(flagKey, identity, attributes)`

An overloaded method that requires that you pass in user attributes that can be used to evaluate targeting rules. For example, suppose you have defined targeting rules for a feature flag as such:

```
if country is "USA" AND subscriber is true
    return "on"
otherwise
    return "off"
```

When evaluating this flag, you must pass in `country` and `subscriber` attributes. If the 
user is from the USA *and* is a subscriber, the "on" variation will be returned. Otherwise, "off". For example, 

```csharp
client.GetVariation(
    "show_bonus_pack", 
    userId, 
    UnlaunchAttribute.NewString("country", "USA"),
    UnlaunchAttribute.NewBoolean("subscriber", true)
);
```

##### `GetFeature(string flagKey, string identity)`

This behaves just like the `GetVariation` method but instead of returning a string, it returns an [UnlaunchFeature] (https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchFeature.cs) object instead. Use this method when you want to get more than just the variation. This is mostly used for fetching *dynamic configuration* associated with the feature flag or for getting the *evaluation reason*. 

For example, say you want to change the color of a button for some users. You'd define the colors for each variation in the Unlaunch Console as a key-value pair. Then in your application, you can fetch like this:

```csharp
var feature = client.GetFeature("new_login_ui", userId);
string colorHexCode = feature.GetVariationConfig().GetString("login_button_color", "#cd5c5c");

RenderButton(colorHexCode);
```

<div class="justify-content-center">
    <img src="/assets/img/feature_flag_config.png" alt="Dynamic Configuration in Unlaunch"/>
</div>

###### Evaluation Reason
When you evaluate a feature flag, the SDK applies various rules to determine which variation should be returned. If you want to know why a certain variation was returned for debugging purposes, you can use the `GetEvaluationReason()` method of the `UnlaunchFeature` class.

```csharp
var feature = client.GetFeature("new_login_ui", userId);
string reason = feature.GetEvaluationReason();

logger.Debug("{variation} variation was returned because: {reason}", feature.GetVariation(), reason);
// This might print
// on variation was returned because: Default Rule match
```

##### `GetFeature(flagKey, identity, attributes)`

Just like the method above but uses attributes that are passed in to evaluate targeting rules.

### Passing Attributes 

The [attributes and associated operators](https://docs.unlaunch.io/docs/features/attributes-operators) are used in [targeting rules](https://docs.unlaunch.io/docs/features/targetingrules). These attributes can be passed to the SDK so it can use them when evaluating rules. 

The SDK method supports six types of attributes: String, Number, Boolean, Date, DateTime, and Set. Here's an example showing how to pass attributes to `GetVariation()` method.

```csharp

var client = UnlaunchClient.Create("SDK_KEY");
 
var userSet = new HashSet<string>(new [] {"value1", "value2"});

var variation = client.GetVariation(
    "FEATURE_FLAG_KEY",
    System.Guid.NewGuid().ToString(),
    new[]
    {
        UnlaunchAttribute.NewBoolean("registered", true),
        UnlaunchAttribute.NewString("device", "ABCS"),
        UnlaunchAttribute.NewNumber("age", 30),
        UnlaunchAttribute.NewDate("start_date", DateTime.UtcNow.Date),
        UnlaunchAttribute.NewDateTime("expiry_date", DateTime.UtcNow.AddMonths(1)),
        UnlaunchAttribute.NewSet("user_ids", userSet)
    }
);

// Print variation
_logger.Info("[DEMO] getVariation() returned {variation}", variation);

// shutdown the client to flush any events or metrics
client.Shutdown();
```

### Shutdown 

When your application is shutting down, you can shutdown the Unlaunch client using the `Shutdown` method. Calling shutdown ensures that any pending metrics are sent to Unlaunch servers.

```csharp
client.Shutdown();
```

### Configuration

When initializing the client, you have several configuration options to fine-tune the performance and adjust to your needs. These options are:

##### `PollingInterval()`
The Unlaunch .NET SDK periodically downloads flags and other data from the servers and stores it in-memory so feature flags can be evaluated with no added latency. The `PollingInterval` controls how often the SDK download flags from the servers if the data has changed. The default value is 60 seconds for production environments and 15 seconds for non-production. For example, to change the polling interval to 5 minutes: 

```csharp
var client = UnlaunchClient.Builder()
                            .PollingInterval(TimeSpan.FromMinutes(5))
                            .SdkKey("<your environment sdk key>")
                            .Build();
```

##### `MetricsFlushInterval()`
The SDK periodically sends events like metrics and diagnostics data to our servers. This controls how frequently this data will be sent. When you shutdown a client using the [`Shutdown()`](https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchClient.cs) method, all pending metrics are automatically sent to the server. The default value is 30 seconds for production and 15 seconds for non-production environments. For example, to change the event flush time to 10 minutes:

```csharp
var client = UnlaunchClient.Builder()
                            .MetricsFlushInterval(TimeSpan.FromMinutes(2))
                            .SdkKey("your_environment_sdk_key")
                            .Build();
```

##### `MetricsQueueSize()`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. The default value is 500.

##### `EventsFlushInterval()`
This controls how frequently tracking events are sent to the server. The default value is 60 seconds for production and 15 seconds for non-production environments.

##### `EventsQueueSize()`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. The default value is 500.

##### `OfflineMode()`
When enabled, this starts the SDK in offline mode where no flags are downloaded from the server, nor anything is sent. All calls to [`GetVariation()`](https://github.com/unlaunch/dotnet-sdk/blob/master/client-sdk/UnlaunchClient.cs) will return `control`. 

##### `ConnectionTimeout()`
Sets the default connection timeout for the HTTPClient SDK uses to connect to Unlaunch servers. The default value is 10 seconds. The minimum value allowed is 1 second.

##### `Host()`
Unlaunch server to connect to for downloading feature flags and for submitting events. Only use this if you are running Unlaunch backend service on-premise or are an enterprise customer. The default value is https://api.unlaunch.io

## Advanced Usage

### Concepts

#### 1. In memory data store
Unlaunch .NET SDK (all server-side SDKs) connect to Unlaunch servers upon initialization to download all feature flags, variations and dynamic configurations, and store these in an in-memory data store (e.g. Map). All subsequent flag evaluations are done using the in memory data store and results are available instantly.

#### 2. Events and Metrics Tracking

Unlaunch .NET SDK periodically sends events to Unlaunch servers. These events are used for showing metrics and to generate data for the Insights Graph.

### Logging

The Unlaunch .NET SDK uses Common.Logging [`3.4.1`](https://www.nuget.org/packages/Common.Logging/) for NET4.5 and Microsoft.Extensions.Logging [`2.2.0`](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) for NetStandard.

## More Questions?

At Unlaunch, we are obsessed about making it easier for developers all over the world to release features safely and with confidence. If you have *any* questions or something isn't working as expected, please email **help@unlaunch.io**.

