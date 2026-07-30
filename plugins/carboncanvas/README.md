# CarbonCanvas plugin

Use CarbonCanvas from ChatGPT, Codex, or Claude to share AI-generated
prototypes, synchronize project files, and work through visual review feedback.

The plugin includes:

- The comprehensive CarbonCanvas CLI skill for multi-file applications, HTML,
  CSS, JavaScript, images, fonts, synchronization, and comment replies.
- The CarbonCanvas MCP server as a fallback for short, self-contained HTML
  documents when Bun or CLI networking is unavailable.

## Requirements

- Bun 1.3 or newer for the bundled CLI.
- A CarbonCanvas account.
- Browser access for the first CLI connection or the MCP OAuth 2.1 flow.

No access token should ever be copied into a prompt. The CLI uses a
browser-approved device login. The MCP server uses dynamic OAuth client
registration with authorization code and PKCE.

## OpenAI and Codex

Add the repository marketplace:

```sh
codex plugin marketplace add carbonteq/carboncanvas-plugins
```

Open the plugin browser, install **CarbonCanvas**, and start a new session.
The remote MCP connection prompts for authorization when it is first used.

## Claude

Add the repository marketplace and install the plugin:

```sh
claude plugin marketplace add carbonteq/carboncanvas-plugins
claude plugin install carboncanvas@carboncanvas-plugins
```

Run `/reload-plugins` after installing or updating the plugin.

## Integration behavior

The bundled CLI is the default integration because it supports the complete
CarbonCanvas workflow. MCP is deliberately limited to projects and short HTML
artifacts. It does not support multi-file application bundles or binary assets.

Production endpoints:

- Application and CLI: `https://carboncanvas.carbontech.build`
- MCP: `https://carboncanvas.carbontech.build/mcp`

## License

Licensed under the [Apache License 2.0](../../LICENSE).
