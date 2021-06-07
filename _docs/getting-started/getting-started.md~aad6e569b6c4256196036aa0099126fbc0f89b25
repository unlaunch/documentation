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

## Next Steps

After you create your feature flag, you might want to try some of the following:

- Learn how to add targeting rules to your feature flag.
- Learn about environments and how to create and edit them.
- Learn how to white user targeting to make it easy for QA and SDETs to test your flags.