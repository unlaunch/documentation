---
title: Unlaunch Go SDK - Official Guide
description: This guide provides complete information about the Unlaunch Go SDK.
---

# Unlaunch Go SDK

This guide provides complete information about the Unlaunch Go SDK and how to integrate it in your applications to use feature flags to safely and confidently release new features to your users. 

The Unlaunch Go SDK provides a Go API to access Unlaunch. Using the SDK, you can easily build Go applications that can evaluate feature flags, access configuration, and more. Unlaunch Go SDK is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/go-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also checkout the Java [example project](https://github.com/unlaunch/hello-go).

### Language Support

This supports Go language version 1.13 and above.

## Prerequisites

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and enabled it. You also know the [Server SDK key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [client-side and server-side SDKs](client-vs-server-side-sdks). This SDK is server-side and optimized for applications that run on the cloud such as web servers, backend services, etc.

## Import the SDK Library
There are several ways to add the Unlaunch Go SDK to your project. It depends on what dependency management system you're using.

- If you are using [Go Modules](https://blog.golang.org/using-go-modules,) you can simply import the Unlaunch SDK in your code and `go build` will automatically download it.
- If you are using dep, import the SDK packages in your code and run dep ensure.
- Or you can simply use `go get` command to:

```
go get github.com/unlaunch/go-sdk
```

## Initializing a New Unlaunch Client Instance

### SDK Architecture

Before we dive into the details, let's us understand at a high-level how the Unlaunch Go SDK works.

1. You initialize the client using one of your project's [SDK keys](sdk-keys). The *SDK key* uniquely identifies the environment within the project and all feature flags in it.
2. When you build the client, it starts a *background task* to download all active feature flags and configuration data and stores them in memory. This process can take a few seconds depending on the size of the data. We'll discuss this process in detail later.
3. You can wait for the initialization to complete using the [`AwaitUntilReady`](TODO) method. You must pass a timeout value as an argument so it doesn't block your application forever.
3. After the initialization is complete, you can evaluate feature flags using the [`Variation`](https://pkg.go.dev/github.com/unlaunch/go-sdk/unlaunchio/client#UnlaunchClient.Variation) method.

### Initialization

To initialize the client, you'll need an SDK key. SDK Keys are available on the **[Settings](https://app.unlaunch.io/settings)** page under the **Projects** tab. There are several different types of [SDK keys](sdk-keys) for each environment. Make sure you copy the *Server Key* for this SDK. Once you have the SDK Key, you can initialize a new client as following:

```go
config := client.DefaultConfig()
factory, _ := client.NewUnlaunchClientFactory("YOUR_SERVER_KEY", config)

unlaunchClient := factory.Client() // create a new Unlaunch Client

// Wait until all data is downloaded

if err = unlaunchClient.BlockUntilReady(3 * time.Second); err != nil {
    fmt.Printf("Unlaunch Client wasn't ready %s\n", err)
}
```

{% include alert-note.html type="danger" content="For performance reasons, we strongly recommend initializing the Unlaunch client as a SINGLETON and reusing it throughout your application and go routines." %}

##### Why should an Unlaunch Client be a Singleton?

When you build an Unlaunch client, it starts a background task to download data and store it in an in memory data strucuture (e.g. Map). This process might take some time depending on the size of the data that needs to be transferred. For performance reasons, it is extremely poor practice to initialize a **new** client *per* incoming request. It will increase response times and hurt system performance. You may also get rate-limited and throttled by Unlaunch servers. 

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

### Evaluating Feature Flags & Getting Variations

The Unlaunch Go SDK provides a few different ways to evaluate feature flags and to get variations, associated configuration and evaluation details. 

##### `Variation(flagKey, identity, attributes)`

This method evaluates and returns the variation (variation key) for this feature flag that you have defined in the [Unlaunch Console](app.unlaunch.io).

This method returns one of the variations according to *targeting or rollout rules* that you may have defined. It will *never panic* nor will it ever return empty string. Instead, it will return `control` if there are any errors such as:

- The flag was not found.
- There was an eror evaluating the feature flag.
- The flag was archived.

```go
variation := unlaunchClient.Variation("new-login-ui", "user123@gmail.com", nil)
```

Here, we pass `nil` for the last argument, which represents `attributes`. When your feature flag depends on targeting rules based on user attributes such as `userRegistrationDate`, you must pass in the attributes, as we'll see later.

{% include alert-note.html type="info" content="The getVariation method will never throw an exception." %}

##### `Feature(flagKey, identity, attributes)`

This method is similar `Variation()` in the sense that it evaluates feature flag and returns its result. However, instead of returning the variation as a `string`, it returns a `struct` that contains more information, such as:

- Configuration attached to variation
- Evaluation reason

This is mostly used for fetching *dynamic configuration* associated with the feature flag or for getting the *evaluation reason*. 

For example, say you want to change the color of a button for some users. You'd define the colors for each variation in the Unlaunch Console as a key-value pair. Then in your application, you can fetch the configuration using this method as shown below. The configuration is returned as `map[string]string`.

```csharp
feature = unlaunchClient.Feature("new_login_ui", "user123@gmail.com", nil);
fmt.Println(fmt.Sprintf("%v", feature.VariationConfiguration)) // print configuration
```

<div class="justify-content-center">
    <img src="/assets/img/feature_flag_config.png" alt="Dynamic Configuration in Unlaunch"/>
</div>



