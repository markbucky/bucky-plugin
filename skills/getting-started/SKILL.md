---
name: getting-started
description: Connect to Bucky, sign in, set up the user's Bucky profile, and choose the next step. Use when the user first connects Bucky, asks what Bucky can do, asks where to start or what to do next, asks what Bucky knows about them or tells you something to remember about how they work, asks how many site checks are left, or when a Bucky tool says the connection is not signed in.
---

# Getting Started with Bucky

Call `getBuckyMcpGuide` with no arguments first. It is free and it needs no sign-in. The result depends on the session, so read it before you plan. Do not answer from memory, and do not list the tool catalogue to the user.

## Follow the guide

- **Not signed in.** Show the user something real before you ask for sign-in. Ask for one street address or one city. For an address, call `previewSiteCoverage`: it says which facts Bucky has for that lot, with no values. For a city, call `cityInventory`. Tell the user what Bucky has there. Then go to "Sign in" below, because the values need an account. Use `explainFeasibilityStatus` if a status in the result is not clear.
- **Signed in, no projects.** Read `profile` in the guide and go to "Set up the profile" below. If the guide has no `profile`, it returns up to three questions about the user's role, market, and scale instead. Ask the user those questions. Do not answer them for the user. Call `getBuckyMcpGuide` again with the answers as the arguments the guide names. The guide then returns one workflow with a first tool and what that tool needs. Ask the user for what it needs, and run it. If the workflow starts at lots and the user has no city in mind, use the `find-markets` skill first.
- **Signed in, with projects.** The guide returns recent projects and one `next` step with a tool, its arguments, and a reason. Tell the user the step and the reason. Run it only if it is a read, or if the user agrees.

`usage.siteLookups` in the guide is the site check allowance. Each `checkSite` with a live answer uses one site check. When the user asks how many are left, read it from the guide. A null value means the guide does not report a number. Do not report null as zero, and do not report it as unlimited.

Before you create a list or a project, call `listMyWork`. The user can already have one.

## Set up the profile

Bucky keeps a short standing profile for each user: what they are trying to do, their markets, their thesis, their timeline, how a deal is capitalized, and their hard constraints. The Bucky app uses the same profile. For a signed-in user, the guide returns it as `profile`, with the `known` answers and the `missing` questions.

1. If `job_to_be_done` is in `missing`, ask that question first, together with the question about markets. Give the options the guide lists. These two questions replace the three first-call questions of the guide.
2. Tell the user that you will save the answers to their Bucky profile, so that Bucky does not ask again. If the user agrees, call `updateMyProfile` with the answers.
3. Call `getBuckyMcpGuide` again. It now returns one workflow with a first tool and what that tool needs. Run that workflow.
4. Ask the other open questions after the first result, when an answer helps the task. Ask at most two in one message. If the user skips a question, do not ask it again in this conversation.

Rules for `updateMyProfile`:

- Pass only answers the user stated. Never infer an answer from the conversation, and never save an address, a lot, or a zone as a profile answer.
- If an answer replaces one in `known`, call it with `preview: true` first, show the user the before and the after, and write only after the user confirms.
- If a result says `conflict_queued`, Bucky kept the earlier answer. Tell the user to choose between the two in the Bucky app.
- The tool cannot delete an answer. Tell the user to remove it in the Bucky app, or with `bucky memory forget` in the Bucky CLI.

Use the `known` answers in your work. Start a city question from the markets of the user. When you present lots or a site, say which results conflict with a hard constraint. Do not hide those results.

## Sign in

Every tool other than the four above needs sign-in. A free Bucky account works.

- Claude Code: run `/mcp`, select `bucky`, and follow the steps in the browser.
- Codex: run `codex mcp login bucky`.
- Other apps: call a gated tool such as `checkSite`. The app opens the Bucky sign-in page. The user approves it and stays in the chat. Do not tell the user to disconnect the connector.

If a tool fails with "This connection is not signed in to Bucky", do not call it again until the user has signed in.

## Choose the skill for the job

| The user wants to                               | Skill                        |
| ----------------------------------------------- | ---------------------------- |
| Know if Bucky covers a city                     | `city-coverage`              |
| Know what one address may support               | `site-feasibility`           |
| Choose which cities to look in                  | `find-markets`               |
| Find lots in one city that match criteria       | `screen-lots`                |
| See or organize saved lots, lists, and projects | `saved-work`                 |
| Save a site as a project and run analysis       | `save-and-analyze-project`   |
| Get a cited report                              | `project-report`             |
| Understand a status in a result                 | `explain-feasibility-status` |

The usual order is: market, then lots, then a shortlist, then a project, then a report. The user can start at any step.

## When the user is set up

The user is set up when they are signed in and have one real result: a `checkSite` answer for an address, or a `screenLots` result for a city. Do not stop at sign-in. After the first result, tell the user the one next step that fits it: read the cited bylaw section, save the lot, save the list, or start a project. Do not list every option.

A read does not authorize a write. Save, attach, analyze, publish, and export only when the user asks.
