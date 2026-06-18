---
icon: envelope
---

Every transactional email the marketplace sends comes from a template: subscription approvals, password recovery, member invitations, account activation, and API deprecation notices. The portal ships with sensible defaults. Edit a template when the default copy does not match your tone, when legal asks for specific footer text, when you want to change which fields appear, or when you want to translate it for a non-English audience. Before you customise copy, point the outbound email transport at a mail server you control, otherwise the edited template never leaves the portal.

![Figure. The Email templates list, grouped by category, with one row per transactional template.](.gitbook/assets/screenshots/provider/admin-structure-email-templates-templates.png)

## What you see

The **Email templates** list sits under **SETTINGS** in the left sidebar. Templates group into collapsible categories (API Deprecation, General, MFA, Organisation, User) so you can find one quickly. Each row exposes:

- **Email type** column: the template name as it appears in the list, within its category.
- **Machine name** column: the read-only internal identifier the marketplace uses to fire the template on the matching event. Useful when an integrator asks which template handles a specific event.
- **Subject line** column: the email's Subject header. Open the template to see and edit it. This is the field operators most often change to match brand voice.
- **Edit template** action: opens the template editor, where you change the subject, body, and the variables substituted at send time.

## The template editor

Each template fires on a specific event and exposes a fixed set of fields:

- **Email type**: read-only label. The template name, identifying which event the template fires on.
- **Machine name**: read-only. Binds the template to its trigger event. Editing the wrong template means your change never goes out.
- **Subject**: text (required). The email's Subject header.
- **Body**: rich text (required). The message, edited in a rich-text editor. Insert variables from the variable list to the right of the editor.
- **Variables**: tokens such as `[user:display-name]`, `[site:name]`, and `[org:name]` that resolve at send time. The editor lists every variable that template exposes.
- **Preview**: a control that renders the email with placeholder values substituted, so you can check layout before saving.

### Templates that ship with the portal

- **Default**: the fallback used when no event-specific template applies.
- **User lifecycle**: Member Invite, User Activation, User Password Recovery, User Created (No Approval), User Created (Awaiting Approval), User Created (Approval Admin), User Created By Admin, User Blocked, User Cancelled, User Cancellation Confirmation.
- **Security**: MFA Email OTP, sent when a user logs in and email-based multi-factor authentication is required.
- **API Deprecation**: Upcoming, Deprecated, and Delete Notice, each fired at its stage of the deprecation schedule.

## Edit a template

1. Expand **SETTINGS** in the sidebar, then click **Email templates**.
2. Find the template by name. Templates group into collapsible categories.
3. Click the template name to open the editor.
4. Update the **Subject** field for a different subject line.
5. Edit the **Body** in the rich-text editor, inserting variables from the list so they resolve at send time.
6. Click **Preview** to see the rendered email with placeholder values.
7. Click **Save**.

{% hint style="warning" %}
**Caution:** A typo in a variable name (for example a broken `[user:display-name]`) renders as the literal string in the outgoing email. Always send a test to yourself after editing.
{% endhint %}

{% hint style="success" %}
**Tip:** Test a change by triggering the matching event for a test user. Edit the Member Invite template, then invite yourself to a test Organisation, and confirm the email looks right.
{% endhint %}

## Configure the outbound transport

Configure the transport before you customise any template. The transport carries every outbound email out of the marketplace, so a misconfigured value means nothing ships and users see silent failures.

![Figure. The email transport configuration where you set the DSN, From address, and From name used for every outbound email.](.gitbook/assets/screenshots/provider/admin-config-system-symfony-mailer-lite-transport.png)

The transport form fields:

- **Transport name**: text (required). The label that identifies this transport in the email-template editor, for example "Production SMTP".
- **DSN**: text (required). The full transport string the marketplace uses to dispatch every outbound email. SMTP DSNs look like `smtp://user:pass@host:port`. Add `?encryption=tls` if your provider does not negotiate TLS by default.
- **From address**: email (required). The sender address in every email's From header. Match it to a mailbox your team monitors for bounces and replies.
- **From name**: text (required). The display name that pairs with the From address. Typically your portal or company name.
- **Save configuration**: persists the transport. New outbound emails use it immediately; queued emails are not retroactively rewritten.

To configure:

1. Expand **Configuration** > **System** in the sidebar, then click **Symfony Mailer Lite transport**.
2. Enter a friendly **Transport name**.
3. Paste the full **DSN** string. The exact format depends on your provider.
4. Enter the **From address** and **From name** that appear on every outbound email.
5. Click **Save configuration**.
6. Trigger a low-impact email (for example, invite yourself to a test Organisation) and confirm it arrives.

{% hint style="warning" %}
**Caution:** A misconfigured DSN does not throw an obvious error in the UI. Always send a test email after saving and watch your inbox.
{% endhint %}

{% hint style="success" %}
**Tip:** Keep DSN credentials in your secrets manager and paste them in only at configuration time. The DSN field stores the credentials, so anyone with admin access can read them back.
{% endhint %}

## Set SMTP server credentials

Use the SMTP settings page when your provider gives you raw server credentials rather than a single DSN string, or when you want to set a backup server and TLS behaviour explicitly.

![Figure. The SMTP settings page with the server host, backup server, port, and encryption controls.](.gitbook/assets/screenshots/provider/admin-config-system-smtp.png)

The SMTP form fields:

- **SMTP server**: text (required). The host of your outgoing SMTP server, for example `smtp.gmail.com`.
- **SMTP backup server**: text (optional). A secondary host the marketplace tries only when the primary cannot be reached.
- **SMTP port**: number (required). Defaults to 25. Use 587 or 465 if 25 is blocked on your network.
- **Use encrypted protocol**: selector (required). The transport encryption (for example Use TLS) for servers that require SSL or TLS.
- **Enable TLS encryption automatically**: toggle. Set to *On* to negotiate TLS automatically where the server supports it.
- **Save configuration**: persists the SMTP settings; new outbound emails dial out through these values immediately.

To set the credentials:

1. Expand **Settings** in the sidebar, then click **SMTP Settings**.
2. Enter the **SMTP server** host.
3. Enter a **SMTP backup server** if your provider offers one.
4. Enter the **SMTP port**. Use 587 or 465 if 25 is blocked.
5. Choose the encryption under **Use encrypted protocol** to match what your provider requires.
6. Set **Enable TLS encryption automatically** to *On*.
7. Click **Save configuration** in the top-right.
8. Trigger a low-impact email and confirm it arrives.

{% hint style="warning" %}
**Caution:** A wrong port or encryption mode fails silently in the UI. Always send a test email after saving and confirm it lands.
{% endhint %}

## Verify

- Trigger the matching event for a test recipient (for example, invite yourself to a test Organisation for Member Invite) and confirm the email arrives with the new subject and body.
- Confirm every variable resolves to a real value. No literal `[user:display-name]` strings remain in the delivered email.
- Confirm the From-name and From-address match what is set on the email transport.

{% hint style="success" %}
**Result:** The next outbound email of that type uses the new subject and body, and dispatches through the configured transport. Queued emails already in flight are not rewritten.
{% endhint %}

## Related

- [Notifications](feat-notifications.md) reach users inside the app for the same lifecycle events.
- [Webhooks](feat-webhooks.md) drive the same events to external systems.
- [Single sign-on](feat-single-sign-on.md) and [Roles & permissions](feat-roles-and-permissions.md) govern the accounts these emails are sent to.