---
icon: globe
---

Register a brand-owned domain so the storefront answers on the company URL instead of the deployment's default hostname. Register additional domains for multi-brand deployments.

![Figure. The custom-domain register screen.](.gitbook/assets/screenshots/provider/admin-config-system-domain-register.png)

## Configure

1. From the left sidebar, expand **SETTINGS** and click **Site Domain Settings**.
2. Click **Register domain** at the top of the page.
3. Enter the fully qualified domain name in the **Hostname** field, for example `developer.acme.com`.
4. Pick **https** in the **Protocol** dropdown. It is the only supported value for production traffic.
5. Tick **Make this the canonical domain** if this is the primary public hostname. Every other registered hostname redirects to it.
6. Optionally enter a label in **Display name** to disambiguate similar hostnames in the admin list.
7. Click **Save**.
8. From a non-VPN browser, open `https://<new-hostname>/` and confirm the storefront renders without a certificate warning.

{% hint style="success" %}
**Result:** The marketplace answers on the new hostname. With the canonical flag set, every other hostname redirects to it.
**Tip:** Register a `www.` variant alongside the bare apex and mark the apex canonical, so either form lands in the same place.
{% endhint %}

Key fields in the figure:

1. **Hostname**: the domain that should answer, lower-case and trimmed of any protocol prefix.
2. **Make this the canonical domain**: every other registered hostname 301-redirects to this one.

Registering a hostname does not configure DNS or TLS. The DNS record (CNAME or A) must already point at the marketplace, or the browser shows a "site not reachable" error until it propagates. TLS certificates live with the deployment platform; confirm the certificate covers the new hostname before pointing DNS at it.