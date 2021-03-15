---
title: Debugger
description: The Debugger is useful in troubleshooting. It allows you to evaluate feature flags from the Unlaunch Console and see the evaluation reason so you can understand why certain variation is being returned.
---

# The Debugger

## Overview

This page explains how to use the ***Debugger*** feature in the Unlaunch app. The Debugger is useful in troubleshooting. It allows you to evaluate feature flags from Unlaunch Console and see the evaluation reason so you can understand why certain variation is served.

Feature Flags are evaluated using server-side and client-side [SDKs](https://docs.unlaunch.io/docs/sdks/). Unlaunch provides **Debugger** an easy way to troubleshoot your flag without using any code.

`It is important to note that Debugger is used just for troubleshooting. In such evaluation, no event is fired and nothing shows in insight. Events are fired using SDKs.   `

## Troubleshoot Using Unlaunch Console
 
From Unlaunch, go to **Debugger's** page. Select feature flag. use id or generate random id as an identity for rollout or bucketing, select SDK key and add attributes (if any).
  
 <div class="justify-content-center border">
    <img src="/assets/img/debugger.png" alt="The Debugger"/>
</div> 
 
### Examples

Few examples are shown below.

#### Disabled Flag

<div class="justify-content-center border">
    <img src="/assets/img/debugger-disable-flag.png" alt="The Debugger"/>
</div> 

#### Enabled Flag

<div class="justify-content-center border">
    <img src="/assets/img/debugger-enable-flag.png" alt="The Debugger"/>
</div> 

#### Flag Using Attribute Values

##### Example-1
Troubleshoot Flag by providing the same value of the attribute as defined in targeting rules.

<div class="justify-content-center border">
    <img src="/assets/img/debugger-rule-match.png" alt="The Debugger"/>
</div> 

##### Example-2
Troubleshoot Flag by providing unmatched value in the attribute.

<div class="justify-content-center border">
    <img src="/assets/img/debugger-rule-not-match.png" alt="The Debugger"/>
</div> 

