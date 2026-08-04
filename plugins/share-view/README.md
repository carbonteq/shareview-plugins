# ShareView plugin

Use ShareView from Claude, ChatGPT, or Codex to share AI-generated
prototypes, synchronize project files, and work through visual review
feedback.

The plugin includes:

- The ShareView MCP server as the default integration for projects and
  short, self-contained HTML documents.
- The ShareView CLI skill (beta/preview) for multi-file applications, HTML,
  CSS, JavaScript, images, fonts, synchronization, and comment replies.

## Requirements

- A ShareView account.
- Browser access for the MCP OAuth 2.1 flow or the first CLI connection.
- Bun 1.3 or newer, only if you opt into the bundled CLI.

No access token should ever be copied into a prompt. The MCP server uses
dynamic OAuth client registration with authorization code and PKCE. The CLI
uses a browser-approved device login.

## Claude

Add the repository marketplace and install the plugin:

```sh
claude plugin marketplace add carbonteq/carboncanvas-plugins
claude plugin install share-view@share-view
```

Run `/reload-plugins` after installing or updating the plugin.

In Claude chat on claude.ai, add
`https://carboncanvas.carbontech.build/mcp` as a custom connector instead —
see the [repository README](../../README.md).

## OpenAI and Codex

Add the repository marketplace:

```sh
codex plugin marketplace add carbonteq/carboncanvas-plugins
```

Open the plugin browser, install **ShareView**, and start a new session.
The remote MCP connection prompts for authorization when it is first used.

## Integration behavior

MCP is the default integration. It covers projects and short,
self-contained HTML artifacts, and does not support multi-file application
bundles or binary assets. The bundled CLI is a beta/preview that supports
the complete ShareView workflow; it is used only when explicitly requested.

Production endpoints:

- Application and CLI: `https://carboncanvas.carbontech.build`
- MCP: `https://carboncanvas.carbontech.build/mcp`

## License

Licensed under the [Apache License 2.0](../../LICENSE).
