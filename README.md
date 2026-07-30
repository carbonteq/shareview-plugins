# CarbonCanvas plugins

Official plugin marketplace for
[CarbonCanvas](https://carboncanvas.carbontech.build), a shared workspace for
reviewing AI-generated prototypes.

## Available plugins

| Plugin | Description |
| --- | --- |
| [CarbonCanvas](plugins/carboncanvas) | Synchronize prototype files, inspect projects, and work through design feedback with the bundled CLI and OAuth-enabled MCP fallback. |

## Install for OpenAI and Codex

```sh
codex plugin marketplace add carbonteq/carboncanvas-plugins
```

Install **CarbonCanvas** from the plugin browser and start a new session.

## Install for Claude

```sh
claude plugin marketplace add carbonteq/carboncanvas-plugins
claude plugin install carboncanvas@carboncanvas-plugins
```

Then run `/reload-plugins`.

## Security

Do not paste CarbonCanvas access tokens into prompts or configuration files.
The bundled CLI uses a browser-approved device login. The remote MCP server
uses dynamic OAuth client registration with authorization code and PKCE.

## License

Licensed under the [Apache License 2.0](LICENSE).
