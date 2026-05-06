# Managing your team

Running the marketplace is a team activity. You bring in colleagues to import APIs, review governance, design Products and plans, approve consumers, and monitor traffic. Everyone you work with sits inside one **Organisation** — the unit of multi-tenancy on the marketplace. Inside that Organisation, **Members** are the individual users who log in. **Teams** group those members into smaller units so that a Payments team sees Payments APIs and a Cards team sees Cards APIs. **Roles** decide what each member can do — import APIs, approve subscriptions, change settings, invite more members. This chapter walks you through the four surfaces you use to run all of that: viewing your Organisation, inviting members, organising them into teams, and assigning roles.

You will learn:

- How Organisations, Members, Teams, and Roles relate, and which surface owns each one.
- How to invite a colleague, pick the right role, and optionally land them in a team on first sign-in.
- How to run a monthly audit of who has access and which accounts are dormant.
- How to create a Team and scope APIs or Products so only its members see them.
- How to recognise the four built-in roles and when to reach for a custom role instead.
- (Portal Admin only) How to add a new Organisation when a fresh tenant is being onboarded.

Allow ~45 minutes to walk through the chapter and complete a first invitation.

The four entities relate like this:

- An **Organisation** is the top-level container. Every API, Product, subscription, App, and member belongs to one Organisation. A Portal Admin can run more than one Organisation on the same marketplace; an API Provider almost always works inside a single Organisation.
- A **Member** is one user account linked to one Organisation. The same email can be a member of more than one Organisation — the user picks which Organisation to act in after signing in.
- A **Team** is a sub-group inside an Organisation. Teams scope visibility for APIs and Products that are flagged Org Level — when a team is picked on the resource, only members of that team see it in the catalog.
- A **Role** is a bundle of permissions — what menu items appear, which buttons are enabled, which records can be edited. A member always has at least one role; roles can stack.

The rest of the chapter takes each of those four ideas in turn.

## Recognising the Organisation surfaces

The **ORGANISATION** group in the left sidebar gathers every page that relates to your Organisation, its members, its teams, and its roles. As an API Provider you will spend most of your time in **Organisation Members** and **Organisation Teams**. A Portal Admin sees one extra page — the top-level **Organisations** list that spans the whole platform.

#### Open the Organisation list

Open the Organisation list when you want a top-level view of every Organisation in the marketplace, or when you need to confirm which Organisation you are currently acting in.

#### Before you start

- **Confirm your role.** As an API Provider you typically see only the Organisation you belong to. As a Portal Admin you see every Organisation on the platform with its member and team counts.
- **Know your active Organisation.** The Organisation switcher at the top of the sidebar shows the Organisation you are currently scoped into. Every action you take applies to that Organisation.

To open the Organisation list:

1. From the left sidebar, expand the **ORGANISATION** group.
2. Click **Organisations**. The page loads with the title **Organisations**.
3. Scan the table to find the Organisation you want to work with.
4. Click an Organisation's name to open its detail page.

![Figure 11-1. The Organisations list.](.gitbook/assets/screenshots/provider/admin-organisations.png)

The numbered callouts in Figure 11-1 are:

1. **Organisations** — The page heading. Each row is one Organisation on the platform.
2. **Name** — The Organisation's display name. Click the name to open the Organisation Overview.
3. **Status** — Active, Suspended, or Pending. A suspended Organisation cannot publish APIs or accept new subscriptions.
4. **Members** — The current member count for the Organisation. Click the number to jump straight into the Members list for that Organisation.
5. **Teams** — The team count. Click to jump into the Teams list.
6. **Created** — The date the Organisation was created. Useful when reconciling against your tenant onboarding records.

> **Result:** You can see every Organisation visible to your role and pick the one you want to manage.

> **Note:** As an API Provider on a single Organisation, this list contains a single row — your own. The page is still useful as a navigation hub: the columns link straight to the corresponding Members and Teams pages.

#### Verify

1. Confirm the table shows at least one row and the column headers (**Name**, **Status**, **Members**, **Teams**, **Created**) are all populated.
2. Click the **Members** count on your Organisation's row and confirm the page navigates to its Members list.
3. Return to the list and confirm the Organisation switcher in the top sidebar still reflects the Organisation you intend to act in.

#### Open your Organisation's detail page

Open the Organisation detail page when you need a single screen that summarises your Organisation and gives you links into every sub-surface — members, teams, roles, settings.

#### Before you start

- **Confirm you can reach the Organisations list.** The detail page is one click away from the list; if the list is empty for your role, your Org Admin must grant you access first.
- **Know which Organisation you intend to inspect.** The breadcrumb at the top of every sub-page reflects the Organisation in scope. If you manage more than one, pick the right name from the list before drilling in.
- **Have your sub-task in mind.** The detail page is a hub — you arrive to invite a member, scope a team, audit roles, or edit branding. Knowing the next step saves backtracking.

To open the detail page:

1. From the **ORGANISATION** group in the sidebar, click **Organisations**.
2. Click the name of your Organisation in the list. The page loads with the title **Organisation Overview**.
3. From the overview header, click **Members**, **Teams**, **Roles**, or **Settings** to drill into the corresponding sub-surface.

![Figure 11-2. The Organisation Overview detail page.](.gitbook/assets/screenshots/provider/admin-organisations-4.png)

The numbered callouts in Figure 11-2 are:

1. **Organisation Overview** — The page heading. The name of the Organisation appears alongside it.
2. **Status badge** — Active, Suspended, or Pending. Mirrors the column on the Organisations list.
3. **Members count** — The number of members currently in the Organisation. Click to open the Members list.
4. **Teams count** — The number of teams in the Organisation. Click to open the Teams list.
5. **Edit organisation** — Opens the form covered later in [Configure Organisation-wide preferences](#configure-organisation-wide-preferences).
6. **Sub-section tabs** — Members, Teams, Roles, and Settings tabs that scope the page to one entity at a time.

> **Result:** You are inside your Organisation's working area, with a link to every sub-surface you need for the rest of this chapter.

> **Tip:** Bookmark the Organisation Overview page in your browser. Most of your day-to-day administrative tasks start there and branch out.

#### Verify

1. Confirm the heading reads **Organisation Overview** and your Organisation's name appears alongside it.
2. Confirm the **Members** and **Teams** counts match what you expect — flag any drift to your Org Admin.
3. Click **Members** in the sub-section tabs and confirm the Members list opens scoped to this Organisation.

#### Configure Organisation-wide preferences

Configure Organisation-wide preferences when you need to set defaults that apply to every API, Product, and member created inside your Organisation — such as enforcing a seat limit, enabling Organisation-level branding, or auto-assigning a default role to new sign-ups from your domain.

The settings live under the **SETTINGS** group, not the Organisation Overview page. The **Organisation Overview** page edits the Organisation's name, theme colour, and seat limit on a per-Organisation basis. The **Organisation Settings** page covers platform-wide toggles that apply across every Organisation.

#### Before you start

- **Decide which toggles you want on.** The toggles change the experience for every member of every Organisation on this marketplace. Read the descriptions carefully before flipping them.
- **Confirm your role.** Some toggles are restricted to Portal Admin. As an API Provider you see the page in read-only mode for those rows.

To configure Organisation-wide preferences:

1. From the left sidebar, expand the **SETTINGS** group.
2. Click **Organisation**. The page loads with the title **Organisation Settings**.
3. Tick or untick the toggles described in the figure callouts.
4. Choose a value from the **Default organisation role** dropdown.
5. Click **Save configuration**.

![Figure 11-3. The Organisation Settings page.](.gitbook/assets/screenshots/provider/admin-config-marketplace-organisation.png)

The numbered callouts in Figure 11-3 are:

1. **Enforce seat limit** — When ticked, the marketplace blocks invites once an Organisation reaches its seat count. When unticked, invites continue and you are billed for the overage.
2. **Enable organisation branding** — When ticked, each Organisation can override the marketplace logo, theme colour, and favicon for its own members. When unticked, every Organisation inherits the marketplace-wide branding.
3. **Enable organisation homepage** — When ticked, each Organisation can pick a custom landing page for its members on sign-in. When unticked, all members land on the marketplace-wide home page.
4. **Auto-assign by domain** — When ticked, a user signing up with an email matching an Organisation's verified domain is auto-added to that Organisation. When unticked, every Organisation membership is by explicit invite.
5. **Enable partner registration** — When ticked, an external partner can request to join an Organisation through a public form. The Org Admin approves or declines.
6. **Enable audit logging** — When ticked, member, role, and team changes are written to an audit log retained for 90 days. Recommended for production.
7. **Default organisation role** — The role assigned to a new member when no role is picked on the invite. Pick the lowest-privilege role that lets a new member do their job.

> **Result:** The toggles save and apply on the next page load. New members and new Organisations pick up the new defaults immediately; existing members keep their current roles.

> **Note:** The per-Organisation theme colour, favicon, and homepage are set on the Organisation Overview's **Edit organisation** form, not on this page. This page controls whether per-Organisation branding is available at all.

> **Caution:** Turning **Enable audit logging** off discards future audit entries. Past entries remain searchable until they age out of the 90-day window. Leave logging on in any production deployment.

#### Verify

1. Confirm the **Configuration saved** banner appears after you click **Save configuration**.
2. Reload the page and confirm every toggle holds its new state.
3. Sign in as a freshly-invited test user and confirm they land on the **Default organisation role** you picked.

Related tasks:

- [Open the Organisation list](#open-the-organisation-list)
- [Configuring access and storefront branding](configuring-access-and-storefront-branding.md#configuring-access-and-storefront-branding)

## Inviting and managing members

The **Organisation Members** page lists every user linked to your Organisation, their role, their team membership, their status, and their last sign-in. From this one page you invite new members, change a member's role or team, suspend a member, or remove them.

#### Invite a colleague to your organisation

Invite a colleague when they need to sign in to the marketplace and act on behalf of your Organisation — importing APIs, approving subscriptions, designing Products, or monitoring traffic.

#### Before you start

- **Have your colleague's work email address.** The invite is sent by email. If the colleague does not yet have a marketplace account, the same email becomes the unique identifier for the new account.
- **Decide on a role before inviting.** The four built-in roles — API Provider, Org Admin, API Consumer, Portal Admin — grant very different permissions. Pick the smallest role that lets the colleague do their job. Roles are covered in [Recognise the built-in roles](#recognise-the-built-in-roles) below.
- **Decide whether they go straight into a team.** If you already know the colleague will work on a specific area — say, the Payments team — pick the team on the invite form so they land with the right scope on first sign-in.
- **Confirm your Organisation has seats available.** When **Enforce seat limit** is on, the form blocks the invite once the seat count is reached. Check the **Members** counter on the Organisation Overview before inviting a large group.

To invite a colleague:

1. From the **ORGANISATION** group in the sidebar, click **Members**.
2. The page loads with the title **Organisation Members**.
3. Click **Invite member**.
4. Enter the colleague's email in the **Email** field.
5. Pick a role from the **Role** dropdown.
6. Optional. Pick a team from the **Also add to team (optional)** dropdown to scope the colleague on first sign-in.
7. Click **Send invitation**.

![Figure 11-4. The Organisation Members list.](.gitbook/assets/screenshots/provider/admin-organisations-members.png)

The numbered callouts in Figure 11-4 are:

1. **Organisation Members** — The page heading. Each row below is one member.
2. **Invite member** — Opens the invite form covered in steps 3 to 7 above.
3. **Name** — The member's full name as it appears in their profile.
4. **Email** — The address the marketplace mails notifications to.
5. **Role(s)** — The roles assigned to the member. A member can hold more than one role.
6. **Teams** — The teams the member belongs to inside the Organisation.
7. **Status** — Pending (invite sent, not accepted), Active (accepted and signed in at least once), or Suspended (sign-in blocked, data preserved).
8. **Last sign-in** — The most recent sign-in timestamp. Useful for spotting dormant members before a seat-count audit.
9. **Edit** — Opens the member's detail page where you change role and team membership.
10. **Remove** — Removes the member from the Organisation. The marketplace prompts you to confirm.

> **Result:** Your colleague receives an email invitation. Once they accept and either create an account or sign in with an existing one, they appear in the **Organisation Members** list with the role and team you picked.

> **Note:** Inviting a colleague does not create a marketplace login from scratch. It links a new or existing account to your Organisation under the role you picked. If your Organisation uses single sign-on, your colleague signs in through your identity provider and the marketplace links the account on first sign-in.

> **Tip:** Send invites in small batches with the same role and team rather than one big batch with mixed roles. The form accepts multiple email rows; keeping each batch homogeneous makes the audit log easier to read.

#### Verify

1. Confirm a new row for the invitee appears in **Organisation Members** with status **Pending**.
2. Ask your colleague to confirm the invitation email arrived in their inbox.
3. Once they accept, refresh the list and confirm the row's status changes to **Active** and the role and team you picked are shown.

#### Review who is in your organisation

Review the Members list when you need an audit — who has API Provider access, who has been dormant for ninety days, who is still pending an invite. The list is the canonical record.

To review who is in your Organisation:

1. From the **ORGANISATION** group in the sidebar, click **Members**.
2. Sort the list by **Last sign-in** ascending to spot dormant accounts.
3. Filter by **Status = Pending** to find invites your colleagues have not yet accepted.
4. Filter by **Role = Org Admin** to see who can change settings or invite further members.

> **Tip:** Make this review a monthly habit. Stale memberships are a common source of seat-limit overruns and a security exposure when staff change roles.

#### Change a member's role

Change a member's role when their responsibilities change — for example, when an API Consumer is promoted to API Provider, or when an outgoing Org Admin steps down to API Provider.

#### Before you start

- **Pick the smallest role that does the job.** A reader who only needs analytics does not need full API Provider write access. If a finer-grained custom role exists in your Organisation, use that instead.
- **Tell the member.** A role change takes effect on their next sign-in. Send them a heads-up so they expect new menu items to appear or disappear.

To change a member's role:

1. From the **ORGANISATION** group, click **Members**.
2. Locate the member's row. Click **Edit** at the right of the row.
3. The page navigates to the member's edit form.
4. Pick a new role from the **Role(s)** multi-select. Hold cmd or ctrl to add a second role.
5. Optional. Add or remove team memberships in the **Teams** multi-select.
6. Click **Save**.

> **Result:** The member's role is updated. The change takes effect on their next request — they may need to refresh the browser to see new menu items appear or disappear.

> **Note:** Custom roles defined by your Org Admin appear in the same dropdown alongside the four built-in roles. See [Create a custom role](#create-a-custom-role) below.

> **Tip:** Start narrow. A role can be widened later in the same dropdown. The damage from a too-wide role granted by mistake is harder to undo.

#### Verify

1. Confirm the member's row in **Organisation Members** shows the new role.
2. Ask the member to refresh their browser and confirm the menu items they expect to gain or lose match the new role.
3. Open the audit log and confirm the role change is recorded against your username with the correct timestamp.

#### Remove a member from your organisation

Remove a member when they have left your company, when they no longer work on the marketplace, or when their account has been compromised and you need to cut access fast.

#### Before you start

- **Confirm the member is no longer the owner of any live App or subscription.** Reassign owners on the Manage Apps page before removing the account, or you risk orphaning consumer subscriptions.
- **Have the member's email address copied.** The confirmation dialog asks you to type the email verbatim to prevent an accidental click.
- **Tell the member.** Even a hostile-departure removal is easier when the affected person hears it from you rather than seeing the access drop without warning.

> **Caution:** Removal is reversible (you can re-invite the member) but the side effects are not always reversible. Subscriptions and Apps that the member personally owns may be detached when their account is removed. Migrate App ownership to a colleague before removal.

To remove a member:

1. From the **ORGANISATION** group, click **Members**.
2. Locate the member's row.
3. Click **Remove** at the right of the row.
4. Confirm in the dialog. The marketplace asks you to type the member's email to confirm an irreversible action.

> **Result:** The member loses access to the Organisation. Their personal account remains so they can sign in to other Organisations they belong to, but they no longer see your Organisation's APIs, Products, subscriptions, or members.

> **Note:** A removed member's audit log entries are preserved. The audit retains a record of who did what, even after the actor has been removed.

#### Verify

1. Confirm the member's row no longer appears in **Organisation Members**.
2. Ask the removed user to confirm they no longer see your Organisation in the sidebar switcher when they sign in.
3. Open the audit log and confirm the removal is recorded against your username with the reason captured.

## Organising members into Teams

Teams are an optional layer between an Organisation and an individual member. A team is a named sub-group that scopes which APIs and Products its members see in the catalog. Use teams when one Organisation runs more than one product line and you want each product line's APIs to stay private to its own crew.

#### Create a Team

Create a team when you need to scope APIs or Products to a subset of your Organisation — for example, when the Payments crew should not see the experimental Identity APIs that the Identity crew is building.

#### Before you start

- **Pick a clear name.** Team names appear in dropdowns on every API and Product visibility picker. Keep them short and recognisable — *Payments*, *Identity*, *Cards* — rather than internal codenames.
- **Have a one-line description ready.** It appears in the team list and helps members joining later understand what the team owns.

To create a team:

1. From the **ORGANISATION** group, click **Teams**.
2. The page loads with the title **Organisation Teams**.
3. Click **Add team**.
4. Enter the team's **Name**.
5. Enter a one-line **Description**.
6. Click **Save**.
7. The page navigates to the new team's detail page, where you add members.

![Figure 11-5. The Organisation Teams list.](.gitbook/assets/screenshots/provider/admin-organisations-teams.png)

The numbered callouts in Figure 11-5 are:

1. **Organisation Teams** — The page heading. Each row below is one team in the Organisation.
2. **Add team** — Opens the create-team form covered in steps 3 to 6 above.
3. **Name** — The team's display name. Click the name to open the team detail page.
4. **Members** — The current member count for the team. Click to jump to the team's member list.
5. **Scoped APIs** — The number of APIs flagged Org Level and scoped to this team.
6. **Scoped Products** — The number of API Products scoped to this team.
7. **Created** — The date the team was created.

> **Result:** The team exists. The next step is to add members and to scope APIs or Products to it.

#### Verify

1. Confirm the new team appears in **Organisation Teams** with the name and description you entered.
2. Confirm the team detail page opens and the **Members** count starts at zero.
3. Confirm the team's name appears in the **Teams** picker on an API or Product Visibility form.

#### Add members to a Team

Add members to a team after the team is created, or when an existing member's responsibilities expand to a new product line.

#### Before you start

- **Confirm every prospective member is already in your Organisation.** The picker lists Organisation members only. If a colleague is missing, invite them first.
- **Have the team's name picked.** Picking the right team is the only point of failure here — adding a member to the wrong team grants them visibility you did not intend.
- **Sanity-check the visibility implications.** Adding a member to a team gives them every Product and API scoped to that team. Review the team's scoped resources before you add anyone.

To add members:

1. From the **ORGANISATION** group, click **Teams**.
2. Click the team's name to open the team detail page.
3. Click **Add members**.
4. Pick one or more members from the dropdown of Organisation members.
5. Click **Save**.

> **Result:** The picked members now belong to the team. They see any APIs and Products scoped to the team on their next page load.

> **Note:** A member can be on more than one team. The visibility rule is additive — a member sees the union of every team's scoped resources.

> **Tip:** Add yourself to every team as you create it, even if you do not actively work in that product line. As an API Provider you may need to triage a subscription request or a governance violation before someone on the team is online.

#### Verify

1. Confirm the team detail page now lists each new member.
2. Ask one of the added members to refresh the catalogue and confirm Org-Level resources scoped to the team are now visible to them.
3. Confirm the **Members** count on **Organisation Teams** matches the new size.

#### Scope APIs and Products to a Team

Scope an API or Product to a team to make it visible only to that team's members. Use scoping when an asset is in early development, when it is a private internal API, or when one product line should not see another product line's roadmap.

#### Before you start

- **Confirm the team exists and has the right members.** Scoping to an empty team makes the resource invisible to everyone except admins.
- **Confirm the asset is at the right stage.** Public APIs ignore team scope; an Internal API is visible to every Org member regardless of team.
- **Tell the affected teams.** Flipping scope on a published asset hides it from members not on the team — communicate before you save.

To scope a resource to a team:

1. Open the API or Product you want to scope. See [Publishing your first API](publishing-your-first-api.md#publishing-your-first-api) for APIs and [Reviewing API Products and Plans](reviewing-api-products-and-plans.md#reviewing-api-products-and-plans) for Products.
2. In the **Visibility** picker, choose **Org Level**. A second **Teams** picker appears below.
3. Pick one or more teams in the **Teams** picker.
4. Click **Save**.

> **Result:** The API or Product is visible only to members of the picked teams, plus to any Org Admin and Portal Admin. Members not on those teams do not see it in the catalog or in search.

> **Note:** Team scoping only applies when **Visibility = Org Level**. Public APIs are visible to everyone on the marketplace; Internal APIs are visible to every member of the Organisation. Both ignore team scope.

> **Caution:** Removing a team from a published API or Product hides it from members of that team on their next page load. Their existing subscriptions still work — the API does not stop responding — but they cannot find the asset in the catalog any more. Tell affected teams before flipping team scope on a live asset.

#### Verify

1. Sign in as a member of the picked team and confirm the asset appears in the catalogue and search.
2. Sign in as a member outside the picked team and confirm the asset is not visible.
3. Open the asset's audit history and confirm the Visibility change is recorded against your username.

## Working with Roles

Roles bundle permissions. A role decides which menu items appear, which buttons are enabled, and which records can be edited. The marketplace ships with four built-in roles that cover most needs, plus a custom-role builder for the rare case where you need a finer slice.

#### Recognise the built-in roles

Recognise the four built-in roles before you assign any of them. They are not interchangeable.

To open the Roles list:

1. From the **ORGANISATION** group, click **Custom Roles**.
2. The page loads with the title **Custom Roles**.
3. Scan the table to see every role available in your Organisation — built-in and custom.

![Figure 11-6. The Custom Roles list with the four built-in roles.](.gitbook/assets/screenshots/provider/admin-organisations-4-roles.png)

The four built-in roles are:

1. **API Provider** — Imports APIs from a connected gateway, runs governance checks, designs API Products and plans, approves subscription requests, monitors analytics. The audience this guide is written for. Scoped to one Organisation.
2. **Org Admin** — Everything an API Provider can do, plus: invite members, change roles, define custom roles, configure single sign-on, brand the storefront, set the Organisation homepage. Scoped to one Organisation.
3. **API Consumer** — Browses the catalog, subscribes to plans, manages personal Apps and API keys, views their own usage analytics. No admin-side access. Scoped to one Organisation but consumers can hold memberships in many.
4. **Portal Admin** — Cross-Organisation administrator. Operates across every Organisation on the platform, configures marketplace-wide policies, manages the Organisation list, runs platform-wide system notifications. Reserved for the platform team running the marketplace itself. Out of scope for most teams.

The numbered callouts in Figure 11-6 are:

1. **Custom Roles** — The page heading.
2. **Role name** — The display name. Click to open the role's permission editor.
3. **Source** — Built-in or Custom. Built-in roles cannot be renamed or deleted; their permission set can only be viewed.
4. **Members** — The number of members currently holding the role. Useful when retiring a role.
5. **Add role** — Opens the create-role form covered in [Create a custom role](#create-a-custom-role) below.

> **Note:** Most Organisations get along with the four built-in roles for the lifetime of the marketplace. Add a custom role only when you have a real need — see the next section.

#### Create a custom role

Create a custom role when one of the built-in roles is too wide or too narrow for a colleague's job. A common case is an *Approver* role that can approve subscription requests but cannot import APIs or change governance.

#### Before you start

- **Write the role's purpose in one sentence.** *"Approves subscription requests for the Payments team. Cannot edit APIs or governance rules."* That sentence becomes the role's description and gets you a faster review by the colleague who reports to you.
- **List the permissions the role needs.** Check the four built-in roles' permission sets first. Often a custom role is a built-in role plus or minus one or two permissions.

To create a custom role:

1. From the **ORGANISATION** group, click **Custom Roles**.
2. Click **Add role**.
3. Enter a **Role name** — short, recognisable.
4. Enter a one-line **Description** of what the role can do.
5. Tick the permission checkboxes the role needs. The list is grouped by surface — APIs, Products, Subscriptions, Members, Teams, Settings.
6. Click **Save**.
7. Assign the new role to one or more members. See [Change a member's role](#change-a-members-role).

> **Result:** The new role is available in the **Role(s)** dropdown on the Members page and on the invite form. Colleagues you assign it to pick up the new permissions on their next sign-in.

> **Tip:** Test a new custom role against a sandbox member account first. Sign in as that member and confirm the menu items, buttons, and records behave as you expect before assigning the role to a real colleague.

> **Caution:** A permission ticked by mistake on a custom role takes effect for every member holding that role on their next request. Review the permission grid carefully before saving.

#### Verify

1. Confirm the new role appears in the **Custom Roles** list with the correct name and **Source = Custom**.
2. Open the role's permission editor and confirm the ticked checkboxes match your intent.
3. Assign the role to a sandbox member and confirm the menu items, buttons, and records they see align with your design.

#### Assign a role to a member

Assigning a role to a member uses the same flow as changing a role. See [Change a member's role](#change-a-members-role) for the steps. The custom role you created appears in the same **Role(s)** multi-select alongside the four built-in roles.

## Adding and removing Organisations (Portal Admin)

This section covers a Portal-Admin-only task. As an API Provider on a single Organisation, the **Add organisation** button does not appear in your sidebar. The information is included so you understand what your Portal Admin sees and can plan a tenant onboarding conversation.

#### Add a new Organisation

Add a new Organisation when a new tenant is being onboarded to the marketplace — a separate company, a separate business unit, or a separate brand that needs its own isolated set of APIs, Products, members, and branding.

#### Before you start

- **Confirm the new tenant's name and contact owner.** The owner becomes the first **Org Admin** of the new Organisation and receives the welcome email.
- **Know the seat limit.** Pick a number that matches the tenant's contracted seat count. The seat limit can be widened later but should not be set arbitrarily high.
- **Confirm domains for auto-assign by domain.** If you have **Auto-assign by domain** turned on, every email matching the listed domains is auto-added to the new Organisation on sign-up. List only domains the tenant fully owns.

To add an Organisation:

1. From the **ORGANISATION** group, click **Organisations**.
2. Click **Add organisation**.
3. The page loads with the title **Create Organisation**.
4. Enter the **Organisation name**.
5. Enter a **Description**.
6. Pick a **Status** — Active for an immediately-live tenant, Pending while you finish setup.
7. Enter a **Seat limit** matching the tenant's contracted seat count.
8. Click **Save**.
9. Open the new Organisation and click **Invite member** to invite the first Org Admin. See [Invite a colleague to your organisation](#invite-a-colleague-to-your-organisation).

![Figure 11-7. The Create Organisation form.](.gitbook/assets/screenshots/provider/admin-organisations-add.png)

The numbered callouts in Figure 11-7 are:

1. **Organisation name** — The tenant's display name. Appears in the sidebar switcher, on every API and Product the tenant owns, and on the storefront. Required.
2. **Description** — A one-line note for your own records — what business the tenant is in, who the owner contact is, what stage the onboarding is at. Visible only to admins.
3. **Status** — Active makes the Organisation live immediately. Pending hides it from the catalog while you finish setup. Required.
4. **Seat limit** — The maximum number of members the Organisation can hold when **Enforce seat limit** is on. Leave blank for no limit.
5. **Save** — Creates the Organisation. The first Org Admin still needs to be invited as a separate step.

> **Result:** The new Organisation exists. It appears in the Organisations list with status Pending or Active and zero members until you invite the first Org Admin.

> **Note:** Multi-tenancy is by Organisation. Each Organisation is isolated — its APIs, Products, subscriptions, members, teams, and branding are all scoped to one Organisation. Cross-Organisation visibility happens only when a resource is flagged **Public** on its visibility picker.

> **Tip:** Set the new Organisation to **Status = Pending** while you finish onboarding — invite the Org Admin, set theme colour, configure single sign-on. Flip to **Active** once the Org Admin has signed in and confirmed the tenant is ready to go live.

> **Caution:** Deleting an Organisation deletes every API, Product, subscription, App, and member inside it. The action is irreversible and is restricted to Portal Admin. Suspend the Organisation first and confirm with the tenant before deleting.

#### Verify

1. Confirm the new Organisation appears in the **Organisations** list with the chosen name and status.
2. Switch into the new Organisation from the sidebar switcher and confirm every count (Members, APIs, Products) starts at zero.
3. Send the first **Org Admin** invitation and confirm they accept and land on the new tenant's Overview page.

## Next steps

- **[Configuring access and storefront branding](configuring-access-and-storefront-branding.md#configuring-access-and-storefront-branding)** — Brand the new Organisation to match its tenant identity before consumers arrive.
- **[Reviewing API Products and Plans](reviewing-api-products-and-plans.md#reviewing-api-products-and-plans)** — Once your team is in place, design the Products and plans they will publish to consumers.
- **[Publishing your first API](publishing-your-first-api.md#publishing-your-first-api)** — Members with the API Provider role can now follow the publish flow against the gateway you connected earlier.
