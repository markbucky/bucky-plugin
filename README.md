# Bucky

Cited site feasibility and pre-development project workflows for Canada and the
United States, as a plugin and a hosted MCP server.

Give it a street address and it returns the lot, the zone, the permitted uses,
and the material envelope — setbacks, height, coverage, density — with each value
carrying its own provenance. A setback the bylaw does not limit and a setback
Bucky could not extract are different answers, and they stay different.

```
https://buckybuild.com/api/mcp
```

Docs: <https://docs.buckybuild.com/docs/mcp>

## Install

### Claude Code

```sh
/plugin marketplace add markbucky/bucky-plugin
/plugin install bucky@bucky
```

Or the connector alone, without the skills:

```sh
claude mcp add --transport http bucky https://buckybuild.com/api/mcp
```

### Claude (web and desktop)

Settings → Connectors → **Add custom connector**, and paste the URL above. Needs
a paid plan.

### Cursor

[Add Bucky to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=bucky&config=eyJ0eXBlIjogImh0dHAiLCAidXJsIjogImh0dHBzOi8vYnVja3lidWlsZC5jb20vYXBpL21jcCJ9)

### VS Code

[Add Bucky to VS Code](vscode:mcp/install?%7B%22name%22%3A%20%22bucky%22%2C%20%22type%22%3A%20%22http%22%2C%20%22url%22%3A%20%22https%3A//buckybuild.com/api/mcp%22%7D)

### Codex

`~/.codex/config.toml`:

```toml
[mcp_servers.bucky]
url = "https://buckybuild.com/api/mcp"
```

Then `codex mcp login bucky`.

### ChatGPT and Grok

Add as a custom connector in ChatGPT's developer-mode Connectors settings, or at
[grok.com/connectors](https://grok.com/connectors) → New Connector → Custom.

### Anything else

Any Streamable HTTP client takes the same object:

```json
{ "type": "http", "url": "https://buckybuild.com/api/mcp" }
```

## What is in the plugin

Four skills, and one MCP connector:

| Skill                        | For                                                     |
| ---------------------------- | ------------------------------------------------------- |
| `site-feasibility`           | Zoning, lot, setback, and height lookup for one address |
| `save-and-analyze-project`   | Save a site, attach its parcel, run analysis            |
| `project-report`             | Prepare and export a cited report                       |
| `explain-feasibility-status` | Interpret a coverage or provenance status               |

## Authentication

Two tools work with no account, so you can confirm the connection before signing
in: `getBuckyMcpGuide` and `explainFeasibilityStatus`.

Everything else uses Bucky's OAuth browser flow. You never paste a token.
Connected apps are listed at Settings → Connected apps, with what each can do and
when it was last used; disconnecting takes effect on the next request rather than
whenever a token expires.

Account-scoped tools need a Professional plan or prepaid developer credit.

## Limits

Coverage varies by jurisdiction. Results are preliminary decision support — not a
permit, an appraisal, a guarantee, or a legal, engineering, or architectural
opinion.

## Contributing

This repo is a **mirror**. The source of truth is Bucky's monorepo, and changes
made here are overwritten on the next sync. Open an issue rather than a pull
request, or send feedback through the `submitBuckyFeedback` tool — it goes to a
human.

## License

Apache-2.0. See `LICENSE`.
