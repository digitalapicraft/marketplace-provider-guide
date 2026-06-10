---
icon: right-to-bracket
---

Configure SAML 2.0 SSO against an external identity provider so members sign in with their corporate identity. Email-and-password stays available as a break-glass fallback.

![Figure. The Add IdP form on the SAML SSO settings screen.](.gitbook/assets/screenshots/provider/admin-config-saml-idp.png)

## Configure

1. From the left sidebar, expand **SETTINGS** and click **SSO Configurations**.
2. Click **Add IdP** to open the form, then enter a recognisable label in **Identity Provider Name**.
3. Enter the **Entity ID** issued by the IdP, copied verbatim from its metadata.
4. Paste the IdP's metadata endpoint into **IdP Metadata URL**, or upload the XML in **IdP Metadata XML**.
5. Enter the **Single Sign-On Service URL**, and the **Single Logout Service URL** if the IdP supports it.
6. Paste the signing certificate into **X.509 Certificate**, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.
7. Map the IdP claims in **Attribute Mapping**: email, first name, last name, and optionally role or group.
8. Set **Default Role on First Sign-In** (API Consumer is the safest), then click **Save**.
9. Test the round-trip in a private browser window before enabling the IdP for everyone.

{% hint style="success" %}
**Result:** Members who click the SSO button are redirected to the IdP and land back on the marketplace under the mapped role.
**Tip:** Map IdP groups to roles, for example `marketplace-admins` to Org Admin, so permissions are managed at the IdP.
{% endhint %}

The key fields in the figure:

1. **Entity ID**: the unique identifier the IdP advertises. Mismatches here are the most common cause of SAML failures.
2. **X.509 Certificate**: every SAML response is verified against this. Rotate it when the IdP rotates its keys.
3. **Default Role on First Sign-In**: the role new federated users land on until an Org Admin promotes them.

The marketplace exposes its Service Provider metadata at `/saml/sp/metadata` once SAML is enabled. Hand that URL to the IdP team. Always keep one local-password Portal Admin account active as the break-glass route.