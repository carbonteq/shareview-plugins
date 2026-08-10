---
name: shareview-cli
description: Operate ShareView review workspaces with the bundled CLI. Use when an agent needs to discover or create projects, upload or download artifact versions, inspect or create version-scoped comments, respond to review feedback, or decide between the ShareView CLI and MCP in a network-restricted environment.
---

# ShareView

Never expose ShareView credentials or token storage.

## Choose CLI or MCP

Check production connectivity unless the user specifies another ShareView
origin:

```sh
curl --silent --show-error --output /dev/null \
  --write-out '%{http_code}\n' \
  https://shareview.carbontech.build/api/health/live
```

Use the CLI when this returns `200`. If the request fails or returns another
status because the environment cannot reach the server, use the configured
ShareView MCP endpoint at `https://shareview.carbontech.build/mcp`.

## Use the CLI

Require Bun 1.3 or newer. Locate `cli.js` beside this file and treat its help as
the command reference:

```sh
bun /absolute/path/to/scripts/cli.js help
```

Prefer `--json` for automation. Treat a nonzero exit as failure. Follow the
CLI's authentication instructions when no session is available.

When authentication is required, run `auth login` without `--no-browser` so
ShareView opens the approval page in the user's browser. Tell the user to
complete approval there, then wait for the CLI to confirm authentication. Use
`--no-browser` only when the user explicitly requests a headless flow or the
browser cannot be opened.

## Keep the domain model straight

- A **project** is a review workspace containing artifacts.
- An **artifact** is a stable identity with append-only immutable versions.
- An upload creates an artifact when `--artifact` is absent and appends a
  version when it is present.
- A **bundle** is a single HTML/Markdown file, a ZIP, or a folder packaged as a
  ZIP. Its entrypoint must be HTML or Markdown.
- A **comment thread** belongs to one artifact version. Read that version,
  apply feedback locally, append a version, reply, then resolve when addressed.

Use identifiers returned by ShareView rather than guessing them from names.
Use `comments anchor-schema` for the exact `--anchor-file` contract and only
create threads from real anchors captured by the ShareView preview.
