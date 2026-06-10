---
icon: comments
---

API GPT is the chat assistant embedded in your storefront that consumers reach at `/api-gpt`. Configure its branding and the APIs it can answer about so it reads as part of your storefront and steers consumers toward your catalog.

![Figure. The Branding section of the API GPT Settings form.](.gitbook/assets/screenshots/provider/mcp-settings-branding.png)

## Configure

1. From the left sidebar, expand **API MANAGEMENT** and click **API GPT Settings**.
2. Choose the **LLM Provider** (Anthropic or Groq) and model, paste the matching API key, and set **Max Tool Iterations** and **API Request Timeout**.
3. Scroll to **Branding**. Enter an **Assistant Name** (max 64 characters) shown in the chat header.
4. Upload an **Avatar** image (PNG, JPG, or SVG up to 1 MB; renders at 32x32).
5. Enter a **Welcome Message** (max 280 characters) shown as the assistant's first bubble.
6. Pick a **Chat Accent Colour** for the send button, user-bubble background, and focus ring.
7. Enter a **Default System Prompt** that scopes the assistant to your APIs, then click **Save configuration**.

API GPT draws its tool layer from the MCP servers you register, so its answer scope is whichever APIs those servers expose. Register at least one MCP server first, or the assistant falls back to discussing APIs from documentation alone.

To surface the chat from the storefront:

1. From the left sidebar, expand **Structure** and click **Blocks**.
2. Find the **API GPT Chat Launcher** block. It ships disabled.
3. Click **Place block** in the region you want (home hero, discovery sidebar, footer).
4. Optionally override the launcher label and set visibility rules, then click **Save block**.

{% hint style="success" %}
**Result:** A floating launcher opens the branded chat on every page that matches the visibility rules, showing your assistant name, avatar, welcome message, and accent colour.
**Tip:** Match the chat accent colour to your storefront brand colour so the panel reads as part of the site, not a default install.
{% endhint %}