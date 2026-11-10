---
name: explain-feasibility-status
description: Explain a Bucky coverage or fact-provenance status and recommend the safe next step. Use when a result contains an unfamiliar status such as not_extracted, no_limit_in_bylaw, cited_conditional, or live_unavailable.
---

# Explain a Feasibility Status

Copy the exact status value from the Bucky result and call `explainFeasibilityStatus`. Do not paraphrase a label into a different status and do not invent a status.

Explain the returned meaning in the context of the user's question, then give the recommended next action. Preserve these distinctions:

- `not_extracted` means Bucky does not have the value. It does not mean zero or no limit.
- `site_specific_schedule` means a per-site bylaw governs that value, not a zone table. It is unknown. Never report it as zero or unlimited.
- `no_limit_in_bylaw` means the reviewed bylaw did not set that limit.
- `cited_conditional` and `conditional_unresolved` require the stated condition to be resolved before treating the rule as applicable.
- `live_unavailable`, `planned_not_live`, `geo_division_uncatalogued`, `unseeded_place`, and `geocode_failed` are coverage or resolution failures. Do not substitute a guessed zoning answer.
- `unverified_default` is not equivalent to a cited fact.

If no exact status appears in the source result, ask the user to provide it instead of guessing.
