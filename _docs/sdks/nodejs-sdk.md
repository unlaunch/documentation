---
title: Unlaunch Node.js SDK - Official Guide
description: This guide provides complete information about the Unlaunch Node.js SDK
---


# Unlaunch Node.js SDK

This guide provides complete information about the Unlaunch Node.js SDK and how to integrate it in your applications to call feature flags.

The Unlaunch Node.js SDK provides a Javascript API to use Unlaunch. You can evaluate feature flags, access configuration, and more by using this SDK.

## Language Support

The Node SDK supports Node.js version 6.x and npm 3.x or later.

## Import the SDK Library

The first step is to install the Unlaunch SDK. Using NPM:

```xml
npm install --save unlaunch-node-sdk
```
Second step is to require the Unlaunch Node SDK.

```xml
const UnlaunchFactory = require("unlaunch-node-sdk")
```

## Initializing a New Unlaunch Client Instance

So to initialize the client, you'll need an SDK key. SDK Keys are available on the **[Settings](https://app.unlaunch.io/settings)** page under **Projects** tab. Once you have it ready, you can initialize a new client.

```javascript
var factory = UnlaunchFactory({
    core: {
        sdkKey: YOUR_SDK_KEY
    },
    intervals: {
        // fetch feature updates each 30 sec
        pollingInterval: 30,
        // publish metrics each 120 sec
        metricsFlushInterval: 120,
        // flush events every 60 seconds after the first flush
        eventsFlushInterval: 60,
        // http connection timeout 
        httpConnectionTimeout: 10
    },
    mode: {
        offlineMode: false
    }
});
const ulClient = factory.client();

```

{% include alert-note.html type="info" content="For performance reasons, we strongly recommend initializing the Unlaunch client as a SINGLETON and reusing it throughout your application." %}

##### Why should an Unlaunch Client be a Singleton?

When you build an Unlaunch client, it starts a background task to download data and store it in an in-memory cache. This process might take some time depending on the size of the data that needs to be transferred. It is extremely poor practice to initialize a new client for each client request. It will add latency to the request and all downloaded data will be discarded once the request is complete. You may also get rate-limited and throttled by our intrusion detection systems.

It is important that you create an Unlaunchclient as a singleton and re-use it throughout your application. If you create more than one instance, we'll print warnings in the logs.

## Using the SDK

After the initialization is complete, you are ready to start evaluating feature flags using the [`variation`] method. This method will return a variation that you have defined in the Unlaunch Console. You can check using a simple `if-else` block to execute code depending on the variation that's returned.

The `variation` method requires that you pass in the `flag key`and the `user id` of the user that you are evaluating the feature flag for. If the flag is an Operations flag such as a global kill switch and doesn't require users, you can define a String constant, e.g. `userId=System` and pass it instead.

```javascript
cosnt userId = "123";
const variation = ulClient.variation("log-levels", userId);

if (variation == "on")) {
    // code to show the feature
} else if (variation == "off") {
    // code to hide the feature
} else {
    // default code when feature isn't found or evaluated
}
```

### Evaluating Feature Flags & Getting Variations

The Unlaunch Javascript SDK provides a few different ways to evaluate feature flags and to get variations. 

##### `variation(flagKey, Identity)`

This method evaluates and returns the variation (variation key) for this feature flag that you have defined in the [Unlaunch Console](https://app.unlaunch.io).

This method returns one of the variations according to *targeting or rollout rules* that you may have defined. It will *never throw an exception* nor will it ever return `null`. Instead, it will return `control` if there are any errors such as:

- The flag was not found.
- There was an exception evaluating the feature flag.
- The flag was archived.

```javascript
const variation = ulClient.variation("new-login-ui", userId);
```

This method is to be used when feature flag targeting rules don't depend on user attributes such as `userRegistrationDate` etc. Use the overloaded method below for passing in attributes.

{% include alert-note.html type="info" content="The variation method will never throw an exception." %}

##### `variation(flagKey, Identity, attributes)`

An overloaded method that requires that you pass in user attributes that can be used to evaluate targeting rules. For example, suppose you have defined targeting rules for a feature flag as such:

```
if country is "USA" AND subscriber is true
    return "on"
otherwise
    return "off"
```

When evaluating this flag, you must pass in `country` and `subscriber` attributes. If the 
user is from the USA *and* is a subscriber, the "on" variation will be returned. Otherwise, "off". For example, 

```javascript
var attributes={
      country: "USA",
      subscribe: true
    } 
ulClient.variation(
    "show_bonus_pack", 
    userId, 
    attributes
);

if (variation == "on")) {
    // code to show the feature
} else if (variation == "off") {
    // code to hide the feature
} else {
    // default code when feature isn't found or evaluated
}
```
### Passing Attributes 

The [attributes and associated operators](../attributes/attributes-operators) are used in [targeting rules](https://docs.unlaunch.io/docs/features/targetingrules). These attributes can be passed to the SDK so it can use them when evaluating rules. 

The SDK method supports six types of attributes: String, Number, Boolean, Date, DateTime, and Set. Here's an example showing how to pass attributes to `variation()` method.

```javascript

const ulClient = factory.client();
client.on('READY',() => {

  const userId = "USER_UNIQUE_ID";
  const currentDate = new Date();
  
  var attributes =  {
        registered: true, // Boolean
        device: "ABCS", // String
        age: 30, // Integer
        start_date: currentDate , // Date
        start_time: currentDate.getTime(),// Datetime
        user_ids: ["1", "2", "3", "4"] // Set
    }

  var variation = ulClient.variation(
    "FEATURE_FLAG_KEY",
    userId,
    attributes
    )

  // shutdown the client to flush any events or metrics
  ulClient.shutdown();
```

##### `feature(flagKey, identity)`

This behaves just like the `variation` method but instead of returning a variation, it returns a `Feature Object` instead. Use this method when you want to get more than just the variation. This is mostly used for fetching *dynamic configuration* associated with the feature flag. 

```javascript
const feature = ulClient.feature("new_login_ui", userId);
const colorHexCode = feature.variationConfiguration()['login_button_color'];

renderButton(colorHexCode);
```

<div class="justify-content-center">
    <img src="/assets/img/feature_flag_config.png" alt="Dynamic Configuration in Unlaunch"/>
</div>

###### Evaluation Reason
When you evaluate a feature flag, the SDK applies various rules to determine which variation should be returned. If you want to know why a certain variation was returned for debugging purposes, you can use the `evaluationReason` property of the `Feature Object` .

```javascript
const feature = ulClient.getFeature("new_login_ui", userId);
const reason = feature.evaluationReason;

console.log("{} variation was returned because: {}", feature.variation, reason);
// This might print
// on variation was returned because: Default Rule match
```

##### `feature(flagKey, identity, attributes)`

Just like the method above but uses attributes that are passed in to evaluate targeting rules.

### Ready Event

After you initialize new client, it will download all flags and this process will take some time depending on data. Once it's complete downloading flags, it will emit ready event. Only after that evaluation of flag would be possible.

```javascript
const ulClient = factory.client();
client.on('READY',() => {
  const userId = "123";
  const variation = ulClient.variation("log-levels", userId);

  if (variation == "on")) {
      // code to show the feature
  } else if (variation == "off") {
      // code to hide the feature
  } else {
      // default code when feature isn't found or evaluated
  }
})
```

### Shutdown

Call `ulClient.shutdown()` method in the end as this method gracefully shutdown the process by closing opened connections, clearing cache and flushing remaining unpublished impression. Once client is destroyed any invocation to variation method will result in returning `control`. 

```javascript
ulClient.shutdown();
```

### Configuration

When initializing the client, you can pass several configuration options as an object as per your requirement.

##### `pollingInterval`
The Unlaunch Java SDK periodically downloads flags and other data from the servers and stores it in-memory so feature flags can be evaluated with no added latency. The `pollingInterval` controls how often the SDK download flags from the servers if the data has changed. The default value is 60 seconds for production environments and 15 seconds for non-production. For example, to change the polling interval to 5 minutes: 

##### `metricsFlushInterval`
The SDK periodically sends events like metrics and diagnostics data to our servers. This controls how frequently this data will be sent. When you shutdown a client using the client.destroy(), all pending metrics are automatically sent to the server. The default value is 30 seconds for production and 15 seconds for non-production environments. For example, to change the event flush time to 10 minutes:

##### `eventsFlushInterval`
This controls how frequently tracking events are sent to the server. The default value is 60 seconds for production and 15 seconds for non-production environments.

##### `eventsQueueSize`
This controls the maximum number of events to keep in memory. Events are sent to the server when either the queue size OR events flush interval is reached, whichever comes first. The default value is 500.

##### `offlineMode`
When enabled, this starts the SDK in offline mode where no flags are downloaded from the server, nor anything is sent. All calls will return the variation defined against each flag.


## Advanced Usage

### Concepts

#### 1. In-memory data store
Unlaunch Java SDK (all server-side SDKs) connect to Unlaunch servers upon initialization to download all feature flags, variations and dynamic configurations, and store these in an in-memory data store. All subsequent flag evaluations are done using the in-memory data store and results are available instantly.

#### 2. Events and Metrics Tracking
Unlaunch Java SDK periodically sends events to Unlaunch servers. These events are used for showing metrics and to generate data for the Insights Graph.

### Offline Mode
In this mode, the SDK neither polls nor updates Unlaunch servers. Instead, it uses JSON file to determine which variation to show to the logged in user for each of the features. You can specify feature flags and variations in a JSON file in your machine and pass its path in configuration against `OfflineModeFilePath`. 

## More Questions?

At Unlaunch, we are obsessed about making it easier for developers all over the world to release features safely and with confidence. If you have *any* questions or something isn't working as expected, please email **help@unlaunch.io**.

