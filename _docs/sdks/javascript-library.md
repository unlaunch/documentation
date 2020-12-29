---
title: Unlaunch JavaScript Library - Official Guide
description: This guide provides complete information about the Unlaunch JavaScript (Browser) Library
---

# Unlaunch JavaScript (Browser) Library

This guide provides complete information about the Unlaunch JavaScript SDK and how to integrate it in your (Browser) based applications to use feature flags.

The Unlaunch JavaScript SDK provides a JavaScript API to access Unlaunch feature flags within the browser and send metrics for analytics. The library is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/javascript-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also checkout the [example project](https://github.com/unlaunch/javascript-sdk/blob/develop/example.html).

### Browser Support

The Unlaunch Javascript Library can be used in all major browsers. However, some browser may not support some features that the library uses, such as ES6 Promises. You may have to use polyfill if your target users use browsers that do not support ES6 Promise.

## Prerequisite

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and enabled it. You also know the [Browser / Public Key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [client-side and server-side SDKs](client-vs-server-side-sdks). Unlaunch JavaScript Library is client-side and optimized to be used by end-users such as embedded within your HTML pages that run in your users' browsers.

## Import the Library

To load the JavaScript Library, include the following in the `<head>` or `<body>` tag of your webpage.

```html
<script crossorigin="anonymous" src="https://unpkg.com/..."></script>
```

## Initialization

Once the library is imported, you'd have to initialize the client to create a new instance. You'd need the [Browser / Public Key](sdk-keys) for your Project & Environment to initialize the client. Browser / Public Keys are not secret and you can expose them in your code. Never use Server Key or Mobile / App Key in public code.

Here's a basic example showing how to initialize the client. You'd have to wrap it in the `<script>` tag.

```javascript

// This is the flag (key) you want to fetch. You can fetch one or multiple flags
let flagKeys = ['new-bkg-img-flag']; 

// User identity 
let identity = 'user@yourdomain.com';

// (Optional) user attributes that are used in flag evaluation (targeting rules)
let attributes = {
    "country": "US"
};

// client configuration options
var options = {
    bootstrap: 'localstorage',
     evaluationReason: true,
    offline: false,
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

When the client is ready, it will emit `ready` event. When this event is emitted, you can start using the flag.

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

### Variation
TODO

### Evaluation Reason
TODO

### Fetching Configuration Attached to Variations
TODO

### Metrics and Impressions
Metrics and Impression events are sent automatically. These events are used for showing metrics and to generate data for the Insights Graph.

## Configuration

When initializing the client, you can use `options` to configure and customize the client.

```javascript
var options = {
     bootstrap: 'localstorage',
     evaluationReason: true,
     offline: false,
}
```

### bootstrap 
LocalStorage TODO

### evaluationReason 
evaluationReason TODO

### offline 
Feature flags start their journey on a developer's computer. A developer should be able to build and run their code locally even if they don't have network connectivity. To achieve this, Unlaunch JavaScript Library can be started in **offline mode**. When running in offline mode, the library will not connect to Unlaunch servers nor will it send any data to it. 

When activating offline mode, you don't need to pass in the API key. When you evaluate a feature flag in offline mode, it will always return the `control` variation. 

## User Identity
TODO
Describe anonymous mode 

## Attributes
TODO

