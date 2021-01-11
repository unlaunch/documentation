---
title: Targeting Rules
description: This page will help you understand what how to use targeting rules to control which variations your users see.
---

# Flag Targeting

## Overview

This page describes how to utilize **targeting rules** to control [variations](flagvariations) to want to serve to your users. 

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

### Attributes

**String**
Unlaunch support String type. The following operators are supported for String:

| Operators | Meanings |
|--|--|
|Equals, Not Equal | Exact Match|
|Starts With, Does not Starts With | Prefix String Match|
|Ends With, Does not Ends With | Suffix String Match|
|Contains, Does not Contains | Value match in list|

**Number**
Unlaunch support Number type. The following operators are supported for Number:

| Operators | Meanings |
|--|--|
|Equals, Not Equal | Exact Match|
|Greater Than | Numeric comparison |
|Greater Than or Equal | Numeric comparison |
|Less Than | Numeric comparison |
|Less Than or Equal | Numeric comparison |

**Boolean**
Unlaunch support Boolean type. The following operators are supported for Boolean:

| Operators | Meanings |
|--|--|
|Equals, Not Equal | Exact Match|

**DateTime**
Unlaunch support DateTime type. The following operators are supported for DateTime:

| Operators | Meanings |
|--|--|
|Equals, Not Equal | Exact Match|
|Greater Than | date comparison |
|Greater Than or Equal | date comparison |
|Less Than | date comparison |
|Less Than or Equal | date comparison |

The time format is in UTC

Dates comparison in UNIX milliseconds ( epoch ). To learn more about UNIX date formatting, read [Current Millis](https://currentmillis.com/).

**Set**
Unlaunch support Set type. The following operators are supported for Set:

| Operators | Meanings |
|--|--|
|Equals, Not Equal | Exact Match|
|Is Part of, Is not Part of | Subset of a set |
|Has Any of, Does not Have Any of | Matches any set value|
|Has All of, Does not Have All of | Matches set |

## Targeting Rules Based On User Attributes

Another way to create targeting users can be constructed by creating targeting rules.

For example, let's define a rule that serves `off` to all users whose e-mail address ends with `yahoo.com`.

To write this rule, we specify the attribute as `email`, the operator as `ends with`, and the user value as `yahoo.com`. Remember to click **Save Changes** to apply the rule.

<div class="justify-content-center">
    <img src="/assets/img/target-rule-1.png" alt="Target Rule"/>
</div> 

## Targeting users based on custom attributes

To create custom attributes, please read [attributes](https://docs.unlaunch.io/docs/features/attributes)

You can add multiple conditions to a rule. Users must meet all the conditions in a rule to match the rule. If any of the conditions are not met, the user will not match the rule.

Here, we've created a custom rule with two conditions. This rule serves `off` to all users whose e-mail address ends with `yahoo.com` and whose country is either `USA` or `CANADA`.

<div class="justify-content-center">
    <img src="/assets/img/target-rule-2.png" alt="Target Rule"/>
</div> 

When you have done setting up the conditions for your rule, you can decide whether a single variation is sent to the user on rule success or a percentage rollout of variations.

## Percentage Rollouts

You can use percentage rollout, set percentages for each variation then appropriate variations are served.

<div class="justify-content-center">
    <img src="/assets/img/target-rule-3.png" alt="Percentage Rollout Target Rule"/>
</div> 

## Default rule: targeting remaining users

If none of the rules matches on the *Targeting* screen including Targeting Users and Targeting Rules then Default Rule is served to the user. As with other rules, the default rule can be served as a single variation or a percentage rollout to a user.

<div class="justify-content-center">
    <img src="/assets/img/default-rule-pr.png" alt="Default Rule PR"/>
</div> 

## Setting the Off variation

When the feature flag is disabled, Unlaunch will serve the *off Variation* for your feature flag. When a feature flag is created, the last variation is set as the off variation by default. You can change the off variation for your feature flag.

To customize the off variation, click on **Feature Flags**. Once the page loads, click on the **Targeting** tab. Off Variation is at the bottom of the screen.

<div class="justify-content-center">
    <img src="/assets/img/off-variation.png" alt="Off Variation"/>
</div> 

## Evaluation order
Flag Targeting evaluates rules as defined in the hierarchy in the targeting screen from top to bottom.

1. Targeting users
2. Targeting rules
3. Default rule
4. Off variation ( if the flag is disabled )

