---
icon: user-shield
---

A role bundles permissions: which menu items appear, which buttons are enabled, and which records a member can edit. Access on the marketplace is default-deny, so a member sees nothing until a role grants it. The marketplace ships with four built-in roles that cover most needs, plus a custom-role builder for the case where one of them is too wide or too narrow for a colleague's job. A member always holds at least one role, and roles can stack. Roles are assigned on the Members surface and reviewed here on the Custom Roles list.

![Figure. The Custom Roles list showing the four built-in roles with their member counts.](.gitbook/assets/screenshots/provider/admin-organisations-4-roles.png)

## What you see

The **Custom Roles** list shows every role available in your Organisation, built-in and custom, with the member count holding each. Reach it from the **ORGANISATION** group in the left sidebar by clicking **Custom Roles**. The table carries:

- **Role name.** The display name. Click it to open the role's permission editor.
- **Source.** Built-in or Custom. Built-in roles cannot be renamed or deleted; their permission set is view-only.
- **Members.** The number of members currently holding the role. Useful when planning to retire a role or running an access audit.
- **Add role.** The button that opens the create-role form.

### The four built-in roles

Recognise the built-in roles before you assign any of them. They are not interchangeable.

- **API Provider.** Imports APIs from a connected gateway, runs governance checks, designs API Products and plans, approves subscription requests, monitors analytics. The audience this guide is written for. Scoped to one Organisation.
- **Org Admin.** Everything an API Provider can do, plus invite members, change roles, define custom roles, configure single sign-on, brand the storefront, and set the Organisation homepage. Scoped to one Organisation.
- **API Consumer.** Browses the catalog, subscribes to plans, manages personal Apps and API keys, views their own usage analytics. No admin-side access. Scoped to one Organisation, but a consumer can hold memberships in many.
- **Portal Admin.** Cross-Organisation administrator. Operates across every Organisation on the platform, configures marketplace-wide policies, manages the Organisation list, runs platform-wide system notifications. Reserved for the platform team running the marketplace itself. Out of scope for most teams.

{% hint style="success" %}
**Tip:** Click each built-in role to view its permission set in read-only mode. The grid is grouped by surface and is the fastest way to learn what each role can and cannot do before you grant it.
{% endhint %}

## Add-role form fields

- **Role name**: text, required, max 100 characters, unique within the Organisation. The display name shown in dropdowns and audit entries.
- **Description**: text, optional, max 255 characters. A one-line summary shown on the Custom Roles list, such as *"Approves subscription requests for the Payments team. Cannot edit APIs or governance."*
- **Base role**: select, optional. Picking a base role pre-ticks every permission that role holds, so you start from "API Provider plus one extra" rather than an empty grid, then add or remove individual checkboxes.
- **Permission grid**: required, grouped by surface. Each surface (APIs, Products, Subscriptions, Members, Teams, Roles, Settings, Analytics) offers a view, edit, create, delete, and approve checkbox where applicable. Tick only the permissions the role needs.

The role-assignment field lives on the member edit form, not here:

- **Role(s)**: multi-select on the member edit form, of every role available in the Organisation, built-in and custom. Roles stack: the member's effective permission set is the union of every role they hold.

![Figure. The Add role form with Name, Description, base role, and the grouped permission grid.](.gitbook/assets/screenshots/provider/admin-organisations-roles-add.png)

## Create a custom role

Create a custom role when one of the built-in roles is too wide or too narrow for a colleague's job. A common case is an *Approver* who can approve subscription requests but cannot import APIs or change governance. Write the role's purpose in one sentence first; that sentence becomes the description and gets you a faster review.

1. From the **ORGANISATION** group in the sidebar, click **Custom Roles** to open the list.
2. Click **Add role**.
3. Enter a short, unique **Role name** and a one-line **Description**.
4. Optional. Pick a **Base role** to pre-tick its permissions, then adjust.
5. Tick the permissions the role needs in the grouped grid. The list is grouped by surface: APIs, Products, Subscriptions, Members, Teams, Roles, Settings, Analytics.
6. Click **Save**. The role returns to the list with **Source = Custom**.

{% hint style="warning" %}
**Caution:** A permission ticked by mistake takes effect for every member holding the role on their next request. Review the grid before saving.
{% endhint %}

{% hint style="success" %}
**Tip:** Test a new role against a sandbox member account before assigning it to a real colleague. Sign in as that member and confirm the menu items, buttons, and records behave as you expect.
{% endhint %}

## Assign a role to a member

Assigning a role uses the same flow as changing a role on the Members surface. The custom role you created appears in the same **Role(s)** multi-select alongside the four built-in roles.

1. From the **ORGANISATION** group, click **Members**.
2. Click **Edit** on the member's row.
3. Pick the role in the **Role(s)** multi-select. Hold cmd or ctrl to add a second role.
4. Click **Save**. The member picks up the new permissions on their next sign-in.

{% hint style="success" %}
**Tip:** Two roles can stack. Assigning both *API Provider* and a custom *Approver* role grants the union of the two permission sets. Use stacking sparingly; a single role is easier to reason about than three layered ones.
{% endhint %}

## Audit role usage

Audit role usage when you are about to retire a custom role, when an annual access review is due, or when a permission you thought was narrow turns out to be granted broadly.

1. From the **ORGANISATION** group, click **Custom Roles**.
2. Sort the table by **Members** descending so the most-granted role sits at the top.
3. For each role with a non-zero count, click the **Members** number to open a filtered Members list scoped to that role.
4. Cross-reference the filtered list against your access matrix and flag any member whose job no longer matches the role.
5. For unused roles (zero members), decide whether to delete or keep as a documented standard.

{% hint style="info" %}
**Note:** Deleting a custom role with zero members is safe. Deleting one that still has members is blocked; reassign the affected members to a different role first.
{% endhint %}

## Verify

- The new role appears in the **Custom Roles** list with the correct name and **Source = Custom**.
- Open its permission editor and confirm the ticked checkboxes match your intent.
- Assign it to a sandbox member, sign in as them, and confirm the menu items, buttons, and records align with your design.
- After an audit, confirm the **Role = Org Admin** filter on the Members list returns only the people who should hold admin.

{% hint style="success" %}
**Result:** The role is available on the Members edit form and the invite form, and every member you assign it to gains exactly the permissions you ticked, nothing more.
{% endhint %}

## Related

- [Team & members](feat-team-and-members.md) for inviting members and assigning roles on the way in.
- [Single sign-on (SAML)](feat-single-sign-on.md) for mapping IdP groups to roles so permissions are managed from the identity provider.
- [Branding & theming](feat-branding-and-theming.md) for storefront branding, an Org Admin responsibility.
- [Custom domains](feat-custom-domains.md) for the brand-owned storefront address.