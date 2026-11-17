---
name: saved-work
description: List and organize what the user saved in Bucky. Use when the user asks what they have saved, asks for their lists or projects, wants to save or unsave a lot without starting a project, wants to put saved items in a list, or before you create a new list or project.
---

# Saved Work

Bucky has two ideas: lists, and the properties in them. A project is a property the user is working on. Do not mix these up.

- A **list** holds properties. Filters can fill it (saved filters for one city), or the user can fill it by hand.
- A **saved lot** is one property the user saved. It is not a project, and it has no analysis.
- A **project** is a site the user is working on. It can have parcels, analysis, and reports.

## Read

- Call `listMyWork` for lists and projects. It returns the last match count of each list and the analysis state of each project. Call it before you create a list or a project, and before you retry an analysis. A null `lastMatchCount` means not measured. It does not mean zero.
- Call `listSaved` for saved lots and saved sites. Pass `include: ["lists"]` to get every list, with or without filters. Each list says `hasFilters` and `addedCount`.
- Pass `listId` to read one list. Each row says `matched` (its filters return it), `added` (the user added it by hand) or `both`. A property that is both appears once. `view` is `matches`, `added` or `all`, and `counts` gives the totals. Follow `nextCursor` for the next page.
- If the result has more pages, say so. Do not present one page as the full list.

## Change

`updateSaved` changes the user's account. Use it only when the user asks. Call it with `preview: true` first, show the change, and write only after the user confirms.

- `save` and `unsave` take the `lotId` from a `screenLots` lot or a `listSaved` entry.
- `unsave` also removes the lot from every list the user added it to, and the tool has no undo. Say this before the user confirms. Its notes, files and status are kept, and saving it again brings them back. A lot whose project is started cannot be unsaved (`PROPERTY_STARTED`); tell the user to archive the project in Bucky.
- `add_to_list` also saves the lot. `remove_from_list` keeps it saved.
- `add_to_list` and `remove_from_list` work on any list, with filters or without. They take `listId` and one item: a `lotId` or a `projectId`. Removing a row does not hide a lot that still matches. Adding the same property twice keeps one row.
- `create_list` makes a new list with no filters on every call. Before you retry it, call `listSaved` with `include: ["lists"]` to see if the list exists.
- `rename_list` takes `listId` and `name`.

MCP cannot delete a list. Tell the user to delete it in the Bucky web app or with the Bucky CLI.

## Related skills

- To make a saved lot or an address into a project, use the `save-and-analyze-project` skill.
- To rerun a list, use the `screen-lots` skill. Pass the `id` of the list from `listMyWork` as `profileId`.

If a tool fails with "This connection is not signed in to Bucky", do not retry it. Tell the user to sign in to Bucky and then ask again.

## How to answer

- Use the user's own words. Do not add a role, market, budget, or criteria that the user did not give and that is not in their Bucky profile. If one missing answer changes the result, ask one short question.
- Lead with the answer to the question, then the few facts that support it.
- Write in plain words. Do not label each sentence as fact, inference, or assumption.
- Say a limit once, where it matters to what the user wants to do. Do not list every gap in the data.
- End with one next step that fits the result. Do not list every option.
