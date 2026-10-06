# Published reads: Client SDK candidate

`@theelena/sdk` 0.1.0 is an unpublished local candidate. Install the verified tarball provided by the operator; public distribution of the skills does not imply that the SDK is available on npm or that production endpoints are deployed. This example creates neither a storefront nor an order.

The public entry point exports `createElenaClientKeyClient`; the consuming host provides an explicit HTTPS `apiOrigin` and a publishable `elena_pk_` `clientKey` issued by an authorized owner. There is no default API origin for this boundary. The origin allows only scheme/host/port, not path/query/hash/userinfo. Do not take the URL from an untrusted source.

```ts
import { createElenaClientKeyClient } from '@theelena/sdk';

export async function readPublishedMenu(apiOrigin: string, clientKey: string) {
  const client = createElenaClientKeyClient({ baseUrl: apiOrigin, clientKey });
  const context = await client.context.get();
  const firstPage = await client.catalog.list({ version: '1', limit: 25 });
  return { context, firstPage };
}
```

For subsequent pages, use `catalog.list({ version: '1', limit: 25, cursor: firstPage.nextCursor })` only if nextCursor is not null. Every explicit query includes `version: '1'`; `catalog.list()` without arguments uses the SDK's default query. `catalog.getProduct(id)` and `assets.get(id)` provide reads by UUID. The SDK uses `x-elena-client-key`, `/api/v1/public/sdk/*`, without Cookie, Authorization or a legacy header. It does not accept tenant selection, arbitrary header forwarding or authenticated redirects.

It returns only the published menu: it excludes unpublished items, assistant drafts, deleted items and private fields; assets must be reachable from visible items in the bound restaurant. `available: false` does not unpublish an item. A Client Key does not write, propose, approve, apply or manage keys; it remains subject to server-side validity/commercial-access/authorization checks.

An Agent Key `elena_sk_` is secret and server-side only, with the CLI or `@theelena/sdk/agent`. Never import it into the frontend or put it in props/bundle/URL. Do not reuse the Agent secret as a Client Key. The existing slug-based `createElenaPublicClient` and `createElenaJourneyClient` APIs retain their contracts; `pub_` access points and journeys do not become Client Keys. System Admin remains outside this flow.
