# CLI candidate 0.1.0: comandos existentes

Estos ejemplos describen gramática, no una sesión autorizada ni un lote para ejecutar en bloque. Sustituir `<api-origin>` solo por HTTPS verificado del operador; IDs UUID, cursor y SHA-256 por respuestas reales; `<revision>` es la revisión **approved actual**, no la pending. Nombres de archivo son relativos al proyecto consumidor. No usar placeholders como datos reales.

## Conexión y lectura

```bash
elena-catalog auth login --base-url <api-origin> --no-open
elena-catalog auth status --json
elena-catalog context --json
elena-catalog catalog categories list --limit 25 --cursor <cursor> --json
elena-catalog catalog categories get <category-id> --json
elena-catalog catalog products list --limit 25 --json
elena-catalog catalog products get <product-id> --json
elena-catalog assets get <asset-id> --json
```

Login requiere `ELENA_AGENT_STAFF_ORIGIN`, muestra verificationUrl literal/user code y espera decisión humana. Primera página de list omite `--cursor`; continuar con nextCursor hasta null. Ni el login ni context crean cuenta, suscripción o restaurante. Confirmar contexto antes de leer/preparar datos.

## Stage opcional y preview

```bash
elena-catalog media stage ./foto.png --idempotency-key carta-foto-1 --json
elena-catalog media status <staging-id> --json
elena-catalog import preview --file ./preview.json --json
elena-catalog import status <import-id> --json
```

No stage sin archivo local verificado y permiso; stage no publica. `preview.json` es request V1 completo: ver [JSON](catalog-json.md). Mostrar filas inválidas/conflictivas y confirmar selección antes de propuesta.

## Propuesta, pausa humana y aplicación

```bash
elena-catalog operations propose --file ./proposal.json --json
elena-catalog operations list --limit 25 --json
elena-catalog operations status <operation-id> --json
```

**Pausa:** entregar approvalUrl literal. Esperar aprobación del dueño en panel; volver a consultar status y usar revisión/hash exactos. No concatenar automáticamente propose y apply en un script. Solo entonces:

```bash
elena-catalog operations apply <operation-id> --revision <revision> --payload-hash <payload-hash> --json
elena-catalog operations status <operation-id> --json
elena-catalog catalog products get <product-id> --json
elena-catalog auth logout --json
```

Releer también categorías afectadas; no interpretar applying como applied. Logout no revoca autoridad remota. Para publicación, el mismo propose recibe payload publication separado, nunca un `--apply` o `--publish` de import.

## Salidas y recuperación

Éxito wire JSON sin envelope; errores JSON seguros por stderr. `--json` es global, no crea otro protocolo. Exit 0 puede ser preview, pending o applying, no prueba efectos. Exit 2: uso/schema; 3: acceso/aprobación; 4: conflicto/terminal no satisfactorio (incluye partial); 5: transitorio/red. Consultar estado antes de reintentar mutación ambigua; no reintentar automáticamente errores de permisos, datos, hash o versión.

Flags desconocidos, posicionales extra, `--key`, `--token`, `--restaurant-id`, `--apply`, `approve` y comandos tenant no existen. La autoría valida estos ejemplos importando parser **source** en tests privados; el paquete CLI distribuye su bin, no un export público de parser.
