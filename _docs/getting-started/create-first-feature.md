---
title: Create Feature Flags
description: Details about how to create feature flags
---

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