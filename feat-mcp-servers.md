---
icon: robot
---

An MCP server registers a Model Context Protocol endpoint that exposes a curated subset of your published APIs to AI agents. Use it to let MCP-aware agents (Claude Desktop, IDE assistants, custom clients) discover and call your APIs as tools. The same registration also powers the in-product API GPT chat, so one server feeds both external agents and the storefront assistant.

![Figure. The MCP Servers list page with registered servers.](.gitbook/assets/screenshots/provider/admin-portal-mcp-servers.png)

## What you configure

The registration form turns a set of published APIs into a single callable endpoint. The fields an operator fills in:

- **APIs (multi-select)**: the published APIs this server exposes, required. Type a partial title to filter, click to add a pill, click the pill's `x` to remove. Only APIs already published in the marketplace appear in the list. Each selected API becomes a callable tool on the server.
- **Server label**: the human-readable name agents see in their tool list and the list page's Label column. Required, max 255 characters. Prefer an identifiable name such as *"Payments API (production)"* over *"mcp-1"*.
- **Description**: an optional internal memo, max 1024 characters. It reminds your team what the server scopes and does not affect agent behaviour.
- **Base URL override**: the externally callable endpoint agents connect to. The portal pre-fills it with the storefront domain and a per-server path. Edit it only when a separate gateway fronts MCP traffic.
- **Custom system prompt**: free text sent to the agent alongside the tool definitions. Use it to set persona, name the scoped APIs, and constrain behaviour, for example *"You are an assistant for the Payments API. Always confirm currency before calling create_payment."*
- **Generated tools**: read-only. The portal builds one tool per endpoint (the POST and GET operations) from each scoped API's OpenAPI spec. The Tools list on the form confirms exactly which operations the agent can call.
- **Status**: Active or Revoked. New servers default to Active; set Revoked to stage a server without exposing it.

## Configure

1. From the left sidebar, expand **API MANAGEMENT** and click **MCP Servers**.
2. Click **Add MCP Server** to open the registration form.
3. Enter a **Label** and an optional **Description**.
4. In the **APIs** multi-select, pick one or more published APIs to expose.
5. Confirm the **Base URL** or override it if a separate gateway fronts MCP traffic.
6. Enter the **System Prompt** that steers how the agent presents and calls these APIs.
7. Set **Status** to Active, then click **Save**.

![Figure. The Add MCP Server form.](.gitbook/assets/screenshots/provider/admin-portal-mcp-servers-add.png)

To re-scope an existing server, open its **Edit** form, add or remove APIs in the multi-select, update the system prompt to match the new selection, and save. The portal re-parses each scoped API's OpenAPI spec on save, so the Tools list refreshes to reflect the current operations. Agents pick up the new tool list on their next handshake.

![Figure. Multiple APIs scoped to a single MCP server on the Edit form.](.gitbook/assets/screenshots/provider/mcp-server-16-edit.png)

## Verify

- Confirm the new server appears in the **MCP Servers** list with the correct Label, APIs, and Base URL.
- Open the server's Edit form and confirm the **Tools list** shows one tool per endpoint for each scoped API.
- From an MCP-aware agent (for example Claude Desktop), connect to the Base URL and confirm the agent enumerates each scoped API's operations as tools, then issue a test call and confirm a `2xx` response.

## Options

A server can scope anywhere from one API to your whole catalog, but tight scopes (two or three related APIs) produce more reliable agents than broad ones. Status is a soft lifecycle control: **Revoke** from the row action menu rejects new connections while keeping the row for audit, and re-activating from the Edit form reuses the same Base URL so consumers need no reconfiguration. **Delete** removes the server and its audit row permanently, so prefer Revoke unless you are purging a record.

{% hint style="info" %}
**Note:** MCP exposure is opt-in per API. Registering a server exposes only the APIs you select, and each tool call still requires the consumer's active subscription on the underlying API plan.
**Result:** Any MCP-aware agent that connects to the Base URL discovers the scoped APIs as tools and can call their operations.
{% endhint %}