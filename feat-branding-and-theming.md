---
icon: palette
---

Replace the default logo, favicon, accent colour, and theme so the storefront looks like the company that owns it rather than a default template. Branding applies across both the public storefront and the operator pages, and the same assets carry through to the sign-in, registration, and forgot-password screens. Use it whenever a new deployment goes live or a brand refresh lands. The look is driven from two surfaces: per-theme settings under **Appearance**, and the auth-screen copy under **Basic Site Settings**.

![Figure. The Appearance screen with installed themes and their Settings links.](.gitbook/assets/screenshots/provider/admin-appearance.png)

## What you see

The **Appearance** page, under the **SETTINGS** group in the left sidebar, lists every installed theme under **Installed themes**, with the current default marked. Each theme card carries:

- **Theme card.** A thumbnail, name, and version for the theme.
- **Settings.** Opens the per-theme settings form where logo, favicon, and accent colour live. Available on themes that support customisation.
- **Set as default.** Switches the storefront to that theme. Available on every theme except the current default.
- **Uninstall.** Removes the theme. Available only on themes that are not currently the default.

The per-theme settings form holds the **Logo image**, **Favicon**, and **Accent color** sections. Each of the logo and favicon sections has a *Use the supplied by the theme* checkbox that hides the upload field until you untick it. The administrative theme, the look of the operator-facing pages, is configured separately from the storefront theme, so a different theme can run for operators than for consumers.

![Figure. The Auth Page Branding screen for sign-in, registration, and reset copy.](.gitbook/assets/screenshots/provider/admin-config-devportal-site-settings-auth-branding.png)

## Theme-settings form fields

- **Logo image**: file, optional. A transparent-background SVG or PNG shown in the top-left of every page. SVG scales best for retina displays; avoid JPEG, whose background clashes with the storefront. Untick **Use the logo supplied by the theme** to expose the upload field.
- **Favicon**: file, optional. A square source at 256x256 pixels or larger, shown in browser tabs and bookmarks. The marketplace down-samples it, so starting larger gives crisper results. Untick **Use the favicon supplied by the theme** to expose the upload field.
- **Accent color**: hex or preset, required. The brand colour applied to buttons, links, focus rings, and selected states across every page. Click a preset swatch or paste an exact brand hex such as `#0F62FE`. Exposed as a CSS custom property so theme customisations stay in sync.
- **Theme (Set as default)**: the active storefront theme, which sets layout, typography, spacing, and component styling. Switching is non-destructive: content, users, APIs, and configuration are untouched, only the visuals change.

## Auth Page Branding form fields

- **Login page content**: rich text, optional. Renders on the sign-in screen beneath the email and password fields. Use it for a company notice, an IT-helpdesk link, or guidance on which corporate credentials to use.
- **Registration page content**: rich text, optional. Renders on the new-user registration screen. Use it to set expectations about approval timelines or to point external developers at terms-of-use.
- **Forgot password page content**: rich text, optional. Renders on the password-reset screen. Use it for the support email a user should contact when the reset email does not arrive.

{% hint style="info" %}
**Note:** The rich-text fields accept links, bold text, and bullet lists. Embedded scripts or iframes are stripped on save.
{% endhint %}

## Upload the logo and favicon

Have a transparent-background PNG or SVG of the logo and a square favicon source at 256x256 pixels or larger before you start.

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. Click **Settings** on the active theme's card to open its settings form.
3. Scroll to **Logo image**, untick **Use the logo supplied by the theme**, then upload the logo file.
4. Scroll to **Favicon**, untick **Use the favicon supplied by the theme**, then upload the favicon source.
5. Click **Save configuration**.

{% hint style="info" %}
**Note:** The logo also appears on the sign-in, registration, and forgot-password screens. Uploading once covers all auth surfaces.
{% endhint %}

{% hint style="success" %}
**Tip:** Test the logo against both light and dark mode. A dark logo on a dark background is invisible; use an SVG with `currentColor` fills, or supply a separate dark-mode logo through the theme files.
{% endhint %}

## Set the accent colour

Have the brand's accent colour as a hex code, and check its contrast against white (light mode) and dark grey (dark mode) before settling on a value.

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. Click **Settings** on the active theme's card.
3. Scroll to the **Accent color** section.
4. Click a preset swatch, or paste a brand hex code into the **Accent color** text field for an exact match.
5. Click **Save configuration**.

{% hint style="success" %}
**Tip:** A common pitfall is picking a brand colour with poor contrast on white; pale yellows and lime greens are typical offenders. If the brand colour fails contrast, use it as an accent on dark headers and pick a darker variant for buttons.
{% endhint %}

## Switch the storefront theme

A theme switch is reversible but disruptive, so look at each theme on a non-production environment first and confirm the logo works against it.

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. Locate the theme to activate under **Installed themes**.
3. Click **Set as default** beneath that theme.
4. Confirm the storefront renders correctly by visiting `<your-portal-domain>/` in a private browser window.

{% hint style="success" %}
**Tip:** Pair a theme switch with a logo and brand-colour update so the storefront looks coherent rather than a partially-themed mix. Tell the team before switching; operators sometimes panic when the marketplace looks different overnight.
{% endhint %}

## Set auth-screen copy and preview

1. From the left sidebar, expand **SETTINGS** and click **Basic Site Settings**.
2. Click the **Auth Page Branding** tab at the top of the page.
3. Enter the **Login page content**, **Registration page content**, and **Forgot password page content** messages.
4. Click **Save configuration**. The new copy is live on the public auth screens immediately.
5. Open a private browser window and visit the storefront root at `<your-portal-domain>/`. Walk the homepage, API catalogue, and a Product detail page, then sign out and confirm the auth screens carry the logo and copy.

{% hint style="info" %}
**Note:** Saved branding applies immediately to the live storefront. To stage and review a change without affecting consumers, make and review it on a non-production environment first, then promote it.
{% endhint %}

{% hint style="warning" %}
**Caution:** Site-wide branding applies to the storefront root. Per-Organisation overrides render only when the URL is scoped to that Organisation. Confirm both views before announcing the change.
{% endhint %}

## Verify

- Reload the storefront and confirm the logo renders top-left and the accent colour drives buttons, links, and focus rings.
- Reload a browser tab and confirm the favicon updates in the tab strip and bookmarks.
- Walk the homepage, API catalogue, and a Product detail page, then sign out and confirm the auth screens carry the logo and any custom copy.
- Run a contrast check (for example WebAIM Contrast Checker) on the accent colour against white and dark backgrounds and confirm it clears WCAG AA.

{% hint style="success" %}
**Result:** The storefront, operator pages, and auth screens render the brand logo, favicon, accent colour, and theme consistently.
{% endhint %}

## Per-Organisation branding

For multi-brand deployments, each Organisation can override the site-wide branding, for example a "Banking" sub-brand and a "Health" sub-brand under one deployment. The override fields live on the Organisation edit form, reached from **SETTINGS > Organisations**:

- **Inherit from site default**: checkbox. When ticked, the Organisation uses the site-wide branding and the override fields below are hidden. Untick to expose them.
- **Organisation logo**: file. The uploaded image replaces the site-wide logo on storefront URLs scoped to this Organisation.
- **Accent color**: hex or preset. Drives buttons, links, and selected states for this Organisation's storefront.
- **Storefront theme**: select. An optional theme override. Defaults to *Inherit*, meaning the site-wide active theme is used.

The precedence is user setting, then Organisation, then site-wide. Per-Organisation branding renders only on URLs scoped to that Organisation, for example `/<org-slug>/`; unscoped URLs keep the site-wide look.

{% hint style="success" %}
**Tip:** When several Organisations need the same alternate brand (for example all child Organisations of one parent), set branding on the parent and leave the children on **Inherit from site default**. Children cascade the parent's branding by default.
{% endhint %}

## Related

- [Custom domains](feat-custom-domains.md) for pointing a brand-owned domain at the branded storefront.
- [Single sign-on (SAML)](feat-single-sign-on.md) for the sign-in screen that carries the auth-page copy and logo.
- [Team & members](feat-team-and-members.md) for the Organisations whose per-Org branding you scope here.
- [Roles & permissions](feat-roles-and-permissions.md) for the Org Admin role that owns branding.