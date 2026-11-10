---
name: save-and-analyze-project
description: Save a site to Bucky, attach its parcel, run project feasibility analysis, update a project's name or status, or list it on the public map. Use only when the user explicitly asks to create, save, attach, analyze, rename, update, or publish a Bucky project.
---

# Save and Analyze a Project

This workflow changes the user's Bucky account. A feasibility lookup alone does not authorize a project write.

Before you create a project, call `listMyWork` to see if the user already has one for this site. To bookmark a lot without a project, use the `saved-work` skill instead.

## Name the project

Each project tool takes `projectId`, or `project` with the name the user said. You do not need a UUID. If the name matches more than one project, or you pass neither, the result lists the projects and writes nothing. Ask the user to choose. Do not choose for the user.

## Save a site

1. If the request starts with an address, call `checkSite` first. Reuse its normalized address, coordinates, and `lot.parcelExternalId`; do not invent parcel identifiers.
2. Call `saveToProject` with `preview: true`. For a new project use `action: create`. Include the address and, when returned by the snapshot, its coordinates.
3. Show the proposed change and geography result. Warn that MCP cannot archive the new project or detach an attached parcel.
4. Ask the user to confirm the preview. Only after confirmation, repeat the same operation with `preview: false`.
5. For creation, use an idempotency key and reuse it only when retrying that exact operation. Do not reuse it for a different project.
6. If a snapshot returned `lot.parcelExternalId` and the user asked to attach the site, preview and confirm a second `saveToProject` call with `action: attach_parcels`.
7. For an assembly of many lots, pass a WGS84 GeoJSON `polygon` with `action: attach_parcels` instead of parcel IDs. Do not combine a polygon with `parcelExternalIds` or `radiusM`. One polygon selects at most 500 parcels. In the preview, report `selectedParcelCount` and say if the selection was truncated.

`action: attach_reference` links a bylaw section to a project. Use it only when the user asks to keep a source with the project.

If the result says `geography.resolved: false`, explain that the project lacks city context and analysis may not resolve. If `coordinateSource` is `geocoded`, ask the user to verify the selected site before continuing.

## Run analysis

Call `runFeasibilityAnalysis` only when the user explicitly asks to run or refresh analysis. A queued analysis cannot be cancelled through MCP. Report the returned `agentStatus` and receipt ID.

Use `getProjectContext` to check the result. Pass `include` only for sections the user asks about: `tasks`, `members`, `insights`, `capital`, or `visualEvidence`. Changes to those sections are made in the Bucky CLI or the web app, not in MCP. Do not poll more often than once every 15 seconds. Stop when the status is `current` or `failed`, or when the user asks you to stop.

## Update or publish a project

- To change a project's name, lifecycle status, or `externalRef`, call `updateProject` with `preview: true`. Show the diff, and write with `preview: false` only after the user confirms. Report the receipt. `undoOperationId` restores the old fields. MCP cannot archive or delete a project.
- `setProjectMapListing` puts a project on the public Bucky map, or takes it off. Anyone can then see the build-lot location, the project name, the zone category, the typology, and the lifecycle status. Ask the user before you list it, every time. A client's site or a private home is not yours to publish. A project with no attached build parcel cannot be listed.

If a write tool fails with "This connection is not signed in to Bucky", do not retry. Tell the user to sign in to Bucky first.

Never claim that a preview wrote data, that a queued run is complete, or that an unresolved geography has usable zoning context.
