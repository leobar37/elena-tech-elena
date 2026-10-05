# Elena Agent Skills

Mirror público de instrucciones de Elena Tech para un agente de catálogo de **un restaurante autorizado**: https://github.com/leobar37/elena-tech-elena. Las skills 0.1.0 se distribuyen aquí; el SDK y la CLI siguen siendo **candidate local, no publicados en npm**. Instalar instrucciones no despliega endpoints, no concede acceso y no acredita aprobación humana.

## Compatibilidad fijada

| Componente                                | Versión             | Estado / alcance                                                  |
| ----------------------------------------- | ------------------- | ----------------------------------------------------------------- |
| elena-key-routing                         | 0.1.0               | Instrucciones de frontera, distribuidas en este mirror            |
| elena-catalog                             | 0.1.0               | Instrucciones de catálogo, distribuidas en este mirror            |
| @theelena/catalog-cli / bin elena-catalog | 0.1.0               | Candidate con build/smoke externo previo; no publicado; Node >=20 |
| @theelena/sdk                             | 0.1.0               | Candidate con build/smoke externo previo; no publicado            |
| Wire Elena Agent Access                   | V1 (`version: "1"`) | Contrato; despliegue/origins deben confirmarse por operador       |

Las skills son Markdown instalable; no necesitan el checkout privado. La CLI/SDK se entregan por tarballs locales verificados hasta que exista un release autorizado. El parser usado por los tests de autoría no es un export público de la CLI.

## Instalación desde GitHub

Desde el proyecto consumidor, con Node/npm disponibles:

```bash
npx --yes skills@1.7.0 add leobar37/elena-tech-elena --list
npx --yes skills@1.7.0 add leobar37/elena-tech-elena --skill elena-key-routing elena-catalog --agent codex --copy --yes
```

Para Claude Code, sustituir `--agent codex` por `--agent claude-code`. La instalación es local al proyecto, sin `--global`. Estos comandos instalan instrucciones, no los paquetes npm del SDK o de la CLI.

## Instalación desde ESTE mirror local

En un proyecto consumidor temporal separado, asignar `MIRROR` a la ruta absoluta del directorio exportado. No usar el directorio privado de autoría como fuente de esta prueba. Node/npm y la herramienta `skills` son dependencias del instalador (puede requerir red). Sin `--global`: no modifica skills personales.

```bash
npx --yes skills add "$MIRROR" --list
npx --yes skills add "$MIRROR" --skill elena-key-routing elena-catalog --agent codex --copy --yes
```

Los archivos quedan bajo `.agents/skills/` del consumidor. Para Claude Code, usar otro consumidor temporal y `--agent claude-code`; los archivos quedan bajo `.claude/skills/`. Comparar cada SKILL.md y references con `skills/` de este mirror. La disponibilidad de las skills en GitHub no implica publicación del SDK/CLI en npm.

- [elena-key-routing](skills/elena-key-routing/SKILL.md): Agent/Admin de producto `elena_sk_`, Client `elena_pk_`, legacy y access points separados.
- [elena-catalog](skills/elena-catalog/SKILL.md): context, fuente no confiable, confirmación, preview, propuesta, approvalUrl literal, espera humana, apply, status/relectura.

## Público, secreto y humano

Público: README, estas dos skills y sus referencias allowlisted. Client Key `elena_pk_` es publicable pero solo lee carta publicada/assets alcanzables. Secreto: Agent Key `elena_sk_`, API Key legacy `sk_`, device code y sesiones; nunca incluirlos en este mirror, prompts, argv, URL, logs o frontend. No hay claves reales ni datasets de clientes aquí. Molly es sample-only, no una carta aprobada.

El dueño usa registro/onboarding existentes si necesita cuenta, espera `pendingApproval` y habilitación vigente, confirma restaurante y autoriza en `/agent-access` o `/agent-access/connect`. Invitaciones no aprovisionan owner/restaurant ni conceden `apiAccess`. Ninguna skill tiene provisioning, System Admin, selección de tenant, autoaprobación, stock o recetas. La propuesta no ejecuta; el dueño decide en approvalUrl literal y apply necesita la revisión/hash exactos.

## Exportación y release

La autoridad privada es `apps/elena/agent-skills`; `scripts/sync-public.ts` exige `--target` externo y explícito. Allowlist exacta: este README, dos SKILL.md y seis archivos references. No exporta tests, scripts, package/config, env, source privado ni dependencias. El exporter rechaza fuente/root/ancestros, destinos dentro del checkout, ubicaciones sensibles y symlinks. No borra archivos: un unexpected detiene exportación y debe revisarse fuera del script. `--check` es read-only y reporta changed/missing/unexpected; repetir export sobre destino limpio es idempotente.

**Una sola fuente editable:** los cambios se realizan en `apps/elena/agent-skills/` del monorepo, se prueban y se exportan. Este repo es solo distribución: no editar aquí las skills ni sincronizar cambios de vuelta. Para actualizarlo, generar y comprobar un directorio limpio externo; después copiar únicamente los mappings permitidos a un checkout de este repo, revisar el diff y publicar el commit. No ejecutar el exporter directamente sobre un checkout con `.git`: lo rechaza como entrada inesperada. La sincronización remota es manual, no automática.

El workflow manual `elena-agent-skills-candidate.yml` solo exporta, comprueba y adjunta artifact candidate con permisos de lectura. No hace push, no crea repo remoto ni publica npm. La publicación de este mirror fue autorizada por separado. **P007 no ejecutado**: siguen pendientes la validación integrada completa y los releases de SDK/CLI/backend; este README no sustituye esos gates.
