---
icon: robot
---

Your APIs are valuable to humans writing code against them. They are equally valuable to AI agents (Claude, GPT, your own LLM-powered apps) that need to call them as tools on a user's behalf. The marketplace provides two complementary surfaces for this. **MCP Servers** wrap selected APIs in the Model Context Protocol so any MCP-aware agent can discover and call them. **API GPT** is the chat assistant embedded in the storefront itself, with a configurable LLM backend, a branded chat panel, and a system prompt that steers it toward your catalog. The same MCP registration powers both: external agents and the in-product chat draw their tool surface from the MCP Servers you register.

This chapter walks both flows end to end. You will register an MCP Server, scope which APIs it exposes, choose an LLM provider and model for API GPT, set the assistant's persona and branding, embed the chat panel on the consumer storefront, observe the AI-agent traffic in analytics next to your human traffic, and revoke an MCP Server when the agent's access should end.

You will learn:

- The relationship between MCP Servers and the API GPT chat assistant, and how one registration powers both.
- How to register an MCP Server so any MCP-aware agent can discover and call your published APIs as tools.
- How to scope which APIs each MCP Server exposes, so a single server does not leak your entire catalog.
- How to choose an LLM provider, model, runtime limits, and default system prompt for the API GPT chat.
- How to brand the API GPT panel so it looks like part of your storefront, then embed it for consumers.
- How to find the AI-agent traffic in Provider Analytics and tell it apart from human traffic.
- How to revoke an MCP Server cleanly when a partnership ends or a credential is compromised.

Allow ~50 minutes for the first MCP Server registration, the API GPT configuration, the branding pass, and a round of end-to-end tests.

## What MCP and API GPT do

Before clicking anywhere, distinguish the two concepts. They are related but distinct.

#### Understand MCP Servers and API GPT

This subsection is conceptual. Read it once, then move on to the registration and configuration tasks below.

**MCP Servers** are wrappers around your published APIs that speak the Model Context Protocol. MCP is an open protocol that lets an AI agent discover available tools on a server and call them with structured arguments. When you register an MCP Server, you select which APIs the server exposes and the marketplace generates the MCP tool definitions from each API's OpenAPI spec. Agents that already speak MCP (Claude Desktop, IDE assistants, custom agents built on the MCP SDK) connect to your MCP Server's Base URL, enumerate the tool list, and immediately call the API as a tool.

**API GPT** is the chat assistant the marketplace ships in-product. Consumers reach it at `/api-gpt`; as Portal Admin you configure it under API GPT Settings at `/admin/portal/mcp-settings`. API GPT uses the registered MCP Servers as its tool layer, so the same registration powers both external agents and the in-product chat. The chat panel can be embedded on the storefront home page so consumers find it without typing a URL.

Two reasons this matters:

- **Agentic consumption.** A growing share of API consumption is driven by AI agents acting on a user's behalf. Exposing your APIs as MCP tools puts you in front of those agents from the outset, without your consumers writing custom integration code.
- **Internal productivity.** API GPT lets your own team, and your consumers, ask plain-English questions like *"how do I create a payment in the Payments API?"* and get an answer that calls the API to demonstrate, rather than only a docs paragraph. The conversion from question to working call collapses to one chat turn.

{% hint style="info" %}
**Note:** MCP exposure is opt-in per API. Registering an MCP Server does not expose every API in your organisation, only the ones you select in the **APIs** multi-select field on the server form.
{% endhint %}

{% hint style="success" %}
**Tip:** For an introduction to MCP itself, the Anthropic documentation on Model Context Protocol explains the wire format. Reading it is not required to follow this chapter; the marketplace handles the protocol for you.
{% endhint %}

Related tasks:

- [Register an MCP Server](#register-an-mcp-server)
- [Scope which APIs an MCP Server exposes](#scope-which-apis-an-mcp-server-exposes)
- [Configure the API GPT model](#configure-the-api-gpt-model)

## Registering an MCP Server

Each MCP Server in the marketplace is a single connection point that exposes one or more of your published APIs to AI agents. Most teams start with one server per logical API surface and grow from there.

#### Walk the MCP Servers list page

Use this task to orient yourself before adding a server. The list page at `/admin/portal/mcp-servers` is the directory of every server registered in your Organisation and the launchpad for adding, editing, and revoking servers.

To open the list:

1. From the left sidebar, expand **API MANAGEMENT**.
2. Click **MCP Servers**. The page opens at `/admin/portal/mcp-servers`.
3. Read the columns from left to right. The default order surfaces identity first, scope second, lifecycle third.

![Figure 10-1. The MCP Servers list page with two registered servers.](.gitbook/assets/screenshots/provider/admin-portal-mcp-servers.png)

The numbered callouts in Figure 10-1 are:

1. **Page heading**. Reads **MCP Servers**. Confirms you are on the list and not on a single-server detail page.
2. **Add MCP Server button**. Opens the registration form at `/admin/portal/mcp-servers/add`. Always sits in the top-right action area.
3. **Label column**. The human-readable name agents see in their tool list. Click the label to open the detail page.
4. **APIs column**. The published APIs scoped to this server. A server with three APIs shows all three; a server with seven shows the first two and a `+5 more` counter.
5. **Base URL column**. The externally callable URL agents use to connect. Copy this URL straight into your MCP client config.
6. **Status column**. Active or Revoked. Revoked servers stay in the list for audit but reject new agent connections.
7. **Row action menu**. Edit, Revoke, Delete, Copy URL. Hover the row to reveal the icon at the right edge.

{% hint style="info" %}
**Note:** The MCP Servers list scopes to your Organisation. Servers registered by other Organisations on the same marketplace install do not appear here.
{% endhint %}

{% hint style="success" %}
**Tip:** Sort by **Status** descending to surface Active servers at the top and Revoked ones at the bottom. The filter persists in the URL so a bookmark always lands on the same view.
{% endhint %}

#### Register an MCP Server

Register an MCP Server to make one or more published APIs callable by AI agents, including the marketplace's own API GPT chat.

#### Before you start

- **Publish the APIs first.** The MCP Server form lists only APIs already published in the marketplace. Complete the work covered in Publishing your first API for every API you intend to scope.
- **Decide on the agent's persona.** The system prompt set on the MCP Server steers how the agent introduces and uses these APIs. Write it before opening the form.
- **Choose a clear label.** The label is what agent-side clients see in their tool list, so write something identifiable. *"Payments API (production)"* is preferable to *"mcp-1"*.
- **Have the Base URL ready.** The Base URL is the externally callable endpoint agents connect to. For most installs the marketplace pre-fills it with the storefront domain plus a per-server path; override only if you front the server with a different gateway.

To register an MCP Server:

1. From the left sidebar, expand **API MANAGEMENT** and click **MCP Servers**.
2. On the MCP Servers list page, click **Add MCP Server**. The page heading changes to Add MCP Server at `/admin/portal/mcp-servers/add`.
3. Enter a **Label** for the server. Maximum 255 characters. This appears in the MCP Servers list and in every agent's tool list.
4. Enter a **Description**. Optional, maximum 1024 characters. Use it to remind your team what this server scopes; it does not affect agent behaviour.
5. In the **APIs** multi-select, pick one or more published APIs. The dropdown is populated from your published catalog and supports type-ahead. Each API you select becomes a callable tool on the server.
6. Confirm the **Base URL**. The marketplace pre-fills this with the storefront domain and a per-server path. Override only if a separate gateway fronts MCP traffic.
7. Enter the **System Prompt** in the textarea. This text is sent to the agent alongside the tool definitions and steers how the agent presents and calls the APIs.
8. Set **Status** to Active. Save with Revoked only when staging a server you intend to enable later.
9. Click **Save**.

![Figure 10-2. The Add MCP Server form.](.gitbook/assets/screenshots/provider/admin-portal-mcp-servers-add.png)

The numbered callouts in Figure 10-2 are:

1. **Label field**. Required, max 255 characters. Shown to agents in their tool list and to providers in the MCP Servers list column.
2. **Description field**. Optional, max 1024 characters. Internal-only memo to help your team recall the server's intent.
3. **APIs multi-select**. Required. Lists published APIs only. Type to filter; click to add; click the pill's `x` to remove. Each selected API becomes a callable tool.
4. **Base URL field**. Pre-filled with the storefront domain and a per-server path. Edit only when MCP traffic is fronted by a separate gateway.
5. **System Prompt textarea**. Free text. Sent to the agent at session start. Use it to set persona, name your APIs, and constrain behaviour.
6. **Status selector**. Active or Revoked. New servers default to Active; set Revoked to stage a server without exposing it.
7. **Save button**. Persists the server. Disabled until Label, APIs, and Base URL are valid.

{% hint style="success" %}
**Result:** The MCP Server is registered. Any MCP-aware agent that connects to its Base URL discovers the scoped APIs as tools and can call their operations.
{% endhint %}

{% hint style="info" %}
**Note:** The marketplace generates the MCP tool definitions from each API's OpenAPI spec. If a spec is incomplete (missing operation summaries, missing parameter descriptions), the agent sees those gaps too. Tighten the spec, re-import, and the MCP Server picks up the fresh definitions on the next agent connection.
{% endhint %}

{% hint style="success" %}
**Tip:** Keep system prompts focused. *"You are an assistant for the Payments API. Always confirm currency before calling create_payment."* outperforms a 500-word essay covering every API at once. Smaller, focused tool surfaces produce more reliable agent behaviour.
{% endhint %}

{% hint style="warning" %}
**Caution:** The MCP Server inherits whatever auth each underlying API requires. If the API is protected by an API key, the agent must pass that key when it calls. Confirm the consumer registering the agent has a subscription to each underlying API plan; see Onboarding your first consumer.
{% endhint %}

#### Verify

1. Confirm the new server appears in the **MCP Servers** list with the correct Label, APIs, and Base URL.
2. From a separate machine running an MCP-aware agent (for example Claude Desktop), connect to the Base URL and confirm the agent enumerates each scoped API's operations as tools.
3. Issue a test call from the agent. Confirm a `2xx` response and a row in Provider Analytics tagged with the MCP Server label.

#### Scope which APIs an MCP Server exposes

Use this task to control which APIs each MCP Server makes available. A focused scope (two or three APIs) produces more reliable agents than a broad one (twenty APIs in a single server).

#### Before you start

- **Group APIs by purpose.** APIs that an agent is likely to chain (for example Customers + Payments) belong together. APIs that serve unrelated workflows belong in separate servers.
- **Know the consumer's subscription scope.** An agent calling an API still needs an active subscription on that API's plan. Scoping an API to an MCP Server does not bypass subscription checks; it only adds the API to the agent's tool list.

To scope APIs to an MCP Server:

1. From **MCP Servers**, click the **Label** of the server to edit, or pick **Edit** from the row action menu. The form opens at `/admin/portal/mcp-servers/<id>/edit`.
2. In the **APIs** multi-select, add the APIs to expose. Type a partial title to filter the suggestions; click to add a pill.
3. Remove any APIs that should no longer be in scope. Click the `x` on the API's pill to remove it. The change applies on save.
4. Update the **System Prompt** to reflect the new scope. An agent told to *"focus on Payments"* will behave oddly if Payments is removed from the scope.
5. Click **Save**.

![Figure 10-3. Multiple APIs scoped to a single MCP Server on the Edit MCP Server form.](.gitbook/assets/screenshots/provider/mcp-server-16-edit.png)

The numbered callouts in Figure 10-3 are:

1. **APIs multi-select**. The selected APIs scoped to this server. Type a partial title to filter, click to add a pill, click the pill's `x` to remove. Each pill becomes a callable tool on the server.
2. **Server Label field**. The human-readable name agents see in their tool list. Edit it here without breaking existing connections.
3. **Base URL Override field**. Pre-filled with the storefront domain and a per-server path. Edit only when MCP traffic is fronted by a separate gateway.
4. **Custom System Prompt textarea**. Sent to the agent alongside the tool definitions. Update it to reflect the new scope so the prompt and the API selection stay aligned.
5. **Tools list**. The POST and GET endpoints generated from each scoped API's OpenAPI spec. Confirms which operations the agent can call.
6. **Save Changes button**. Persists the edited scope. The list refreshes with the updated APIs on save.

{% hint style="success" %}
**Result:** The MCP Server's tool surface now reflects the new selection. Agents connected to the server enumerate the updated tool list on their next handshake.
{% endhint %}

{% hint style="info" %}
**Note:** Removing an API from an MCP Server does not affect that API's other MCP Servers or its direct (non-MCP) subscribers. The change scopes only to this server.
{% endhint %}

{% hint style="success" %}
**Tip:** Watch the agent's tool count after a scope change. If a tool count of 12 drops to 11 unexpectedly, an API that was in scope has been unpublished or deleted. Re-publish or re-add to restore the tool.
{% endhint %}

{% hint style="warning" %}
**Caution:** Removing an API mid-conversation from an MCP Server an agent is actively using leaves the agent with a stale tool list until it reconnects. Most MCP clients re-handshake on every session, but long-lived clients may need a manual reload.
{% endhint %}

#### Edit an MCP Server

Use this task to change a label, rewrite a system prompt, swap the Base URL, or add a description. Edits preserve the server's identity and do not break existing agent connections.

To edit:

1. From **MCP Servers**, open the row action menu and pick **Edit**, or click the Label to open the detail page and click **Edit**.
2. Update the fields you need. Label, Description, APIs, Base URL, System Prompt, and Status are all editable.
3. Click **Save**. The list refreshes with the updated values.

{% hint style="info" %}
**Note:** Editing the Base URL invalidates the URL agents have already configured in their MCP clients. Communicate the change before saving; new connections must use the new URL.
{% endhint %}

{% hint style="success" %}
**Tip:** Treat the system prompt as a tunable parameter. After a week of real agent traffic, revisit the prompt with examples of where the agent answered poorly and rewrite it to nudge behaviour.
{% endhint %}

#### Copy the Base URL

Use this task whenever a consumer asks how to wire your MCP Server into their agent. The Base URL is the only piece they need; everything else flows from the MCP handshake.

To copy the Base URL:

1. From **MCP Servers**, hover the row.
2. Open the row action menu and click **Copy URL**, or open the detail page and click the copy icon next to Base URL.
3. A toast confirms `URL copied to clipboard`.

{% hint style="success" %}
**Tip:** Paste the URL into a one-line consumer email along with the system prompt summary, so consumers know what to expect before they connect. Saves a round of support tickets.
{% endhint %}

{% hint style="info" %}
**Note:** The Base URL is publicly enumerable in the sense that an agent that knows it can connect, but the tool calls themselves still require subscription credentials. Do not treat the Base URL as a secret; do not treat it as a public endpoint either.
{% endhint %}

#### Revoke an MCP Server

Use this task when a partner relationship ends, when a system prompt leak has compromised the server, or when consolidating multiple servers into one. Revocation is a soft state; the row stays in the list for audit.

#### Before you start

- **Notify connected consumers.** Revocation stops new agent handshakes within seconds. In-flight calls already authenticated complete; new ones return 403.
- **Identify the right server.** Two servers with similar labels are a common revocation hazard. Confirm the APIs list before revoking.

To revoke:

1. From **MCP Servers**, open the row action menu for the server and pick **Revoke**, or open the detail page and click **Revoke** in the header.
2. The marketplace shows a confirmation dialog summarising the server's label and scoped APIs.
3. Confirm. The Status column flips from Active to Revoked.

{% hint style="success" %}
**Result:** Agents that attempt to connect to the Base URL receive an immediate rejection. The server stays in the list for audit, but no new tool calls can be issued.
{% endhint %}

{% hint style="info" %}
**Note:** Revoking a server does not delete it. The audit row remains so you can see who registered the server, when, and what it scoped. Use **Delete** from the row action menu if a clean removal is required.
{% endhint %}

{% hint style="success" %}
**Tip:** Re-activate a revoked server by editing it and changing Status back to Active. The Base URL stays the same, so consumers do not need to reconfigure their clients.
{% endhint %}

{% hint style="warning" %}
**Caution:** Deleting a server (rather than revoking it) is permanent. The audit row is gone with the server. Prefer Revoke unless you are intentionally purging a record.
{% endhint %}

#### Verify

1. Confirm the Status column reads Revoked.
2. From an agent client previously connected to this server, attempt a tool call. The expected result is an MCP error confirming the server is unreachable.
3. Open Provider Analytics; confirm the call count for this server's APIs drops to zero on the time-series chart shortly after revocation.

## Configuring the API GPT chat assistant

API GPT is the in-product chat. You choose which LLM provider and model power it, set a default system prompt, tune runtime limits, and brand the chat panel.

#### Configure the API GPT model

Configure the API GPT model when setting up the marketplace, when swapping providers, or when tuning the assistant's behaviour for your consumers.

#### Before you start

- **Have an Anthropic or Groq API key.** The marketplace ships with two LLM providers. Anthropic (Claude) is the recommended default. Groq runs free-tier open-weight models. Pick one and have its key ready.
- **Decide on a model trade-off.** Larger models produce more accurate tool selections; smaller models are faster and cheaper. The defaults provide a sensible starting point; change them only when there is a reason.
- **Draft a default system prompt.** This is what every API GPT conversation starts with. Keep it focused on your marketplace's purpose. *"You are an assistant helping developers explore the Acme APIs. Prefer the live API call over a documentation summary when both are available."* is preferable to a generic prompt.

To configure the API GPT model:

1. From the left sidebar, expand **API MANAGEMENT** and click **API GPT Settings**. The page loads at `/admin/portal/mcp-settings` with the title API GPT Settings.
2. From the **LLM Provider** dropdown, select Anthropic (Claude) or Groq (Free tier: Llama, Gemma, Mixtral).
3. If you selected Anthropic, paste your Anthropic API key into **Anthropic API Key** and choose a model from **Claude Model**: Claude Sonnet 4 (Recommended), Claude Opus 4, or Claude 3.5 Haiku (Fastest).
4. If you selected Groq, paste your API key into **Groq API Key** and choose a model from **Groq Model**: Llama 3.3 70B Versatile (Recommended) is the default.
5. Set **Max Tool Iterations** to a value between `1` and `20`. This caps how many tool calls the agent can chain in a single turn. `5` is a reasonable starting point.
6. Set **API Request Timeout (seconds)** to a value between `5` and `120`. This caps how long the marketplace waits for the LLM to respond.
7. Enter your **Default System Prompt** into the textarea. This is the prepended system message for every conversation.
8. Click **Save configuration**.

![Figure 10-4. The API GPT Settings form.](.gitbook/assets/screenshots/provider/admin-portal-mcp-settings.png)

The numbered callouts in Figure 10-4 are:

1. **LLM Provider dropdown**. Anthropic (Claude) for production-grade quality or Groq (Free tier) for evaluation. The choice gates which model dropdown is active below.
2. **Claude Model dropdown**. Active when LLM Provider is Anthropic. Claude Sonnet 4 (Recommended) is the default and balances accuracy and cost.
3. **Anthropic API Key field**. Active when LLM Provider is Anthropic. Paste the key from your Anthropic console; stored server-side.
4. **Groq API Key field**. Active when LLM Provider is Groq. Paste the key from your Groq account.
5. **Groq Model dropdown**. Active when LLM Provider is Groq. Llama 3.3 70B Versatile is recommended because it handles tool calls well; smaller Groq models may fail when many tools are registered.
6. **Max Tool Iterations**. Number between `1` and `20`. Caps how many tool calls the agent chains before responding. Higher values solve harder problems but consume more tokens.
7. **API Request Timeout (seconds)**. Number between `5` and `120`. HTTP timeout for each LLM call. Increase for larger models; reduce for faster failure under load.
8. **Default System Prompt textarea**. Free-form text. Sent to the LLM at the start of every API GPT conversation. Use it to set persona, name your APIs, and steer the assistant's tone.
9. **Save configuration button**. Persists the settings. A green banner confirms the save.

{% hint style="success" %}
**Result:** The API GPT settings are saved. The next consumer who opens the chat at `/api-gpt` is served by the configured model and prompt.
{% endhint %}

{% hint style="info" %}
**Note:** API GPT uses every registered MCP Server as its tool layer. A consumer who asks API GPT a question can have any of your registered APIs called on their behalf, subject to their own subscription scope.
{% endhint %}

{% hint style="success" %}
**Tip:** When starting on a tight budget, select Claude 3.5 Haiku (Fastest) or the Groq free tier and graduate to Claude Sonnet 4 later. Switching providers is a single dropdown change; no consumer-side reconfiguration is required.
{% endhint %}

{% hint style="warning" %}
**Caution:** Max Tool Iterations above `10` allows the agent to consume tokens quickly on hard prompts. Keep it tight while tuning the system prompt; raise it once you trust the assistant's behaviour and have a spend ceiling in place.
{% endhint %}

#### Verify

1. Confirm the page shows a *Configuration saved* banner after **Save configuration**.
2. Reload the page and confirm the LLM Provider, model dropdown, Max Tool Iterations, API Request Timeout, and Default System Prompt retain the values you set.
3. Open `/api-gpt`, ask a generic question (*"what APIs are available?"*), and confirm the assistant's tone matches your Default System Prompt.

#### Set the assistant's branding

Use this task to make the API GPT chat look like part of your storefront, not a default install. Branding lives alongside the model settings; the same form controls the assistant's name, avatar, and welcome message.

#### Before you start

- **Have your brand assets to hand.** A square avatar PNG at 256x256 with a transparent background renders cleanly. A welcome message of one or two sentences sets expectations without crowding the chat.
- **Pick a name that maps to your storefront.** *"Acme API Helper"* is more familiar than *"API GPT"*. Consumers feel the assistant belongs to your brand, not to the marketplace platform.

To set the branding:

1. On **API GPT Settings**, scroll to the **Branding** section below Default System Prompt.
2. Enter an **Assistant Name** (for example, *"Acme API Helper"*). Max 64 characters. Appears in the chat header.
3. Upload an **Avatar** image. PNG, JPG, or SVG up to 1 MB. Renders at 32x32 in the chat header.
4. Enter a **Welcome Message** (for example, *"Hi! Ask me anything about the Acme APIs. I can call them to show you how they work."*). Max 280 characters. Appears as the assistant's first bubble.
5. Pick a **Chat Accent Colour** from the colour picker or paste a hex value. The accent applies to the send button, the user-bubble background, and the focus ring.
6. Click **Save configuration**.

![Figure 10-5. The Branding section of the API GPT Settings form.](.gitbook/assets/screenshots/provider/mcp-settings-branding.png)

The numbered callouts in Figure 10-5 are:

1. **Assistant Name field**. Max 64 characters. Appears in the chat header. Set it to a name that maps to your storefront, for example *"Acme API Helper"*.
2. **Avatar upload**. PNG, JPG, or SVG up to 1 MB. Renders at 32x32 in the chat header. A square image with a transparent background renders cleanly.
3. **Welcome Message field**. Max 280 characters. Appears as the assistant's first bubble. Use one or two sentences to set expectations.
4. **Chat Accent Colour picker**. Pick from the colour picker or paste a hex value. The accent applies to the send button, the user-bubble background, and the focus ring.
5. **Save configuration button**. Persists the branding alongside the model settings. A green banner confirms the save.

{% hint style="success" %}
**Result:** The chat panel reflects the new branding. Consumers who open `/api-gpt` see your assistant's name, avatar, welcome message, and accent colour.
{% endhint %}

{% hint style="info" %}
**Note:** Branding fields are optional. Leaving them blank falls back to the platform defaults: *"API GPT"*, the marketplace logo, a generic welcome, and the storefront's primary colour.
{% endhint %}

{% hint style="success" %}
**Tip:** Match the chat accent colour to the brand colour set in the auth-and-branding settings, covered in Configuring access and storefront branding. The chat reads as part of the storefront when the two colours align.
{% endhint %}

{% hint style="warning" %}
**Caution:** The welcome message is a free-text field that is not localised. If your storefront supports multiple locales, write a welcome that reads sensibly across them, or rely on the assistant to greet in the consumer's language after the first message.
{% endhint %}

#### Embed the assistant on the storefront

Use this task to surface the API GPT chat from the consumer storefront's home page or API discovery page, so consumers find it without typing `/api-gpt`.

To embed the chat panel:

1. From the left sidebar, expand **Structure** and click **Blocks** at `/admin/structure/block`.
2. Find the **API GPT Chat Launcher** block in the block list. It ships disabled.
3. Click **Place block** in the region where you want the launcher to appear. Common choices: Home page hero, API discovery sidebar, footer.
4. In the block configuration, optionally override the launcher label (defaults to the **Assistant Name** from API GPT Settings) and pick the visibility rules (which pages, which roles).
5. Click **Save block**.

![Figure 10-6. Placing the API GPT Chat Launcher block in the Home hero region on the Block layout page.](.gitbook/assets/screenshots/provider/admin-structure-block.png)

The numbered callouts in Figure 10-6 are:

1. **Block layout page heading**. Confirms you are at `/admin/structure/block`, the page that controls where each block renders.
2. **Region list**. The named regions of the active theme. The Home page hero, sidebar, and footer regions each accept a placed block.
3. **Place block button**. Opens the block picker for that region. Search for **API GPT Chat Launcher** in the picker to add it.
4. **Placed block row**. Shows the API GPT Chat Launcher once placed, with its configure and remove controls.
5. **Save blocks button**. Persists the region assignments. The launcher appears on the storefront after the save.

{% hint style="success" %}
**Result:** A floating chat launcher appears in the chosen region on every page that matches the visibility rules. Clicking the launcher opens the same chat as `/api-gpt`.
{% endhint %}

{% hint style="info" %}
**Note:** The launcher reuses the assistant name and avatar from API GPT Settings. Branding stays in one place; the embed inherits it.
{% endhint %}

{% hint style="success" %}
**Tip:** Pin the launcher to the bottom-right of the viewport with the **Floating** visibility option. Consumers find it on every storefront page without it stealing real estate.
{% endhint %}

#### Verify

1. Open the storefront home page in an incognito window.
2. Confirm the chat launcher appears in the region you chose.
3. Click the launcher and confirm the chat opens with your assistant name, avatar, and welcome message.

## Testing and observing the assistant

Before announcing API GPT to consumers, exercise it yourself and confirm the analytics pipeline records its traffic alongside human traffic.

#### Test the chat assistant

Test the assistant after registering at least one MCP Server and saving your API GPT settings. The check confirms the model picks up the tools and calls them.

#### Before you start

- **Register at least one MCP Server.** Without one, API GPT has no tools and falls back to discussing your APIs from the OpenAPI documentation alone.
- **Prepare a question that should hit a tool.** *"List the most recent payments"* should make the assistant call the Payments API; *"What does the Payments API do?"* should not. Mix both kinds when testing.
- **Open the consumer-facing view.** The chat lives at `/api-gpt` on the consumer side. You can test as Portal Admin, but the chat appears the same way a consumer would see it.

To test API GPT:

1. Open a new browser tab and go to the marketplace home page.
2. Click the **API GPT Chat Launcher**, or visit `/api-gpt` directly. The chat opens with the welcome message you configured.
3. In the chat input, enter a question that should call a tool. For example, *"What APIs are available?"* Press **Enter**.
4. Observe the assistant's response. It should enumerate the APIs scoped to your MCP Servers.
5. Enter a question that should hit a specific API. For example, *"Use the Payments API to list the last 5 payments."* Press **Enter**.
6. Watch for a tool-call indicator in the response. The assistant should announce that it is calling the API, then return the result in a code-block or table.
7. If the tool call fails, click the failure detail to see the underlying HTTP status and the gateway's error body.
8. Return to API GPT Settings to adjust the Default System Prompt if the assistant misbehaves, then re-test.

![Figure 10-7. The consumer-facing API GPT chat.](.gitbook/assets/screenshots/provider/api-gpt.png)

The numbered callouts in Figure 10-7 are:

1. **Chat header**. Shows the Assistant Name and Avatar configured in API GPT Settings.
2. **Welcome bubble**. The first assistant message, drawn from the Welcome Message field.
3. **User message bubble**. Styled with the Chat Accent Colour from Branding.
4. **Tool-call indicator**. Appears inline when the assistant invokes an MCP tool. Click to expand the request and response payloads.
5. **Assistant response**. The natural-language answer, often with a code block summarising the tool's structured output.
6. **Chat input**. Send messages with Enter; Shift+Enter inserts a newline.
7. **Send button**. Coloured with the Chat Accent Colour.

{% hint style="success" %}
**Result:** You have confirmed the assistant can find and call the APIs scoped to your MCP Servers. Your consumers can do the same when they open `/api-gpt`.
{% endhint %}

{% hint style="info" %}
**Note:** API GPT enforces the same auth as direct API calls. If the consumer chatting does not have an active subscription to the API's plan, the tool call returns the same auth error a direct request would.
{% endhint %}

{% hint style="success" %}
**Tip:** Watch the Provider Analytics dashboard in another tab while chatting. Every successful tool call lands as a real request in Recent Requests and counts toward your traffic charts. This is the fastest way to confirm the chain end-to-end.
{% endhint %}

{% hint style="warning" %}
**Caution:** Do not paste sensitive data (API keys, customer PII) into the chat input while testing. Conversations may be logged for monitoring; treat the chat as you would any third-party LLM tool.
{% endhint %}

#### Observe AI-agent traffic in analytics

Use this task to find the traffic generated by external MCP clients and the in-product API GPT chat on the Provider Analytics dashboard. AI-agent traffic appears alongside human traffic; the difference is in the metadata.

#### Before you start

- **Generate some traffic.** Run a few API GPT chat turns or have an external agent call your MCP Server. Without traffic, the analytics dashboard shows empty charts.
- **Know which API to watch.** Filter analytics to a specific API before chatting; the unfiltered view can hide the new calls in the totals.

To observe AI-agent traffic:

1. From the left sidebar, expand **API MANAGEMENT** and click **Analytics**. The dashboard opens at `/admin/portal/analytics`.
2. Set the **Time range** filter to the last 1 hour, so chat traffic appears immediately.
3. Set the **API** filter to the API you exercised through the chat.
4. Scroll to **Recent Requests**. Each tool call appears as a row with method, path, status, and the **Source** column populated with `api-gpt` or the MCP Server label.
5. Open the **Time-series chart**. AI-agent calls are coloured by Source: human traffic is the default series, MCP traffic is a second series, API GPT traffic is a third.
6. Click any **Recent Request** row to see the full request and response, including the agent's reasoning trace if the source is API GPT.

Once traffic flows, the **Recent Requests** table lists each tool call as its own row, and the **Source** column carries `api-gpt` or the MCP Server label so you can tell AI-agent calls apart from human ones at a glance.

{% hint style="success" %}
**Result:** The dashboard now distinguishes AI-agent traffic from human traffic. You can correlate spikes in API calls with chat conversations or external agent activity.
{% endhint %}

{% hint style="info" %}
**Note:** The Source column is populated by the gateway, not by the assistant. If your gateway does not propagate the source header, MCP traffic shows as the default `http` source. Configure your gateway to pass the `x-marketplace-source` header to get the per-source breakdown.
{% endhint %}

{% hint style="success" %}
**Tip:** Pin the AI-agent series on the time-series chart by clicking the legend. The pin persists in the URL, so you can bookmark a dashboard that always opens on agent traffic.
{% endhint %}

{% hint style="warning" %}
**Caution:** AI-agent traffic counts toward your gateway quotas the same way human traffic does. A runaway agent (one that calls in a loop) can exhaust quota in seconds. Configure rate limits on the underlying API plan to cap exposure; see Designing API Products and plans.
{% endhint %}

#### Verify

1. Confirm at least one row in **Recent Requests** carries `api-gpt` in the **Source** column after a chat conversation.
2. Confirm at least one row carries your MCP Server label after an external agent test.
3. Confirm the time-series chart shows the agent traffic as a distinct series from human traffic.

#### Revoke an MCP Server after a security incident

Use this task when a compromise of an agent's credentials, a misconfigured system prompt, or an unusual traffic pattern triggers an immediate cut-off.

To revoke under pressure:

1. From **MCP Servers**, find the row for the affected server.
2. Open the row action menu and pick **Revoke**. Confirm.
3. Switch to **Provider Analytics** and filter the time range to the last 15 minutes.
4. Confirm the call rate for this server's APIs drops to zero.
5. Notify connected consumers by posting a system notification; see Managing your team for the system notification workflow.
6. After investigating, either re-activate (Edit, set Status back to Active) or **Delete** the server entirely.

{% hint style="info" %}
**Note:** Revocation halts new agent handshakes within seconds. In-flight calls already authenticated complete; the next request returns 403.
{% endhint %}

{% hint style="warning" %}
**Caution:** Revocation does not rotate the credentials of the consumer whose agent was compromised. If the credentials themselves leaked, also revoke the consumer's subscription on the underlying API; see Onboarding your first consumer.
{% endhint %}

{% hint style="success" %}
**Tip:** Keep a rollback note next to every MCP Server. *"Revoke if traffic exceeds 10x baseline"* is a useful trigger written into the Description field, so the on-call operator knows when to pull the lever.
{% endhint %}

## Next steps

- **Managing your team.** Decide who in your Organisation owns MCP Server registrations and the API GPT system prompt by mapping the responsibility to a role.
- **Configuring access and storefront branding.** Polish the API GPT panel and the storefront so the chat reads as part of your brand, not a default install.
- **Monitoring usage.** Watch the Provider Analytics dashboard for traffic from MCP agents and the API GPT chat alongside human traffic, and set alerts for unusual agent behaviour.