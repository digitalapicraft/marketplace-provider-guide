---
icon: globe
---

Point a brand-owned domain at the marketplace so the storefront answers on the company URL instead of the deployment's default hostname. Use it when a deployment goes live, and register additional hostnames for multi-brand deployments. The custom-domain register works alongside the branding settings: branding sets the look, the domain sets the address users type into the browser.

![Figure. The custom-domain register screen.](.gitbook/assets/screenshots/provider/admin-config-system-domain-register.png)

## What you configure

Each entry registers one hostname. The register binds the hostname to the marketplace; DNS and TLS are configured outside this screen.

- **Hostname**: the fully qualified domain name that should answer, for example `developer.acme.com`. Enter it lower-case and trimmed of any protocol prefix.
- **Protocol**: the scheme served for this hostname. **https** is the only supported value for production traffic; the marketplace rejects plain HTTP outside development mode.
- **Make this the canonical domain**: when ticked, every other registered hostname 301-redirects to this one. Mark exactly one canonical hostname per environment.
- **Display name**: an optional admin-only label shown in the domain list to disambiguate similar hostnames.
- **DNS record**: a CNAME or A record, added by a DNS administrator, that points the brand domain at the marketplace deployment. Required before the hostname resolves; not set on this screen.
- **TLS certificate**: an HTTPS certificate covering the new hostname. Either the deployment auto-provisions one (Let's Encrypt or platform-managed) or the security team supplies a wildcard. Lives with the deployment platform (Kubernetes Ingress, an ALB listener, or a managed certificate service), not here.

## Configure

1. From the left sidebar, expand **SETTINGS** and click **Site Domain Settings**.
2. Click **Register domain** at the top of the page.
3. Enter the fully qualified domain name in the **Hostname** field, for example `developer.acme.com`.
4. Pick **https** in the **Protocol** dropdown.
5. Tick **Make this the canonical domain** if this is the primary public hostname.
6. Optionally enter a label in **Display name** to disambiguate similar hostnames in the admin list.
7. Click **Save**.

## Verify

- From a non-VPN browser, open `https://<new-hostname>/` and confirm the storefront renders without a certificate warning.
- Open the old hostname and confirm it 301-redirects to the canonical one.
- From the **Site Domain Settings** list, confirm the new entry appears with the correct protocol and canonical flag.

{% hint style="warning" %}
**Caution:** Registering a hostname does not configure DNS or TLS. The DNS record must already point at the marketplace, and the certificate must cover the hostname, or the browser shows a "site not reachable" or certificate error until both are in place. Coordinate the DNS change and the registration so they land in the same window.
**Result:** The marketplace answers on the new hostname. With the canonical flag set, every other hostname redirects to it.
{% endhint %}

## Options

For brands that want both forms of a domain, register a `www.` variant alongside the bare apex (`developer.acme.com` and `www.developer.acme.com`) and mark the bare apex as canonical. Users typing either form land in the same place, and the canonical redirect keeps SEO clean. Plan the cutover so anyone with the old hostname bookmarked is redirected the first time the new domain answers, and confirm the TLS certificate covers the new hostname before pointing DNS at the marketplace.