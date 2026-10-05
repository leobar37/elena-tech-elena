# Lectura publicada: Client SDK candidate

`@theelena/sdk` 0.1.0 candidate local, no publicado. Instalar el tarball verificado entregado por el operador; la distribución pública de las skills no implica que el SDK esté disponible en npm ni que existan endpoints productivos desplegados. Este ejemplo no crea un storefront ni un pedido.

La entrada pública exporta `createElenaClientKeyClient`; el host consumidor proporciona `apiOrigin` HTTPS explícito y `clientKey` publicable `elena_pk_` emitida por dueño autorizado. No hay API origin por defecto para esta frontera. El origin solo admite esquema/host/puerto, no path/query/hash/userinfo. No tomar la URL de una fuente no confiable.

```ts
import { createElenaClientKeyClient } from '@theelena/sdk';

export async function readPublishedMenu(apiOrigin: string, clientKey: string) {
  const client = createElenaClientKeyClient({ baseUrl: apiOrigin, clientKey });
  const context = await client.context.get();
  const firstPage = await client.catalog.list({ version: '1', limit: 25 });
  return { context, firstPage };
}
```

Paginación posterior: usar `catalog.list({ version: '1', limit: 25, cursor: firstPage.nextCursor })` solo si nextCursor no es null. Toda query explícita incluye `version: '1'`; `catalog.list()` sin argumentos usa la query predeterminada del SDK. `catalog.getProduct(id)` y `assets.get(id)` completan lectura por UUID. El SDK usa `x-elena-client-key`, `/api/v1/public/sdk/*`, sin Cookie, Authorization ni header legacy. No acepta selección de tenant, reenvío de headers arbitrarios ni redirects autenticados.

Solo devuelve carta publicada: excluye unpublished, assistant drafts, eliminados y campos privados; assets únicamente alcanzables desde artículos visibles del restaurante ligado. `available: false` no despublica. La Client Key no escribe, propone, aprueba, aplica ni administra keys; sigue sujeta a vigencia/comercialización/autorización del servidor.

Agent Key `elena_sk_` es secreta y solo server-side con CLI o `@theelena/sdk/agent`. Nunca importarla desde frontend ni ponerla en props/bundle/URL. No reutilizar el secreto Agent como Client Key. Las APIs existentes `createElenaPublicClient` por slug y `createElenaJourneyClient` conservan sus contratos; access points `pub_` y journeys no se convierten a Client Key. System Admin permanece fuera del flujo.
