---
title: Access Control
description: An overview of team permissions on Unlaunch
---

# Overview

Unlaunch has a built-in comprehensive and powerful roles based access control system. This allows you to control and grant granular access to Unlaunch projects to the members of your team.

## Organization and Project Based Roles

Permissions are divided into two groups:

- Global Permissions (Organization Level): Applies to everything under the organization. A new organizations is created when you first create a new Unlaunch account.
- Project Permissions (Project Level): Applies to everything under the project such as feature flags, attributes, etc.

### Global Permissions

Global permissions apply to everything under an Unlaunch Organization. This includes all projects, settings, all users, etc. Unlaunch currently support two Organization-level roles: Owner and Admin. Except these, everyone who joins the organization is considered a member.

- **Organization Owner**: This is the super user. Typically the first account that created the organization. There can be only ONE owner. Ownership can be transferred from one account to another. An Organization Owner cannot be assigned another role such as Organization Admin, Project Admin, etc.

- **Organization Admins**: There can be many Organization Admins. They can essentially do everything that Organization Owner can do except to change Organization settings or delete the Organization. Note: This role is currently not supported through the Unlaunch Console. Please contact us if you need to assign this role.

- **Organization Members**: All new members who join the organization and aren't assigned 'Organization Owner' or 'Organization Admins' roles, will get assigned as 'Organization Member'.

### Project Permissions

Project permissions apply to everything under a Project (All Feature Flags, Attributes, etc.) Typically, these permissions are assigned to individual teams who own the project and team leads manage and control their projects.

The following roles are supported for projects:

- **Project Admin**: Full access to do anything under the Project except the ability to delete the project itself. Can add or remove members to the project.

- **Production Access**: Can create and launch feature flags in Production and all the other environments.

- **Non-Production Access**: Restricted access to production. This role can create feature flags, but can’t launch them in production. They also cannot enable or disable feature flags in production. They can edit feature flags in **non-production** [environments](../projects/projectsandenvs) (that is, any environment that is not production.) For example, you can use this role to give access to your Unlaunch projects to new users who are onboarding or to QA engineers. This allows them to try feature flags, but restrict them from making any changes to production environment. In other words, users who are assigned this role can see everything in production environment but cannot change anything.

- **Reader**: Read-only access. They can view feature flags, their targeting rules, settings, but aren't allowed to modify anything. They can also access Live Tail, Debugger, Project Settings, and Activity Log.

Here's a table to help you understand what each role has access to.

<div class="justify-content-center">
    <img src="/assets/img/project-permissions.png" alt="project permissions table"/>
</div>

_*1_ = Can create in [non-production environments](../projects/projectsandenvs) only; The flag will be created in all environments but will be in disabled state.

_*2_ = Can edit flags in pre-production environments ONLY: targeting rules, dynamic configuration. For any flag that is active in production, can’t edit anything that affects the flag in production such as adding more variations. Can’t Enable/Disable flag in production environment, also cannot change Targeting Rules or anything in production.

_*3_ = Because flags are deleted from all environments, pre-production access can’t delete flags.