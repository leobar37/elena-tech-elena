# CLI candidate 0.1.0: existing commands

These examples describe grammar, not an authorized session or a batch to execute all at once. Replace `<api-origin>` only with operator-verified HTTPS; replace UUID IDs, cursor and SHA-256 with real responses; `<revision>` is the **current approved** revision, not the pending one. Filenames are relative to the consuming project. Do not use placeholders as real data.

## Connection and reads

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

Login requires `ELENA_AGENT_STAFF_ORIGIN`, shows the literal verificationUrl/user code and waits for a human decision. Omit `--cursor` on the first list page; continue with nextCursor until null. Neither login nor context creates an account, subscription or restaurant. Confirm context before reading/preparing data.

## Optional stage and preview

```bash
elena-catalog media stage ./foto.png --idempotency-key carta-foto-1 --json
elena-catalog media status <staging-id> --json
elena-catalog import preview --file ./preview.json --json
elena-catalog import status <import-id> --json
```

Do not stage without a verified local file and permission; stage does not publish. `preview.json` is a complete V1 request: see [JSON](catalog-json.md). Show invalid/conflicting rows and confirm the selection before proposing.

## Proposal, human pause and apply

```bash
elena-catalog operations propose --file ./proposal.json --json
elena-catalog operations list --limit 25 --json
elena-catalog operations status <operation-id> --json
```

**Pause:** provide the literal approvalUrl. Wait for owner approval in the dashboard; query status again and use the exact revision/hash. Do not automatically chain propose and apply in a script. Only then:

```bash
elena-catalog operations apply <operation-id> --revision <revision> --payload-hash <payload-hash> --json
elena-catalog operations status <operation-id> --json
elena-catalog catalog products get <product-id> --json
elena-catalog auth logout --json
```

Also re-read affected categories; do not interpret applying as applied. Logout does not revoke remote authority. For publication, the same propose command receives a separate publication payload, never an import `--apply` or `--publish` flag.

## Outputs and recovery

Successful responses are wire JSON without an envelope; safe JSON errors go to stderr. `--json` is global and does not create another protocol. Exit 0 may mean preview, pending or applying; it does not prove effects. Exit 2: usage/schema; 3: access/approval; 4: conflict/unsuccessful terminal state (including partial); 5: transient/network. Query status before retrying an ambiguous mutation; do not automatically retry permission, data, hash or version errors.

Unknown flags, extra positionals, `--key`, `--token`, `--restaurant-id`, `--apply`, `approve` and tenant commands do not exist. Authoring validates these examples by importing parser **source** in private tests; the CLI package distributes its bin, not a public parser export.
