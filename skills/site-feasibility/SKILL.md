---
name: site-feasibility
description: Check zoning, permitted uses, lot dimensions, setbacks, height, coverage, and density for a specific Canadian or US street address using Bucky. Use when the user asks what they may be able to build on a site.
---

# Site Feasibility

Use Bucky's `getFeasibilitySnapshot` MCP tool for one specific street address. If the address is incomplete or ambiguous, ask for the missing municipality, province or state, and country before calling the tool.

Choose `source` from the user's question:

- `zoning_lookup` for zone or permitted-use questions.
- `lot_dimensions` for area, width, or depth questions.
- `setback_lookup` for yard or property-line questions.
- `height_limit_lookup` for height or storey questions.
- `full` when the user asks generally what the site may support.

## Present the result

- Lead with the decision the result supports, then show the lot, zone, permitted or conditional uses, and building envelope that matter to the request.
- Preserve the status attached to every envelope value. A null with `no_limit_in_bylaw` means the bylaw sets no limit. A null with `not_extracted` is unknown to Bucky. Never turn either into zero.
- Link the governing source returned by Bucky when available. Distinguish cited facts from conditional, unresolved, default, or missing facts.
- State the coverage result when it is not `live`. Do not fill a coverage gap with an uncited web result or a guess.
- Describe the result as preliminary decision support, not a permit, appraisal, or legal, engineering, or architectural opinion.

Do not save a project, attach parcels, or start analysis unless the user explicitly asks for that additional action.
