# Client SDK: a separate public boundary

`@theelena/sdk` 0.1.0 is an unpublished local candidate; obtain the tarball through the authorized channel. Do not assume npm availability or production endpoints. `createElenaClientKeyClient` belongs to the browser-safe public entry point. `createElenaAgentClient` belongs exclusively to the server-only `@theelena/sdk/agent` entry point.

The Client Key `elena_pk_` uses `x-elena-client-key` and a server-bound restaurant. It can read `context.get()`, `catalog.list({ version: '1', limit: 25 })`, `catalog.getProduct(id)` and `assets.get(id)`; every explicit list query includes `version: '1'`. It has no mutations, tenant selection or approval. Public context contains no private permissions. The catalog excludes unpublished items, assistant drafts and deleted items; assets are included only if reachable from visible products. `available: false` does not mean unpublished.

The consuming host provides an explicit HTTPS `baseUrl` (origin only, no path/query/hash/userinfo) and a Client Key issued by an authorized owner. Do not add cookies, Bearer or forwarded headers; do not use legacy defaults as proof of deployment. Preserve the existing `createElenaPublicClient` (slug) and `createElenaJourneyClient` without requiring a Client Key.

This reference does not promise a storefront, orders, payments, image generation or provisioning. If you need a private catalog or proposals, return to the Agent boundary; never substitute one key for another in the same transport.
