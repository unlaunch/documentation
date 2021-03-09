---
title: Getting Started
description: Getting started with Unlaunch
---

# Getting Started

In this tutorial, you'll learn how to create and use Unlaunch feature flags. 

Let's get started.

## Prerequisites

**Sign up for Unlaunch:**

To get started, you must sign up for Unlaunch. If you haven't done so already, please [visit our registration page](https://app.unlaunch.io/signup) to create a new account for free. 

To create a new account, you'll need a valid email address. To complete registration, we'll ask you to provide your company name and create your **first project**. Each projects gets two environments by default: *Production* and *Test*. For this tutorial, you will use the Production environment. To learn more about projects and environments, please see [this guide](features/projects-and-environments).

#### Main Concepts

Let's review a few important **concepts** quickly that will help you understand Unlaunch and this guide better.

Feature flags are akin to having `if/else` blocks in your code where you can change the condition on the fly without changing or deploying your code. The conditions are known as **variations**. A typical use of variation is to indicate whether the feature is **on** or **off**:

```javascript
let newLoginVariation = // fetch feature flag

if (newLoginVariation === 'on') {
  // show new login form
} else {
  // show old login form
}
```

Variations are for deciding which branch (if or else) to execute in your code. They are always of type `string`. Unlaunch feature flags have two variations by default but you can add more than 2 variations (multivariate flags.) 

You can also *attach* **dynamic configurations** to variations to change configuration on the fly such as UI button color, banner text, special offers, algorithm weights, and anything else. Dynamic configuration is not covered in this tutorial.

For more information on feature flags, please see this [blog post](https://blog.unlaunch.io/2020-08-01-feature-flags/).

<hr/>

## Step 1: Create New Feature Flag

In this step, you'll create your first feature flag. A feature flag can have many options including targeting rules, configuration, and more. We won't cover all possible options here. In this example, we'll create a simple feature flag that has *two* variations that you can control to show or hide a feature.

**To create a feature flag:**

1. Open the Unlaunch Console at [https://app.unlaunch.io](https://app.unlaunch.io/)
2. Switch to *Production* environment using the dropdown on the top-left corner. 
3. From the *Feature Flags* page, click on **Create Feature Flag**.
4. The **New Feature Flag** screen asks you to input values to create a new flag. In the **Name** field, type in the name that you'd like to call this flag e.g. `2fa-rollout`. Leave the rest of the fields to their default values and click on **Save.**
5. The feature flag is created immediately and you're taken to the **Flag Details** page. On this page, click **Enable Flag** to enable your flag in the current environment. On the popup asking you to confirm, click **Yes**.

<div class="d-flex justify-content-center mb-3">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/7ltRZNpKCzQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

At this time, you have created a feature flag with two *variations* : **on** and **off** and have **enabled** it. Because this flag is enabled and there are no targeting rules, it will serve variation specified under **Default Rule**, which is *on*. In other words, anyone who calls this flag will get the *on* variation. We can change this behavior, as we'll see later.

Your feature flag is now ready to use. In Step 2, we'll learn how to call Unlaunch to evaluate flags from your SDKs using one of Unlaunch SDKs.

<hr>

## Step 2: Copy SDK Keys

To use feature flags in your application, you'd need to integrate one of Unlaunch SDKs depending on which programming language your app is written in. In order to integrate SDKs with your project, you'd need to initialize it with an SDK key

Each Unlaunch environment has a unique set of SDK keys for security and performance reasons. You can read [more here](sdks/sdk-keys). 

For this tutorial, we'll use the *Production* environment. Follow the steps in the diagram below to choose an SDK key.

<div class="justify-content-center border">
    <img src="/assets/img/sdk_keys.png" alt="Choose SDK keys"/>
</div>

<hr>

## Step 3: Integrate Unlaunch SDK in Your Application

This step will depend on the type of SDK that you choose depending on the programming language you're using. In a nutshell, these are the steps you need to follow to start using feature flags in your applications.

1. Integrate the Unlaunch SDK in your project. This is usually done using a dependency manager like Maven, NPM, or Nuget.
2. Import the Unlaunch libraries in your project to initialize the client. The client is the primary way your project uses the Unlaunch SDK and communicates with Unlaunch servers. 
3. To initialize the client, you must provide it with an appropriate SDK/API key. The SDK key can be of three types: Server key, Mobile/App key or Browser/Public key. It uniquely identifies your project and environment and authorizes your project to use your feature flags.
4. Evaluate feature flags using the client to control which variations your users will see. Feature flags are unique identified by keys. You can also control targeting by providing user IDs and attributes and defining targeting rules based on user attributes. For example, show features only to registered users.

The first step is deciding whether you should use **[client-side or server-side SDK](sdks/client-vs-server-side-sdks)**. 

Follow SDK integration guides to integrate SDK in your application:

**Server-side SDKs:**

- [Java](sdks/java-sdk) (Also explained below)
- [Node.js](sdks/nodejs-sdk)
- [.NET](sdks/dotnet-sdk)
- [Go](sdks/go-sdk)

**Client-side SDKs:**

- [Javascript Library](sdks/javascript-library) 
- React

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

## Step 4: Clean up

After you've finished with the feature flag that you created for this tutorial, you should clean up by archiving the flag. If you want to do more with this flag before you clean up, see Next steps.

**To clean up your flag:**

1. In the right sidebar, click on **Feature Flags**. In the list of instances, select the feature flag you created for this tutorial.
2. Click on the **Setting** tab.
3. Click on **Archive**.
4. Choose **Yes** when prompted for confirmation.

<hr>

## Next Steps

After you create your feature flag, you might want to try some of the following:

- Learn how to add targeting rules to your feature flag.
- Learn about environments and how to create and edit them.
- Learn how to white user targeting to make it easy for QA and SDETs to test your flags.