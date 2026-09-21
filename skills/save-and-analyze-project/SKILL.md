---
name: save-and-analyze-project
description: Save a site to Bucky, attach its parcel, or run project feasibility analysis. Use only when the user explicitly asks to create, save, attach, or analyze a Bucky project.
---

# Save and Analyze a Project

This workflow changes the user's Bucky account. A feasibility lookup alone does not authorize a project write.

## Save a site

1. If the request starts with an address, call `getFeasibilitySnapshot` first. Reuse its normalized address, coordinates, and `lot.parcelExternalId`; do not invent parcel identifiers.
2. Call `saveToProject` with `preview: true`. For a new project use `action: create`. Include the address and, when returned by the snapshot, its coordinates.
3. Show the proposed change and geography result. Warn that MCP cannot archive the new project or detach an attached parcel.
4. Ask the user to confirm the preview. Only after confirmation, repeat the same operation with `preview: false`.
5. For creation, use an idempotency key and reuse it only when retrying that exact operation. Do not reuse it for a different project.
6. If a snapshot returned `lot.parcelExternalId` and the user asked to attach the site, preview and confirm a second `saveToProject` call with `action: attach_parcels`.

If the result says `geography.resolved: false`, explain that the project lacks city context and analysis may not resolve. If `coordinateSource` is `geocoded`, ask the user to verify the selected site before continuing.

## Run analysis

Call `runFeasibilityAnalysis` only when the user explicitly asks to run or refresh analysis. A queued analysis cannot be cancelled through MCP. Report the returned `agentStatus` and receipt ID.

Use `getProjectContext` to check the result. Do not poll more often than once every 15 seconds. Stop when the status is `current` or `failed`, or when the user asks you to stop.

Never claim that a preview wrote data, that a queued run is complete, or that an unresolved geography has usable zoning context.
