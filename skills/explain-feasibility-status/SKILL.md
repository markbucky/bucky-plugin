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

## How to answer

- Use the user's own words. Do not add a role, market, budget, or criteria that the user did not give and that is not in their Bucky profile. If one missing answer changes the result, ask one short question.
- Lead with the answer to the question, then the few facts that support it.
- Write in plain words. Do not label each sentence as fact, inference, or assumption.
- Never show tool names, argument names, field names, or raw values such as `find_deals`, `preview: true`, or `null`. Say what the step does for the user: "run the feasibility analysis", "preview an investor report". Do not say "MCP" or "meter".
- Say a limit once, where it matters to what the user wants to do. Do not list every gap in the data.
- End with one next step that fits the result. Do not list every option.
