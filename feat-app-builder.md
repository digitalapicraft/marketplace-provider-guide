---
icon: wand-magic-sparkles
---

App Builder is a low-code surface where you assemble small demonstration Apps on top of the APIs in your catalogue. Use it to prove an integration pattern, give a partner a runnable starter, or seed a Workspace that consumers can clone for their own Apps. It is a tool for demos and prototyping, not for production Apps: it runs against your published catalogue to show what an integration looks like.

![Figure. The App Builder landing page with the API Catalog summary, template tiles, and the assistant prompt.](.gitbook/assets/screenshots/provider/app-builder.png)

## What you work with

The landing surface gives you several starting points and one assistant:

- **API Catalog panel**: a summary of the APIs and endpoints the builder can call, with live counts. Click **Sync** to refresh the inventory after you publish a new API.
- **TEMPLATES**: pre-built starter Apps, such as **Customer Support System**, that you clone as a basis for a new build. Each template packages an API selection, a UI layout, and routing wiring.
- **Shared Workspaces**: workspaces shared with you or your team, so you can keep working on a build someone else started.
- **Workspaces / + New**: your own workspaces. Click **+ New** to start a blank one.
- **Prompt box**: type what you want to build, pick a model, and the assistant drafts a starting App you then refine.
- **Discover / Ideate / Build / Visualize phases**: the four phases along the bottom of the prompt area that you move through as you turn an idea into a runnable demo App.

## Configure

1. From the left sidebar under **API MANAGEMENT**, click **App Builder**.
2. Review the **API Catalog** panel on the left, which summarises the APIs and endpoints the builder can call. Click **Sync** to refresh the inventory after you publish a new API.
3. To start from a starter App, pick a tile from the **TEMPLATES** list. Each template packages an API selection, a UI layout, and routing wiring.
4. To start blank, choose **Workspaces** > **+ New**, or open a shared workspace someone else began.
5. Type your goal into the prompt box on the right (for example, "a dashboard that lists active subscriptions") and pick a model.
6. Let the assistant draft a starting App, then refine it through the **Discover**, **Ideate**, **Build**, and **Visualize** phases along the bottom.
7. Preview the App, then publish the workspace.

## Verify

- Confirm the **API Catalog** panel shows the API you intend to use, with current counts, before you build against it.
- Preview the generated App and confirm it calls the chosen catalogue APIs and renders the expected screens.
- After publishing, sign in as a consumer and confirm the workspace appears as a clonable starter on their dashboard.

{% hint style="info" %}
**Note:** App Builder runs against your published APIs. To demo an API still in draft, publish it to a private Plan first so the builder can call it.
**Result:** You have a demo workspace App composed from catalogue APIs. Once published, consumers see it as a clonable starter on their own dashboard.
{% endhint %}