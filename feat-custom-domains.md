---
icon: globe
---

Point a brand-owned domain at the marketplace so the storefront answers on the company URL instead of the deployment's default hostname. Without a custom domain the storefront answers on the deployment's default hostname, which rarely matches the company brand; after registration it answers on `https://developer.acme.com` (or the equivalent) instead. Use it when a deployment goes live, and register additional hostnames for multi-brand deployments. The custom-domain register works alongside the branding settings: branding sets the look, the domain sets the address users type into the browser.

![Figure. The custom-domain register screen.](.gitbook/assets/screenshots/provider/admin-config-system-domain-register.png)

## What you see

The **Site Domain Settings** page, under the **SETTINGS** group in the left sidebar (direct path `/admin/config/system/domain/register`), lists every registered hostname with its protocol, canonical flag, and display name. **Register domain** at the top opens the registration form. Each entry registers one hostname. The register binds the hostname to the marketplace; DNS and TLS are configured outside this screen.

## Register-domain form fields

- **Hostname**: text, required. The fully qualified domain name that should answer on the marketplace, for example `developer.acme.com`. Enter it lower-case and trimmed of any protocol prefix.
- **Protocol**: select, required. The scheme served for this hostname. **https** is the only supported value for production traffic; the marketplace rejects plain HTTP outside development mode.
- **Make this the canonical domain**: checkbox, optional. When ticked, every other registered hostname 301-redirects to this one. Mark exactly one canonical hostname per environment.
- **Display name**: text, optional. An admin-only label shown in the domain list to disambiguate similar hostnames.

Two prerequisites are configured outside this screen but are required for the hostname to work:

- **DNS record**: a CNAME or A record, added by a DNS administrator, that points the brand domain at the marketplace deployment. Required before the hostname resolves.
- **TLS certificate**: an HTTPS certificate covering the new hostname. Either the deployment auto-provisions one (Let's Encrypt or platform-managed) or the security team supplies a wildcard. Lives with the deployment platform (Kubernetes Ingress annotations, an ALB listener, or a managed certificate service), not here.

## Before you start

- **Own the DNS for the domain.** A DNS administrator must add a CNAME or A record pointing the brand domain at the marketplace deployment.
- **Have a valid TLS certificate.** The marketplace requires HTTPS. Either the deployment auto-provisions a certificate or the security team supplies a wildcard certificate covering the new hostname.
- **Plan the cutover.** The first time the domain answers on the marketplace, anyone who has the old hostname bookmarked needs a redirect. The canonical flag handles that for other registered hostnames.

## Register a custom domain

1. From the left sidebar, expand **SETTINGS** and click **Site Domain Settings**.
2. Click **Register domain** at the top of the page.
3. Enter the fully qualified domain name in the **Hostname** field, for example `developer.acme.com`.
4. Pick **https** in the **Protocol** dropdown.
5. Tick **Make this the canonical domain** if this is the primary public hostname. Traffic on every other registered hostname then redirects to it.
6. Optional. Enter a label in **Display name** to disambiguate similar hostnames in the admin list.
7. Click **Save**. The hostname starts answering once DNS resolves to the marketplace.

{% hint style="warning" %}
**Caution:** Registering a hostname does not configure DNS or TLS. The DNS record must already point at the marketplace, and the certificate must cover the hostname, or the browser shows a "site not reachable" or certificate error until both are in place. Coordinate the DNS change and the registration so they land in the same window.
{% endhint %}

{% hint style="info" %}
**Note:** TLS certificates are not configured on this screen. Confirm the certificate covers the new hostname with your deployment platform before pointing DNS at the marketplace.
{% endhint %}

{% hint style="success" %}
**Tip:** Register a `www.` variant alongside the bare apex (`developer.acme.com` and `www.developer.acme.com`) and mark the bare apex as canonical. Users typing either form land in the same place, and the canonical redirect keeps SEO clean.
{% endhint %}

## Verify

- From a non-VPN browser, open `https://<new-hostname>/` and confirm the storefront renders without a certificate warning.
- Open the old hostname and confirm it 301-redirects to the canonical one.
- From the **Site Domain Settings** list, confirm the new entry appears with the correct protocol and canonical flag.

{% hint style="success" %}
**Result:** The marketplace answers on the new hostname. With the canonical flag set, every other hostname redirects to it.
{% endhint %}

## Per-Organisation entry points

For multi-brand deployments, per-Organisation branding renders when a user lands on a URL scoped to that Organisation, for example `/<org-slug>/` or a dedicated subdomain. Where an Organisation has its own subdomain, register that hostname here the same way, then scope its look on the Organisation edit form. Confirm both the site-wide root and the per-Organisation entry point answer correctly before announcing the change. See [Branding & theming](feat-branding-and-theming.md) for the per-Organisation branding fields.

## Related

- [Branding & theming](feat-branding-and-theming.md) for the logo, accent colour, and theme that render on the branded hostname.
- [Single sign-on (SAML)](feat-single-sign-on.md) for the hostname the IdP redirects back to after sign-in.
- [Team & members](feat-team-and-members.md) for the Organisations whose subdomains you register.
- [Roles & permissions](feat-roles-and-permissions.md) for the Org Admin role that owns domain settings.