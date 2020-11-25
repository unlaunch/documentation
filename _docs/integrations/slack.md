---
title: Slack
description: Connect your Slack account with Unlaunch to stay up to date. Get immediately notified on changes to feature flags and more. 
---

# Slack

This tutorial explains how to connect Unlaunch to your Slack account to get real-time notifications as Slack messages on feature flag changes by your team. Using the Slack integration, you can get notified on Slack channel when you or someone on your team:

- Enables or disables a feature flag.
- Increase or decrease gradual rollout.
- Create a new feature flag.
- Archive a feature flag.

Please follow these steps to connect Unaunch to your Slack account.

### Step 1: Add 'Incoming WebHooks' App to Slack

Make sure you are signed into your Slack account. Open the Incoming Webhook in [Slack app directory](https://crovate.slack.com/apps/A0F7XDUAZ-incoming-webhooks).If you are a Slack Workspace Owner, you'll see "Add to Slack" button. Click it and move to Step #2. 

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/slack_add_app.png" alt="Add Incoming Webhooks by clicking the Add to Slack button" width="600"/>
        <figcaption class="figure-caption text-center">Add Incoming Webhooks by clicking the Add to Slack button</figcaption>
    </figure>
</div>

If you are **not an owner** of the Slack account, you'll see the **"Request to Install"** button instead. Click on this button and follow instructions to ask your Slack Workspace Owner to enable this app. Proceed to step #2 once the app is enabled.

### Step 2: Choose a Channel to Receive Notifications
Next, you'll select the channel where Unlaunch notifications will be displayed. After selecting an existing channel (or creating a new one), click "Add Incoming Webhooks Integration" and proceed to Step 3.

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/slack-configure-channel.png" alt="Choose a Slack channel to receive Unlaunch notifications" width="600"/>
        <figcaption class="figure-caption text-center">Choose a Slack channel to receive Unlaunch notifications</figcaption>
    </figure>
</div>

### Step 3: Copy the Webhook URL
In this step, you'll copy the Webhook URL. This is a unique URL that will allow Unlaunch to send notifications to this configuration. After you have copied the URL (by pressing ⌘+C or Ctrl+C), proceed to step 4.

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/slack-copy-webhook-url.png" alt="Copy Webhook URL which you'll need to provide in the Unlaunch Console" width="600"/>
        <figcaption class="figure-caption text-center">Copy Webhook URL which you'll need to provide in the Unlaunch Console</figcaption>
    </figure>
</div>

### Step 4: Open Unlaunch Console
Login to the Unlaunch Console by visiting [https://app.unlaunch.io/](https://app.unlaunch.io/). After you log in, click on 'Integrations' on the sidebar. In the page that opens, click on the "Connect" button where you see Slack listed.

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/slack-open-integrations.png" alt="Open Unlaunch Integrations and Choose Slack" width="600"/>
        <figcaption class="figure-caption text-center">Open Unlaunch Integrations and Choose Slack</figcaption>
    </figure>
</div>

### Step 5: Paste Webhook URL and Enable Slack Integration
This is the final step. First click on the "Enable" checkbox. In the text field, paste the webhook URL you copied from step #3. Then click the "Save Changes" button. You should receive a welcome message on Slack from Unlaunch confirming that the integration is successful. 

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/slack-complete-integration.png" alt="Paste Slack Webhook URL and Enable Integration" width="600"/>
        <figcaption class="figure-caption text-center">Paste Slack Webhook URL and Enable Integration</figcaption>
    </figure>
</div>

If you face any issues or have trouble connecting your Unlaunch account to Slack, please contact us at unlaunch@gmail.com.