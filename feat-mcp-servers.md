---
icon: robot
---

An MCP server registers a Model Context Protocol endpoint that exposes a curated subset of your published APIs to AI agents. Use it to let MCP-aware agents (Claude Desktop, IDE assistants, custom clients) discover and call your APIs as tools. The same registration also powers the in-product API GPT chat.

![Figure. Multiple APIs scoped to a single MCP server on the Edit form.](.gitbook/assets/screenshots/provider/mcp-server-16-edit.png)

## Configure

1. From the left sidebar, expand **API MANAGEMENT** and click **MCP Servers**.
2. Click **Add MCP Server** to open the registration form.
3. Enter a **Label** (shown to agents in their tool list, max 255 characters) and an optional **Description** for your team.
4. In the **APIs** multi-select, pick one or more published APIs to expose. Type to filter, click to add. Each selected API becomes a callable tool.
5. Confirm the **Base URL**. The portal pre-fills it with the storefront domain and a per-server path. Override only when a separate gateway fronts MCP traffic.
6. Enter the **System Prompt** that steers how the agent presents and calls these APIs.
7. Set **Status** to Active, then click **Save**.

The form's key fields:

1. **APIs multi-select.** Lists published APIs only. Each pill becomes a callable tool.
2. **System Prompt.** Sent to the agent with the tool definitions. Keep it focused on the scoped APIs.
3. **Tools list.** The POST and GET endpoints generated from each API's OpenAPI spec.

{% hint style="success" %}
**Result:** Any MCP-aware agent that connects to the Base URL discovers the scoped APIs as tools and can call their operations.
**Tip:** Keep the scope tight. Two or three related APIs per server produce more reliable agents than twenty in one.
{% endhint %}

To edit scope later, open the server's **Edit** form, add or remove APIs in the multi-select, update the System Prompt to match, and save. Agents pick up the new tool list on their next handshake. To stop access, pick **Revoke** from the row action menu: the server stays in the list for audit but rejects new connections.