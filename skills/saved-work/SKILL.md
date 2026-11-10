---
name: saved-work
description: List and organize what the user saved in Bucky. Use when the user asks what they have saved, asks for their lists or projects, wants to save or unsave a lot without starting a project, wants to put saved items in folders, or before you create a new list or project.
---

# Saved Work

Bucky keeps three different things. Do not mix them.

- A **list** is saved search criteria for one city.
- A **saved lot** is one bookmarked lot. It is not a project, and it has no analysis.
- A **project** is a site the user is working on. It can have parcels, analysis, and reports.

## Read

- Call `listMyWork` for lists and projects. It returns the last match count of each list and the analysis state of each project. Call it before you create a list or a project, and before you retry an analysis. A null `lastMatchCount` means not measured. It does not mean zero.
- Call `listSaved` for saved lots and saved sites. Pass `include: ["folders"]` to get the folders. Pass `folderId` to read one folder.
- If the result has more pages, say so. Do not present one page as the full list.

## Change

`updateSaved` changes the user's account. Use it only when the user asks. Call it with `preview: true` first, show the change, and write only after the user confirms.

- `save` and `unsave` take the `lotId` from a `screenLots` lot or a `listSaved` entry.
- `unsave` also removes the lot from every folder, and the tool has no undo. Say this before the user confirms.
- `add_to_folder` and `remove_from_folder` take `folderId` and one item: a `savedLotId` or a `projectId`. The `savedLotId` is the `id` of a `listSaved` lot entry. It is not the `lotId`.
- `create_folder` makes a new folder on every call. Before you retry it, call `listSaved` with `include: ["folders"]` to see if the folder exists.

MCP cannot delete a folder. Tell the user to delete it in the Bucky web app, or with `bucky saved folders delete <folder-id>` in the Bucky CLI.

## Related skills

- To make a saved lot or an address into a project, use the `save-and-analyze-project` skill.
- To rerun a list, use the `screen-lots` skill. Pass the `id` of the list from `listMyWork` as `profileId`.

If a tool fails with "This connection is not signed in to Bucky", do not retry it. Tell the user to sign in to Bucky and then ask again.
