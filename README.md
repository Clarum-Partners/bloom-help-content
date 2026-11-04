# bloom-help-content

Help articles for Bloom, the school HR and performance management platform. Articles appear in **Tutorials**, the in-app side panel (opened from the **"?"** button at the bottom right of every page) and will later power a standalone Help Center. All content is written by the Bloom design team.

## Structure

| Path                | Contents                                              | Published |
|---------------------|-------------------------------------------------------|-----------|
| `help-inventory.md` | Every user-facing page: route, name, roles, purpose   | No        |
| `facts/`            | One fact sheet per page, extracted from the Bloom code | **Never** |
| `pages/`            | Page guides: what a page is for, what you can do there | Yes      |
| `tasks/`            | Task guides: how to do one specific thing             | Yes       |
| `faqs/`             | One question per file, tagged by route                | Yes       |
| `_templates/`       | Starting points for each file type                    | No        |

## Audiences

Each article targets roles in its frontmatter: `school-owner` (main focus), `hr`, `direct-report`, `supervisor` (staff who manage others). Direct reports never see owner, HR, or supervisor content.

## How content gets made

1. Inventory pages and role access.
2. Extract a fact sheet per page from the Bloom code.
3. Write page guides, task guides, and FAQs from the fact sheet.
4. Pat verifies each article in the live app (`draft` → `reviewed` → `verified`).
5. After each Bloom release, compare changes to fact sheets and update affected articles.

See `CLAUDE.md` for the full conventions and writing rules.
