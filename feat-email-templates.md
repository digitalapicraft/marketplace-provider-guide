---
icon: envelope
---

Customise the transactional emails the marketplace sends: subscription approvals, password recovery, member invitations, API deprecation notices, and more. Edit a template when the default copy does not match your tone or when legal asks for specific footer text.

![Figure. The Email templates list, grouped by category, with one row per transactional template.](.gitbook/assets/screenshots/provider/admin-structure-email-templates-templates.png)

## Configure

1. Expand **SETTINGS** in the sidebar, then click **Email templates**.
2. Find the template by name. Templates group into collapsible categories (API Deprecation, General, MFA, Organisation, User).
3. Click the template name to open the editor.
4. Update the **Subject** field for a different subject line.
5. Edit the **Body** in the rich-text editor. Insert variables such as `[user:display-name]`, `[site:name]`, and `[org:name]` from the variable list. They resolve at send time.
6. Click **Preview** to see the email rendered with placeholder values.
7. Click **Save**.

{% hint style="success" %}
**Result:** The next outbound email of that type uses the new subject and body. Queued emails already in flight are not rewritten.
**Tip:** A typo in a variable name renders as the literal string in the email. After editing, trigger the matching event for a test recipient and confirm every variable resolves.
{% endhint %}

### Key columns

1. **Email type.** The template name, grouped by category for quick lookup.
2. **Machine name.** The read-only identifier that fires the template on its matching event.
3. **Subject line.** The email's Subject header. The field operators most often change to match brand voice.