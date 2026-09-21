---
name: project-report
description: Prepare and export a cited Bucky project report for an investor, owner, board, or resident. Use when the user asks for a feasibility report, project brief, or PDF export.
---

# Project Report

Use `getProjectContext` before composing a report. If the user has not identified a project, omit `projectId` so Bucky can ask them to select one. Choose bounded document and evidence limits that fit the request.

Call `produceProjectReport` first with:

- `preview: true`
- `confirm: false`
- the requested `audience`: `investor`, `owner`, `board`, or `resident`
- `detail: brief` unless the user asks for a detailed report

Present the report plan and identify material unknown, conditional, or uncited facts. A preview does not create a PDF and does not spend the export meter.

Export only after the user explicitly confirms the preview. Then call `produceProjectReport` with `preview: false` and `confirm: true`. Explain that export spends the `records.exports` meter and cannot be undone. Return the signed download link promptly because it is short-lived, and include the receipt ID.

Keep the report's conclusions proportional to the evidence. Bucky reports are preliminary decision support, not permits, appraisals, guarantees, or professional legal, engineering, or architectural opinions.
