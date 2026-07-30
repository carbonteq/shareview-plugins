---
name: carboncanvas-cli
description: Use the CarbonCanvas MCP server by default to work with shared design projects, prototypes, versions, and comments. Use the bundled CarbonCanvas CLI only when the user explicitly asks to use the CLI; treat the CLI as a beta/preview integration.
---

# CarbonCanvas

CarbonCanvas is a remote review workspace. Its project and artifact records are
authoritative. Use the configured CarbonCanvas MCP server for every
CarbonCanvas request by default:

```text
https://carboncanvas.carbontech.build/mcp
```

Never print, log, request, or paste a CarbonCanvas access token or OAuth token.

## MCP and CLI selection

Use MCP for normal CarbonCanvas work, including project, artifact, version, and
comment operations. Complete the request with MCP whenever its available tools
support the action.

### CLI beta/preview

Treat the bundled CLI as a beta/preview integration. Use it if and only if the
user explicitly asks to use the CarbonCanvas CLI or clearly requests a
command-line workflow. Do not infer permission from an MCP limitation, a
missing MCP tool, a multi-file task, the presence of `scripts/cli.js`, or the
CLI's broader capabilities.

If MCP cannot perform the requested action, explain the limitation and offer
the beta CLI as an option. Wait for the user to explicitly choose the CLI
before checking Bun, authenticating, or running any CLI command. If the user
does not opt in, stop after explaining what MCP can and cannot do.

After explicit CLI opt-in, locate `scripts/cli.js` relative to this file and
invoke it with Bun 1.3 or newer:

```sh
bun /absolute/path/to/carboncanvas-cli/scripts/cli.js help
```

The preview CLI supports multi-file projects, application bundles, binary
assets, synchronization, and the full feedback workflow. All remaining CLI
instructions in this skill apply only after the user has explicitly opted in.

## Communicate for users

Assume the user is a non-technical user. Operate MCP yourself by default. If
the user explicitly opts into the preview CLI, operate it yourself and
communicate in plain language about prototypes, versions, and feedback rather
than commands, flags, APIs, or storage details. Lead with what changed or what
decision is needed. Do not ask the user to run or interpret CLI commands;
involve them only for browser approval, meaningful product choices, and
confirmation before overwriting or deleting work.

If the selected CarbonCanvas integration does not support an action,
communicate it directly instead of trying unsupported workarounds such as
browser or computer-use tools.

## CLI beta/preview connection

Follow these steps only after explicit user opt-in to the CLI:

1. Check `bun --version`. Make sure it is installed (v1.3+).
2. Use the default production server, `https://carboncanvas.carbontech.build`,
   unless the user specifies another origin. Only then run
   `config set-server <origin>` to override it.
3. Run `auth login`. Explain that browser approval securely connects the agent,
   share the displayed URL and device code, then wait for approval.
4. Run `auth status` and confirm who is connected without exposing credentials
   or unnecessary session details.

## CLI project and file workflow

List before acting:

```sh
bun <cli> projects list --json
bun <cli> files list --project <slug> --json
```

When the user provides a CarbonCanvas document URL, read the project slug,
document ID, and optional `version` query value directly from it. Inspect or
pull that exact HTML version without first resolving its path:

```sh
bun <cli> files list --project <slug> --document <document-id> --version <number-or-id> --json
bun <cli> files pull <document-id> --project <slug> --version <number-or-id> --to <local>
```

The version value may be the short positive number shown in current browser
URLs or an immutable version UUID from an older URL. Omit `--version` only when
the user wants the current version.

Before every push, list the destination and run `files push ... --dry-run`.
Summarize which files will be created, which HTML documents will receive a
**New version**, which non-HTML assets will be **Replaced**, and which items
will be removed.

CarbonCanvas manages HTML history under the existing path. Appending HTML
does not require `--confirm-overwrite` because the older HTML remains available.
Pass `--confirm-overwrite` only after the user approves replacing the listed
non-HTML assets.

Historical HTML preserves the HTML bytes only. Linked stylesheets, scripts,
images, fonts, and nested pages resolve to the project's current assets. Explain
this limitation when exact historical rendering or a self-contained archive is
material to the task.

Mirror mode is directory-only and requires an explicit non-root `--to` destination. It deletes remote-only entries after all uploads succeed. Never pass `--confirm-delete` for mirror mode until the user explicitly approves the exact deletion plan.

Pull refuses existing local targets unless `--overwrite` is supplied. Ask before replacing a local file or folder and identify it plainly. Remote delete always requires explicit user approval of the prototype file or folder, then `--confirm-delete`.

## Comments

Use `comments list --project <slug> --file <path> --json` to read threads, replies, and anchor metadata. Use `comments reply <thread-id> --body ...` (or `--body-file`/stdin) for replies. Creating a new anchored thread remains a browser-preview task.

## Product feedback

When a CLI workflow is broken, unclear, or missing a useful capability, explain the issue plainly and offer to send concise feedback to the CarbonCanvas developers. After the user agrees, use `feedback --body ...` (or `--body-file`/stdin). Include the attempted command, expected and observed behavior, and safe error details when relevant. Never include credentials, secrets, or unnecessary project contents.

Prefer `--json` for automation. Treat any nonzero exit as a failed operation, translate the safe error into plain language, and propose the next useful step. Show technical codes or per-file details only when they help resolve the problem. On partial push failure, do not attempt mirror deletion; fix the failed files and rerun the same command.
