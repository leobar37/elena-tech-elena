# V1 requests: synthetic examples, not execution orders

The UUIDs/hashes/versions below are test placeholders. They are neither real rows nor approval. The confirmed-price example simulates a review to test the contract: real operation requires explicit human review, a read of the complete aggregate and current versions. Do not copy these JSON examples directly into production. Responses use server-provided IDs/hashes, never values calculated from the transcription. Spanish strings such as “Bebidas” and “Agua” are synthetic catalog data, not agent instructions; preserve them as data.

## preview.json

Complete request. External identity is stable within the restaurant, not a name match. `expectedVersion: null` asserts absence; if the entity already exists, resolve the conflict using current reads/identity/versions. Also confirm availability, description, gallery and absence of extras, not just the price.

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

An illegible price is represented as `{ "status": "unconfirmed", "reason": "illegible", "rawText": null }`. It is valid preview evidence, but its row remains invalid with no executable change: do not select it. `basis: "sample"` belongs only to the neutral Molly fixture, not preview/proposal. PEN uses a positive string with two decimal places; no number, zero, comma, exponents or approximation. A `proposed` description must not be presented as a transcription or as already applied.

## Import proposal.json

Only after reviewing the preview: replace importId/planHash with the returned values and selectedRefs with the explicit human selection. Include dependent categories even if they are skip rows. Do not include invalid/conflict rows or assume that the server will expand the set.

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

Propose returns the literal approvalUrl. Wait for the human, re-read the approved state, then apply exactly; approval does not execute. The same idempotencyKey requires the same payload; changed data needs another key. Preview/proposal expire after 24h, with no extension on approval. Version drift requires a new proposal/approval.

## Separate publication proposal

A new product remains unpublished after import. With `catalog:publish`, read the created product's actual ID/version and prepare **another** proposal. This example does not authorize implicit batch publication:

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

Do not use `isDraft`, stock, recipes, units, ingredients, costs or unknown upstream fields in requests. `extras.confirmed` requires verification of the commercial offering: an “Extras” category is not evidence of modifiers. Molly combos are single sample-only items, not recipes. The Molly original is unavailable; never attribute exact OCR or real approval to it.
