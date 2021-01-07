---
title: Flag Enable/Diable
description: This page will help you understand how to enable or disable flag.
---

# Flag Enable/Disable

## Overview
This topic explains to you how to enable or disable flag. When you create a feature flag, it is disabled by default. 

Feature Flags screen has an indicator to show whether the flag is enabled or disable. Enable flags are shows by *Green* dot and disable flags are shows by *gray* dot beside the feature flag name. 

<div class="justify-content-center">
    <img src="/assets/img/enable-icon.png" alt="Enable Flag icon"/>
</div> 


#### Disable Feature Flag
By default, a feature flag is disabled on creation. When a flag is disabled, off variation ( default variation ) is served to all users. No evaluation is done based on targeting users and targeting rules.

To disable a feature flag, click on **Feature Flags** in the right sidebar. On page load, click on the **Targeting** tab.
Click on the **Disable Flag** button on this screen.

<div class="justify-content-center">
    <img src="/assets/img/disable-btn.png" alt="Disable Flag"/>
</div> 


#### Enable Feature Flag
To enable a feature flag, click on **Feature Flags** in the right sidebar. On page load, click on the **Targeting** tab.
Click on the **Enable Flag** button on this screen.

<div class="justify-content-center">
    <img src="/assets/img/enable-btn.png" alt="Enable Flag"/>
</div> 


```Flag enabling/disabling is depend on the selected environment. It only applies to the current environment. No change is reflected on the flag in other environments in a project.```

