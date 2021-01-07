---
title: Flag Archiving/Deleting
description: This page will help you understand how to archive and delete flag
---

# Archiving and Deleting Feature Flags
This topic explains how to archive and delete a feature flag that is no longer required in your project. Unlaunch Console lets you archive and delete feature flags that are not needed any more. 

[Enabled flags](enable-disable-flags) can not be archived. You must disable a feature flag before archiving it. 

As a good practice, we recommend archiving feature flags that are not in use anymore or have been rolled out at a 100%. This reduces the amount of data that is sent back to your server-side SDKs on initial sync. 

## Difference Between Archiving and Deleting
Archiving recycles the flag and hides it from your users and SDKs. It doesn't permanently delete the flag and you have the option to Un-archive it later. Delete on the other hand will permanently delete feature flag and its associated data. It is an irreversible operation. We have provided this distinction to encourage the practice of safely archived feature flags that are not in use, without worrying about losing anything permanently. One thing to note is that you **cannot** re-use the *flag key* of an archived flag when creating a new flag. If you want to re-use the same flag key, you must delete the old flag first.

## Archiving Feature Flags
To archive a flag, click on **Feature Flags** in the right sidebar. Once the page loads, click on the **‘Settings’** tab. Click on the *Archive* button at the bottom of the screen.
  
<div class="justify-content-center">
    <img src="/assets/img/archive.png" alt="Archive Flag"/>
</div> 

### Viewing and Restoring Archive Feature Flags
To view archive flag, click on **‘Feature Flags’** in the right sidebar. Click **Archived** option on the top of the screen. It will show all the archived flags. 

<div class="justify-content-center">
    <img src="/assets/img/archived-flags.png" alt="View Archived Flags"/>
</div>

Note: We will add the option to restore archive flag in the near future.

## Deleting Feature Flags
Archived feature flags are deleted permanently from Unlaunch. You can create a new feature flag with the deleted flag key, it will not flagged as a duplicate. 

To delete a flag, click on **‘Feature Flags’** in the right sidebar. Click **Archived** option on the top of the screen, now click on *Delete* button besides feature flag name.

<div class="justify-content-center">
    <img src="/assets/img/delete-flag.png" alt="Delete Flags"/>
</div>

{% include alert-note.html type="danger" content="Delete operation permanently deletes the feature flag and is irreversible. Make sure it is what you want to do before deleting." %}