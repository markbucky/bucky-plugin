---
name: city-coverage
description: Check whether Bucky covers a city and what zoning, parcel, and overlay data it has on file there. Use when the user asks if Bucky works in a place, which cities are supported, or why a site result came back with no coverage.
---

# City Coverage

Call `cityInventory` with exactly one of these:

- `cityName`
- `lat` and `lng` together
- `geoDivisionId` from an earlier result

Never pass a slug. You do not need to sign in to use this tool.

If a city name matches more than one place, the result lists the choices. Ask the user which one they mean. Then call again with the `geoDivisionId` of that choice. Do not choose for the user.

## Present the result

- Group the datasets the way the result does: on file, not loaded yet, unavailable, or not confirmed. Do not say that a "not loaded yet" or "not confirmed" dataset is available.
- This result is city coverage. It is not lot feasibility, and it does not tell you about service health. To check one address, use `previewSiteCoverage` (no sign-in) or `checkSite` (sign-in).
- If Bucky has not mapped the place, say so plainly. Do not give a zoning answer from a web search or from memory instead. The result tells you how to record demand for the place. The call itself does not record demand.

To find lots across a covered city, use the `screen-lots` skill.
