---
title: Install SDK
description: Install SDK and evaluate flag using SDKs
---

## Install SDK

## Step 3: Copy SDK Keys

To use feature flags in your application, you'd need to integrate one of Unlaunch SDKs depending on which programming language your app is written in. In order to integrate SDKs with your project, you'd need to initialize it with an SDK key

Each Unlaunch environment has a unique set of SDK keys for security and performance reasons. You can read [more here](projects/sdk-keys). 

For this tutorial, we'll use the *Production* environment. Follow the steps in the diagram below to choose an SDK key.

<div class="justify-content-center border">
    <img src="/assets/img/sdk_keys.png" alt="Choose SDK keys"/>
</div>

## Step 4: Integrate Unlaunch SDK in Your Application

This step will depend on the type of SDK that you choose depending on the programming language you're using. In a nutshell, these are the steps you need to follow to start using feature flags in your applications.

1. Integrate the Unlaunch SDK in your project. This is usually done using a dependency manager like Maven, NPM, or Nuget.
2. Import the Unlaunch libraries in your project to initialize the client. The client is the primary way your project uses the Unlaunch SDK and communicates with Unlaunch servers. 
3. To initialize the client, you must provide it with an appropriate SDK/API key. The SDK key can be of three types: Server key, Mobile/App key or Browser/Public key. It uniquely identifies your project and environment and authorizes your project to use your feature flags.
4. Evaluate feature flags using the client to control which variations your users will see. Feature flags are unique identified by keys. You can also control targeting by providing user IDs and attributes and defining targeting rules based on user attributes. For example, show features only to registered users.

The first step is deciding whether you should use **[client-side or server-side SDK](sdks/client-vs-server-side-sdks)**. 

Follow SDK integration guides to integrate SDK in your application:

**Server-side SDKs:**
<<<<<<< HEAD

- [Java](sdks/java-sdk) (Also explained below)
- [Node.js](sdks/nodejs-sdk)
- [.NET](sdks/dotnet-sdk)
- [Go](sdks/go-sdk)

**Client-side SDKs:**

- [Javascript Library](sdks/javascript-library) 
- React
=======
- [Java](sdks/server-side-sdks/java-sdk) (Also explained below)
- [Node.js](sdks/server-side-sdks/nodejs-sdk)
- [.NET](sdks/server-side-sdks/dotnet-sdk)
- [Go](sdks/server-side-sdks/go-sdk)

**Client-side SDKs:**

- [Javascript Library](sdks/client-side-sdks/javascript-library) 
- [React SDK](sdks/client-side-sdks/react-sdk)
>>>>>>> aad6e569b6c4256196036aa0099126fbc0f89b25

### (Optional) Integration Instructions for the Java SDK

If you are using the Java SDK, here's how you can integrate it. This assumes that the feature flag you have created has the key `2fa-rollout`.

First add the dependency:

```xml
<dependency>
    <groupId>io.unlaunch.sdk</groupId>
    <artifactId>unlaunch-java-sdk</artifactId>
    <version>0.0.2</version>
</dependency>
```

Then, use the following code where you wish to use the feature flag:

```java
// initialize the client
UnlaunchClient client = UnlaunchClient.create(->"INSERT_YOUR_SDK_KEY_HERE"<-);

// wait for the client to be ready
try {
  client.awaitUntilReady(2, TimeUnit.SECONDS);
} catch (InterruptedException | TimeoutException e) {
  System.out.println("client wasn't ready " + e.getMessage());
}
// get variation
String variation = client.getVariation("2fa-rollout", "user-id-123");

// take action based on the returned variation
if (variation.equals("on")) {
    System.out.println("Variation is on");
} else if (variation.equals("off")) {
    System.out.println("Variation is off");
} else {
    System.out.println("control variation");
}

// shutdown the client to flush any events or metrics 
client.shutdown();
```
<hr>

## Step 5: Clean up

After you've finished with the feature flag that you created for this tutorial, you should clean up by archiving the flag. If you want to do more with this flag before you clean up, see Next steps.

**To clean up your flag:**

1. In the right sidebar, click on **Feature Flags**. In the list of instances, select the feature flag you created for this tutorial.
2. Click on the **Setting** tab.
3. Click on **Archive**.
4. Choose **Yes** when prompted for confirmation.

<hr>
