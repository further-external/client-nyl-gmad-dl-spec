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

## Today vs recommended

**You asked to keep today's page names wherever a current equivalent exists** (2026-10-25), so
your segments, dashboards and history carry across the change of template. The recommended names
apply where there is no equivalent, or where you choose them. The table is completed from the
current page names in Adobe (page name, eVar1).

| Page name today | Recommended `page.pageName` | Note |
|---|---|---|
| `contact` | `application\|contact` | The Contact page; feeds Contact (event21) |
| `signature` | `application\|docusign` | The Read & Sign page; feeds Signature (event39) |
| (the rest, from the current page names) | | |

Three Executive segments key on page names today. You asked to keep today's names for them where
an equivalent exists; otherwise they are rebuilt on these:

| Segment | Recommended page name |
|---|---|
| Page Name: Resume Email Confirmation | `resume\|resume` (the email step) |
| Page Name: Resume OTP Request | `resume\|resume-method` (choosing text or call) |
| Page Name: Resume OTP Validation | `resume\|resume-code` (entering the code) |

The MOAA "thank-you page" segment becomes `confirmation\|receipt`, or the
[Application Completed](../events/application-completed.md) event.
