# Globus Platform Map

A one-screen model of what Globus is and which service does what, so you can frame answers correctly.

## The mental model

Globus is a **hosted platform**: it brokers identity/authorization and orchestrates work against **storage systems** that run Globus data-transfer software. The application generally does _not_ move bytes itself — it asks the Transfer service to move data between collections, and the transfer happens server-to-server.

Two layers to keep straight:

- **Globus Auth** — the identity and authorization layer (OAuth2 + OpenID Connect). Everything else sits behind it. Access to a service is granted by an access token carrying the right **scopes**, issued after the user (or client) gives **consent**.
- **The platform services** — Transfer, Flows, Compute, Search, Groups, Timers — each is a "resource server" with its own API and its own scopes.

## The services

- **Globus Auth** — authentication and authorization. Registers apps/clients, issues tokens, manages scopes/consents, identities, and groups-based access.

- **Globus Transfer** — reliable, fault-tolerant, third-party (server-to-server) file transfer, sync, and deletion between **collections**, plus directory listing and permission management.

- **Globus Connect Server (GCS)** — the software institutions install on their storage to expose it to Globus, as an **endpoint** hosting **collections** (mapped and guest). The GCS Manager API manages storage gateways, collections, roles, and user credentials.

- **Globus Connect Personal** — the lightweight single-user agent that turns a laptop/workstation into an endpoint.

- **Globus Flows** — a managed automation/orchestration service. You define a **flow** (a state-machine-style definition), then start **runs** of it; flows commonly chain transfers, compute, and custom actions with human-in-the-loop steps.

- **Globus Compute** _(formerly funcX)_ — a fire-and-forget **function-as-a-service** for running Python functions on remote compute endpoints (clusters, clouds, edge). **This has its own SDK, `globus-compute-sdk`** (and `globus-compute-endpoint` for hosting), separate from `globus-sdk`. `globus-sdk` only carries Compute _scopes_ and a `ComputeAPIError`, not a full client.

- **Globus Search** — a schema-agnostic, access-controlled search index service; ingest JSON documents into an index and run structured/faceted queries, commonly to power data portals.

- **Globus Groups** — user group management used for access control across the platform (grant access to a group rather than to individuals).

- **Globus Timers** _(the scheduling service; older material may call it "Timer" or lump it under "Globus Automate")_ — schedules recurring jobs, most commonly recurring transfers, and can schedule flow runs.

## Terminology traps worth stating plainly

- **Endpoint vs collection.** In the current (GCSv5) world, a physical/logical deployment is an **endpoint**; the things you actually transfer to/from are **collections** hosted on it. Older material still uses "endpoint" loosely for what are now collections — expect the words to blur in real usage, and confirm which the person means.
- **Guest collection = the old "share."** If someone says "share," they most likely mean a guest collection.
- **funcX = Globus Compute.** Same thing, renamed. If you see funcX, translate.

## Registering an application

Every real integration needs a registered OAuth2 client (a `client_id`, and for confidential clients a secret). Register at the **developer console**: `https://app.globus.org/settings/developers`. You choose a client/app type there (native/thick client, portal/science gateway, service account, API/advanced).
