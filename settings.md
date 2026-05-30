---
icon: gear
---

Most settings in the marketplace are Org-scoped and live with your Org Admin. Branding, SSO, custom roles, system-wide email templates. This chapter covers the smaller set of settings you can change yourself as an API Provider: your personal profile, your notification preferences, the layout of your dashboard, and your default landing page. Each task is short and runs only once. Where a setting requires Org Admin rights, the task points to the Org Admin guide rather than walking through the change.

## Personal-scope settings

Your personal-scope settings affect only your own account. You can change them without involving an Org Admin and without affecting anyone else in your Organisation.

#### Update your profile

Use this task when your name, email, or password needs to change.

#### Before you start

- **Confirm whether SSO is configured.** If your Organisation signs in through SAML, your email and password are managed by your identity provider, not the marketplace. The marketplace profile page hides the password field in that case.
- **Have your current password ready.** Password changes prompt for the current password before accepting a new one.

To update your profile:

1. Click your name chip in the top-right corner of any page. A dropdown opens.
2. Click **My account**. The page navigates to `/user/<your-id>`.
3. Click **Edit**. The page navigates to `/user/<your-id>/edit`.
4. Update the **Name**, **Email**, or **Password** fields to change.
5. Optionally update **Time zone** and **Locale** to match your working region; these affect how the marketplace renders timestamps and dates.
6. Click **Save**.

{% hint style="success" %}
**Result:** Your profile is updated. Email changes take effect immediately; the next session uses the new email as the sign-in identifier.
{% endhint %}

{% hint style="info" %}
**Note:** If your Organisation uses SSO, the Email field is read-only and the Password field is hidden. To change either, request that your identity-provider administrator update your account upstream.
{% endhint %}

{% hint style="success" %}
**Tip:** For SSO configuration itself. Provisioning, attribute mapping, on-the-fly account creation. See the Org Admin guide. The marketplace surfaces those settings under SETTINGS > SAML IdP, but only Org Admins can edit them.
{% endhint %}

#### Choose your notification preferences

Use this task to mute or unmute specific marketplace events from reaching your inbox or browser.

To choose notification preferences:

1. Click your name chip in the top-right corner. A dropdown opens.
2. Click **My account**, then click **Notifications** in the secondary navigation.
3. For each event category. Subscription requests, Governance scan results, System notifications, Webhook delivery failures. Toggle whether you receive an email, an in-app notification, both, or neither.
4. Click **Save**.

{% hint style="success" %}
**Result:** The marketplace honours your preferences from the next event onwards. Existing notifications in your inbox remain.
{% endhint %}

{% hint style="success" %}
**Tip:** Mute everything except Subscription requests to be alerted only when a consumer is waiting for approval. Most other events require no immediate action.
{% endhint %}

{% hint style="info" %}
**Note:** Org Admin-issued System notifications marked as critical override your preference and always appear in your in-app inbox.
{% endhint %}

## Layout settings

A handful of layout settings configure the dashboard and the page you land on when you sign in. They affect only your account.

#### Re-arrange dashboard tiles

Use this task to change the mix of panels on `/admin/dashboard`. For example, hiding Quick Actions if your day focuses on review rather than creation.

To re-arrange dashboard tiles:

1. Sign in and land on `/admin/dashboard`.
2. Click **Customise** in the top-right corner of the dashboard. The dashboard switches to edit mode and each panel gains a drag handle and a hide toggle.
3. Drag the handles to reorder panels.
4. Click the eye icon on any panel to hide it. Hidden panels are listed at the bottom of the screen and can be re-shown.
5. Click **Save layout**.

{% hint style="success" %}
**Result:** Your dashboard layout persists across sessions. Other members of your Organisation see their own layout; nothing on this page affects them.
{% endhint %}

{% hint style="info" %}
**Note:** A small number of panels (for example API Health Overview) cannot be hidden because they are considered essential. The hide toggle is greyed out for those panels.
{% endhint %}

#### Set your default landing page

Use this task when `/admin/dashboard` is not the page you want to see first after sign-in.

To set your default landing page:

1. Click your name chip in the top-right corner. A dropdown opens.
2. Click **My account**, then click **Preferences**.
3. From the **Default landing page** dropdown, select one of:
   - Admin dashboard (default)
   - Manage APIs
   - Subscriptions
   - Provider Analytics
4. Click **Save**.

{% hint style="success" %}
**Result:** Your next sign-in takes you to the selected page. The change applies only to your account.
{% endhint %}

{% hint style="success" %}
**Tip:** Set the landing page to Subscriptions if approvals are your daily bottleneck. The page loads filtered to Pending by default, so each session starts on the queue requiring action.
{% endhint %}

{% hint style="info" %}
**Note:** Org-wide settings. Site branding, default Visibility, default plan. Are not on this page. They live with the Org Admin under SETTINGS > Site Settings and SETTINGS > Auth & Branding.
{% endhint %}

## Session and credential management

Two settings live with the user because they affect only your account but have security consequences: where you are currently signed in, and which API tokens you have issued for personal automation.

#### Review and revoke active sessions

Use this task when you sign in from a new device, when a teammate borrows your laptop, or whenever you suspect your account has been used somewhere unexpected.

#### Before you start

- **Have a second sign-in path ready.** Revoking a session immediately signs that browser out. If the only browser you are signed in on is also the one about to be revoked, have your password or SSO ready to sign in again afterwards.
- **Distinguish revoking a session from changing your password.** Revoking a session signs out a single device without affecting your password. Changing your password signs out every session everywhere; use it after a confirmed credential leak.

To review and revoke active sessions:

1. Click your name chip in the top-right corner. A dropdown opens.
2. Click **My account**, then click **Sessions** in the secondary navigation.
3. Read the list of active sessions. Each row shows the browser and operating system, the IP address (or a coarse geographical region), and the time the session was last used.
4. Locate any row you do not recognise. Look for a browser, OS, or location that does not match a device you own.
5. Click **Revoke** on any row to sign out. The browser at that session is signed out within a few seconds.
6. Click **Revoke all other sessions** to sign every other device out at once, leaving only your current session active.

{% hint style="warning" %}
**Caution:** Revoking a session is immediate and irreversible. Anyone using the revoked session is signed out without warning. If you revoke a session you actually own, sign back in to recover.
{% endhint %}

{% hint style="success" %}
**Result:** The revoked sessions are signed out. Your current session remains active.
{% endhint %}

{% hint style="info" %}
**Note:** If you cannot reach the Sessions page because you are locked out of every device, request that your Org Admin revoke your sessions from their administrative view. Org Admins can revoke any member's sessions; Portal Admins can revoke across Organisations.
{% endhint %}

#### Manage your personal API tokens

Use this task to call the marketplace's own management API from a script. For example, to automate routine subscription approvals or to export analytics data on a schedule.

#### Before you start

- **Confirm your Organisation allows personal tokens.** Some Organisations restrict personal tokens to Org Admins. If the API Tokens tab is not visible on your My account page, your Org has disabled them; request that your Org Admin issue a service-account credential instead.
- **Plan the token's scope.** A token inherits your role's permissions. If only read access is required for a script, request that your Org Admin provision a narrower role for token-only use rather than reusing your full Provider permissions.

To manage your personal API tokens:

1. Click your name chip in the top-right corner. A dropdown opens.
2. Click **My account**, then click **API Tokens** in the secondary navigation.
3. To issue a new token, click **Create token**, assign a **Name** describing what the token is for (for example "Nightly subscription export"), select an **Expiry** (30, 60, 90, or 365 days), and click **Save**.
4. Copy the token value shown on the next screen. The marketplace shows the value only once; if it is lost, revoke the token and issue a new one.
5. To revoke an existing token, locate it in the list and click **Revoke**. The token stops working within seconds.

{% hint style="warning" %}
**Caution:** Treat a personal token like a password. Anyone who has the token can act as you within the scope of your role. Store tokens in a secret manager, never in source control.
{% endhint %}

{% hint style="success" %}
**Result:** You have a working personal token (or have revoked one no longer needed). The token list reflects the current state.
{% endhint %}

{% hint style="success" %}
**Tip:** Set every personal token to expire. Never issue a non-expiring token. The shortest expiry that covers your automation's lifecycle is the appropriate choice; rotate on expiry rather than extending.
{% endhint %}

{% hint style="info" %}
**Note:** Personal tokens authenticate management-plane calls only. They do not authenticate runtime API calls. Runtime calls use the API keys issued to consumer Apps as described in [Onboarding your first consumer](onboarding-your-first-consumer.md#onboarding-your-first-consumer).
{% endhint %}