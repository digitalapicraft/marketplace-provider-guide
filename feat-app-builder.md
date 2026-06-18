---
icon: wand-magic-sparkles
---

App Builder is a low-code surface where you assemble small demonstration Apps on top of the APIs in your catalogue. Use it to prove an integration pattern, give a partner a runnable starter, or seed a Workspace that consumers can clone for their own Apps. It is a tool for demos and prototyping, not for production Apps: it runs against your published catalogue to show what an integration looks like. Reach it from the left sidebar under **API MANAGEMENT** > **App Builder**.

![Figure. The App Builder landing page with the API Catalog summary on the left, template tiles, and the assistant prompt on the right.](.gitbook/assets/screenshots/provider/app-builder.png)

## What you see

The landing surface splits into a left rail that summarises what the builder is wired to, a set of starting points, and an assistant prompt on the right:

- **API Catalog panel**: a summary of the APIs and endpoints the builder can call, with live counts. It tells you, at a glance, what the builder has to work with. Click **Sync** to refresh the inventory after you publish a new API.
- **TEMPLATES**: pre-built starter Apps, such as **Customer Support System**, that you clone as the basis for a new build. Each template packages an API selection, a UI layout, and routing wiring.
- **Shared Workspaces**: workspaces shared with you or your team. Open one to keep working on a build someone else started.
- **Workspaces / + New**: your own workspaces. Click **+ New** to start a blank one.
- **Prompt box**: type what you want to build, pick a model, and the assistant drafts a starting App you then refine.
- **Discover / Ideate / Build / Visualize phases**: the four phases along the bottom of the prompt area that you move through as you turn an idea into a runnable demo App.

## Starting points

App Builder gives you three ways to begin a build, each suited to a different need:

- **From a template**: pick a template tile when you want a working starting structure. The template arrives with an API selection, a UI layout, and routing already wired, so you adjust rather than assemble from nothing.
- **From a blank workspace**: choose **Workspaces** > **+ New** when you want full control over the structure from the first screen.
- **From a prompt**: type a goal into the prompt box when you want the assistant to draft the first version. You then refine its output through the four phases.

## Prompt configuration

The prompt box on the right takes a goal and a model choice:

- **Prompt**: text (required). A plain-language description of what you want to build, for example "a dashboard that lists active subscriptions". The assistant uses it to draft a starting App.
- **Model**: select (required). The assistant model that drafts and refines the App. Pick the model before you submit the prompt.

## Build phases

The four phases along the bottom of the prompt area move an idea from concept to a runnable demo. Move between them as the build matures:

- **Discover**: clarify the goal and the APIs the App needs.
- **Ideate**: shape the App's screens and flows.
- **Build**: generate and refine the App against the chosen APIs.
- **Visualize**: preview the running App and confirm it renders the expected screens.

## Build a demo App

Open App Builder when you want to assemble a runnable demo on top of your published catalogue.

1. From the left sidebar under **API MANAGEMENT**, click **App Builder**.
2. Review the **API Catalog** panel on the left, which summarises the APIs and endpoints the builder can call. Click **Sync** to refresh the inventory after you publish a new API.
3. To start from a starter App, pick a tile from the **TEMPLATES** list. Each template packages an API selection, a UI layout, and routing wiring.
4. To start blank, choose **Workspaces** > **+ New**, or open a Shared Workspace someone else began.
5. Type your goal into the prompt box on the right, for example "a dashboard that lists active subscriptions", and pick a model.
6. Let the assistant draft a starting App, then refine it through the **Discover**, **Ideate**, **Build**, and **Visualize** phases along the bottom.
7. Preview the App, then publish the workspace.

{% hint style="info" %}
**Note:** App Builder runs against your published APIs. To demo an API still in draft, publish it to a private Plan first so the builder can call it.
{% endhint %}

## Refresh the catalogue inventory

Sync the builder after you change what is in your catalogue, so it builds against the current set of APIs.

1. Open App Builder from the left sidebar.
2. Look at the live counts in the **API Catalog** panel and confirm whether they reflect the API you intend to use.
3. If a newly published API is missing, click **Sync** in the **API Catalog** panel.
4. Confirm the counts update and the new API appears in the inventory the builder can call.

{% hint style="success" %}
**Tip:** Sync before every new build. A stale inventory is the most common reason a freshly published API does not appear in the builder.
{% endhint %}

## Verify

- Confirm the **API Catalog** panel shows the API you intend to use, with current counts, before you build against it.
- Preview the generated App and confirm it calls the chosen catalogue APIs and renders the expected screens.
- After publishing, sign in as a consumer and confirm the workspace appears as a clonable starter on their dashboard.

{% hint style="success" %}
**Result:** You have a demo workspace App composed from catalogue APIs. Once published, consumers see it as a clonable starter on their own dashboard.
{% endhint %}

## Related

- [Apps and credentials](feat-apps-and-credentials.md)
- [Products and plans](feat-products-and-plans.md)
- [Publishing APIs](feat-publishing-apis.md)