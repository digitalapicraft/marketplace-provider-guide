---
icon: user-shield
---

Roles bundle permissions: which menu items appear, which buttons are enabled, and which records a member can edit. Use the four built-in roles for most needs, and build a custom role when one of them is too wide or too narrow for a colleague's job.

![Figure. The Add role form with Name, Description, and the grouped permission grid.](.gitbook/assets/screenshots/provider/admin-organisations-roles-add.png)

## Configure

1. Expand the **ORGANISATION** group in the sidebar, then click **Custom Roles** to open the **Custom Roles** list. Built-in and custom roles both appear here.
2. Click **Add role**.
3. Enter a short **Role name**, unique within the Organisation (max 100 characters).
4. Enter a one-line **Description** of what the role can do (max 255 characters).
5. Optional. Pick a **Base role** to pre-tick every permission that role holds, then add or remove individual checkboxes.
6. Tick the permissions the role needs in the grouped grid. Surfaces include APIs, Products, Subscriptions, Members, Teams, Roles, Settings, and Analytics, each with view, edit, create, delete, and approve where applicable.
7. Click **Save**. The role returns to the Custom Roles list with **Source = Custom**.
8. To assign it, open **Members**, click **Edit** on a member's row, pick the role in the **Role(s)** multi-select, then **Save**. The member picks it up on their next sign-in.

{% hint style="success" %}
**Result:** The role is available on the Members edit form and the invite form, and every member you assign it to gains the permissions you ticked.
**Tip:** Test a new role against a sandbox member account first. Sign in as that member and confirm the menu items, buttons, and records behave as designed before assigning it to a real colleague.
{% endhint %}

### Built-in roles

1. **API Provider.** Imports APIs, runs governance, designs Products and plans, approves subscriptions, monitors analytics. Scoped to one Organisation.
2. **Org Admin.** Everything an API Provider can do, plus invite members, change roles, define custom roles, and brand the storefront.
3. **API Consumer.** Browses the catalog, subscribes, manages personal Apps and keys. No admin-side access.