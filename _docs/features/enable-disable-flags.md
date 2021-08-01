---
title: Flag Enable/Diable
description: This page will help you understand how to enable or disable flag.
---

# Enable or Disable Feature Flags

This topic explains to you how to enable or disable a feature flag. A flag's state can be changed anytime, if you have the right permission for the environment you're changing on. By default, all new flags are created in disabled state. You must enable them manually when you are ready to use newly created feature flags in your application. 

Feature flag state changes are only applicable to the current [environment](../projects/projectsandenvs). For example, you can enable a feature flag on 'Test' environment, while leaving it disabled on the 'Production' environment.

To know if a feature flag is enabled or disabled, we have provided an indicator. Enabled flags have a *Green* dot (icon) beside their name and disabled flags have a *Red* icon. 

<div class="justify-content-center">
    <img src="/assets/img/enable-icon.png" alt="Enable Flag icon"/>
</div> 

## Enabling Feature Flags
To enable a feature flag, click on **Feature Flags** in the right sidebar. On feature flag details page, click on the **Targeting** tab.
Then click on the **Enable Flag** button on this screen and confirm that you want to enable the flag.

<div class="justify-content-center">
    <img src="/assets/img/enable-btn.png" alt="Enable Flag"/>
</div> 

When a feature flag is enabled, it will start applying the 'Target Users', 'Targeting Rules' and 'Default Rule'.

{% include alert-note.html type="info" content="When you create a new feature flag, it will be in disabled state. When you are ready to use it, you must manually enable it." %}


## Disabling Feature Flags
By default, a feature flag is disabled on creation. When a flag is disabled, off variation ( default variation ) is served to all users. No evaluation is done based on targeting users and targeting rules.

To disable a feature flag, click on **Feature Flags** in the right sidebar. On feature flag details page, click on the **Targeting** tab.
Click on the **Disable Flag** button on this screen.

<div class="justify-content-center">
    <img src="/assets/img/disable-btn.png" alt="Disable Flag"/>
</div> 

When a feature flag is disabled, it will serve the 'Off Variation' to all users. **No** evaluation will be performed based on 'Target Users', 'Targeting Rule' and 'Default Rule'.
