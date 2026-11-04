# CLAUDE.md — bloom-help-content

Help articles for **Bloom**, a school HR and performance management SaaS platform. All content is written by the Bloom design team; schools never author content. Pat (Patricia Tumulak) owns this repo and verifies every article.

## Where the content appears

- **Tutorials, the in-app side panel (now).** Opens from the existing **"?"** button at the bottom right of every Bloom page, next to **Share Feedback** and **Ask Bloom**. It shows help for the page the user is currently on, so every article is tied to a route.
- **Standalone Help Center (later).** Will reuse the same files. Write nothing that only makes sense inside Tutorials. (Whether this site is also called Tutorials is undecided.)

Tutorials is a narrow panel and users want an answer in seconds. Write for that.

## Audiences (role ids)

| Role id         | Who                          | Priority   |
|-----------------|------------------------------|------------|
| `school-owner`  | School owner / leadership    | Main focus |
| `hr`            | HR staff                     |            |
| `direct-report` | Staff member being managed   |            |
| `supervisor`    | Staff member who manages others (has team members) |  |

- Every article lists its roles in frontmatter (`roles: [school-owner]`).
- **A direct report must never see school-owner, HR, or supervisor content.** Never put `direct-report` on an article that mentions owner-, HR-, or supervisor-only actions, data, or pages.
- `supervisor` covers staff (not the owner) who have people reporting to them. In Bloom this unlocks **Professional Development** (Team View, Approvals) and changes what **My Tasks** and **1:1 Reviews** show. Owner-facing versions of those pages stay `school-owner` articles.
- One person can hold more than one role: a supervisor is also a direct report, and HR can also be a supervisor. Write each article for the role that needs it; don't repeat direct-report content inside supervisor articles.
- > **TODO (Pat):** Confirm Tutorials shows a user every article matching *any* of their roles (e.g. a supervisor sees both `direct-report` and `supervisor` articles).
- If a task works *identically* for two roles, one article can list both. If *anything* differs (steps, labels, what they can see, what happens next), write separate articles.
- If you can't confirm a role can see something, say so explicitly (a TODO for Pat) — don't assume.

## Source of truth: the Bloom codebase (READ-ONLY)

- The Bloom app lives at `/Users/fullscale/Documents/GitHub/bloom` (inside `/Users/fullscale/Documents/GitHub`). It has its own `CLAUDE.md` describing its structure; read it before extracting facts.
- **Treat everything under `/Users/fullscale/Documents/GitHub` except this repo as read-only.** Never create, edit, or delete files there. Never commit, branch, stash, or run git commands that change state. Never run its migrations, seeds, or any script that changes data. Don't install dependencies there.
- Only read it to extract facts. Read-only git commands (`git log`, `git diff`, `git show`) are fine for the update workflow.

## Folder structure

| Path                  | Contents                                                        | Published |
|-----------------------|-----------------------------------------------------------------|-----------|
| `help-inventory.md`   | Table of every user-facing page: route, UI page name, roles, purpose | No   |
| `facts/`              | One fact sheet per page, extracted from Bloom code              | **Never** |
| `pages/`              | Page guides: what this page is for, what you can do here        | Yes       |
| `tasks/`              | Task guides: how to do one specific thing                       | Yes       |
| `faqs/`               | One file per question; shared, searchable pool tagged by route  | Yes       |
| `_templates/`         | `page-guide.md`, `task.md`, `faq.md`, `fact-sheet.md`           | No        |

Always start a new file by copying the matching template from `_templates/`.

## Workflow

1. **Inventory.** Map every user-facing page, its route, and which roles can access it into `help-inventory.md`.
2. **Facts.** One page at a time, write `facts/<page>.md` from the template: every action and what it does, form fields (required/optional, validation rules), statuses, empty states, error/success messages, and role differences.
   - Quote UI strings *exactly* as users see them (resolve i18n keys/constants to the displayed text).
   - Note the source file path for each fact.
   - Flag anything behind a feature flag, unreachable, or unclear.
3. **Articles.** Turn a fact sheet into page guides, task guides, and FAQs using the templates. Articles may only state what the fact sheet supports.
4. **Verification.** Pat checks every article against the live app before it is marked `verified`.
5. **Updates.** After a Bloom release, compare user-facing changes in the Bloom repo against the fact sheets, update the fact sheets, and list which articles need updating (don't silently rewrite verified articles; list them for Pat).

Do one page at a time unless Pat asks otherwise. Stop at the end of each step and summarize.

## Frontmatter

Example values below are illustrative only, not real Bloom routes.

```yaml
# Page guide
id: survey-management-owner
type: page-guide
title: Survey management
route: /surveys
roles: [school-owner]
tasks: [send-surveys-owner]
status: draft
last_verified:

# Task guide
id: send-surveys-owner
type: task
title: Send surveys to staff
route: /surveys
roles: [school-owner]
related: [survey-management-owner, why-cant-i-send-surveys]
status: draft
last_verified:

# FAQ
id: why-cant-i-send-surveys
type: faq
question: Why can't I send surveys?
roles: [school-owner, hr]
routes: [/surveys]
tags: [surveys]
related: [send-surveys-owner]
status: draft
last_verified:
```

- `id` matches the filename (without `.md`), kebab-case.
- Role suffix when an article is role-specific: `-owner` (school-owner), `-hr`, `-direct-report`, `-supervisor`. Multiple roles: combine (`-owner-hr`). No suffix only when one article covers every role that has access to that task.
- `route` is the route as listed in `help-inventory.md`. FAQs use `routes` (a list) because one question can apply to several pages.
- `tasks` / `related` hold article ids, not paths.
- `status` is `draft`, `reviewed`, or `verified`. **Claude only ever sets `draft`.** Never change an existing status.
- `last_verified` stays empty. Only Pat fills it in after checking the live app.

## Writing rules

- Plain, friendly language for school staff, not developers. No code terms, API names, database/table names, or internal jargon.
- Lead with the user's goal, not the feature.
- Task titles are verb phrases: "Send surveys to staff", not "Survey Sending".
- **Bold** UI labels and quote them exactly as they appear in the app (e.g. click **Send survey**).
- Steps: 7 or fewer. If longer, split into separate tasks.
- FAQ answers: 1–3 sentences, then link to a task guide for full steps.
- One idea per paragraph; keep paragraphs short.
- Link between articles with relative links (`../tasks/send-surveys-owner.md`).

## Accuracy rules

- **Never invent behavior.** If the code is ambiguous or you can't confirm something, write a TODO question for Pat instead of guessing: `> **TODO (Pat):** Does the survey send immediately or at the scheduled time?`
- Code can include dead or flagged-off features. Flag these in the fact sheet; don't document them as live in articles.
- When unsure whether something is visible to a role, say so explicitly.
- Fact sheets are internal: they may contain file paths, flag names, and code details. Published articles (`pages/`, `tasks/`, `faqs/`) must not.
