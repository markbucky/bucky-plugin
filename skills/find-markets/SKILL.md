---
name: find-markets
description: Find and rank cities by their rental market, such as vacancy, rent growth, or supply. Use when the user asks which cities or markets to look in, where rental demand is strong, or wants to rerun a saved market filter set. This skill filters places, not lots.
---

# Find Markets

Use `findPlacesByMarket` to choose which places to look in. It needs sign-in.

## Run a query

- Pass `country`: `CA` or `US`. Places rank only inside one country. If the user wants both, make two calls and show two lists. Do not merge them into one ranking.
- Pass a `preset`, `filters`, or both. Add `region` for one province or state. The tool description lists the presets and the filter keys. If you pass an unknown key, the result returns the keys you can use.
- Map the words of the user to a preset or to filters. If the words fit more than one, ask the user which one.

To rerun a saved set, call `listPlaceMarketSets`. Then call `findPlacesByMarket` with that `setId`, and `limit` and `offset` only. If the result has `unavailableFilters`, tell the user which filters Bucky skipped.

## Present the result

- Give the counts: places searched, matched, in the user's plan, and unknown.
- A place with a missing value is unknown. It is not a match and it is not a fail. Always state the unknown count.
- Each matched place has `access`. `in_plan` means the user can screen its lots. `available` means the user can upgrade to add it. `not_sold` means the user can request it. Say how many matches are in the plan.
- Cite each figure with the `sources` lines in the result.
- If `truncation` shows more places, say so. Read the next page only when the user asks.

## Go from places to lots

This is two steps, and you must tell the user it is two steps.

1. This skill ranks the places.
2. The `screen-lots` skill finds lots. Use it once for each matched place with `access: in_plan`, in rank order, with the `geoDivisionId` of the place. The tool description gives the maximum number of places.

Never screen a place that is unknown or not `in_plan`. Say how many places you left out and why.

Market figures describe a place. Do not apply them to one lot as if they were facts about that lot.

If `findPlacesByMarket` fails with "This connection is not signed in to Bucky", do not retry it. Tell the user to sign in to Bucky and then ask again.
