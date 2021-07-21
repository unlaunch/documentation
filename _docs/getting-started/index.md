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

To create a new account, you'll need a valid email address. To complete registration, we'll ask you to provide your company name and create your **first project**. Each projects gets two environments by default: *Production* and *Test*. For this tutorial, you will use the Production environment. To learn more about projects and environments, please see [this guide](../projects).

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

Your feature flag is now ready to use. In Step 2, we'll learn how to create attributes and use rules to target specific segments of your users. 

<hr>

## Step 2: Use Attributes and Targeting Rules 

Targeting segments of users is a common use case. For example, you might want to show a feature only to users belonging to a certain geographic location such as South America. Unlaunch provides powerful mechanisms to target specific segment of your users using attributes and targeting rules. To learn more about attributes and targeting rules, [see this](../attributes/).

Let's create a targeting rule that returns the "on" variation for registered users. Everyone else gets the "off" variation. 

### Create New Attribute
1. Go to **Attributes** page using side navigation.
2. Click on **Create Attribute**.
3. Define name and select appropriate type for attribute. Hit save button.
4. Go back to feature flag targeting screen.

<div class="justify-content-center">
    <img src="/assets/img/attributes/create.png" alt="create a new attribute"/>
</div>

### Creating Rules Using Attributes
1. Click on the **Add Rules** button.
2. Select attribute along operator and fill in value for attribute.
3. Click **Save Changes**.
4. To get your feature evaluated through rule(s) you must enable targeting by clicking **Enable** button on top. If your flag is not enabled you will be served default variation.

<div class="justify-content-center">
    <img src="/assets/img/attributes/target.png" alt="target users by registered attribute"/>
</div>
