# Getting started

Before connecting a gateway or importing an API, spend ten minutes orienting yourself in the marketplace. Sign in once, learn the four sidebar groups used in routine work, and recognise the common UI patterns — status badges, search and filter controls, action menus — that repeat on every list page. This chapter walks you through your first sign-in, the Portal Admin dashboard you land on, the four left-sidebar groups (**API MANAGEMENT**, **ADMINISTRATION**, **SETTINGS**, **ORGANISATION**), and the conventions you will see on every page from chapter 3 onwards.

## Signing in

The marketplace runs as a hosted product. Sign in with the credentials your Org Admin issued — either a marketplace username and password, or your corporate SSO identity if your Organisation has SAML configured.

#### Sign in to the marketplace

Use this task on your first day with the product, or any time you need to switch from a consumer-side session to a provider-side session.

#### Before you start

- **Confirm your account has the API Provider or Portal Admin role.** The provider-side sidebar groups described in this chapter are hidden for users who hold only the API Consumer role. If API MANAGEMENT is not visible in the sidebar after sign-in, ask your Org Admin to grant the API Provider role.
- **Identify your Organisation's marketplace URL.** The reference URL throughout this guide is `https://<your-portal-domain>/`, but your production URL will differ. The path conventions (`/admin/dashboard`, `/admin/apim/connections`, and so on) are the same.
- **Decide whether to use SSO or a local password.** If your Organisation has SAML configured, the sign-in form redirects you to your identity provider. Otherwise, sign in with the email and password set when you accepted your invitation.

To sign in:

1. Open your marketplace URL in a browser. The landing page is the public consumer-facing storefront.
2. Click **Login** in the top-right corner of the page.
3. Enter your email address and password, or click the SSO button if your Organisation uses SAML.
4. If your Organisation has multi-factor authentication enabled, complete the second factor when prompted.
5. After authentication, the marketplace lands at one of two surfaces:
   - As an API Provider or Portal Admin, you land on the admin dashboard at `/admin/dashboard`.
   - As an API Consumer, you land on the consumer dashboard. If this occurs despite holding the Provider role, navigate to `/admin/dashboard` directly in the address bar.

> **Result:** You are signed in and viewing the admin dashboard described in the next task.

> **Tip:** Bookmark `/admin/dashboard` rather than the marketplace home page. As an API Provider, this is the most frequent destination.

> **Note:** Sign-in sessions expire after a period of inactivity set by your Org Admin (typically 8 hours). When the session expires you are returned to the public landing page; sign in again to continue.

Related tasks:

- [Recognise the Portal Admin dashboard](#recognise-the-portal-admin-dashboard)
- [Find your way around the sidebar](#find-your-way-around-the-sidebar)

## Recognising the dashboard

After sign-in, the page that loads at `/admin/dashboard` is the entry point. It summarises the health of your APIs, the governance scans that have run recently, the gateways you have connected, and the subscriptions consumers have requested. Every panel on the dashboard links to the deeper screen that owns the data.

#### Recognise the Portal Admin dashboard

Use this task to learn what each dashboard panel does, so you can identify at a glance whether something requires attention.

To recognise the dashboard:

1. After sign-in, look at the page heading. It reads API Marketplace Dashboard.
2. Scan the five panels arranged below the heading. Each panel is a snapshot; click any heading to drill into the full screen.
3. Note the four sidebar groups on the left — **API MANAGEMENT**, **ADMINISTRATION**, **SETTINGS**, **ORGANISATION**. Every task in this guide starts in one of these groups.
4. Look at the top-right corner. The first chip shows your Organisation name (for example QAOrganisationOrganisation); the second chip shows your name and a dropdown for My account and Logout.

![Figure 2-1. The Portal Admin dashboard at `/admin/dashboard`.](.gitbook/assets/screenshots/provider/admin-dashboard.png)

The numbered callouts in Figure 2-1 are:

1. **API Marketplace Dashboard** — The page heading. The dashboard is recognisable by this title.
2. **API Health Overview** — A panel that summarises the count of APIs by status (Draft, Published) and any health flags. Click the heading to open Manage APIs.
3. **Recent Governance Scans** — A panel listing the most recent governance scan runs with their average score. Click a row to open the detailed report; the panel is the shortcut into chapter 5 [Reviewing API governance](reviewing-api-governance.md#reviewing-api-governance).
4. **Gateway Connections** — A panel listing every connected source with status. Click the heading to open Manage API Sources — the entry point for chapter 3 [Connecting your first gateway](connecting-your-first-gateway.md#connecting-your-first-gateway).
5. **Recent Subscriptions** — A panel showing the latest subscription requests, with Pending rows highlighted. Click any Pending row to jump to the approval flow described in chapter 8 [Onboarding your first consumer](onboarding-your-first-consumer.md#onboarding-your-first-consumer).
6. **Quick Actions** — A panel with shortcut buttons for the most common create flows: Create API, Create API Product, Create Article.

> **Result:** You can read each panel at a glance and click straight through to the underlying screen.

> **Tip:** If a panel appears empty during onboarding, this is expected. The dashboard fills in as you connect a gateway, import APIs, run scans, and onboard consumers across the chapters that follow.

> **Note:** The dashboard refreshes on page reload; it does not push live updates. If you have made a change elsewhere and want to see it on the dashboard, reload.

Related tasks:

- [Find your way around the sidebar](#find-your-way-around-the-sidebar)
- [Recognise common UI patterns](#recognise-common-ui-patterns)

## Finding your way around

Every action in this guide starts from one of the four left-sidebar groups. Learn what each group holds and screen-hunting is no longer required.

#### Find your way around the sidebar

Use this task during onboarding to map every screen you will touch in the rest of the guide to its sidebar group.

To find your way around the sidebar:

1. Look at the left edge of the page. The sidebar shows four group headings, each with a list of links beneath it. The headings are **API MANAGEMENT**, **ADMINISTRATION**, **SETTINGS**, and **ORGANISATION**.
2. Hover over a group to reveal its links if they are collapsed. Click a group heading to expand or collapse it.
3. Match each group to the chapters it owns:
   - API MANAGEMENT holds Manage API Sources, Manage APIs, Manage API Products, API Governance Report. This group covers chapters 3 through 7.
   - ADMINISTRATION holds Manage Apps, Subscriptions, Product Subscriptions, Provider Analytics, MCP Servers. This group covers chapters 8 through 10.
   - SETTINGS holds Site Settings, Auth & Branding, API Linting, Webhooks, System Notifications, SAML IdP, SMTP. This group is mixed scope: some links are Provider-accessible, some require Org Admin.
   - ORGANISATION holds Organisation, Members, Teams, Roles. This group covers chapter 11.
4. Locate the **Collapse sidebar** control at the bottom of the sidebar. Click it when more horizontal space is required; the sidebar collapses to icons-only and remembers the selection.

> **Result:** You know where every chapter's tasks live in the sidebar.

> **Note:** A link missing from the sidebar indicates the current role does not grant access to it. Consult your Org Admin to identify which role grants the link before assuming a feature is missing from the product.


Related tasks:

- [Recognise common UI patterns](#recognise-common-ui-patterns)
- [Connecting your first gateway](connecting-your-first-gateway.md#connecting-your-first-gateway)

## Recognising the conventions

Every list page in the marketplace — APIs, API Products, subscriptions, members — uses the same controls. Learn them once and every screen is familiar.

#### Recognise common UI patterns

Use this task before chapter 3 so the screens that follow are familiar.

To recognise the conventions:

1. **Status badges.** Look for coloured pill labels near a record's title. APIs and API Products use Draft (grey), Published (green), and Archived (amber). Subscriptions use Pending (amber), Active (green), Suspended (grey), Revoked (red).
2. **Search and filter.** Every list page has a search field at the top and a row of filter dropdowns below it. The search matches title and description; the filters narrow by status, gateway, owner, or date range. The filter set persists per page until cleared or until sign-out.
3. **Action menus.** Each row in a list ends with an action column. The action column either lists the actions inline (Edit, Delete) or shows a vertical-dots menu that reveals them on click. Destructive actions (Delete, Revoke) always prompt for confirmation before they run.
4. **Form layout.** Create and edit forms place required fields first, optional fields below, and a fixed footer with Save and (where relevant) Test connection buttons. Forms do not autosave — changes apply only when Save is clicked.
5. **Breadcrumbs.** The breadcrumb above each page title shows the trail from Home to the current screen. Click any breadcrumb segment to navigate up.
6. **Notification toasts.** A green toast in the top-right corner confirms a successful save; a red toast names the failing field. Toasts dismiss automatically after a few seconds.

> **Result:** You can navigate any list page, run any create or edit flow, and read the result without re-learning the controls each time.

> **Tip:** If a form's Save button is greyed out, scroll up to locate a required field with a red border. The field name and its error message appear above the form on submit.

> **Note:** A few specialist surfaces (the Governance Report, the Provider Analytics dashboard, the MCP Servers page) layer charts and tables on top of these conventions. The same search, filter, and action patterns still apply to the underlying tables.

Related tasks:

- [Connecting your first gateway](connecting-your-first-gateway.md#connecting-your-first-gateway)
- [Importing your first API](importing-your-first-api.md#importing-your-first-api)

## Switching context and finding things fast

Two skills repay the time spent learning them: switching organisations when you are a member of more than one, and jumping straight to a screen by name without navigating the sidebar.

#### Switch between organisations

Use this task when you are a member of more than one Organisation in the marketplace and need to change which one you are acting in.

#### Before you start

- **Confirm membership in more than one Organisation.** If your name chip in the top-right corner shows only one Organisation name, there is nothing to switch between. Membership is granted by an Org Admin in each Organisation; request access from them if required.
- **Recognise that the active Organisation scopes everything.** APIs in Manage APIs, subscriptions in Subscriptions, members in Organisation > Members — all are filtered to your active Organisation. Switching Organisations swaps the entire dataset.

To switch organisations:

1. Click the Organisation chip in the top-right corner of any page (the chip immediately to the left of your name chip). It typically reads something like QAOrganisationOrganisation.
2. In the dropdown, look at the list under Switch organisation. Each Organisation you belong to appears once with a checkmark next to the active one.
3. Click the Organisation to switch to. The page reloads and the chip updates to show the new active Organisation.
4. Confirm the switch by glancing at the dashboard tiles. The numbers under API Health Overview and Recent Subscriptions should reflect the new Organisation's data.

> **Result:** Every screen visited subsequently is scoped to the newly-active Organisation, until another switch or sign-out.

> **Note:** Sessions are scoped to a single Organisation. If you open a second browser tab and switch the Organisation in that tab, the original tab's Organisation also changes on next reload. To work in two Organisations at once, use two separate browser profiles.

> **Tip:** If you spend most of your day in one Organisation but occasionally need to act in a second one, set the more frequently-used Organisation as your default in My account > Preferences. The marketplace lands there at every sign-in regardless of which Organisation was switched to last.

Related tasks:

- [Use search to jump to any screen](#use-search-to-jump-to-any-screen)
- [Managing your team](managing-your-team.md#managing-your-team)

#### Use search to jump to any screen

Use this task when you know the name of a screen, an API, an API Product, or a member but do not want to navigate through the sidebar.

To use search:

1. Press the `/` (forward-slash) key from any page. The cursor jumps to the global search field at the top of the page.
2. Start typing. The results list opens beneath the field as you type, grouped by type: Pages, APIs, API Products, Apps, Members, Notifications.
3. Use the up and down arrow keys to highlight a result.
4. Press **Enter** to navigate to the highlighted result, or click any result with the mouse.
5. Press **Escape** to close the results list without navigating.

> **Result:** You arrive on the searched screen without clicking through the sidebar.

> **Tip:** Search is keyboard-first. Pressing `/` from anywhere is faster than reaching for the sidebar, and the up/down/Enter combination avoids the mouse.

> **Note:** Search results respect your role and Organisation scope. If an expected result is not visible, you may lack access to it, or it may belong to a different Organisation than the one you are currently in.

Related tasks:

- [Find your way around the sidebar](#find-your-way-around-the-sidebar)
- [Recognise common UI patterns](#recognise-common-ui-patterns)
