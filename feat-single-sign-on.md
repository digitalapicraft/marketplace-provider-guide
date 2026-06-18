---
icon: right-to-bracket
---

Federate the marketplace against a corporate identity provider over SAML 2.0 so members sign in with their existing credentials. Use it when the security team requires every user to authenticate through an external IdP such as Okta, Azure AD, Ping, ADFS, or Auth0. SAML delegates authentication to the IdP and creates the local account on first sign-in. Email-and-password sign-in stays available alongside SAML as a break-glass fallback, so federation stacks on top of the default login rather than replacing it, until you choose to force SSO for an Organisation.

![Figure. The Add IdP form on the SAML SSO settings screen.](.gitbook/assets/screenshots/provider/admin-config-saml-idp.png)

## What you see

The **SSO Configurations** page, under the **SETTINGS** group in the left sidebar, lists every registered identity provider with its name, status, and row actions for **Edit** and **Delete**. **Add IdP** at the top opens the registration form. Each registered IdP that is enabled adds a **Sign in with <IdP name>** button to the public sign-in screen.

The marketplace acts as the SAML Service Provider. Once SAML is enabled, it exposes its Service Provider metadata at `/saml/sp/metadata`. Hand that URL to the IdP team so they register the marketplace as a Service Provider on the IdP side.

## Add-IdP form fields

Register one identity provider per IdP. The marketplace needs the IdP's metadata, endpoints, signing certificate, and a mapping from its claims to marketplace accounts.

- **Identity Provider Name**: text, required. A label shown on the sign-in button and in the SSO Configurations list, for example *Okta Production* or *Azure AD Corp*. Distinguishes IdPs when more than one is registered.
- **Entity ID**: text, required. The unique identifier the IdP advertises, usually a URL such as `http://www.okta.com/exk1abc23`. Copy it verbatim from the IdP metadata; a mismatch here is the most common cause of SAML failures.
- **IdP Metadata URL**: URL, conditional. A live metadata endpoint the marketplace fetches periodically. Preferred over the XML upload because it picks up certificate rotations automatically. Required if the XML field is empty.
- **IdP Metadata XML**: file upload, conditional. The exported metadata file. Required if the Metadata URL is empty.
- **Single Sign-On Service URL**: URL, required. The IdP endpoint the marketplace redirects the user to for sign-in.
- **Single Logout Service URL**: URL, optional. The IdP endpoint for federated sign-out. Leave blank if the IdP does not support logout federation.
- **X.509 Certificate**: text in PEM form, required. The IdP's signing certificate, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines. Every SAML response is verified against it. Rotate it whenever the IdP rotates its keys.
- **Attribute Mapping**: a set of claim fields. Email is required; first name, last name, and a role or group claim are optional. Defaults match Okta and Azure AD; customise for other IdPs.
- **Default Role on First Sign-In**: select, required. The role new federated users land on until an Org Admin promotes them. API Consumer is the safest default.
- **Just-in-Time Provisioning**: checkbox, optional. On to auto-create accounts on first sign-in; off to require pre-provisioning by an Org Admin.
- **Force SSO for Organisation**: checkbox, optional. Disables email-and-password sign-in for the listed Organisation, leaving SAML as the only route.
- **Enabled**: checkbox, required. Off hides the IdP from the sign-in screen without deleting it.

## Before you start

- **Have the IdP metadata URL or XML file ready.** Most IdPs expose a metadata endpoint at a URL like `https://idp.example.com/saml/metadata`. If the IdP does not, export the metadata as XML and have the file open.
- **Know which attributes the IdP releases.** At minimum the marketplace needs an email and a display-name claim. A role or group claim is recommended; without one, every federated user lands on the default role.
- **Have a Portal Admin local-password account ready as break-glass.** If SAML is misconfigured, this is the only way back in.

{% hint style="warning" %}
**Caution:** SAML changes take effect immediately. Testing SAML in production without a verified configuration can lock everyone out except a Portal Admin with a local-password fallback. Always test SAML in a non-production environment first, and keep one local-password Portal Admin account active as the break-glass route.
{% endhint %}

## Configure a SAML identity provider

1. From the left sidebar, expand **SETTINGS** and click **SSO Configurations**.
2. Click **Add IdP** to open the form.
3. Enter a recognisable label in **Identity Provider Name**.
4. Enter the **Entity ID** issued by the IdP, copied verbatim from its metadata.
5. Paste the IdP's metadata endpoint into **IdP Metadata URL**, or upload the file in **IdP Metadata XML**.
6. Enter the **Single Sign-On Service URL**, and the **Single Logout Service URL** if the IdP supports it.
7. Paste the signing certificate into **X.509 Certificate**, including the BEGIN and END lines.
8. Map the IdP claims in **Attribute Mapping**: email, first name, last name, and optionally role or group.
9. Set **Default Role on First Sign-In**, then decide whether to tick **Just-in-Time Provisioning**.
10. Click **Save**.

{% hint style="success" %}
**Tip:** If the IdP supports group claims, map a group like `marketplace-providers` to the API Provider role and `marketplace-admins` to Org Admin. Permissions are then managed from the IdP rather than on every user by hand. See [Roles & permissions](feat-roles-and-permissions.md) for the roles you can map to.
{% endhint %}

## Test SSO sign-in end to end

Validating the round-trip before rolling SAML out organisation-wide is the single largest factor in avoiding "everyone is locked out" incidents. Run the test in a non-production environment, with a test user who is in whatever group or role you intend to map, in a private browser window to avoid stale session cookies.

1. In a private browser window, open `<your-portal-domain>/user/login`.
2. Click the **Sign in with <your IdP name>** button. The browser redirects to the IdP.
3. Sign in with the test user's credentials. The browser redirects back to the marketplace.
4. Confirm the test user lands on the dashboard.
5. From the user menu in the top-right, confirm the email and display name match the IdP user.
6. Open **Edit profile** and confirm the role matches the **Default Role on First Sign-In** value, or the mapped IdP group if role attribute mapping is in use.
7. Sign out and confirm the marketplace returns you to the sign-in screen, not back to the IdP.

{% hint style="info" %}
**Note:** If the redirect from the IdP fails with a SAML response error, check the X.509 Certificate first. The second-most-common failure is a clock skew greater than 60 seconds between the IdP and the marketplace.
{% endhint %}

{% hint style="warning" %}
**Caution:** Once **Force SSO** is on for an Organisation, email-and-password sign-in is disabled for that Organisation's users. Confirm the round-trip works for at least three test users across different groups before flipping that switch.
{% endhint %}

## Edit or remove an identity provider

1. From **SSO Configurations**, locate the IdP row.
2. Click **Edit** to re-open the form with saved values, update the field that changed (for example a rotated certificate or a corrected claim path), and **Save**. Future responses are validated against the updated configuration.
3. To retire an IdP softly, edit it and untick **Enabled** to hide its button while keeping the configuration. To remove it entirely, click **Delete** and confirm; the entry is removed and its sign-in button disappears.

{% hint style="warning" %}
**Caution:** Removing an IdP does not delete the local accounts created from it. Federated users keep their accounts but cannot sign in until they reset their password through the email-and-password flow, assuming that flow is still enabled.
{% endhint %}

{% hint style="success" %}
**Tip:** When rotating a certificate, paste the new value into the existing IdP entry rather than creating a parallel IdP. Two IdPs with the same Entity ID confuse the SAML response handler and cause intermittent sign-in failures.
{% endhint %}

## Verify

- Confirm the new IdP appears in **SSO Configurations** with status **Active**.
- Confirm the marketplace's Service Provider metadata at `/saml/sp/metadata` is reachable and matches what the IdP team registered.
- Confirm sign-in, account creation, role assignment, and sign-out succeed for at least three test users covering different IdP groups.
- Confirm the local-password break-glass Portal Admin account still signs in via the email-and-password fallback.

{% hint style="success" %}
**Result:** Members who click the SSO button are redirected to the IdP, sign in there, and land back on the marketplace under the mapped role, with their account created on first sign-in.
{% endhint %}

## Related

- [Team & members](feat-team-and-members.md) for how SAML-provisioned users land as Active members and how to widen their role.
- [Roles & permissions](feat-roles-and-permissions.md) for the roles an IdP group claim can map to.
- [Branding & theming](feat-branding-and-theming.md) for the copy on the sign-in screen that carries the SSO button.
- [Custom domains](feat-custom-domains.md) for the hostname the IdP redirects back to.