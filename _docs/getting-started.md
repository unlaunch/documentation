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

To create a new account, you'll need a valid email address. To complete registration, we'll ask you to provide your company name and create your **first project**.

<hr>

## Step 1: Create New Feature Flag

In this step, you'll create your first feature flag. A feature flag can have many options including targeting rules, configuration, and more. We won't cover all possible options here.

**To create a feature flag:**

1. Open the Unlanch Console at [https://app.unlaunch.io](https://app.unlaunch.io/)
2. From the *Feature Flags* page, click on **Create Feature Flag**.
3. The **New Feature Flag** screen asks you to input values to create a new flag. In the **Name** field, type in the name that you'd like to call this flag e.g. `2fa-rollout`. Leave rest of the fields to their default values and click on **Save.**
4. The feature flag is created immediately and you're taken to **Flag Details** page. On this page, click **Enable Flag** to enable your flag in the current environment. On the popup asking you to confirm, click **Yes**.

<div class="d-flex justify-content-center mb-3">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/7ltRZNpKCzQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

Your feature flag is now ready to use. In Step 2, we'll learn how to call Unlaunch to evaluate flags from your SDKs using one of Unlaunch SDKs.

<hr>

## Step 2: Call Your Feature Flag From Your Application

To use the feature flag you've created from your application, you'd need to integrate one of Unlaunch SDKs depending on which programming language your app is written in. 

**Prerequisite:**

1. Decide whether you should use **[client-side or server-side SDK](sdks/client-vs-server-side-sdks)**. Choosing the right type of SDK is important for security and performance reasons.
2. Find the **SDK key** to use. You can find SDK keys by clicking **Settings** in the right sidebar. More Info: to intialize any Unlaunch SDK, you must pass it an SDK key. All environments within projects have unique SDKs keys. Depending on your use case, you'll have to choose between Server, Mobile/App or Browser/Public keys. See this post which describes the [different categories](sdks/sdk-keys) and when you use which key.

**Integrate Unlaunch SDK in your application:**

Follow SDK integration guides to integrate SDK in your application:

Server-side SDKs:

- [Java](sdks/java-sdk)
- Node.js (Server-side)

Client-side SDKs:

- Javascript 
- React

<hr>

## Step 3: Clean up

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