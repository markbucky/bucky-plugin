---
name: site-feasibility
description: Check zoning, permitted uses, lot dimensions, setbacks, height, coverage, and density for a specific Canadian or US street address using Bucky. Use when the user asks what they may be able to build on a site.
---

# Site Feasibility

Use Bucky's `checkSite` MCP tool for one specific street address. If the address is incomplete or ambiguous, ask for the missing municipality, province or state, and country before calling the tool.

Choose `source` from the user's question:

- `zoning_lookup` for zone or permitted-use questions.
- `lot_dimensions` for area, width, or depth questions.
- `setback_lookup` for yard or property-line questions.
- `height_limit_lookup` for height or storey questions.
- `full` when the user asks generally what the site may support.

## If the user is not signed in

`checkSite` needs sign-in. If it fails with "This connection is not signed in to Bucky", do not call it again in the same session. Do these two things instead:

1. Call `previewSiteCoverage` with the same address. It needs no sign-in. It tells you if Bucky covers the place and which facts it has on file, but it gives no values. Do not report a zone code, height, or setback from a preview.
2. Tell the user to sign in to Bucky. A free account works. Then ask again for the numbers.
   - Claude Code: run `/mcp`, select `bucky`, and follow the steps in the browser.
   - Codex: run `codex mcp login bucky`.
   - Claude, ChatGPT, or another app: approve the Bucky sign-in page when the app opens it, and stay in the chat. Do not disconnect the connector.

If you do not know whether Bucky covers the city at all, use the `city-coverage` skill first.

## Site checks

Each `checkSite` call with a live answer uses one site check from the monthly allowance of the plan. `usage` in the result says if the call was charged and how many are left. Call it one time for each address. Do not repeat a call to get a different `source`: the result is the same for every `source`. To read the allowance without a charge, call `getBuckyMcpGuide`.

`checkSite` takes one address. It cannot search or compare. To find lots, use the `screen-lots` skill.

## Read the bylaw before you treat a number as available

For each cited envelope number you will report as available on this lot, call `getBylawSection` with that citation's `knowledgeSectionId`. Or pass `coverage.geoDivisionId`, `zone.code`, and the section number. Read the returned section. A number in the snapshot can still be conditional on frontage, tenure, or an adjoining district.

Do not fetch `zone.bylawSourceUrl`.

If the result has `exception`, read it before you report the envelope. It identifies a site-specific exception on this lot, and its `chain` has the cited text. Use that text first. `text_missing` means the evidence is incomplete. It never means that there is no exception.

## Present the result

- Lead with the decision the result supports, then show the lot, zone, permitted or conditional uses, and building envelope that matter to the request.
- Preserve the status attached to every envelope value. A null with `no_limit_in_bylaw` means the bylaw sets no limit. A null with `not_extracted` is unknown to Bucky. Never turn either into zero.
- Read `lot.minLotFrontageM` and `lot.lotConformity` before quoting a unit count or yield. State every check whose status is `fail`. Treat `unknown` as unknown, never as a pass. Do not quote a zone max as buildable yield when a check fails.
- Link the governing source returned by Bucky when available. Distinguish cited facts from conditional, unresolved, default, or missing facts.
- Cite market figures, such as rental survey data, with the source and period that the result gives. A missing market figure is not zero.
- State the coverage result when it is not `live`. Do not fill a coverage gap with an uncited web result or a guess.
- Describe the result as preliminary decision support, not a permit, appraisal, or legal, engineering, or architectural opinion.

Do not save a project, attach parcels, or start analysis unless the user explicitly asks for that additional action.

If a Bucky value looks wrong, tell the user. If the user wants to report it, call `submitBuckyFeedback`. A person reads that report. Call it only when the user asks to send feedback.
