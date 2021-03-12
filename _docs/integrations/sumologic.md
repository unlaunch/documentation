---
title: SumoLogic
description: Connect your SumoLogic account with Unlaunch to stay up to date. Get immediately notified on changes to feature flags and more. 
---

# SumoLogic

This tutorial explains how to connect Unlaunch to your SumoLogic account to get real-time messages on feature flag changes by your team.

- Enables or disables a feature flag.
- Increase or decrease gradual rollout.
- Create a new feature flag.
- Archive a feature flag.

Please follow these steps to connect Unlaunch to your SumoLogic account.

### Step 1: Get receiving webhook from SumoLogic

We will get incoming webhook URL from SumoLogic for Unlauch to stream data to. 

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/wizard.png" alt="Click on Collection then Setup Wizard" width="600"/>
        <figcaption class="figure-caption text-center">Click on Collection then Setup Wizard</figcaption>
    </figure>
</div>

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/stream.png" alt="Click on Start streaming data to SumoLogic" width="600"/>
        <figcaption class="figure-caption text-center">Click on Start streaming data to SumoLogic</figcaption>
    </figure>
</div>

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/custom.png" alt="Now click on Your Custom App" width="600"/>
        <figcaption class="figure-caption text-center">Now click on Your Custom App</figcaption>
    </figure>
</div>

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/https.png" alt="Choose HTTPS source" width="600"/>
        <figcaption class="figure-caption text-center">Choose HTTPS source</figcaption>
    </figure>
</div>


<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/source.png" alt="Type 'Unlaunch' in Source Category and click Next" width="600"/>
        <figcaption class="figure-caption text-center">Type 'Unlaunch' in Source Category and click Next</figcaption>
    </figure>
</div>

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/copy.png" alt="Copy your webhook and click Next" width="600"/>
        <figcaption class="figure-caption text-center">Copy your webhook and click Next</figcaption>
    </figure>
</div>

### Step 2: Open Unlaunch Console
Login to the Unlaunch Console by visiting [https://app.unlaunch.io/](https://app.unlaunch.io/). After you log in, click on 'Integrations' on the sidebar. In the page that opens, click on the "Click to Connect" button where you see Sumologic listed.

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/integration.png" alt="Open Unlaunch Integrations and choose SumoLogic" width="600"/>
        <figcaption class="figure-caption text-center">Open Unlaunch Integrations and choose SumoLogic</figcaption>
    </figure>
</div>

### Step 5: Paste Webhook URL and Enable SumoLogic Integration
This is the final step. First click on the "Enable" checkbox. In the text field, paste the webhook URL you copied from step #1. Then click the "Save Changes" button. 

<div class="d-flex justify-content-center">
    <figure class="figure">
        <img class="figure-img border img-fluid rounded" src="/assets/img/sumo/save.png" alt="Paste SumoLogic Webhook URL and Enable Integration" width="600"/>
        <figcaption class="figure-caption text-center">Paste SumoLogic Webhook URL and Enable Integration</figcaption>
    </figure>
</div>

If you face any issues or have trouble connecting your Unlaunch account to SumoLogic, please contact us at help@unlaunch.io.
