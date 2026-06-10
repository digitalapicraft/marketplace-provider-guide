---
icon: toggle-on
---

Feature Management is the master switchboard for your Organisation. Each toggle turns a whole portal capability on or off for your tenancy without a code deployment. Use it to roll a feature out gradually, retire one your team does not use, or gate an in-progress feature until it is ready.

![Figure. Feature Management with the User Management, API Engagement, and Portal Workflows groups, each showing a feature count.](.gitbook/assets/screenshots/provider/admin-portal-features.png)

## What you configure

Features are organised into collapsible groups, each showing a pill with the count of features it holds. Expand a group to reach its individual on/off toggles. The groups are:

- **User Management**: user-facing toggles that govern how accounts, sign-up, and member-related surfaces behave.
- **API Engagement**: API-discovery and engagement toggles that control catalogue and consumer-facing capabilities.
- **Portal Workflows**: workflow toggles such as content moderation gates and approval steps.

Each toggle inside a group enables or disables one capability for the whole tenancy. Turning a toggle off hides every surface that depends on that capability for every user, not only yourself.

## What each toggle affects

A toggle controls more than a single button. It governs every surface, menu link, and workflow tied to that capability, so the effect is felt across the portal the moment you flip it:

- A **User Management** toggle changes how accounts behave: whether self-sign-up is open, which fields a new user fills in, and whether member-related menus appear.
- An **API Engagement** toggle changes the consumer-facing catalogue: whether a discovery surface, an engagement panel, or a related consumer page renders at all.
- A **Portal Workflows** toggle changes whether a workflow gate is in force: turning a moderation or approval workflow off lets content publish directly, while turning it on routes content through review first.

Because a toggle reaches every dependent surface, treat each change as portal-wide. Confirm with the team that no one relies on a capability before you retire it.

## Configure

1. From the left sidebar, expand **Settings** and click **Manage Features**.
2. The **Feature Management** page loads with the feature groups collapsed. Each group shows a count of the features it holds.
3. Click a group (for example **API Engagement**) to expand it and reveal the individual toggles.
4. Read what the feature controls before changing it. Toggling a feature off hides every surface that depends on it for every user.
5. Flip the toggle on or off for the feature you want to change.
6. The change saves on toggle. No separate save step is required.

## Verify

- Confirm the toggle holds its new on or off position after the page reloads.
- Open a surface the feature controls and confirm it renders when the feature is on, and stops rendering when it is off.
- After enabling a feature, check the menu or page it adds appears for the intended users.
- Change one toggle at a time so you can attribute any change in behaviour to the right feature.

{% hint style="warning" %}
**Caution:** Disabling a feature other surfaces depend on can hide functionality your team relies on day to day. Toggle one feature at a time and confirm the affected surfaces still behave as expected before moving on.
**Result:** The capability is on or off across the portal immediately. Surfaces that depend on a disabled feature stop rendering for every user.
{% endhint %}