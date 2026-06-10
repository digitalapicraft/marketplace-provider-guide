---
icon: palette
---

Replace the default logo, favicon, accent colour, and theme so the storefront matches the company brand. Branding applies across both the storefront and the operator pages.

![Figure. The Appearance screen with installed themes and their Settings links.](.gitbook/assets/screenshots/provider/admin-appearance.png)

## Configure

1. From the left sidebar, expand **SETTINGS** and click **Appearance**.
2. Click **Settings** on the active theme's card to open its settings form.
3. In **Logo image**, untick **Use the logo supplied by the theme**, then upload a transparent-background SVG or PNG.
4. In **Favicon**, untick **Use the favicon supplied by the theme**, then upload a square source at 256x256 pixels or larger.
5. In **Accent color**, click a preset swatch, or paste a brand hex code such as `#0F62FE` into the **Accent color** field.
6. Click **Save configuration**.
7. To switch themes, return to **Appearance** and click **Set as default** beneath the theme you want.
8. Preview the result by opening the storefront root in a private browser window.

{% hint style="success" %}
**Result:** The storefront and operator pages render the brand logo, favicon, and accent colour, with buttons, links, and focus rings in the brand colour.
**Tip:** Test the logo against both light and dark mode. A dark logo on a dark background is invisible; use an SVG with `currentColor` fills.
{% endhint %}

Key actions in the figure:

1. **Settings** link: opens the per-theme form where logo, favicon, and accent colour live.
2. **Set as default** link: switches the storefront to that theme. Content, users, and configuration are untouched.

The accent colour sits on white in light mode and on dark grey in dark mode. Check both pairings against WCAG AA contrast before settling on a value. The logo also appears on the sign-in, registration, and forgot-password screens, so uploading once covers every auth surface.