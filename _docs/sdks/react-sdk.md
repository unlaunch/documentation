
---
title: Unlaunch React.js SDK - Official Guide
description: This guide provides complete information about the Unlaunch React.js SDK
---
# Unlaunch React.js SDK

This guide provides complete information about the Unlaunch React.js SDK and documents all of the methods available in our client-side React SDK. This guide help to understand how to use its functions and how to integrate it in your application to call feature flags.

{% include alert-note.html type="info" content="The Unlaunch React.js SDK uses Javascript client side SDK under the hood." %}

{% include alert-note.html type="warning" content="The Unlaunch React.js SDK requires React 16.3.0 or later. This SDK uses the Context API, which requires React version 16.3.0 or later." %}

## Import the SDK Library

The first step is to import the Unlaunch SDK as npm dependency in your application. 

For Npm, 
```xml
npm install --save unlaunch-react-sdk
```
## Initializing client

After you install the dependency, initialize the React SDK. For this, you need your project's client-side ID, which is available in Unlaunch's Settings page. Client-side IDs are public so it can be exposed in your client-side code with no risk.

React SDK can be initializes in two ways:
 1. asyncWithUnlaunchProvider
 2. withUnlaunchProvider

These two methods rely on React's Context API so it would be easy to access flags from any level of component without passing flags as props. Both functions accept a ProviderConfig object used to configure the React SDK.



