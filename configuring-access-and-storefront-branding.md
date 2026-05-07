This chapter covers the operator-facing surfaces under **SETTINGS** — sign-in methods, single sign-on (SSO/SAML), and how the public storefront looks. These are typically Org Admin or Portal Admin tasks. If you are an API Provider without those permissions, your Org Admin owns these screens and you can request changes from them. The chapter walks through enabling email-and-password sign-in, configuring SAML against a corporate identity provider, branding the public catalog with your logo and colours, and switching the active theme. By the end of the chapter, your marketplace will sign users in the way your security team requires and look like part of your company rather than a default template.

You will learn:

- How to set the messaging that appears on the sign-in, registration, and forgot-password screens.
- How to configure SAML against your corporate identity provider, including attribute mapping and a default first-sign-in role.
- How to test SAML safely before flipping it on for the full user base.
- How to upload a logo and favicon, set a brand-accurate accent colour, and switch themes.
- How to set site-wide preferences such as the site name, contact email, and default API visibility.
- How to define a custom role with a tailored permission set when the four built-ins do not fit.

Allow ~60 minutes when SAML is in scope; ~30 minutes for branding and basic preferences alone.

## Configuring sign-in

Sign-in lives in two places. The **Auth Page Branding** screen controls what users see on the login, register, and forgot-password screens. The **SSO Configurations** screen controls SAML federation against your corporate identity provider. Email-and-password sign-in is on by default; SAML is opt-in.

#### Configure sign-in methods

Use this task when you want to change the messaging shown on the sign-in, registration, and password-reset screens — for example, to add a "company use only" notice, an internal support contact, or a banner explaining which corporate credentials to use.

#### Before you start

- **Decide which sign-in methods you want exposed.** Email-and-password is on by default. If your organisation runs SSO, you typically still want a fallback for break-glass accounts; the two methods can co-exist.
- **Draft the wording in advance.** The messages on the sign-in screens are public-facing copy. Run them past your security or comms team before you paste them in.
- **Have your support email handy.** Users who land on the forgot-password screen need somewhere to escalate when the reset email does not arrive.

To configure sign-in screen messages:

1. From the left sidebar, expand **SETTINGS** and click **Basic Site Settings**.
2. Click the **Auth Page Branding** secondary tab at the top of the page.
3. Enter the message that should appear on the sign-in screen in the **Login page content** rich-text field.
4. Enter the message that should appear on the registration screen in the **Registration page content** rich-text field.
5. Enter the message that should appear on the forgot-password screen in the **Forgot password page content** rich-text field.
6. Click **Save configuration**.

![Figure 12-1. The Auth Page Branding screen.](.gitbook/assets/screenshots/provider/admin-config-devportal-site-settings-auth-branding.png)

The numbered callouts in Figure 12-1 are:

1. **Auth Page Branding** — The page heading. The three rich-text fields below it correspond to the three public-facing auth screens.
2. **Login page content** — A rich-text field. Whatever you enter renders on the sign-in screen, beneath the email and password fields. Use it for a company notice, a link to your IT helpdesk, or guidance on which corporate credentials to use.
3. **Registration page content** — A rich-text field. Renders on the new-user registration screen. Use it to set expectations about approval timelines or to point external developers at terms-of-use.
4. **Forgot password page content** — A rich-text field. Renders on the password-reset screen. Use it for the support email a user should contact when the reset email does not arrive.
5. **Save configuration** — Persists the changes. The new copy appears immediately on the public auth screens.

> **Result:** Your sign-in, registration, and forgot-password screens carry your messaging. The change is live for the next user who lands on those screens.

> **Note:** The fields accept rich text. You can include links, bold text, and bullet lists. Avoid embedded scripts or iframes — the marketplace strips them on save.

> **Tip:** Keep each message under three sentences. Users who reach the sign-in screen want to sign in, not read a wall of text.

#### Verify

1. Open the marketplace sign-in screen in a private browser window and confirm the new copy renders.
2. Repeat for the registration and forgot-password screens.
3. Confirm any embedded link in the rich-text fields opens the right destination.

Related tasks:

- [Configure SAML single sign-on](#configure-saml-single-sign-on)
- [Upload your logo and favicon](#upload-your-logo-and-favicon)

#### Configure SAML single sign-on

Use this task when your organisation requires every user to sign in through a corporate identity provider — Okta, Azure AD, Ping, ADFS, Auth0, or any other SAML 2.0 IdP. SSO replaces email-and-password authentication for federated users; the marketplace creates the local account on first sign-in.

#### Before you start

- **Have your IdP metadata URL or XML file ready.** Most IdPs expose a metadata endpoint at a URL like `https://idp.example.com/saml/metadata`. If your IdP does not expose one, export the metadata as XML and have the file open.
- **Know which attributes your IdP releases.** At minimum the marketplace needs an email and a display-name claim. A role or group claim is recommended; without one, every federated user lands on the default role.
- **Pick the default role for first-time SSO users.** New SSO users land on this role until an Org Admin promotes them. API Consumer is the safest default.
- **Have a Portal Admin local-password account ready as break-glass.** If SAML is misconfigured, this is the only way back in.

> **Caution:** SAML changes take effect immediately. Testing SAML in production without a verified configuration can lock everyone out except a Portal Admin with a local-password fallback. Always test SAML in a non-production environment first, and keep one local-password Portal Admin account active as your break-glass route.

To configure a SAML identity provider:

1. From the left sidebar, expand **SETTINGS** and click **SSO Configurations**.
2. Click **Add IdP**.
3. Enter a recognisable label in the **Identity Provider Name** field — for example, *Okta Production* or *Azure AD Corp*.
4. Enter the **Entity ID** issued by your IdP. It usually looks like a URL, e.g. `http://www.okta.com/exk1abc23`.
5. Paste your IdP's metadata URL into the **IdP Metadata URL** field, or upload the XML file in the **IdP Metadata XML** field.
6. Enter the **Single Sign-On Service URL** (the IdP endpoint the marketplace redirects to for sign-in).
7. Enter the **Single Logout Service URL** if your IdP supports logout federation. Leave blank if it does not.
8. Paste the IdP's signing certificate into the **X.509 Certificate** field. Include the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.
9. Map your IdP's attribute claims in the **Attribute Mapping** section: email, first name, last name, and (optionally) role or group.
10. Set **Default Role on First Sign-In** to the role new federated users should land on.
11. Tick **Just-in-Time Provisioning** to create marketplace accounts automatically on first sign-in. Untick it if accounts must be pre-provisioned by an Org Admin.
12. Click **Save**.

![Figure 12-2. The Add IdP form.](.gitbook/assets/screenshots/provider/admin-config-saml-idp.png)

The numbered callouts in Figure 12-2 are:

1. **Identity Provider Name** — A label that appears on the marketplace sign-in screen and in the SSO Configurations list. Pick a name that distinguishes IdPs if you run more than one.
2. **Entity ID** — The unique identifier your IdP advertises. Copy it verbatim from the IdP's metadata; mismatches here are the most common cause of SAML failures.
3. **IdP Metadata URL** — A live URL the marketplace fetches periodically. Preferred over uploading XML, because it picks up certificate rotations automatically.
4. **Single Sign-On Service URL** — The IdP endpoint for sign-in. The marketplace redirects the user here when they click the SSO button.
5. **X.509 Certificate** — The IdP's signing certificate. The marketplace verifies every SAML response against this certificate. Rotate it whenever the IdP rotates its keys.
6. **Attribute Mapping** — A set of fields that tell the marketplace which IdP claim contains the user's email, name, and (optionally) role. Defaults match Okta and Azure AD; customise for other IdPs.
7. **Default Role on First Sign-In** — The role new federated users land on. API Consumer is the safest default; an Org Admin can promote a user later.
8. **Just-in-Time Provisioning** — A checkbox. Tick to create accounts on first sign-in; untick to require pre-provisioning by an Org Admin.

> **Result:** SAML is configured. The next user who clicks the SSO button on the sign-in screen is redirected to your IdP, signs in there, and lands back on the marketplace under the default role.

> **Note:** The marketplace's Service Provider metadata is exposed at `/saml/sp/metadata` once SAML is enabled. Hand that URL to your IdP team — they will configure the marketplace as a registered Service Provider on the IdP side.

> **Tip:** If your IdP supports group claims, map a group like `marketplace-providers` to the API Provider role and a group like `marketplace-admins` to Org Admin. This lets you manage marketplace permissions from your IdP rather than on every user manually.

#### Verify

1. Confirm the new IdP appears in the **SSO Configurations** list with status **Active**.
2. Confirm the marketplace's Service Provider metadata at `/saml/sp/metadata` is reachable and matches what your IdP team has registered.
3. Run the round-trip in [Test SAML against your identity provider](#test-saml-against-your-identity-provider) before enabling the IdP for the full user base.

Related tasks:

- [Test SAML against your identity provider](#test-saml-against-your-identity-provider)
- [Configure sign-in methods](#configure-sign-in-methods)

#### Test SAML against your identity provider

Use this task to validate a SAML configuration before rolling it out organisation-wide. Skipping this step is the single largest cause of "everyone is locked out" incidents.

#### Before you start

- **Have a non-production marketplace.** Run the test there, not in production.
- **Have a test user in the IdP.** The user should be in whatever group or role you intend to map.
- **Open a private/incognito browser window.** This avoids stale session cookies from confusing the test.

To test SAML:

1. In a private browser window, open `<your-portal-domain>/user/login`.
2. Click the **Sign in with <your IdP name>** button.
3. The browser redirects to your IdP. Sign in with the test user's credentials.
4. The browser redirects back to the marketplace. Confirm the test user lands on the dashboard.
5. From the user menu in the top-right, confirm the email and display name match the IdP user.
6. Open the **Edit profile** page and confirm the role assigned matches the **Default Role on First Sign-In** value (or the mapped IdP group, if attribute mapping for role is in use).
7. Sign out. Confirm the marketplace returns you to the sign-in screen, not back to the IdP.

> **Result:** You have validated the round-trip — sign-in, account creation, role assignment, and sign-out. SAML is safe to enable for the full user base.

> **Note:** If the redirect from the IdP fails with a SAML response error, check the X.509 Certificate first. The second-most-common failure is a clock skew greater than 60 seconds between the IdP and the marketplace; if both run NTP this is rarely the cause.

> **Tip:** Repeat the test for each role you have mapped — one user in the API Consumer group, one in the API Provider group, one in the Org Admin group. This catches attribute-mapping mistakes that affect only some users.

> **Caution:** Once you turn on **Force SSO** for an organisation, email-and-password sign-in is disabled for that organisation's users. Confirm the round-trip works for at least three test users before flipping that switch.

#### Verify

1. Confirm sign-in, account creation, role assignment, and sign-out succeed for at least three test users covering different IdP groups.
2. Confirm your local-password break-glass Portal Admin account still signs in via the email-and-password fallback.
3. Confirm the round-trip works in a non-production environment before enabling SAML on production.

Related tasks:

- [Configure SAML single sign-on](#configure-saml-single-sign-on)
- [Configure sign-in methods](#configure-sign-in-methods)

## Branding the storefront

The storefront is what consumers see when they land on the marketplace homepage. Three knobs control the look: your logo and favicon, your brand colours, and the active theme.

#### Upload your logo and favicon

Use this task to replace the default DevPortal logo and favicon with your company's. The logo appears in the top-left of every page; the favicon appears in browser tabs and bookmarks.

#### Before you start

- **Have a transparent-background PNG or SVG of your logo.** SVG scales best for retina displays. PNG is fine if you do not have an SVG. Avoid JPEG — its background colour will clash with the storefront.
- **Have a square favicon source at 256×256 pixels minimum.** The marketplace down-samples for the actual favicon sizes; starting larger gives crisper results.
- **Know your logo's intended display height.** Around 40 pixels works for most marketplaces. A logo too tall pushes the navigation bar down and crowds the page.

To upload your logo and favicon:

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. Scroll to the **Logo image** section.
3. Untick **Use the logo supplied by the theme** to expose the upload field.
4. Click **Choose File** under **Upload logo image** and select your logo file.
5. Scroll to the **Favicon** section.
6. Untick **Use the favicon supplied by the theme** to expose the upload field.
7. Click **Choose File** under **Upload favicon image** and select your favicon source.
8. Click **Save configuration**.

![Figure 12-3. The Logo and Favicon sections of the Appearance screen.](.gitbook/assets/screenshots/provider/admin-config-devportal-site-settings-auth-branding.png)

The numbered callouts in Figure 12-3 are:

1. **Use the logo supplied by the theme** — A checkbox, ticked by default. Untick to upload a custom logo. Tick again later to revert to the theme default.
2. **Upload logo image** — A file picker. Accepts PNG, SVG, and JPEG; SVG is recommended. The uploaded file replaces the default logo on every page.
3. **Use the favicon supplied by the theme** — A checkbox, ticked by default. Untick to upload a custom favicon.
4. **Upload favicon image** — A file picker. Accepts PNG and ICO. The marketplace generates the various favicon sizes from your source image.
5. **Save configuration** — Persists both uploads. The new logo and favicon appear immediately on the storefront and on the auth screens.

> **Result:** Your logo replaces the DevPortal mark in the top-left of every page. Your favicon replaces the DevPortal favicon in browser tabs.

> **Note:** The logo also appears on the sign-in, registration, and forgot-password screens. Uploading once covers all auth surfaces.

> **Tip:** Test your logo against both light and dark mode. The marketplace supports a dark theme; a dark logo on a dark background is invisible. Use an SVG with `currentColor` fills, or supply a separate dark-mode logo through your theme files.

#### Verify

1. Reload the storefront and confirm the new logo renders in the top-left of every page.
2. Reload a browser tab and confirm the favicon updates in the tab strip and in your bookmarks.
3. Confirm the logo also appears on the sign-in, registration, and forgot-password screens.

Related tasks:

- [Set brand colours](#set-brand-colours)
- [Choose and activate a theme](#choose-and-activate-a-theme)

#### Set brand colours

Use this task to make the marketplace match your company's brand palette. The accent colour drives buttons, links, and selected states across every page.

#### Before you start

- **Have your brand's accent colour as a hex code.** For example, `#0F62FE` or `#E04E27`. The marketplace also accepts the named CSS palette (red, blue, green, etc.) but a hex code matches your brand exactly.
- **Check contrast.** Your accent colour will sit on white in light mode and on dark grey in dark mode. Both pairings need to clear WCAG AA contrast. Use a tool like the WebAIM Contrast Checker before settling on a value.
- **Decide whether to use a preset or a custom hex.** Presets are quicker; a custom hex gives an exact brand match.

To set the accent colour:

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. Scroll to the **Accent color** section.
3. Click one of the preset accent colour swatches to apply that colour, or skip to the custom picker for an exact brand match.
4. To set a custom colour, click the colour swatch in **Custom accent color** and pick a colour from the colour picker, or paste a hex code into the **Accent color** text field beside it.
5. Click **Save configuration**.

![Figure 12-4. The Accent color section of the Appearance screen.](.gitbook/assets/screenshots/provider/admin-appearance.png)

The numbered callouts in Figure 12-4 are:

1. **Accent color** — The section heading. The accent colour drives buttons, links, focus rings, and selected states across the storefront.
2. **Preset swatches** — A row of pre-curated colour swatches. Click one to apply. Use these when a perfect brand match is not required.
3. **Custom accent color** — A native colour picker. Click the swatch to open your operating system's colour picker.
4. **Accent color** (text field) — A hex code input. Type or paste a value like `#0F62FE`. The text field and the colour picker stay in sync.
5. **Save configuration** — Persists the colour. The change appears immediately on every page.

> **Result:** The marketplace renders buttons, links, focus rings, and selected states in your brand colour. The colour applies to both the storefront and the operator pages.

> **Note:** The marketplace exposes the accent colour as a CSS custom property. Front-end customisations made through your theme can read it and stay in sync if your colour changes later.

> **Tip:** A common pitfall is picking a brand colour that has poor contrast on white — pale yellows and lime greens are typical offenders. If your brand colour fails contrast, use it as an accent on dark headers and pick a darker variant for buttons.

#### Verify

1. Reload the storefront and confirm primary buttons, links, and focus rings render in the new colour.
2. Confirm the same colour applies on the operator pages — sidebar accents, the Save buttons, and any active-state indicators.
3. Run a contrast check (for example WebAIM Contrast Checker) on the colour against white and dark backgrounds and confirm it clears WCAG AA.

Related tasks:

- [Upload your logo and favicon](#upload-your-logo-and-favicon)
- [Choose and activate a theme](#choose-and-activate-a-theme)

#### Choose and activate a theme

Use this task to switch between the available marketplace themes. Themes change layout, typography, spacing, and component styling. Switching themes is non-destructive — your content, users, APIs, and configuration stay the same; the visual changes.

#### Before you start

- **Look at each theme on a non-production environment first.** A theme switch is reversible but disruptive. Confirm the new theme renders your APIs and Products correctly before flipping production.
- **Confirm your logo works against the new theme.** Some themes have darker backgrounds where a dark logo disappears. Re-upload a contrasting logo if needed.
- **Tell your team before switching.** Operators sometimes panic when the marketplace looks different overnight. A two-line note in your team's chat saves a flood of "is the site broken" messages.

To switch themes:

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. The page lists the installed themes under **Installed themes**, with the current default marked.
3. Locate the theme you want to activate.
4. Click **Set as default** beneath that theme.
5. Confirm the storefront renders correctly by visiting `<your-portal-domain>/` in a private browser window.

![Figure 12-5. The Appearance screen with installed themes.](.gitbook/assets/screenshots/provider/admin-appearance.png)

The numbered callouts in Figure 12-5 are:

1. **Installed themes** — The section listing themes available on the marketplace. The default theme is marked with a label.
2. **Theme card** — Each theme is shown with a thumbnail, name, and version. The card surfaces the actions for that theme.
3. **Set as default** — The action that switches the storefront to that theme. Available on every theme except the current default.
4. **Settings** — Per-theme settings, exposed for themes that support customisation (logo, favicon, accent colour are theme settings on the default theme).
5. **Uninstall** — Removes the theme from the marketplace. Available only on themes that are not currently the default. Use this when retiring an old custom theme.

> **Tip:** Pair a theme switch with a logo and brand-colour update so the storefront looks coherent rather than a partially-themed mix. Switching the theme without updating the logo and accent colour leaves you with a half-rebranded site.

> **Note:** The administrative theme — the look of the operator-facing pages where you import APIs and review governance — is configured separately from the storefront theme. You can run a different theme for operators than for consumers.

#### Verify

1. Open the storefront in a private browser window and confirm the new theme's typography, spacing, and component styling render.
2. Confirm your logo and accent colour still look right against the new theme.
3. Walk a couple of consumer-facing pages — homepage, API catalogue, a Product detail page — and confirm none of the layouts have broken.

Related tasks:

- [Upload your logo and favicon](#upload-your-logo-and-favicon)
- [Set brand colours](#set-brand-colours)

## Setting global site preferences

Beyond auth and branding, a handful of global settings shape the marketplace's identity and the defaults applied to new content.

#### Configure site name, slogan, and contact email

Use this task to set the marketplace's display name (used in the page title and emails), an optional slogan, and the from-address used for system-generated email like password resets and notifications.

#### Before you start

- **Pick a site name that matches your domain.** "Acme Developer Portal" reads better in browser tabs than "Acme Inc Corporate API Marketplace v2 Production".
- **Have a monitored email mailbox for the contact email.** Replies to system-generated emails go here. A shared mailbox like `developer-portal@acme.com` works better than one person's address.
- **Decide whether you want a slogan.** Many marketplaces leave it blank. If you use it, keep it under eight words.

To configure site basics:

1. From the left sidebar, expand **SETTINGS** and click **Basic Site Settings**.
2. Enter your marketplace's display name in the **Site name** field.
3. Enter a slogan in the **Slogan** field, or leave it blank.
4. Enter the email address that should appear as the from-address on system-generated email in the **Email address** field.
5. (Optional) Enter the path to a custom homepage in the **Default front page** field. Leave blank to use the marketplace default.
6. (Optional) Enter the path to a custom 403 page in the **Default 403 (access denied) page** field.
7. (Optional) Enter the path to a custom 404 page in the **Default 404 (not found) page** field.
8. Click **Save configuration**.

![Figure 12-6. The Basic Site Settings screen.](.gitbook/assets/screenshots/provider/admin-config-devportal-site-settings.png)

The numbered callouts in Figure 12-6 are:

1. **Site name** — The marketplace's display name. Appears in the browser tab title and as the from-name on system-generated email.
2. **Slogan** — An optional one-line tagline. Themes may render it in the header; some themes ignore it.
3. **Email address** — The from-address on every system-generated email — password resets, invitations, subscription approvals, governance alerts. Replies land here.
4. **Default front page** — The internal path the marketplace serves for the homepage. Leave blank for the default storefront.
5. **Default 403 (access denied) page** and **Default 404 (not found) page** — Custom error pages for users who hit a permission denial or a missing page. Leave blank for the marketplace defaults.

> **Result:** The site name, slogan, and contact email apply across the marketplace. The next system-generated email sent will use the new from-address.

> **Note:** The contact email also appears on the public Contact page if your theme renders one. Pick an address you are comfortable being public.

#### Verify

1. Reload any page and confirm the new site name appears in the browser tab title.
2. Trigger a system email — for example, a password-reset request — and confirm the from-name and from-address match what you set.
3. Visit one of the custom error paths (a deliberate 404 URL) and confirm the configured page renders.

Related tasks:

- [Configure sign-in methods](#configure-sign-in-methods)
- [Choose default visibility for new APIs and Products](#choose-default-visibility-for-new-apis-and-products)

#### Choose default visibility for new APIs and Products

Use this task to set the default visibility (public, organisation, private) applied to every API and Product created on the marketplace. Setting site-wide defaults saves Providers from configuring the same Visibility on every new API.

#### Before you start

- **Decide on the safest default for your business.** A public-by-default marketplace surfaces every imported API to anonymous visitors; a private-by-default marketplace requires the Provider to explicitly publish each API. Most enterprises start private-by-default.
- **Know whether your APIs are catalogued before they are reviewed.** If a governance review must pass before publication, default to private; the reviewer flips visibility once the review passes.

To set default visibility:

1. From the left sidebar, expand **SETTINGS** and click **Basic Site Settings**.
2. Click the **User Settings** secondary tab.
3. Locate the **Default API visibility** dropdown and pick *Public*, *Organisation*, or *Private*.
4. Locate the **Default API Product visibility** dropdown and pick *Public*, *Organisation*, or *Private*.
5. Click **Save configuration**.

> **Result:** Every API and Product created from now on takes the default visibility. Existing APIs and Products are unchanged.

> **Note:** Org-scoped overrides exist. Each Organisation can override the site default for its own APIs and Products, set during onboarding by the Org Admin.

> **Tip:** Pair a private-by-default policy with a clear publication checklist in your team's runbook — governance review, documentation review, plan review, then flip Visibility to public. Without a checklist, APIs sit private and unreachable for weeks.

#### Verify

1. Create a new API or Product and confirm its Visibility picker defaults to the value you selected.
2. Confirm an existing API's Visibility is unchanged.
3. Sign in to a non-Provider account and confirm an API created with the new default behaves as you expect — visible if Public, hidden if Private.

Related tasks:

- [Configure site name, slogan, and contact email](#configure-site-name-slogan-and-contact-email)
- [Reviewing API governance](reviewing-api-governance.md#reviewing-api-governance)

## Managing custom roles

The four built-in roles — API Provider, Org Admin, API Consumer, Portal Admin — cover most teams. When the built-ins do not fit, an Org Admin can define custom roles.

#### Create a custom role with custom permissions

Use this task when you need a permission set that does not match any built-in role — for example, a "Read-only Analyst" who sees analytics but cannot publish, or a "Plan Designer" who manages plans but cannot import APIs.

#### Before you start

- **List the permissions the role should have.** Be explicit. "Manages plans" is ambiguous; "Create plans, edit plans, but not delete plans" is actionable.
- **Confirm an existing role does not already fit.** A custom role widens the permission matrix, which makes audits harder. Stick to the built-ins when you can.
- **Have an Org Admin run this task.** Custom roles are an Org Admin responsibility. API Providers can request a new role but cannot create one.

To create a custom role:

1. From the left sidebar, click **People**.
2. Click the **Roles** secondary tab.
3. Click **Add role**.
4. Enter a recognisable name in the **Role name** field — for example, *Plan Designer*.
5. Click **Save**.
6. The new role appears in the Roles list. Click **Edit permissions** in its row.
7. Tick the checkboxes for the permissions the role should have. Use the search box at the top of the page to narrow the list.
8. Click **Save permissions**.

> **Result:** The new role appears in the **Role** dropdown when inviting members and when editing existing members. Org Admins can assign it like any built-in role.

> **Note:** A custom role inherits no permissions by default — every checkbox starts unticked. Build the role from the smallest viable permission set rather than from a built-in role minus exclusions; the audit story is cleaner.

> **Tip:** Document each custom role in your team's runbook with a one-line statement of intent — *"Plan Designer: creates and edits plans, no other permissions."* Six months later you will not remember why the role exists.

#### Verify

1. Confirm the new role appears in the Roles list with the expected name.
2. Open the role's permission editor and confirm only the intended permissions are ticked.
3. Assign the role to a sandbox account and confirm the menu items, buttons, and records visible to that account match your design.

## Next steps

- **[Day-to-day operations](day-to-day-operations.md#day-to-day-operations)** — Tune email templates, governance rules, and webhooks now that your storefront looks and signs in the way you want.
- **[Managing your team](managing-your-team.md#managing-your-team)** — Apply the new custom role and theme settings to your members.
- **[Onboarding your first consumer](onboarding-your-first-consumer.md#onboarding-your-first-consumer)** — With sign-in and branding ready, the consumer experience is now ready to receive its first subscriber.
