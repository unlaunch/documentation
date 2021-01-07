---
title: Managing Flag Variations
description: This page will help you understand what flag variations are, how to create and manage them.
---

# Flag variations

## Overview

This page describes how to use a flag's **Variations** tab to add and alter your feature flag's variations.

To add or update variations, click on **‘Feature Flags’** in the right sidebar. Once the page loads, click on the **‘Variations’** tab.

<div class="justify-content-center">
    <img src="/assets/img/flag-variations.png" alt="Flag Variations"/>
</div> 

Variations define the return type when you evaluate the flag in your code using SDKs.

## Managing flag variations

In the flag's Variations tab, you can add or edit variations of existing flags.

<div class="justify-content-center">
    <img src="/assets/img/update-variation.png" alt="Add Flag Variation"/>
</div>

The Variations tab displays all variations of the selected flag.

## Default flag variations

When you create a feature flag, the last variation is set as a default variation. A flag's default variation is served when your feature flag is disabled.

For example, a  flag could have two variations, variation 1 is *on* and variation 2 is *off*. Then every time *off* variation is set as default variation.

<div class="justify-content-center">
    <img src="/assets/img/off-variation.png" alt="Default Variation"/>
</div> 

The default variation options are on the flag's **Targeting** tab.

Changing Default Variation

On updating default variation, it impacts only on the current environment. Other environments have the same standard default variations set at the time of feature flag creation.

## Default Rule

When you create a feature flag, the first variation defined is set as a default rule. A default rule is a variation that is served when your feature flag is enabled and no rule is matched with your flag's rule defined while evaluating.

The default rule can be set as *on*, *off* or some percentage of *on* and some percentage of *off* variation. It is referred to as **Percentage Rollout**

<div class="justify-content-center">
    <img src="/assets/img/default-rule.png" alt="Default Rule"/>
</div> 

The default rule options are on the flag's **Targeting** tab.

Changing the Default Rule

Default rule updating will impact only on the current environment. No change reflects on other environments.
 

`Default variations and Default rule are designated automatically every time you create a feature flag. You can use the default variation and default rule or change them. When you change, it will impact only on the selected environment of your project`.


