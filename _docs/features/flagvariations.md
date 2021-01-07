---
title: Managing Flag Variations
description: This page will help you understand what flag variations are, how to create and manage them.
---

# Flag Variations

This page describes how to use a flag **Variations** tab to add and alter your feature flag's variations.

## What are Variations?

A feature flag can have two or more variations. Variations determine the action to take or the path to choose in your applications. For example, suppose you create a new feature flag with two *variations*: *on* and *off*. In your application, you'd define the logic to handle both these variations e.g. when the variation is *on*, show the feature. Otherwise, don't show the feature. When you **evaluate** a feature flag using one of Unlaunch SDKs, it returns a *variation* based on the evaluation logic and input (user identity and attributes.)

## Managing Feature Flag Variations
To add or update variations, click on **Feature Flags** in the right sidebar. Once the page loads, click on the **Variations** tab. This tab displays all variations of the selected flag. You can add or edit variations of existing flags. To Add new variations, click on the "Add Variation" button at the bottom.

<div class="justify-content-center">
    <img src="/assets/img/update-variation.png" alt="Add Flag Variation"/>
</div>

### Default Variation

When you create a feature flag, the last variation is set as a default variation. A flag's default variation is served when your feature flag is [disabled](enable-disable-flags).

For example, you create a flag with two variations: variation 1 is *on* and variation 2 is *off*. Then by default, variation 2 (*off*) will be set as the default variation. You can easily change the variation through the **Targeting** tab.

<div class="justify-content-center">
    <img src="/assets/img/off-variation.png" alt="Default Variation"/>
</div> 

### Default Rule

When you create a feature flag, the first variation defined is set as a default rule. A default rule is a variation that is served when your feature flag is [enabled]((enable-disable-flags)) and no other targeting rule is matches when evaluating the feature flag.

The default rule can be set as *on*, *off* or some percentage of *on* and some percentage of *off* variation. It is referred to as **Percentage Rollout**

<div class="justify-content-center">
    <img src="/assets/img/default-rule.png" alt="Default Rule"/>
</div> 

Please note that any changes to flag rules or variations after the flag has been created only applies to the current environment (for safety reasons.) You must make changes manually to all other environments.



