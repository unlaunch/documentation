---
title: Targeting Rules
description: This page will help you understand what how to use targeting rules to control which variations your users see.
---

# Flag Targeting

## Overview

This page describes how to utilize a flag's **Targeting** tab to control which users see a variation of a feature flag. Configuring user's identity as users ids, email addresses, or anything that can uniquely identify the user.

Targeting users will override any targeting rules for the user included in the allow-list of any variation of a feature flag. You can add a list of users in a certain variation, this list is referred to as allow-list.  

## Assigning Users To A Variation

Under the "Target users" section of the targeting, the tab permits you to assign individual users to a particular flag variation.

We recommend using only a small number of individual users in targeting the user. Targeting more than 10,000 users individually may cause performance degradation because the SDK takes longer to initialize when the targeting rules payload is large. 

In the below screenshot, we can see which users are seeing the *on* variation of the feature flag. This means *on* variation is served to users added in the allow-list.

<div class="justify-content-center">
    <img src="/assets/img/target-user.png" alt="Target Users"/>
</div> 

## Removing Targeted Users

Targeting users can be removed from the flag's targeting users that are not included in the user list any longer.

To remove targeted users, click on **Feature Flags**. Once the page loads, click on the **Targeting** tab. Then click the **x** next to their name to remove them.

<div class="justify-content-center">
    <img src="/assets/img/target-user-remove.png" alt="Remove Target Users"/>
</div> 

## Targeting Rules

Each flag can have targeting rules, that can specify which variation is served to which users. Target a specific group of users based on some attributes.

For example, "a new message package will only be used by 10% of users or for a specific region". This rule serves the appropriate users and shows them a "new package".

Targeting Rule consists of three parts:

- an **attribute**, which defines the scope of the flag's impact, such as only impacting an email address
- an **operator**, which evaluates the user value with the defined feature flag value.
- a **user value**, which identifies a user or resource by a value you specify, such as `yahoo.com`.  

### Attributes
---

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

