---
title: Targeting Rules
description: This page will help you understand what how to use targeting rules to control which variations your users see.
---

# Flag Targeting

## Overview

This page describes how to utilize **targeting rules** to control [variations](flagvariations) you want to serve to your users. 

It is **important** to note that that targeting rules only apply to [enabled](enable-disable-flags) feature flags. A *disabled* feature flag will allow you to attach targeting rules, but it will always serve the **Default Variation**.   

## Targeting Users by Id

Using Unlaunch, you can target users by their *IDs*. User Ids are passed as `identity` field in SDKs e.g. see [getVariation() in Java](../sdks/java-sdk#evaluating-feature-flags--getting-variations). To target users, click on **Feature Flags** in the sidebar. Then click on your feature flag. You can specify users IDs you want to target under **Targeting** tab, in the **Target Users** section.

In the screenshoot below, we have specified that user IDs "123", "345", and "1234" will get the "on" variation. Targeting users by IDs take *precedence* over all other rules and if there's a match, other rules will skipped. If there isn't a match, the evaluation continues until it finds another matching rule.

<div class="justify-content-center border">
    <img src="/assets/img/target-user.png" alt="Target Users"/>
</div> 

We recommend keeping the target list of users to a *small* number. Such as your team, QA engineers or beta users. Targeting more than a few thousands users may cause performance degradation and your SDKs will take longer to initialize.

### Removing Targeted Users

You can also remove users from targeting. Follow the steps shown in the image below. 

<div class="justify-content-center">
    <img src="/assets/img/target-user-remove.png" alt="Remove Target Users"/>
</div> 

## Targeting Rules

Feature flags can have targeting rules, that allows you to control variation served on the basis of certain **[attributes](attributes)** and **conditions**.

Targeting Rules consists of three parts:

- an **attribute**. E.g. type of user, their registration date, whether they are registered or not. Attributes have types such as Boolean, String, and more.
- an **operator**, which evaluates the value you pass when evaluating feature flag with the value your provide below.
- a **value** to compare against value you'll pass at the time of evaluation.

Please see examples below.

### Attributes and Associated Operators 

Unlaunch supports different attribute type and their associated operators to perform operations on user values. To check out, see [**attribute types and their operators**](attributes-operators)

### Example 1: Email Ending With yahoo.com

Let's define a rule that serves `off` variation to all users whose e-mail address ends with `yahoo.com`. First, we'll create a [new attribute](attributes) of type **string** called `email`. Then we'll use the `ends with` operator, and provide `yahoo.com` as value. Click **Save Changes** to save the rule.

<div class="justify-content-center">
    <img src="/assets/img/target-rule-1.png" alt="Target Rule"/>
</div> 

### Example 2: Targeting by Countries

Now let's add another **condition** to our rule to only target users in "USA" and "Canada". You can add multiple conditions to a rule. For a rule to match, all its conditions must match. If any condition doesn't match, the rule will be skipped.

Let's create a new attribute to type **Set** called `country`. Add the condition as shown below. Now This rule will serve `off` variation to all users whose e-mail address ends with `yahoo.com` **AND** whose country is either `USA` or `CANADA`.

<div class="justify-content-center">
    <img src="/assets/img/target-rule-2.png" alt="Target Rule"/>
</div> 

When you have done setting up the conditions for your rule, you can decide whether a single variation is sent to the user on rule success or a percentage rollout of variations.


## Default Rule - If Nothing Else Matches

If none of the rules matches on the *Targeting* screen including Targeting Users and Targeting Rules then Default Rule is served to the user. As with other rules, the default rule can be served as a single variation or a percentage rollout to a user.

<div class="justify-content-center">
    <img src="/assets/img/default-rule-pr.png" alt="Default Rule PR"/>
</div> 

## Default Variation - When Flag is Disabled

When the feature flag is disabled, Unlaunch will serve the *Default Variation* for your feature flag. When a feature flag is created, the last variation is set as the off variation by default. You can change the off variation for your feature flag.

To customize the off variation, click on **Feature Flags**. Once the page loads, click on the **Targeting** tab. Off Variation is at the bottom of the screen.

<div class="justify-content-center">
    <img src="/assets/img/off-variation.png" alt="Off Variation"/>
</div> 

## Percentage Rollouts

So far in our examples, we have been serving a specific variation to users. Instead of serving specific variation, you can use "Percentage Rollout" to distribute users dynamically between variations. At the time of evaluation, SDKs will use **identity** parameter determine the bucket based on percentage.

<div class="justify-content-center">
    <img src="/assets/img/target-rule-3.png" alt="Percentage Rollout Target Rule"/>
</div> 

## Rules Evaluation Order
You can specify many targeting rules. The evaluation starts at the top and keeps going down until it finds a matching rule. If you specify multiple **Targeting Rules**, the first rule will be matched first, then second (if there is no match) and so on. This is the order of evaluation by rule types.

If the flag is **enabled**, the following rules will be evaluated in order until a match is found. If there is no match, then the **Default Rule** is served.
1. Target Users
2. Targeting Rule(s)
3. Default Rule

If the flag is **disabled**:
1. Default Variation is served always.

