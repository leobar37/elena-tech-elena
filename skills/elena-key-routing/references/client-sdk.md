# Client SDK: frontera pública separada

`@theelena/sdk` 0.1.0 es candidate local, no publicado; obtener el tarball por el canal autorizado. No asumir disponibilidad npm ni endpoints de producción. `createElenaClientKeyClient` pertenece a la entrada pública browser-safe. `createElenaAgentClient` pertenece exclusivamente a `@theelena/sdk/agent`, server-only.

La Client Key `elena_pk_` usa `x-elena-client-key` y un restaurante fijado por servidor. Puede leer `context.get()`, `catalog.list({ version: '1', limit: 25 })`, `catalog.getProduct(id)` y `assets.get(id)`; toda query explícita de list incluye `version: '1'`. No tiene mutaciones, selección de tenant ni aprobación. El contexto público no contiene permisos privados. El catálogo excluye unpublished, assistant drafts y eliminados; assets solo si son alcanzables desde productos visibles. `available: false` no significa despublicado.

El host consumidor proporciona `baseUrl` HTTPS explícito (solo origin, sin path/query/hash/userinfo) y Client Key emitida por dueño autorizado. No introducir cookies, Bearer ni headers reenviados; no usar defaults legacy como prueba de despliegue. Conservar `createElenaPublicClient` (slug) y `createElenaJourneyClient` existentes sin exigirles Client Key.

Esta referencia no promete un storefront, pedidos, pagos, generación de imágenes o provisioning. Si necesitas catálogo privado o propuestas, vuelve a la frontera Agent; nunca sustituyas una key por otra en el mismo transporte.
