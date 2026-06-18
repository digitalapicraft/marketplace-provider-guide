---
icon: diagram-project
---

Astra splits cleanly into two planes: a management plane that decides policy and reads metadata, and a data plane that enforces that policy on every live call. Understanding this split, the product family around it, and the stack it runs on explains how the marketplace publishes an API without ever standing in the request path.

![Figure. The Astra platform architecture.](.gitbook/assets/screenshots/provider/architecture.png)

## Management plane vs data plane

Astra is the management plane and stays out of the request path. The gateway is the data plane and terminates every live call. The same concern is split across the two: Astra defines a plan's limits and the gateway enforces them; Astra provisions a credential and the gateway validates it on every call; Astra reads usage and the gateway meters it.

*Astra decides policy and reads metadata; the gateway enforces it on every live call.*

```mermaid
flowchart LR
    subgraph Mgmt["Management plane, Astra"]
        I["Import specs and metadata"]
        G["Govern and score"]
        PUB["Publish catalogue"]
        PL["Define products and plans"]
        CR["Provision credentials"]
    end
    subgraph Data["Data plane, gateway"]
        AU["Authenticate credential"]
        EN["Enforce limits and policy"]
        RT["Route to backend"]
        MT["Meter usage"]
    end
    PL -->|"limits"| EN
    CR -->|"credentials"| AU
    MT -->|"usage metadata"| G
```

<details><summary>What each plane owns</summary>

- **Management plane (Astra):** imports specs and metadata, governs by scoring specs against a ruleset, publishes the catalogue, defines products and plans, manages subscription state and approvals, provisions app credentials, and reads usage for analytics.
- **Data plane (gateway):** authenticates the credential or API key, enforces rate limits and quotas, routes to the backend, applies security and traffic policy, meters usage, and allows or rejects each call in real time.

</details>

## The product family

Three products form one path from publishing an API to consuming it. Marketplace manages and configures the gateway, then publishes its catalogue to the Developer Portal. Consumers subscribe in the portal; their apps then call the gateway directly. Astra Gateway is first-class, and third-party gateways federate the same way.

*Marketplace configures the gateway and publishes to the portal; consumer apps call the gateway directly.*

```mermaid
graph TD
    MP["Astra Marketplace, control plane"]
    GW["Astra Gateway, data plane"]
    DP["Developer Portal, consumer experience"]
    APP["Consumer app"]
    MP -->|"configure and meter"| GW
    MP -->|"publish catalogue"| DP
    DP -->|"subscribe and get keys"| APP
    APP -->|"live API calls"| GW
```

<details><summary>What each product does</summary>

- **Astra Marketplace:** the provider control plane that federates gateways, governs and publishes, manages products, plans, and subscriptions, and provides provider analytics.
- **Astra Gateway:** the data plane that terminates live calls, authenticates credentials, enforces limits and policy, and meters usage.
- **Developer Portal:** the consumer experience, a branded catalogue for discovering, subscribing, getting keys, and tracking usage.

</details>

## The technology stack

Astra is built on Drupal CMS and PHP, deployed on Kubernetes, and multi-tenant by design so that every data access is scoped to its tenant. The stack runs in five layers, from the experience consumers touch down to the platform it all runs on.

*The stack in layers, from the consumer experience down to the runtime platform.*

```mermaid
flowchart TD
    EXP["Experience, Developer Portal, API Studio, MCP servers, API GPT"]
    APP["Application, PHP services, governance and rating engines, subscription state machine"]
    DATA["Data, PostgreSQL, cache bins, Queue API"]
    INT["Integration, gateway connectors, billing adapters, identity"]
    PLAT["Platform, Docker, Kubernetes, GitHub Actions"]
    EXP --> APP
    APP --> DATA
    DATA --> INT
    INT --> PLAT
```

<details><summary>What each layer holds</summary>

- **Experience:** Developer Portal (Drupal / Twig theme), API Studio test console, MCP servers, API GPT assistant.
- **Application:** PHP 8.4 services, plugins and event subscribers, governance and rating engines, the subscription state machine.
- **Data:** PostgreSQL 17, cache bins, Queue API for async usage, billing, and email.
- **Integration:** gateway connectors, billing adapters, identity (Okta OAuth2 / SAML SSO).
- **Platform and DevOps:** Docker Compose locally, Kubernetes at runtime, GitHub Actions CI/CD.

The ecosystem is open across gateways (Astra Gateway, Apigee, Kong, APISIX, AWS, Azure APIM, IBM), spec and repo sources (SwaggerHub, GitHub, Bitbucket, Postman), identity (Okta, Azure AD, SAML SSO), billing adapters (Stripe, Kill Bill, Zuora, Chargebee), and an AI layer (MCP servers, API GPT assistant).

</details>

## Deployment models

The same multi-tenant codebase ships in three operating models, so you can pick the one that fits your security and compliance needs:

- **SaaS:** DigitalAPI-hosted, fully managed and multi-tenant, with upgrades handled for you. Best for teams that want zero ops.
- **Private cloud:** single-tenant and dedicated, running in your AWS, Azure, or GCP account with VPC isolation. Best for stricter isolation and control.
- **On-premises:** single-tenant and self-hosted in your data centre, with full data residency and air-gap capability on customer-managed Kubernetes. Best for regulated and sovereign workloads.

> **How-to:** for step-by-step configuration, see the How-to guides.