# Conectar sin aprovisionar

CLI/SDK 0.1.0 / V1 son candidate local, no publicados en npm. Recibirlos por canal autorizado; Node >=20 para CLI. No presentar `npm install` desde registry como disponible ni adivinar un API origin de producción. El operador debe confirmar el origen HTTPS API habilitado y el staff origin HTTPS configurado en servidor. El origin no admite path, query, hash o userinfo; los enlaces de docs conservan el origin/prefix elegido por su host.

## Checkpoints humanos

Para piloto usar restaurante existente, autorizado, activo, aprobado y con `apiAccess` efectivo. Para alta nueva, el dueño visita `/register` y completa `/onboarding/step-1`, `/onboarding/step-2`, `/onboarding/step-3` con Better Auth. El complete puede responder `pendingApproval`: esperar el proceso vigente en `/pending-approval`. No es autorización para emitir keys.

Tras habilitación, el dueño confirma restaurante activo en `/agent-access`. Para pairing, abre la `verificationUrl` **literal** de CLI (pantalla `/agent-access/connect`), introduce user code y revisa label, restaurante, modo/scopes y TTL antes de decidir. No introducir Agent Key ni device code en esa pantalla. CLI espera y guarda secreto en almacenamiento protegido, nunca lo imprime. Un fallo de entrega requiere pairing nuevo y revocación humana de la credencial huérfana, no recuperar plaintext.

Configurar `ELENA_AGENT_STAFF_ORIGIN` con el origen del panel autorizado antes de login; no es un secreto ni se deduce desde la carta. Luego ejecutar el login de [comandos](commands.md). La versión candidate muestra URL/código para apertura humana; no asumir que `--no-open` implica que exista un navegador automatizado.

Para una key preemitida existe el par protegido `ELENA_AGENT_KEY` + `ELENA_AGENT_BASE_URL` (sin valor en argv/chat); no combinarlos con login pairing. Para operaciones también se necesita `ELENA_AGENT_STAFF_ORIGIN`. Si existe perfil, la configuración debe coincidir con el origen fijado. No introducir keys en archivos JSON de request. Logout elimina perfil local, **no revoca** la key remota ni borra el entorno del proceso padre.

Después de conexión, `context` y confirmación humana del restaurante retornado son obligatorios. Ausencia de cuenta, denegación, `API_ACCESS_DISABLED`, `RESTAURANT_NOT_APPROVED`, expiración o restaurante inesperado detienen trabajo: checkpoint humano/soporte. No hay create/select/switch de tenant en CLI, SQL, seed, issuer de plataforma ni System Admin en esta skill.

`/invitations/:token` solo agrega staff existente: no aprovisiona owner/restaurant, no concede `apiAccess` y no sustituye onboarding. API Key legacy `sk_` continúa separada en `/api-keys`; Client `elena_pk_` solo lee publicación. Ninguna key técnica equivale a sesión del dueño o permiso para aprobar.
