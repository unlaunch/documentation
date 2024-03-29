---
title: Server Keys, Mobile / App Keys and Browser / Public Keys
description: This page will help you understand how Unlaunch uses different categories of SDK keys to provide an additional layer of security.
---

# SDK Keys

When you are integrating Unlaunch SDKs with your application, you'll need to provide an appropriate SDK key to initialize the Unlaunch SDK in your application to give it required permission to access feature flags in that environment. 

An SDK key uniquely identifies an Unlaunch [environment](../projects) within a project in which you have defined your feature flags.

**Each environment** in your project has a unique SDK key. A good practice is to keep the keys separate from your application configuration and supply them at run-time. You could do this by setting and reading the keys as environment variables, or have them supplied via [Vault](https://www.vaultproject.io/), [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) or whichever solution you use to manager secrets in your organization.

For enhanced security, *each environment* gets **three** different SDK keys belonging to one of the following categories:

- **Server Keys** - Can access all feature flags. Use only within applications that run on your own Cloud or servers.
- **Mobile / App Keys** - Can access all feature flags (at this time.) Use this key in mobile applications such as Android or iOS using Unlaunch SDKs.
- **Browser / Public Keys** - Very limited access. This key is considered public and you can embed in client-side SDKs like React or Angular.

#### Where to find Unlaunch SDK Keys?

1. Open the Unlaunch Console at [https://app.unlaunch.io](https://app.unlaunch.io/)
2. On the right sidebar, click on **Settings**
3. In the *Projects* tab, you should see all your projects and SDK keys for each environment.

<div class="justify-content-center border">
    <img src="/assets/img/sdk_keys.png" alt="Choose SDK keys"/>
</div>

### Server Keys

Server keys allow **read-only** access to **all** feature flags, attributes and configurations. These are designed to be used for internal applications that you run on your own servers or Cloud. Examples: web servers, backend services, etc. 

Server keys are sensitive and should be kept **secret**. You must never share it with your users. If you suspect that the Server key has been compromised, you can reset it using the **[Settings](https://app.unlaunch.io/settings)** page.

### Mobile / App Keys

Mobile / App keys allow **read-only** access to **all** feature flags, attributes and configurations, and are identical to SDK keys. These are designed to be used for mobile applications such as Android and iPhone, or desktop applications that run on your client machine.

Mobile keys are considered sensitive and should be kept secret. 

{% include alert-note.html type="info" content="While there is no difference between SDK and Mobile / App keys at this time, we might limit access in future." %}

### Browser / Public Keys

Browser / Public keys allow **read-only** access only to feature flags that are marked for client-side access. All other flags are not visible to applications using this key. These are intended to be used in client-side applications such as React or Angular apps.

You can enable client-side access for a feature flag by clicking the "Make this flag available to public clients" check when you are creating it from the Settings tab of your feature flag.

Browser keys are **public** and you don't need to keep them secret from your users and hence, they can be freely shared.

# Summary

Each *environment* in an Unlaunch *project* has three different SDK keys. You must use the right type of key when integrating Unlaunch SDKs in your applications. Here's a table to better help you understand which SDK key to use.


| SDK          | Key Type             | Examples                                  |
|--------------|----------------------|-------------------------------------------|
| Java         | Server Key           | SpringBoot Backend Service, etc.          |
| Java         | Mobile / App Key     | iPhone apps, Desktop applications, etc.   |
| Javascript   | Browser / Public Key | Javascript code that runs in the browser. |
| React        | Browser / Public Key |                                           |
| Angular      | Browser / Public Key |                                           |
| Node.js      | Server Key           | Express.js based servers, gateways, etc.  |
| Node.js      | Browser / Public Key | Client side based Node.js applications    |
| Android      | Mobile / App Key     |                                           |
| iOS          | Mobile / App Key     |                                           |
| React Native | Mobile / App Key     |                                           |