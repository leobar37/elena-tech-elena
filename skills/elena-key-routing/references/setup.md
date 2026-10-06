# Assisted setup: not provisioning

CLI/SDK 0.1.0 / V1 are local candidates, not published on npm. A documented endpoint does not prove deployment. The pilot starts with an existing, active, approved and authorized restaurant; confirm the enabled HTTPS API origin and HTTPS staff origin with the operator. Do not guess domains or replace the documentation host's origin/prefix.

For a new account, the **human owner** uses the existing dashboard:

1. `/register` with their Better Auth session.
2. `/onboarding/step-1`, `/onboarding/step-2`, `/onboarding/step-3`.
3. Completion may return `pendingApproval`; wait for the current human checkpoint at `/pending-approval`. Do not approve automatically or use SQL/seed/System Admin to unblock it.
4. With effective approval and `apiAccess`, confirm the active restaurant at `/agent-access`. The owner manages Agent/Client Keys there; the legacy API Key remains at `/api-keys`.
5. For pairing, the human opens the literal `verificationUrl` issued by the CLI, corresponding to `/agent-access/connect`, and enters only the user code. They review the restaurant, device name, grants and TTL. Never enter an Agent Key or device code on that screen. The agent waits for the decision.
6. After connecting, read `context` and ask for confirmation of the returned restaurant. If it does not match, stop: the CLI has no tenant selection. The owner must correct the connection through the human-facing surfaces.

Invitations at `/invitations/:token` add staff to an existing restaurant: they do **not** provision an owner/restaurant or grant `apiAccess`. If an account, eligibility, approval or an enabled endpoint is missing, the next step is a human/support checkpoint, not a create/select/switch command.

The dashboard session does not replace an Agent Key. Do not share secrets in chat. The Agent secret stays in protected local CLI storage or in the process's protected environment, never in the frontend. A Client Key is publishable, but issuing/replacing it remains an owner action. Do not copy the server secret into a public project.
