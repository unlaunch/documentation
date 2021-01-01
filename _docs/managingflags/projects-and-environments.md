---
title: Projects & Environments
description: This page will help you understand Projects and Environments in Unlaunch.
---

# Projects & Environments

This page will help you understand Projects and Environments in Unlaunch and how to use them to manage feature flags, permissions and protect production flags from accidental changes.

## Projects

Unlaunch projects allow you to separate feature flags by applications or products. For example, you can create one project for your iOS mobile application called *MyMobileAppIPhone*, and another one for Android *MyMobileAppAndroid* and one for backend *MyMobileBackend*.

When you register a new account or Organization, you are asked to create your first project. Projects are cheap. We encourage creating a new project for each GitHub repository. You can also create multiple projects for each repository by teams. For example if you have a monolith, you could create multiple projects, for Frontend, Backend, Database, and Core teams.

Project Admins control who gets access to their projects. For more information, see [Access Control](access-control).

### Create, Edit & Delete Projects

You can create, edit and delete projects from the *Projects* tab in the [Settings](https://app.unlaunch.io/settings) page from your [Unlaunch Console](https://app.unlaunch.io/).

## Environments

Each Unlaunch project can have many environments to help you manage your product development and deployment life-cycle. 

By default, all projects get **two** environments when they are created: *Production* and *Test*. You can create more environments to map to your deployment life-cycle, for example,

- Production / Canary
- Stage
- QA
- Development

When you create a feature flag, it is created on all environments within that project. However, you can edit targeting rules, flag status, etc. separately for each environment. For example, during the early development cycle, you can enable the feature flag on the Development environment, while keeping it disabled on all other environments so the feature remains hidden from your users.

You can switch environments using the environments **dropdown** in the sidebar of [Unlaunch Console](https://app.unlaunch.io/). Each environment gets assigned a different color to help you easily distinguish and prevent mistakes when managing flags through Console.

<div class="justify-content-center">
    <img src="/assets/img/environment-switcher.png" alt="environment switcher dropdown" width="220"/>
</div>

### Create, Edit & Delete Environments

You can create, edit and delete environments from the *Projects* tab in the [Settings](https://app.unlaunch.io/settings) page from your [Unlaunch Console](https://app.unlaunch.io/).

## Organization, Project, Environments Hierarchy Diagram

Here's a diagram to show you how it all fits together.

<div class="justify-content-center">
    <img src="/assets/img/o-p-e-hierarchy.png" alt="Organization, Project, Environments hierarchy diagram" width="900"/>
</div>
