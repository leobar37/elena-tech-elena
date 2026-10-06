# Connect without provisioning

CLI/SDK 0.1.0 / V1 are local candidates, not published on npm. Receive them through an authorized channel; the CLI requires Node >=20. Do not present registry-based `npm install` as available or guess a production API origin. The operator must confirm the enabled HTTPS API origin and the HTTPS staff origin configured on the server. The origin does not allow a path, query, hash or userinfo; documentation links retain the origin/prefix chosen by their host.

## Human checkpoints

For a pilot, use an existing, authorized, active, approved restaurant with effective `apiAccess`. For a new account, the owner visits `/register` and completes `/onboarding/step-1`, `/onboarding/step-2`, `/onboarding/step-3` with Better Auth. Completion may return `pendingApproval`: wait for the current human process at `/pending-approval`. This is not authorization to issue keys.

Once access is enabled, the owner confirms the active restaurant at `/agent-access`. For pairing, they open the CLI's **literal** `verificationUrl` (`/agent-access/connect` screen), enter the user code and review the label, restaurant, mode/scopes and TTL before deciding. Do not enter an Agent Key or device code on that screen. The CLI waits and saves the secret in protected storage, never printing it. A delivery failure requires new pairing and human revocation of the orphaned credential, not plaintext recovery.

Configure `ELENA_AGENT_STAFF_ORIGIN` with the authorized dashboard origin before login; it is not a secret and is not inferred from the menu. Then run login as shown in [commands](commands.md). The candidate version shows a URL/code for a human to open; do not assume that `--no-open` implies an automated browser exists.

For a pre-issued key, the protected pair `ELENA_AGENT_KEY` + `ELENA_AGENT_BASE_URL` is available (no value in argv/chat); do not combine them with login pairing. Operations also require `ELENA_AGENT_STAFF_ORIGIN`. If a profile exists, the configuration must match the bound origin. Do not put keys in request JSON files. Logout removes the local profile, **does not revoke** the remote key and does not clear the parent process's environment.

After connecting, `context` and human confirmation of the returned restaurant are mandatory. A missing account, denial, `API_ACCESS_DISABLED`, `RESTAURANT_NOT_APPROVED`, expiration or an unexpected restaurant stops work: a human/support checkpoint is required. This skill offers no tenant create/select/switch in the CLI, SQL, seed, platform issuer or System Admin.

`/invitations/:token` only adds existing staff: it does not provision an owner/restaurant, does not grant `apiAccess` and does not replace onboarding. The legacy API Key `sk_` remains separate at `/api-keys`; Client `elena_pk_` only reads published content. No technical key is equivalent to the owner's session or permission to approve.
