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

Nine skills, and one MCP connector:

| Skill                        | For                                                       |
| ---------------------------- | --------------------------------------------------------- |
| `getting-started`            | Connect, sign in, set up your profile, choose a next step |
| `city-coverage`              | What Bucky has mapped for a city, no sign-in needed       |
| `site-feasibility`           | Zoning, lot, setback, and height lookup for one address   |
| `find-markets`               | Rank places by their rental market                        |
| `screen-lots`                | Find lots in one live city that match a list              |
| `saved-work`                 | List and organize saved lots, lists, and projects         |
| `save-and-analyze-project`   | Save a site, attach its parcels, run analysis             |
| `project-report`             | Prepare and export a cited report                         |
| `explain-feasibility-status` | Interpret a coverage or provenance status                 |

The skills carry the workflow and the confirmation rules. Each tool's own
description carries its arguments.

## How the work flows

The workflow is market → list → matching lots → shortlist → optional candidate
assessment → project → report. You can start at any step.

- **Profile.** `getBuckyMcpGuide` returns your standing profile: what you are
  trying to do, your markets, thesis, timeline, capital, and hard constraints.
  `updateMyProfile` saves an answer you gave. Once Bucky knows what you are
  trying to do, a new chat starts from it instead of asking again.
- **Market.** `findPlacesByMarket` ranks places on rental-market facts. It
  filters places, not lots.
- **List.** `screenLots` finds the lots in one city that match a list. It returns
  `screeningRunId` and the selected parcel IDs, with a `countOutcome` of exact,
  partial, at least, estimate, or unavailable.
- **Shortlist.** `updateSaved` saves a lot without starting a project, and files
  saved lots and projects in folders. `listSaved` reads them back.
- **Your work.** Call `listMyWork` before creating another list or project. It
  lists your lists and the projects you own, with city, stored criteria, build-
  parcel count, and feasibility state (`none`, `current`, `stale`, `queued`,
  `running`, or `failed`). `lastMatchCount` is the latest exact total for the
  current revision of a list; null means unmeasured, partial, unavailable, or
  edited since the run. It does not mean zero.
- **Project.** `saveToProject` creates a project and attaches parcels. To attach
  an assemblage, pass `action: "attach_parcels"` with a WGS84 GeoJSON `polygon`
  and `preview: true` to see the selected parcels first. A polygon selects at
  most 500 parcels, and cannot be combined with `parcelExternalIds` or `radiusM`.
- **Report.** `produceProjectReport` previews a report for free, then exports
  the exact text you approved.

Candidate assessments are bounded checks against a development brief. Their
counts are separate from a search total. They run in the Bucky CLI:
`bucky opportunities assess --box-id <id> --screening-run <execution-id> --input
<brief.json> --dry-run`. Deleting a folder is also a CLI or web-app action.

## Authentication

Four tools work with no account, so you can confirm the connection before signing
in: `getBuckyMcpGuide`, `explainFeasibilityStatus`, `cityInventory` (what Bucky
has mapped for a city), and `previewSiteCoverage` (which facts Bucky holds for an
address, without the values). Any other tool called before sign-in returns
"This connection is not signed in to Bucky". In Claude Code, run `/mcp`,
select `bucky`, and follow the browser steps. In Codex, run `codex mcp login bucky`.

Everything else uses Bucky's OAuth browser flow. You never paste a token.
Connected apps are listed at Settings → Connected apps, with what each can do and
when it was last used; disconnecting takes effect on the next request rather than
whenever a token expires.

The connector is free on every plan, including a free account. Higher plans give you more usage and more benefits.

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
