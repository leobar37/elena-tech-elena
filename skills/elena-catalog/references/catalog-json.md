# Requests V1: ejemplos sintéticos, no órdenes de ejecución

Los UUID/hash/versiones siguientes son placeholders de prueba. No son filas reales ni aprobación. El ejemplo de precio confirmado simula una revisión para probar el contrato: en operación real se requiere revisión explícita del humano, lectura del agregado completo y versiones actuales. No copiar estos JSON directamente a producción. Las respuestas usan IDs/hashes del servidor, nunca se calculan desde la transcripción.

## preview.json

Request completo. La identidad externa es estable dentro del restaurante, no una coincidencia por nombre. `expectedVersion: null` afirma ausencia; si ya existe, resolver el conflicto con lectura/identidad/versiones actuales. Confirmar también disponibilidad, descripción, galería y ausencia de extras, no solo precio.

```json
{
  "version": "1",
  "idempotencyKey": "example-preview-1",
  "currency": "PEN",
  "source": { "kind": "manual", "label": "Ejemplo sintético revisado en test" },
  "categories": [
    {
      "kind": "category.upsert",
      "ref": "category-bebidas",
      "target": {
        "kind": "external",
        "namespace": "example",
        "key": "bebidas",
        "expectedVersion": null
      },
      "data": { "name": "Bebidas", "description": null, "order": 0 }
    }
  ],
  "items": [
    {
      "ref": "item-agua",
      "target": {
        "kind": "external",
        "namespace": "example",
        "key": "agua",
        "expectedVersion": null
      },
      "data": {
        "type": "menu",
        "name": "Agua",
        "price": { "status": "confirmed", "amount": "3.00", "basis": "operator" },
        "description": { "status": "proposed", "text": "Agua, de la sección Bebidas." },
        "category": { "kind": "ref", "ref": "category-bebidas" },
        "available": true,
        "images": [],
        "extras": { "confirmed": true, "groups": [] }
      }
    }
  ]
}
```

Una price ilegible se representa con `{ "status": "unconfirmed", "reason": "illegible", "rawText": null }`. Es válida como evidencia de preview pero su fila queda invalid, sin cambio ejecutable: no seleccionarla. `basis: "sample"` solo pertenece a la fixture neutral Molly, no a preview/proposal. PEN usa string positivo con dos decimales; no number, cero, coma, exponentes o aproximación. Una descripción `proposed` no se presenta como transcripción ni como ya aplicada.

## proposal.json de import

Solo después de revisar preview: reemplazar importId/planHash por los retornados y selectedRefs por la selección humana explícita. Incluir categorías dependientes incluso si son skip. No incluir filas invalid/conflict ni asumir que servidor ampliará el conjunto.

```json
{
  "version": "1",
  "idempotencyKey": "example-proposal-1",
  "payload": {
    "kind": "import",
    "importId": "11111111-1111-4111-8111-111111111111",
    "planHash": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
    "selectedRefs": ["category-bebidas", "item-agua"]
  }
}
```

Propose devuelve approvalUrl literal. Esperar humano, releer estado approved y luego apply exacto; aprobación no ejecuta. Misma idempotencyKey requiere mismo payload; datos cambiados necesitan otra key. Preview/propuesta expiran a las 24h sin extensión por aprobación. Drift de versiones requiere propuesta/aprobación nuevas.

## Propuesta de publicación separada

Nuevo producto queda no publicado tras importar. Con `catalog:publish`, leer ID/version real del producto creado y preparar **otra** propuesta. Este ejemplo no autoriza publicación por lote implícito:

```json
{
  "version": "1",
  "idempotencyKey": "example-publication-1",
  "payload": {
    "kind": "publication",
    "published": true,
    "products": [
      { "kind": "id", "id": "22222222-2222-4222-8222-222222222222", "expectedVersion": 1 }
    ]
  }
}
```

No usar `isDraft`, stock, recetas, unidades, ingredientes, costos ni campos upstream desconocidos en los requests. `extras.confirmed` exige verificación comercial: una categoría Extras no constituye evidencia de modificadores. Combos de Molly son un artículo sample-only, no receta. El original de Molly no está disponible; nunca atribuirle OCR exacto o aprobación real.
