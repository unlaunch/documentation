---
title: Client-side and Server-side SDKs
description: High level overview of Unlaunch SDKs and concepts.
---

# Client-side and Server-side SDKs

We have provided several SDKs in several different languages to help you access feature flags from your applications. All of Unlaunch's SDKs are divided into two main categories, regardless of the programming language:

- Client-side SDKs
- Server-side SDKs

This page will help you understand the difference between Unlaunch's client-side and server-side SDKs.

#### Client-side SDKs

Client-side SDKs are designed to be used in applications that your users run directly on their own devices, such as mobile, desktop and web applications.

Client-side SDKs are optimized to be used by a single user and low-bandwidth consumption. Examples: JavaScript Library, Angular, and React SDKs.

#### Server-side SDKs

Server-side SDKs are designed to be used in server-side applications such as web servers and backend services that you run on your own servers.

Server-side SDKs are optimized to be used in multi-user and secure environments. Examples, Java, .NET, Go and Node.js SDKs.

## Differences

While there are many similarities between SDKs for the same programming language, there are some key functional differences between client-side and server-side SDKs for security and performance considerations.

### Security

Your security and security of your users is our main concern. We have built security into our SDKs and how they operate.

**Client-side**

Client-side SDKs are embedded in applications that you distribute to your users, e.g. mobile apps or React/Angular apps. As such, these are considered inherently unsafe as your users can intercept and inspect network traffic to see what feature flags you have created and view your targeting rules.

Client-side SDKs can only fetch feature flags by its key (in contrast to server-side SDKs that can download **all** the flags you have defined in a project.) This limits the blast radius and it won’t allow hackers to fetch other flags (because they won’t know its key.) 

To further increase security, you should initialize client-side SDKs using [Mobile/App or Browser/Public SDK keys](sdk-keys). Never use Server SDK keys in client-side applications.

**Server-side**

Server-side SDKs are embedded in applications that run on your servers such as web servers or backend servers. These are considered safe environments. Server-side SDKs download all feature flags that you have defined in a project and store them in memory. You must use [Server SDK keys](sdk-keys) to initialize an Unlaunch SDK in your server-side applications.

### Performance

Unlaunch is built for performance. We understand that adding even a millisecond of latency can have undesired effects and can lead to user churn.

**Client-side**

To improve performance, client-side SDKs download only as much data as is needed to evaluate a feature flag. In addition, you control when and how often a flag is downloaded. For example, in a webpage, you might want to get a feature flag when the page starts loading, or you can delay it until the user navigates to the appropriate section.

**Server-side**

To improve performance in multi-user environments, server-side SDKs download **all** feature flags you have defined for a project upon initialization. The initial sync will take some time to complete if you have many feature flags and dynamic configuration. Once the initial sync is complete, the flags are stored in memory. When a new request comes in from your users, the flag evaluation happens very quickly because the SDK already has all the information available in memory. Flags are periodically refreshed asynchronously and will only fetch changes. You can configure the polling interval when initializing the SDK.

# Summary

Unlaunch SDKs come in two main categories, client-side and server-side for security and performance considerations.