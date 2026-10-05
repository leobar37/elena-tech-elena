# Setup asistido: no es provisioning

CLI/SDK 0.1.0 / V1 son candidate local, no publicados en npm. Un endpoint documentado no prueba despliegue. El piloto parte de un restaurante existente, activo, aprobado y autorizado; confirmar con el operador API origin HTTPS y staff origin HTTPS habilitados. No adivinar dominios ni reemplazar origin/prefix del host de documentación.

Para una cuenta nueva, el **dueño humano** usa el panel ya existente:

1. `/register` con su sesión Better Auth.
2. `/onboarding/step-1`, `/onboarding/step-2`, `/onboarding/step-3`.
3. La finalización puede devolver `pendingApproval`; esperar el checkpoint humano vigente en `/pending-approval`. No aprobar automáticamente ni usar SQL/seed/System Admin para desbloquearlo.
4. Con aprobación y `apiAccess` efectivos, confirmar el restaurante activo en `/agent-access`. Allí el dueño administra Agent/Client Keys; la API Key legacy permanece en `/api-keys`.
5. Para pairing, el humano abre la `verificationUrl` literal emitida por CLI, correspondiente a `/agent-access/connect`, y escribe únicamente el user code. Revisa restaurante, nombre del dispositivo, grants y TTL. Nunca introducir una Agent Key ni device code en esa pantalla. El agente espera la decisión.
6. Después de conectar, leer `context` y pedir confirmación del restaurante retornado. Si no corresponde, detenerse: no hay selección de tenant en CLI. El dueño debe corregir la conexión por las superficies humanas.

Las invitaciones `/invitations/:token` agregan staff a un restaurante existente: **no** aprovisionan dueño/restaurant ni conceden `apiAccess`. Si falta cuenta, elegibilidad, aprobación o endpoint habilitado, el siguiente paso es humano/soporte; no un comando create/select/switch.

La sesión del panel no reemplaza Agent Key. No compartir secretos en chat. El secreto Agent queda en almacenamiento local protegido de la CLI o en el entorno protegido del proceso, nunca en frontend. La Client Key es publicable pero su emisión/reemplazo siguen siendo acciones del dueño. No copiar el secreto de servidor a un proyecto público.
