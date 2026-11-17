---
name: screen-lots
description: Find lots in one live Bucky city that match a list. Use when the user asks which sites, parcels, or lots in a city fit size, price, zoning, or other criteria, wants land for something they plan to build, or wants lots to assemble.
---

# Screen Lots

Use `screenLots` for one live city. Pass `city`, or `geoDivisionId` from an earlier result. Pass `thesis` in plain words, `criteria` in square feet and dollars, or both. A field set in `criteria` replaces the same field read from `thesis`.

Put the user's own words in `thesis`, including what they want to build ("lots to assemble for a small apartment building"). Do not make up criteria the user did not give. If a number that changes the list is missing, such as the lot size or the number of homes, ask one short question. Otherwise run the search and report the defaults the result names in `notes`.

When the user wants to combine lots, also pass `assembly` (for example `maxLots` and `minCombinedAreaSqft`). If the result says assemblies cannot be searched in that city, tell the user at the start of your answer, then give the single-lot results.

To rerun a list the user already saved, pass only `profileId`. Do not combine `profileId` with `city`, `thesis`, or `criteria`. `listMyWork` returns the saved lists; the `id` of a list is its `profileId`. Call `listMyWork` before you save a new list, because the user can already have one.

For a height the user wants to build, such as "10 to 14 storeys", use `criteria.permitted_stories` or `criteria.permitted_height_m`. These are the maximum the zone allows. `criteria.stories` is the building that stands on the lot now.

Report:

- the match count, with its `countOutcome`: `exact`, `partial`, `at_least` (a lower bound), `estimate` (say "about"), or `unavailable`. Only `exact` is a total. When the outcome is `unavailable`, give no total: `totalMatches` can be only the number of lots on this page.
- the lots returned, in the order of the result, with a pass, fail, or `not_available` verdict on each filter
- every filter this city cannot apply, and every default in `notes` that checked only part of the city, such as public owners where few lots have owner data

A `not_available` filter was not applied. Never present the count as if that filter had narrowed it.

One call returns at most 25 lots. When `page.truncated` is true, tell the user there are more. Call `screenLots` with `page.next` only when the user wants the next lots. Paging saves nothing.

The result can also have `candidates`, `suggestions`, and `savedMatches`. These are alternative criteria and lists that exist already. Show them to the user before you offer to save a list.

Each lot has `address` for `checkSite`, `lotId` for `updateSaved`, and `parcelExternalId` for `saveToProject`. Call those only when the user asks to look up or save a specific lot. To save a lot without starting a project, call `updateSaved` with `action: save` and the `lotId`. Use `preview: true` first.

To put lots the user chose into one project, call `saveToProject` with `action: create` and the address of one lot, then `action: attach_parcels` with the `parcelExternalId` of each lot. Do not call `checkSite` on each lot first: `checkSite` uses one of the user's monthly site checks per call, and `screenLots` already returned the ids.

End by offering to save the list. A saved list gives a CSV link of every match.

`save: true` saves the list and needs write scope. It counts against the list limit of the plan. Use it only when the user asks to save the list, and pass the criteria the user approved. It returns a CSV link of every match. Without a saved list, there is no CSV link. The default writes nothing.

## Flag results that are not a fit

When the user reviews the results of a saved list and you see a lot that is clearly not a fit, such as an operating school or a condo with many owners, you can call `proposeResultFlags` with the `profileId`, the `lotId`, a reason code, and a few words of evidence. A proposal changes nothing until the user confirms it on the list. Tell the user what you proposed and where to confirm it. If a result says `declined`, the user said no: never propose that lot again. Do not use this tool for feedback about Bucky.

If the city is not live, say so and stop. Do not invent lots for a city Bucky does not cover. To show the user what Bucky has on file for that city, use the `city-coverage` skill.

`screenLots` needs sign-in. If it fails with "This connection is not signed in to Bucky", do not retry it. Tell the user to sign in to Bucky and then ask again.

## How to answer

- Use the user's own words. Do not add a role, market, budget, or criteria that the user did not give and that is not in their Bucky profile. If one missing answer changes the result, ask one short question.
- Lead with the answer to the question, then the few facts that support it.
- Write in plain words. Do not label each sentence as fact, inference, or assumption.
- Say a limit once, where it matters to what the user wants to do. Do not list every gap in the data.
- End with one next step that fits the result. Do not list every option.
