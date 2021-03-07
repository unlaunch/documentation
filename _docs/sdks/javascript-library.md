---
title: Unlaunch JavaScript Library - Official Guide
description: This guide provides complete information about the Unlaunch JavaScript (Browser) Library
---

# Unlaunch JavaScript (Browser) Library

This guide provides complete information about the Unlaunch JavaScript SDK and how to integrate it in your (Browser) based applications to use feature flags.

This is a [client-side SDK](client-vs-server-side-sdks). It supports using feature flags in Browser or be used in client-side rendered apps. In other words, it provides a JavaScript API to access Unlaunch feature flags and is optimized to be used by end-users such as embedded within your HTML pages that run in your users' browsers.

The JavaScript library is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/javascript-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also check out the [example project](https://github.com/unlaunch/javascript-sdk/blob/develop/example.html).

### Compatibility
The Unlaunch JavaScript library doesn't require or depend on any specific JavaScript framework. You can use it with your favorite framework like Angular. If you want to integrate with **React**, we have a separate React SDK available.

[Angular example on GitHub](https://github.com/unlaunch/hello-angular)

#### Browser Support
The Unlaunch Javascript Library can be used in all major browsers. However, some browsers may not support some features that the library uses, such as ES6 Promises. You may have to use polyfill if your target users use browsers that do not support ES6 Promise.

## Prerequisite

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. Create a new feature flag that you want to access in your HTML pages. If you are new to Unlaunch, please see our [Getting Started guide](../getting-started).
3. Make sure you enable **client-side access** for the feature flag. You can find this option in the 'Setting' tab when you open the feature flag in the Unlaunch Console.
4. Copy the [Browser / Public Key](sdk-keys). You can find this key for your project and environment by clicking the 'Settings' link on the sidebar. This is a public key and it is safe to embed it in your apps and distribute it to your users. You must **never** use *Server Key* or *Mobile / App Key* with this SDK. Those keys are supposed to be kept secret.

<div class="justify-content-center border">
    <img src="/assets/img/publickey.png" alt="Choose SDK keys"/>
</div>

## Import the Library

Depending on whether you want to integrate the JavaScript library in your HTML pages or the JavaScript app, you can use on the following:

### Embed directly in your HTML
To load the JavaScript Library, include the following in the `<head>` or `<body>` tag of your webpage.

```html
<script crossorigin="anonymous" src="https://unpkg.com/unlaunch-js-client-lib/dist/ulclient.min.js">
</script>
```

### Integrate with a JavaScript framework
Or using, `npm install`:

```
npm i unlaunch-js-client-lib
```

and then,

```javascript
import * as ULClient from "unlaunch-js-client-lib";
```

## Initialization

Once the library is imported, you'd have to initialize the client to create a new instance. You'd need the [Browser / Public Key](sdk-keys) for your Project & Environment to initialize the client. Browser / Public Keys are not secret and you can expose them in your code. Never use Server Key or Mobile / App Key in public code.

Here's a basic example showing how to initialize the client. You'd have to wrap it in the `<script>` tag.

```javascript
// This is the flag (key) you want to fetch. You can fetch one or multiple flags
let flagKeys = ['new-bkg-img-flag']; 

// User identity. You can also set this to 'anonymous'. 
// See 'Anonymous Users' section below.
let identity = 'user@yourdomain.com';

// (Optional) user attributes that are used in flag evaluation (targeting rules)
let attributes = {
    "country": "US"
};

// client configuration options
// See 'Client Configuration' section below
var options = {
    localStorage: true,
    evaluationReason: false,
    offline: false,
    requestTimeoutInMillis: 1000,
    logLevel: 'debug'   // Available logLevels are ['error','warn','info','debug','none'].
}

// initialize the client
let ulclient = ULClient.initialize(
    '<BROWSER_PUBLIC_KEY>', 
    flagKeys, 
    identity, 
    attributes, 
    options
);
```

After you initialize the client, it will call Unlaunch Service to initialize the flag (or flags) that you passed, get the result and store it. On average, it takes up anywhere from 100-250 milliseconds to initialize the client. We recommended initializing the client ahead of time and not when right you need it.

When the client is ready, it will emit a `ready` event. When this event is emitted, you can start using the flag.

```javascript
ulclient.on('ready', function() {
let variation = ulclient.variation('new-bkg-img-flag');

if (variation === 'on') {
    // show feature
} else {
    // hide feature
}
```

## Using the Library

### Fetching Feature Flag Variation
The `variation()` method will return the variation for the feature flag. It's method signature is:

```javascript
unlaunchclient.variation(flagKey)
```

It only takes the `flagKey` as an argument. It will return the variation for the flag. This method will never throw an error. If there's an error such as no internet connection or flag not found, it will return a special string value: `control`. 

### Fetching Configuration Attached to Variations
To get configuration (key-value properties) attached to variations. It's method signature is:

```javascript
unlaunchclient.variationConfiguration(flagKey)
```

It will return a JSON object containing your configuration as key-value pairs. If there's no configuration attached, it will return an empty object.


### Evaluation Reason
Evaluation reason is used for debugging purposes. It will tell you why a certain variation for a feature flag was returned. For example, it might tell you that "off" variation was returned because the flag was disabled or that "on" was returned because userId matched targeting rules. To get evaluation reason and status use `variationDetail(flag)` method. This will return a JSON object.

```javascript
{
      value:    result,
      status:   flagStatus,
      reason:   evaluationReason
}
```

For example,

```javascript
const details = ulclient.variationDetail("js-flag");
logger.log(details.reason)
```

### Metrics and Impressions
Metrics and Impression events are sent automatically. These events are used for showing metrics and to generate data for the Insights Graph.

## Client Configuration

When initializing the client, you can use `options` to configure and customize the client.

```javascript
var options = {
     localStorage: true,    // Use local storage to store results
     evaluationReason: false,
     offline: false,               // Run in offline mode. All calls to variation() will return control
     requestTimeoutInMillis: 1000  // HTTP timeout
}
```

Each option is described below.

#### localStorage 
When this option is enabled, the library will store evaluated feature flags in Browser's Local Storage after the first evaluation so that the result is immediately available the next time the user visits the page. Here's how Local Storage works:

1. Page is loaded and the Unlaunch client is initialized.
2. Check the Local Storage to see if we have a result already available for the given identity and attributes.
3. If the result is not available, call the backend service to evaluate and store the result in memory.
4. If the result is available, immediately fire the 'ready' event, marking the library as ready to use. Asynchronously, call the backend service to evaluate again and store the result in memory, if changed.

You can disable Local Storage although we don't recommend it. When you disable Local Storage, each time the page loads and the Unlaunch Client is initialized, it will call the backend service to evaluate the feature flag.

#### evaluationReason 
If you set this to false, the evaluation reason will not be available.

#### offline 
Feature flags start their journey on a developer's computer. A developer should be able to build and run their code locally even if they don't have network connectivity. To achieve this, Unlaunch JavaScript Library can be started in **offline mode**. When running in offline mode, the library will not connect to Unlaunch servers nor will it send any data to it. 

When activating offline mode, you don't need to pass in the API key. When you evaluate a feature flag in offline mode, it will always return the `control` variation. 

#### requestTimeoutInMillis
This specifies the timeout in milliseconds that the client waits for the Unlaunch servers to evaluate and return feature flag response. If the timeout is reached the the server hasn't responded, the `ready` event will fire and **control** will be returned by the `variation()` method


## Anonymous Users
When initializing the client, you provide the `identity` parameter. Set this to the User Id of your user, such as their email address. If the User Id is not available, set the value to `anonymous`. The library will internally generate and assign a unique identifier to the user and will keep it consistent across browser sessions.
To define user as anonymous set identity value to 'anonymous' or ''. The library will assign a unique id to user , and the id will remain constant across browser sessions

```javascript
let identity = 'anonymous' // generate a new UUID and save it across browser sessions

let ulclient = ULClient.initialize(
    '<BROWSER_PUBLIC_KEY>', 
    flagKeys, 
    identity, 
    attributes, 
    options
);
```

## Attributes
You can define attributes that are used in Targeting Rules to calculate feature flag variation. You can pass attributes as a simple key-value JSON object when initializing the client.

```javascript

let attributes = {

  // String attribute country
  country: "US",

  // Number attribute age
  age: 25,

  // Boolean attribute can be pass with either true or false value`
  premiumCustomer: true,

  // DateTime attribute must be pass in the format 'YYYY-MM-DDTHH:mm:ss.sssZ'
  startDateTime: "2021-01-15T15:00:00.001Z"
   
  // Date attribute must be pass in the format 'YYYY-MM-DDTHH:mm:ss.sssZ'`
  campaignDate: "2021-01-15T00:00:00.000Z",

  // Set type attribute is passed as array 
  tags: ['daily', 'weekly', 'monthly', 'yearly']

};

let ulclient = ULClient.initialize(
    '<BROWSER_PUBLIC_KEY>', 
    flagKeys, 
    identity, 
    attributes, 
    options
);
```

## Logging
By default, `unlaunch-js-client-lib` sends log output to the browser console. Available logLevels are: 'error','warn','info','debug' and 'none'. Default logLevel is 'error'. To disable logging use 'none'. If empty '' logLevel is provided all logs are printed. Log levels severity are in the order 'error','warn','info','debug'. Error has high severity and debug has lowest. All logs which have high severity level then the level provided in options will be printed. So if logLevel 'info' is provided then 'error', 'warn' and 'info' logs are printed. If logLevel is set to 'debug' in options then only debug logs are printed. 

## More Questions?

At Unlaunch, we are obsessed about making it easier for developers all over the world to release features safely and with confidence. If you have *any* questions or something isn't working as expected, please email **help@unlaunch.io**.
