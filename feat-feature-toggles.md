---
icon: toggle-on
---

Feature Management is the master switchboard for your Organisation. Each toggle turns a whole portal capability on or off for your tenancy without a code deployment. Use it to roll a feature out gradually, retire one your team does not use, or gate an in-progress feature until it is ready.

![Figure. Feature Management with the User Management, API Engagement, and Portal Workflows groups, each showing a feature count.](.gitbook/assets/screenshots/provider/admin-portal-features.png)

## Configure

1. From the left sidebar, expand **Settings** and click **Manage Features**.
2. The **Feature Management** page loads with the feature groups collapsed. Each group shows a count of the features it holds.
3. Click a group (for example **API Engagement**) to expand it and reveal the individual toggles.
4. Read what the feature controls before changing it. Toggling a feature off hides every surface that depends on it for every user.
5. Flip the toggle on or off for the feature you want to change.
6. The change saves on toggle. No separate save step is required.

{% hint style="success" %}
**Result:** The capability is on or off across the portal immediately. Surfaces that depend on a disabled feature stop rendering for every user.
**Caution:** Disabling a feature other surfaces depend on can hide functionality your team relies on day to day. Toggle one feature at a time and confirm the affected surfaces still behave as expected before moving on.
{% endhint %}