---
icon: comments
---

API GPT is the chat assistant embedded in your storefront that consumers reach at `/api-gpt`. Configure its branding and the APIs it can answer about so it reads as part of your storefront and steers consumers toward your catalog. Its tool layer comes from the MCP servers you register, so register at least one first.

![Figure. The consumer-facing API GPT chat.](.gitbook/assets/screenshots/provider/api-gpt.png)

## What you configure

Branding and scope live together on the API GPT Settings form under the model settings. The fields an operator fills in:

- **Assistant name**: the name shown in the chat header, max 64 characters. Set it to something that maps to your storefront, for example *"Acme API Helper"*, so the assistant reads as your brand rather than the platform.
- **Avatar**: an image shown in the chat header, PNG, JPG, or SVG up to 1 MB. It renders at 32x32, so a square image with a transparent background is cleanest.
- **Welcome message**: the assistant's first bubble, max 280 characters. Use one or two sentences to set expectations, for example *"Hi! Ask me anything about the Acme APIs. I can call them to show you how they work."*
- **Chat accent colour**: picked from the colour picker or pasted as a hex value. The accent applies to the send button, the user-bubble background, and the focus ring.
- **API scope**: the assistant answers about whichever APIs your registered MCP servers expose, since those servers are its tool layer. To widen or narrow what API GPT can discuss and call, change the API selection on the MCP servers, not on this form.
- **Default system prompt**: the system message prepended to every conversation. Keep it focused on your marketplace's purpose so the assistant prefers a live API call over a documentation summary.

Branding fields are optional; leaving them blank falls back to the platform defaults (*"API GPT"*, the marketplace logo, a generic welcome, and the storefront's primary colour).

## Configure

1. From the left sidebar, expand **API MANAGEMENT** and click **API GPT Settings**.
2. Choose the **LLM Provider** and model, paste the matching API key, and set **Max Tool Iterations** and **API Request Timeout**.
3. Scroll to **Branding**. Enter an **Assistant Name** shown in the chat header.
4. Upload an **Avatar** image.
5. Enter a **Welcome Message** shown as the assistant's first bubble.
6. Pick a **Chat Accent Colour** for the send button, user-bubble background, and focus ring.
7. Enter a **Default System Prompt** that scopes the assistant to your APIs, then click **Save configuration**.

![Figure. The Branding section of the API GPT Settings form.](.gitbook/assets/screenshots/provider/mcp-settings-branding.png)

To surface the chat from the storefront, place the launcher block in a region:

1. From the left sidebar, expand **Structure** and click **Blocks**.
2. Find the **API GPT Chat Launcher** block. It ships disabled.
3. Click **Place block** in the region you want (home hero, discovery sidebar, footer).
4. Optionally override the launcher label and set visibility rules, then click **Save block**.

## Verify

- Reload API GPT Settings and confirm the Assistant Name, Welcome Message, accent colour, and Default System Prompt retain the values you set.
- Open the storefront home page in an incognito window and confirm the chat launcher appears in the region you chose.
- Click the launcher (or visit `/api-gpt`) and confirm the chat opens with your assistant name, avatar, and welcome message, then ask *"what APIs are available?"* and confirm the answer matches the scope of your registered MCP servers.

{% hint style="info" %}
**Note:** API GPT enforces the same auth as direct API calls. If the consumer chatting has no active subscription to an API's plan, the tool call returns the same auth error a direct request would.
**Result:** A floating launcher opens the branded chat on every page that matches the visibility rules, showing your assistant name, avatar, welcome message, and accent colour.
{% endhint %}