# ShareView plugins

Official plugin marketplace for
[ShareView](https://carboncanvas.carbontech.build), a shared workspace for
reviewing AI-generated prototypes, by [Carbonteq](https://carbonteq.com).

## Available plugins

| Plugin | Description |
| --- | --- |
| [ShareView](plugins/share-view) | Create, synchronize, and review ShareView prototypes with the MCP server and an optional bundled CLI (beta). |

## Install for Claude Code

```sh
claude plugin marketplace add carbonteq/carboncanvas-plugins
claude plugin install share-view@share-view
```

Then run `/reload-plugins`. This works in the Claude Code CLI, the desktop
app, and claude.ai/code.

## Use in Claude on the web

Claude chat on claude.ai does not install GitHub plugins. Add the ShareView
MCP server as a custom connector instead:

1. Open claude.ai and go to **Settings → Connectors**.
2. Choose **Add custom connector**.
3. Enter `https://carboncanvas.carbontech.build/mcp`.
4. Complete the OAuth authorization in your browser.

## Install for OpenAI and Codex

```sh
codex plugin marketplace add carbonteq/carboncanvas-plugins
```

Install **ShareView** from the plugin browser and start a new session.

## Security

Do not paste ShareView access tokens into prompts or configuration files.
The remote MCP server uses dynamic OAuth client registration with
authorization code and PKCE. The bundled CLI uses a browser-approved device
login.

## License

Licensed under the [Apache License 2.0](LICENSE).
