---
title:  "Can I create a feature flag in just one environment only e.g. Test but not in Production?"
---

When you create a new feature flag, it becomes available for all environments of the project. Flags are created in **disabled** state. You can enable flag only for specific environments, leaving it disabled for all others. Targeting rules are also environment specific. 



For example, say you are adding a new feature flag. Because you are still actively developing, you want to work in **Test** environment and don't want to bring your work to the **Production** environment. You can safely create flag in the Test environment. While the flag will be visible in Production (because we need to reserve flag key across all environments), it will be in disabled state. Any changes you make to flag in **Test** environment will not be reflected in the Production environment. 

So you can safely test your changes in Test environment, and enable the flag in the Production environment only when you are ready.