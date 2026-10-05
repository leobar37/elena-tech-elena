---
name: elena-key-routing
description: Identifica la frontera de credenciales de Elena antes de conectar un agente, usar Client SDK o integrar una carta. Úsala cuando el tipo de key sea incierto, para Agent/Admin Key, Client Key, API legacy, QR o setup asistido; nunca para System Admin.
metadata:
  version: '0.1.0'
---

# Elena: elegir acceso sin mezclar autoridad

Skill 0.1.0, contrato V1. SDK/CLI 0.1.0 son candidate local, no publicados en npm; distribuir esta skill no demuestra endpoints desplegados. Antes de conectar, pide al operador el origen HTTPS habilitado y la superficie necesaria, **no el valor de una clave**. El prefijo ayuda a enrutar, no prueba permisos ni identidad.

| Necesidad                                 | Familia y transporte                                      | Límite                                                                        |
| ----------------------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Agente de catálogo / “Admin” del producto | `elena_sk_`, `x-elena-agent-key`, `/api/v1/agent/*`       | Secreto server-only, un restaurante inmutable; proponer no autoriza ni aplica |
| Lectura pública con Client SDK            | `elena_pk_`, `x-elena-client-key`, `/api/v1/public/sdk/*` | Publicable; solo carta publicada y assets alcanzables                         |
| Integración legacy                        | `sk_`, `x-api-key`, rutas de integración existentes       | Contrato separado; no migrar ni sustituir el issuer                           |
| Carta por slug, QR/access points, journey | Slug; `pub_`; Bearer de journey, respectivamente          | Contratos existentes, no necesitan Client Key                                 |
| Dueño humano                              | Sesión Better Auth en el panel                            | Emite/revoca credenciales, autoriza pairing y decide propuestas               |
| System Admin                              | Frontera de plataforma separada                           | Fuera del flujo: derivar a operador autorizado, sin comandos ni provisioning  |

“Admin” aquí no significa dueño, sesión humana ni System Admin. La Agent Key no puede aprobarse, administrar keys, crear/select/switch restaurantes ni elevar scopes. Client Key nunca escribe. No mezclar Cookie, Authorization, `x-api-key` o cabeceras de otra familia con las rutas nuevas; tampoco poner claves en URL/body/argv/chat/logs/repositorios.

1. Si no hay cuenta o habilitación, seguir [setup humano](references/setup.md); detenerse en cada checkpoint, no fabricar un tenant.
2. Para Agent, instalar la CLI candidate recibida por canal local autorizado y efectuar pairing. No pedir que peguen la key en el chat. Tras autorización, ejecutar `context` y confirmar restaurante, moneda PEN, modo, scopes y acciones efectivas con el dueño.
3. Para Client, seguir [lectura pública](references/client-sdk.md). No usar Agent SDK en navegador.
4. Para legacy/access points/journey, conservar la integración existente. Estas skills no implementan esos clientes ni convierten sus credenciales.
5. Si el objetivo exige escritura de catálogo, usar la skill **elena-catalog** instalada por separado. Nunca improvisar HTTP privilegiado como alternativa a una denegación.

Leer contexto no crea suscripción, acceso comercial ni aprobación. `API_ACCESS_DISABLED`, `RESTAURANT_NOT_APPROVED`, expiración, revocación o restaurante inesperado son pausas para el humano. No inferir elegibilidad por nombre de plan.
