# Help inventory

Every user-facing page in Bloom. Internal, not published.

- **Extracted from:** Bloom repo commit `e4350be` (2026-11-03), read-only.
- **Routes** are written relative to the school, e.g. `/employees` means `/<school-slug>/employees`. Pages outside a school are listed separately below.
- **Access key:** ✓ yes · — no · ? unconfirmed (see Notes and Open questions). Access is based on the **default seeded permissions**. Bloom admins can change staff permissions in the live app (see Q2), so Pat must confirm in the live app.
- **Help panel:** the **"?"** button appears on every school page except `/onboarding/complete`. It does not appear on any page outside a school.

## How Bloom's code models our roles

| Help role id    | In the code                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| `school-owner`  | User type `school-owner` (the school's owner). Gets the full school permission set. |
| `hr`            | A regular staff member (user type `employee`) who is set as the school's HR contact. One per school. Gets staff permissions plus HR-only extras. |
| `direct-report` | Any regular staff member (user type `employee`).                            |
| *(supervisor)*  | Not a role in the code. It's a staff member who has team members under them. Unlocks extra pages. **Not in our audience list (see Q1).** |

Sources: `client/src/hoc/withAccess.tsx`, `client/src/contexts/AuthContext.tsx`, `client/src/hooks/sidebar.tsx`, `supabase/migrations/20260429150305_employee_survey_reviews.sql` (`get_user_context`), `supabase/migrations/20260429150307_post_squash_seed_data.sql` (permission seed).

## School pages (help panel available)

| Route | UI page name | school-owner | hr | direct-report | Purpose (one line) | Fact sheet | Notes |
|-------|--------------|:-:|:-:|:-:|--------------------|------------|-------|
| `/` | **My Tasks** | ✓ | ✓ | ✓ | See everything that needs your attention in one place. | | Content differs by role and supervisor status. Source: `pages/MyTasks.tsx`. |
| `/onboarding` (+ `/overview`, `/school/employees`, `/school/positions`, `/school/assignments`, `/school/report-structure`, `/survey/transition`, `/survey/school-core-values`, `/survey/competencies`) | Onboarding *(no single page title found)* | ✓ | ? | ? | Set up your school and its first survey, step by step. | | Opening the school home page redirects here until setup is complete. No role check found in the app's page code (Q4). Source: `routes/school/onboarding.tsx`, `components/guards/SchoolSlug.tsx`. |
| `/my-surveys` | **My Surveys** | ✓ | ✓ | ✓ | See and complete the surveys sent to you. | | Source: `pages/MySurveys.tsx`. |
| `/my-professional-development` (+ `/recommendations`, `/history`) | Menu: **My Development**; page heading: **Professional Development** | ✓ | ✓ | ✓ | Find, track, and manage your own PD activities. | | Tabs: **My Activities**, **Recommendations**, **History**. Page description: "Find, track, and manage your PD activities". Naming mismatch (Q6). Source: `routes/school/professional-development.tsx`, `hooks/utils/pdTabs.ts`. |
| `/employees` (+ `/:id`, `/:id/edit`, `/:id/goals`, `/:id/manage-goals`) | **Employees** | ✓ | — | — | Manage your school's staff list and each person's details and goals. | | Detail, edit, and goals views open on top of the list. HR has no access by default (Q2). Source: `pages/Employees.tsx`, `pages/Employees/*`. |
| `/positions` (+ `/:id`, `/:id/edit`) | Menu: **Positions**; page heading: **Position** | ✓ | — | — | Manage and organize position descriptions for evaluations. | | Description quoted from the page: "Manage and organize position descriptions for evaluations". Heading/menu mismatch (Q6). Source: `pages/Positions.tsx`. |
| `/surveys` | **Survey Management** | ✓ | — | — | Set up, send, and track staff surveys. | | Source: `pages/Surveys.tsx`. |
| `/surveys/:id/:view?` | **"<Name>'s Survey Preview"** | ✓ | ? | ? | Preview a staff member's survey (self and supervisor versions). | | Tabs: **Self Survey**, **Supervisor…**. This page has no access check of its own (Q8). Source: `pages/ViewSurvey.tsx`. |
| `/surveys/respondent/:respondentId/tracking` | **Send Survey** | ✓ | — | — | Track who a survey was sent to and their progress. | | Purpose inferred from the route name; check in the fact sheet. Source: `pages/SurveyTrackingDetails.tsx`. |
| `/professional-development` (+ `/all`, `/budget`, `/approvals`, `/team-view`, `/team-view/member/:employeeId`) | **Professional Development** | ✓ | ? | ? | Manage your school's (or your team's) PD activities, budgets, and approvals. | | Owners see tabs **All PDs**, **Budget**, **Approvals**. Supervisors see **Team View**, **Approvals**. HR and direct reports get it only if they are supervisors (Q1). `/departments` redirects to `/all`. Source: `hooks/utils/pdTabs.ts`, `hooks/sidebar.tsx`. |
| `/reviews` | **1:1 Reviews** | ✓ | ✓ | ✓ | Draft, review, and acknowledge 1:1 reviews. | | Page description: "Draft, review, and acknowledge individual reviews · Cycle <year>". Shows reviews the signed-in person is part of; what owners see vs staff is unconfirmed (Q9). Source: `pages/Reports/SurveyReports.tsx`. |
| `/reviews/surveys/:id/:version?` | 1:1 review details *(title not confirmed)* | ✓ | ✓ | ✓ | Read, complete, and sign one 1:1 review. | | Empty state: "No survey report data generated yet". Source: `pages/SurveyReportDetails.tsx`. |
| `/reviews/surveys/:id/pdf` | **PDF Preview** | ✓ | ✓ | ✓ | Preview and download a signed review as a PDF. | | "The PDF is available once the review has been signed by all parties." Source: `pages/Reports/ReviewPDFViewer.tsx`. |
| `/school-survey-reviews` | **School 1:1 Reviews** | ? | ✓ | — | See every 1:1 review across the school. | | HR only. An owner would see it only if the owner is also the school's HR contact (Q3). Source: `pages/Reports/SurveyComparisons.tsx`. |
| `/reports` | Menu & tab title: **School Insights**; page heading: **School Reports** | ✓ | — | — | View school-wide reports built from survey results. | | Page description: "View and manage reports for your school". Naming mismatch (Q6). Source: `pages/Reports.tsx`, `pages/Reports/SchoolWideReports.tsx`. |
| `/reports/:reportSlug` | One page per report | ✓ | — | — | Read one school-wide report. | | Six reports: organizational-structure, school-wide-ratings, position-attributes, manager-alignment, job-clarity, strategic-alignment. Position attributes only shows when the school has them. Two more (capacity-building, manager-support) are defined in code but never listed, so treat them as unreachable. Source: `hooks/reports.ts`, `constants/reports.ts`. |
| `/settings/school` | **School Settings** | ✓ | — | — | Manage school details, including the HR contact and PD secondary approver. | | `/settings` redirects here. All Settings pages require owner-level access by default. Source: `pages/Settings.tsx`, `pages/Settings/School.tsx`. |
| `/settings/survey` | **Survey Settings** | ✓ | — | — | Set up survey themes, competencies, and position attributes. | | Settings tab **Survey**. Source: `pages/Settings/Survey.tsx`. |
| `/settings/professional-development` | **Professional Development Settings** | ✓ | — | — | Configure how professional development works at your school. | | Settings tab **Professional Development**. Source: `pages/Settings/ProfessionalDevelopment.tsx`. |
| `/settings/budget` | **Budget** | ✓ | — | — | Set up PD budgets. | | Settings tab **Budget**. Purpose inferred; check in the fact sheet. Source: `pages/Settings/Budget.tsx`. |
| `/profile` | **Profile Settings** | ✓ | ✓ | ✓ | Update your own profile and account details. | | Purpose inferred; check in the fact sheet. Source: `pages/ProfileSettings.tsx`. |

## Pages outside a school (no help panel)

These pages have no "?" button, so they can't show side-panel help. They may still matter for the future Help Center or for FAQs (Q7).

| Route | UI page name | Who | Purpose | Notes |
|-------|--------------|-----|---------|-------|
| `/sign-in` | Sign In | Everyone | Sign in to Bloom. | |
| `/send-reset-email` | Send Reset Email | Everyone | Request a password reset email. | |
| `/reset-password` | Create New Password | Everyone | Set a new password from the reset email. | |
| `/password-updated` | Password Updated | Everyone | Confirms your password was changed. | |
| `/account-setup` | Set Up Your Account | New users | Set a password or **Link with Google** for a new account. | |
| `/otp/account-setup` | Account Setup | New users from a survey email | Set up an account before answering a survey. | |
| `/survey/respond/:id` | Survey | Anyone sent a survey | Answer a survey. | Empty states include "Survey Not Found", "You Do Not Have Access", "Survey Not Available Yet". |
| `/checkpoint/survey/:id`, `/reviews/checkpoint/:supervisorId/:directReportId` | *(no page)* | n/a | Redirect-only links from emails. | Not help targets. |

## Out of scope: Bloom internal admin

`/admin/*` (Schools, Survey Themes, Imports, Admins, Permissions, Scrape, AI, Font, Feedbacks, Settings) and `/reviews/all` (**All 1:1 Reviews**) are for Bloom super admins only. Schools never see them. Source: `routes/admin/*`, `components/guards/OnlySuperuser.tsx`.

## Open questions for Pat

1. **Supervisors.** Being a supervisor unlocks **Professional Development** (Team View, Approvals) and changes what **My Tasks** and **1:1 Reviews** show. Supervisors are staff, so in our model they're `hr` or `direct-report`. Do we add a `supervisor` audience, or write supervisor-only articles some other way? Either way, a direct report who isn't a supervisor must not see them.
2. **Live permissions may differ from the defaults.** Bloom admins can change the staff permission set for all schools at once from **Admin > Permissions** (`pages/Admin/EmployeePermissions.tsx`). By default HR has none of Employees, Positions, Survey Management, Settings, or School Insights. Is that true in the live app?
3. **HR is one person.** HR is the single staff member set as the HR contact in **School Settings**. Can the owner also be the HR contact? If so, the owner would also see **School 1:1 Reviews**.
4. **Onboarding access.** No role check was found on the onboarding pages, and opening the school home page redirects anyone there until setup is finished. Can staff land in onboarding before the owner finishes it?
5. **Professional Development by direct link.** The page is hidden from the menu for non-supervisor staff, but nothing stops them opening the link. They'd see no tabs. We'll treat it as no access; please confirm.
6. **Which name do articles use?** Menu and page heading disagree in three places: **My Development** vs **Professional Development**, **Positions** vs **Position**, and **School Insights** vs **School Reports**.
7. **Pages without the "?" button.** Do sign-in, account setup, and survey response pages need content now (FAQs or Help Center), or later?
8. **Survey Preview** (`/surveys/:id`) has no access check of its own. Who can reach it and from where?
9. **1:1 Reviews for owners.** The list shows reviews the signed-in person is part of. Do owners see more than their own reviews here?
10. **Existing Ask Bloom content.** Bloom already stores per-page help text for the **Ask Bloom** chatbot (`page_context_documents`, keys in `client/src/constants/chatbot.ts`). Should help articles stay consistent with it, replace it, or ignore it? It is not treated as a source of truth here.
11. **Owners as staff.** Owners also see **My Surveys** and **My Development**. Are owners surveyed and given PD like staff?
