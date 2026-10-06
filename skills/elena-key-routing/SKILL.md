---
name: elena-key-routing
description: Identify Elena credential boundaries before connecting an agent, using the Client SDK or integrating a menu. Use it when the key type is uncertain, for Agent/Admin Keys, Client Keys, the legacy API, QR or assisted setup; never for System Admin.
metadata:
  version: '0.1.0'
  language: 'en'
---

# Elena: choose access without mixing authority

Skill 0.1.0, contract V1. SDK/CLI 0.1.0 are local candidates, not published on npm; distributing this skill does not prove that endpoints are deployed. Before connecting, ask the operator for the enabled HTTPS origin and the required surface, **not the value of a key**. The prefix helps route credentials; it does not prove permissions or identity.

| Need                                    | Family and transport                                      | Boundary                                                                               |
| --------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Catalog agent / product “Admin”         | `elena_sk_`, `x-elena-agent-key`, `/api/v1/agent/*`       | Server-only secret, one immutable restaurant; proposing neither authorizes nor applies |
| Public reads with Client SDK            | `elena_pk_`, `x-elena-client-key`, `/api/v1/public/sdk/*` | Publishable; published menu and reachable assets only                                  |
| Legacy integration                      | `sk_`, `x-api-key`, existing integration routes           | Separate contract; do not migrate or replace the issuer                                |
| Menu by slug, QR/access points, journey | Slug; `pub_`; journey Bearer, respectively                | Existing contracts, no Client Key required                                             |
| Human owner                             | Better Auth session in the dashboard                      | Issues/revokes credentials, authorizes pairing and decides on proposals                |
| System Admin                            | Separate platform boundary                                | Outside this flow: refer to an authorized operator, no commands or provisioning        |

“Admin” here does not mean owner, human session or System Admin. An Agent Key cannot approve itself, manage keys, create/select/switch restaurants or elevate scopes. A Client Key never writes. Do not mix Cookie, Authorization, `x-api-key` or headers from another family with the new routes; never put keys in URL/body/argv/chat/logs/repositories either.

1. Without an account or enabled access, follow [human setup](references/setup.md); stop at every checkpoint, do not fabricate a tenant.
2. For Agent access, install the candidate CLI received through an authorized local channel and pair it. Do not ask anyone to paste the key into chat. After authorization, run `context` and confirm the restaurant, PEN currency, mode, scopes and effective actions with the owner.
3. For Client access, follow [public reads](references/client-sdk.md). Do not use the Agent SDK in a browser.
4. For legacy/access points/journey, preserve the existing integration. These skills do not implement those clients or convert their credentials.
5. If the goal requires catalog writes, use the separately installed **elena-catalog** skill. Never improvise privileged HTTP as a workaround for a denial.

Reading context does not create a subscription, commercial access or approval. `API_ACCESS_DISABLED`, `RESTAURANT_NOT_APPROVED`, expiration, revocation or an unexpected restaurant require a human checkpoint. Do not infer eligibility from a plan name.
