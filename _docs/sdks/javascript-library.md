---
title: Unlaunch JavaScript Library - Official Guide
description: This guide provides complete information about the Unlaunch JavaScript (Browser) Library
---

# Unlaunch JavaScript (Browser) Library

This guide provides complete information about the Unlaunch JavaScript SDK and how to integrate it in your (Browser) based applications to use feature flags.

The Unlaunch JavaScript SDK provides a JavaScript API to access Unlaunch feature flags within the browser and send metrics for analytics. The library is *open source*. SDK source code is available on <a href="https://github.com/unlaunch/javascript-sdk" rel="nofollow">GitHub <i class="fab fa-github fa-fw"></i></a> You can also checkout the Java [example project](https://github.com/unlaunch/javascript-sdk/blob/develop/example.html).

### Browser Support

The Unlaunch Javascript Library can be used in all major browsers. However, some browser may not support some features that the library uses, such as ES6 Promises. You may have to use polyfill if your target users use browsers that do not support ES6 Promise.

## Prerequisite

1. You'll need an Unlaunch account. Register for a free account at: [https://app.unlaunch.io/signup](https://app.unlaunch.io/signup)
2. You have created an Unlaunch feature flag and enabled it. You also know the [Browser / Public Key](sdk-keys). If you haven't already, please see our [Getting Started](../getting-started) tutorial.
3. (Optional) Understand the difference between [client-side and server-side SDKs](client-vs-server-side-sdks). Unlaunch JavaScript Library is client-side and optimized to be used by end-users such as embedded within your HTML pages that run in your users' browsers.

## Import the Library

To load the JavaScript Library, include the following in the `<head>` or `<body>` tag of your webpage.

```javascript
<script crossorigin="anonymous" src="https://unpkg.com/..."></script>
```

## Initialization

Once the library is imported, you'd have to initialize the client to create a new instance. You'd need the [Browser / Public Key](sdk-keys) for your Project & Environment to initialize the client. Browser / Public Keys are not secret and you can expose them in your code. Never use Server Key or Mobile / App Key in public code.

Here's a basic example showing how to initializw the client

```javascript
<script>
const flagKey = 'distribution-flag';

let flagKeys = [flagKey];
let identity = 'user123';

let attributes = {
    "country": "US"
};

var options = {
    bootstrap: 'localstorage',
     evaluationReason: true,
    offline: false,
}

let ulclient = ULClient.initialize('<BROWSER_PUBLIC_KEY>', flagKeys, identity, attributes, options);
</script>
```

After you initialize the client, it will call Unlaunch Service to initialize the flag (or flags) that you passed, get the result and store it. On average, it takes up anywhere from 100-250 milliseconds to initialize the client. We recommended initializing the client ahead of time and not when right you need it.

When the client is ready, it will emit `ready` event. When this event is emitted, you can start using the flag.

```javascript
<script>
ulclient.on('ready', function() {
let variation = ulclient.variation(flagKey);

if (variation === 'on') {
    // show feature
} else {
    // hide feature
}
</script>
```

## Configuration

You can use options to configure and customize the client.

...