# Elena Agent Skills

Public mirror of Elena Tech instructions for a catalog agent for **one authorized restaurant**: https://github.com/leobar37/elena-tech-elena. Skills 0.1.0 are distributed here; the SDK and CLI remain **local candidates, not published on npm**. Installing instructions does not deploy endpoints, grant access or establish human approval.

## Pinned compatibility

| Component                                 | Version             | Status / scope                                                                 |
| ----------------------------------------- | ------------------- | ------------------------------------------------------------------------------ |
| elena-key-routing                         | 0.1.0               | Boundary instructions, distributed in this mirror                              |
| elena-catalog                             | 0.1.0               | Catalog instructions, distributed in this mirror                               |
| @theelena/catalog-cli / bin elena-catalog | 0.1.0               | Candidate with prior external build/smoke validation; not published; Node >=20 |
| @theelena/sdk                             | 0.1.0               | Candidate with prior external build/smoke validation; not published            |
| Wire Elena Agent Access                   | V1 (`version: "1"`) | Contract; deployment/origins must be confirmed by the operator                 |

The skills are installable Markdown; they do not require the private checkout. The CLI/SDK are delivered as verified local tarballs until an authorized release exists. The parser used by authoring tests is not a public CLI export.

## Install from GitHub

From the consuming project, with Node/npm available:

```bash
npx --yes skills@1.7.0 add leobar37/elena-tech-elena --list
npx --yes skills@1.7.0 add leobar37/elena-tech-elena --skill elena-key-routing elena-catalog --agent codex --copy --yes
```

For Claude Code, replace `--agent codex` with `--agent claude-code`. Installation is project-local, without `--global`. These commands install instructions, not the SDK or CLI npm packages.

## Install from THIS local mirror

In a separate temporary consuming project, set `MIRROR` to the absolute path of the exported directory. Do not use the private authoring directory as the source for this test. Node/npm and the `skills` tool are installer dependencies (network access may be required). Without `--global`, personal skills are not modified.

```bash
npx --yes skills add "$MIRROR" --list
npx --yes skills add "$MIRROR" --skill elena-key-routing elena-catalog --agent codex --copy --yes
```

Files are placed under the consumer's `.agents/skills/`. For Claude Code, use another temporary consumer and `--agent claude-code`; files are placed under `.claude/skills/`. Compare each SKILL.md and its references with this mirror's `skills/`. Availability of the skills on GitHub does not imply publication of the SDK/CLI on npm.

- [elena-key-routing](skills/elena-key-routing/SKILL.md): separate product Agent/Admin `elena_sk_`, Client `elena_pk_`, legacy and access points.
- [elena-catalog](skills/elena-catalog/SKILL.md): context, untrusted source, confirmation, preview, proposal, literal approvalUrl, human wait, apply, status/re-read.

## Public, secret and human

Public: this README, the two skills and their allowlisted references. A Client Key `elena_pk_` is publishable but only reads the published menu/reachable assets. Secret: Agent Key `elena_sk_`, legacy API Key `sk_`, device code and sessions; never include them in this mirror, prompts, argv, URL, logs or frontend. There are no real keys or customer datasets here. Molly is sample-only, not an approved menu.

The owner uses existing registration/onboarding if they need an account, waits for `pendingApproval` and current access enablement, confirms the restaurant and authorizes at `/agent-access` or `/agent-access/connect`. Invitations do not provision an owner/restaurant or grant `apiAccess`. No skill provides provisioning, System Admin, tenant selection, self-approval, stock or recipes. A proposal does not execute; the owner decides at the literal approvalUrl, and apply requires the exact revision/hash.

## Export and release

The private authoritative source is `apps/elena/agent-skills`; `scripts/sync-public.ts` requires an explicit external `--target`. Exact allowlist: this README, two SKILL.md files and six reference files. It does not export tests, scripts, package/config, env, private source or dependencies. The exporter rejects the source/root/ancestors, destinations inside the checkout, sensitive locations and symlinks. It does not delete files: an unexpected entry stops the export and must be reviewed outside the script. `--check` is read-only and reports changed/missing/unexpected; repeating an export to a clean target is idempotent.

**One editable source:** make changes in the monorepo's `apps/elena/agent-skills/`, test them and export them. This repo is distribution-only: do not edit skills here or sync changes back. To update it, generate and check a clean external directory; then copy only the allowed mappings into a checkout of this repo, review the diff and publish the commit. Do not run the exporter directly against a checkout containing `.git`: it rejects that as an unexpected entry. Remote synchronization is manual, not automatic.

The manual `elena-agent-skills-candidate.yml` workflow only exports, checks and attaches a candidate artifact with read-only permissions. It does not push, create a remote repo or publish to npm. Publication of this mirror was authorized separately. **P007 not executed**: full integrated validation and SDK/CLI/backend releases remain pending; this README does not replace those gates.
