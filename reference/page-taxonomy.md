# Page taxonomy

[Back to the overview](../README.md) · Proposed, needs your approval ([decisions 5 and 16](../open-questions.md))

How every page and step of the new portal is named in the data layer and in Adobe Analytics. The
names come from the portal's route table, so they stay the same on every association and do not
change when the page's wording changes.

## The structure

Each page view sends three names on [Page Load Started](../events/page-load-started.md):

| Name | What it is | Example |
|---|---|---|
| `page.pageId` | The page's id: the route, never the title on screen | `eligibility` |
| `page.pageType` | The group the page belongs to | `application` |
| `page.pageName` | The reporting name: page type, a pipe, then page id | `application\|eligibility` |

Application pages also send `page.stage`, the step group shown in the progress bar
(`getting-started`, `coverage-options`, `profile`, `application-review`, `esign-submit`).

## Naming rules

1. **Lower-case words joined by hyphens** (`resume-method`, `health-si`).
2. **The route, never the title.** Associations can change a page's wording; its name stays.
3. **The same name on every association.** The association is a separate detail
   (`page.association`), so reports can compare portals page by page.
4. **One name per route.** A page that shows different content (for example a welcome-back
   landing) keeps its name; the difference goes in `page.experience`.
5. **No personal data and no ids** in a name: no names, emails, receipt or policy numbers.
6. **An unknown page is `error|not-found`**, never the address that was requested.
7. **A new page** takes its route id as `pageId`, joins an existing `pageType`, and gets its
   `pageName` the same way. Adding a page type is a change to this specification.

## The recommended names

| `page.pageId` | `page.pageType` | `page.stage` | `page.pageName` |
|---|---|---|---|
| `landing` | `home` | none | `home\|landing` |
| `membership` | `application` | `getting-started` | `application\|membership` |
| `eligibility` | `application` | `getting-started` | `application\|eligibility` |
| `coverage` | `application` | `coverage-options` | `application\|coverage` |
| `beneficiary` | `application` | `profile` | `application\|beneficiary` |
| `contact` | `application` | `profile` | `application\|contact` |
| `profile` | `application` | `profile` | `application\|profile` |
| `review` | `application` | `application-review` | `application\|review` |
| `health-si`, `health-li`, `health-qd`, `health-di`, `health-cir` | `application` | `application-review` | `application\|health-si` and so on |
| `payment` | `application` | `application-review` | `application\|payment` |
| `docusign` | `application` | `esign-submit` | `application\|docusign` |
| `receipt` | `confirmation` | none | `confirmation\|receipt` |
| `resume` | `resume` | none | `resume\|resume` |
| `resume-method` | `resume` | none | `resume\|resume-method` |
| `resume-code` | `resume` | none | `resume\|resume-code` |
| `advisor-login` | `advisor` | none | `advisor\|advisor-login` |
| `advisor-send-confirmation` | `advisor` | none | `advisor\|advisor-send-confirmation` |
| `application-edit-confirmation` | `advisor` | none | `advisor\|application-edit-confirmation` |
| `not-found` | `error` | none | `error\|not-found` |

The routes of the health pages the design does not show yet, and the production route table, are
[decision 16](../open-questions.md).

## Page names for the new template

**Agreed at the 2026-11-17 office hours:** the new template uses these names, not today's. Each
page reports on its own: "Profile" is only the profile page, and Beneficiary and Payment report
separately (today one "Profile" page covers all three). Today's names stay on the current
template until each site moves.

**The `gmad:` prefix is kept.** Every page name in Adobe starts with `gmad:` today. Adobe Launch
adds it in front of `page.pageName`, so the data layer sends the name without it and Adobe shows,
for example, `gmad:application|profile`.

| `page.pageName` (data layer) | Page name in Adobe | Today's page on the current template |
|---|---|---|
| `home\|landing` | `gmad:home\|landing` | |
| `application\|membership` | `gmad:application\|membership` | |
| `application\|eligibility` | `gmad:application\|eligibility` | |
| `application\|coverage` | `gmad:application\|coverage` | |
| `application\|beneficiary` | `gmad:application\|beneficiary` | part of Profile |
| `application\|contact` | `gmad:application\|contact` | `contact` |
| `application\|profile` | `gmad:application\|profile` | part of Profile |
| `application\|review` | `gmad:application\|review` | |
| `application\|health-si` and the other health pages | `gmad:application\|health-si` and so on | |
| `application\|payment` | `gmad:application\|payment` | part of Profile |
| `application\|docusign` | `gmad:application\|docusign` | `signature` (Read & Sign) |
| `confirmation\|receipt` | `gmad:confirmation\|receipt` | the thank-you page |
| `resume\|resume` | `gmad:resume\|resume` | (segment: Resume Email Confirmation) |
| `resume\|resume-method` | `gmad:resume\|resume-method` | (segment: Resume OTP Request) |
| `resume\|resume-code` | `gmad:resume\|resume-code` | (segment: Resume OTP Validation) |
| `advisor\|advisor-login` | `gmad:advisor\|advisor-login` | |
| `advisor\|advisor-send-confirmation` | `gmad:advisor\|advisor-send-confirmation` | |
| `advisor\|application-edit-confirmation` | `gmad:advisor\|application-edit-confirmation` | |
| `error\|not-found` | `gmad:error\|not-found` | |

Please confirm or correct the names, and fill the last column where today's page is known (the
page id to page name mapping, [decision 16](../open-questions.md)). Blank means not filled yet.

**Error pages (later, not needed for the first release).** The new template adds an error code
page and a session expired page. Each gets page-level tracking like any other page, under the
`error` type, for example `error|session-expired`, once the Adobe Launch embed is in those
pages' head (your developers). The final names follow their routes. The maintenance page is your
vendor's and is not tracked.

**Comparing the two templates.** Because the names differ, a comparison of the current and new
templates is built on purpose: segment on Environment (`page.environment`), which tells the new
portal from the current site ([decision 25](../open-questions.md)), and map today's pages to the
new ones with the table above.

## Today vs recommended

The earlier approach, kept for the record: on 2026-10-25 you asked to keep today's page names
wherever a current equivalent exists. **The 2026-11-17 decision above replaces it for the new
template.** Three Executive segments key on the resume page names today and are rebuilt on the
new names (2026-10-27):

| Segment | New page name |
|---|---|
| Page Name: Resume Email Confirmation | `resume\|resume` (the email step) |
| Page Name: Resume OTP Request | `resume\|resume-method` (choosing text or call) |
| Page Name: Resume OTP Validation | `resume\|resume-code` (entering the code) |

The MOAA "thank-you page" segment becomes `confirmation\|receipt`, or the
[Application Completed](../events/application-completed.md) event.
