# Adobe variables

[Back to the overview](../README.md)

Which Adobe Analytics variables (eVars, props and events) the new data layer will fill. The goal:
your two reports keep their history, and nothing new collides with a variable already in use.

**Status: proposed (Further, 2026-11-18), for review.** Nothing has been configured in Adobe yet.
Each data layer value goes into one of three groups:

1. **Keep:** the variables your two reports read today. They stay where they are.
2. **Reuse:** an older variable that held the same thing. Its history continues.
3. **New:** a free variable, for data the current portal never sent.

## What we checked

The report suite **GMAD Launch Prod**, 2026-10-01 to 2026-10-31, through the Adobe Analytics API.
We read variable names and counts only, never values.

| Type | Enabled | With data in October | Without data |
|---|---|---|---|
| eVars | 67 | 63 | 4 (eVar36, 59, 69, 71) |
| Props | 14 | 14 | 0 |
| Events | 79 | 61 (11 of them with fewer than 100) | 18 |

**Free (not enabled):** eVar15, 16, 19, 20, 22, 32, 37 to 45, 47 to 50, 60, 64 to 66, 73 to 75, 78,
89, 90; prop8, 9, 11; event4, 11, 27 to 38, 47 to 49, 60, 74 to 76. Everything above eVar96,
prop17 and event100 is also free.

The MOAA Monthly Workspace reads a different report suite, **MOAA Launch Prod**, which we have not
checked yet. See [To confirm](#to-confirm).

## 1. Keep: what your two reports read

All 15 had data in October 2026. Details per component: [Reports](reports.md).

| Variable | Today | Filled by |
|---|---|---|
| eVar1 | Page name | `page.pageName` on [Page Load Started](../events/page-load-started.md) |
| eVar6 | Platform (MOAA Device Type panel) | Nothing in the data layer yet ([decision 5](../open-questions.md)) |
| eVar7 | New/Repeat Visitor | Proposed: Adobe's own Visit Number instead, a report change |
| eVar8 | Association | `page.association` |
| prop2 | Page URL (the reports use its entry value) | `page.url` |
| Tracking Code | Campaign | `page.campaign`, first view only |
| Products | Product | `application.product[]`: the G-number, quantity 1, no price |
| Server | Server | `page.host` |
| event2 | Login success | [Login Activity](../events/login-activity.md) `succeeded` |
| event12 | Form Start | [Application Started](../events/application-started.md) |
| event16, 19, 21, 22, 39 | Eligibility, Coverage, Contact, Preview, Signature | [Application Activity](../events/application-activity.md) `step-started` on that page |
| event44 | Order Page | [Application Completed](../events/application-completed.md) |
| event77 | Application Submit | Application Completed, once per receipt number |
| event79 | Advisor Confirmation | Application Activity `advisor-confirmation-sent` ([decision 7](../open-questions.md)) |

## 2. Reuse: an older variable with the same meaning

These variables already exist and hold the same thing today, so their history continues.

| Variable | Today | Filled by |
|---|---|---|
| prop1 | Page Name | `page.pageName` (for path reports) |
| eVar2 | Page URL | `page.url` |
| eVar3, prop3 | Page Type | `page.pageType` |
| eVar9, 10, 11 | Link Type, Link Text, Link Position | [Click Interaction](../events/click-interaction.md): type, label, page area |
| eVar12 | Modal Name | Click Interaction: the window's id, when it opens or closes |
| eVar17, 18 | Form Name, Form Type | `form.name`, `form.category` (engagement forms) |
| eVar23 | Form Step | `application.step.number` |
| eVar25 | Quick Decision Available | `quickDecisionEligible`, per product |
| eVar26 | File name | Click Interaction `download`: the file's fixed label |
| eVar34 | Login Status | `user.loginStatus` |
| eVar36 | Modal Closed Type (empty today) | Click Interaction `modal-close`: how it was closed |
| eVar46 | Order ID | `application.receiptId` |
| eVar58 | User Type | `user.userType` |
| eVar70 | Experience | `page.experience` |
| event1 | Login initiated | Login Activity `started` |
| event3 | CTA Link Clicked | Click Interaction, every type below not listed separately |
| event5 | Download | Click Interaction `download` |
| event8, event9 | Modal View, Modal Closed | Click Interaction `modal-open`, `modal-close` |
| event45 | DocuSign initiate | Application Activity `esign-started` |
| event66 | Quote Start | Application Activity `quote-started` |
| event98 | Application Saved | Application Activity `saved` |
| Product views, cart add, cart remove, purchase | Adobe's own commerce events | `products-viewed`, `product-added`, `product-removed`; Application Completed is the purchase, with no revenue |

## 3. New: free variables for new data

| Variable | New data | Why it helps |
|---|---|---|
| eVar15 | `page.environment` | Compares the new template with the current one ([decision 25](../open-questions.md)) |
| eVar16 | `application.channel` | Self-service or advisor-assisted applications |
| eVar19 | `application.knockoutReason` | Why applicants are stopped: not a member, eligibility not met |
| eVar20, eVar22 | `application.error.type`, `application.error.code` | Which errors block applicants, never the message text |
| eVar37 | `interaction.name` | Which control was used, by its fixed id |
| eVar38, eVar39 | `login.method`, `login.failureReason` | How people log in or resume, and why it fails |
| event4 | Application Activity `resumed` | Saved applications that come back |
| event11 | Application Activity `knocked-out` | How often a knockout message shows |
| event27 | Application Activity `error` | Blocked attempts, by error type and code |
| event28 | Login Activity `failed` | Failed logins and resume codes |
| event29, 30, 31 | [Form Viewed](../events/form-viewed.md), [Form Started](../events/form-started.md), [Form Completed](../events/form-completed.md) | Engagement forms, such as the dividend notice. Kept apart from Form Start (event12), which is the application |
| event32, event33 | Click Interaction `print`, `contact` | Print, and Chat now, Schedule a call, phone and email links |

"Step completed" stays in the data layer only until [decision 9](../open-questions.md) is
answered.

## 4. Left empty by the new portal

- **Never filled (privacy):** eVar14, 21, 24, 27 and 29 (form fields and page sections), eVar77 (advisor
  code), eVar82 and 83 (IP address, user agent), eVar91 to 96 (applicant age, gender, income, city,
  state, ZIP). We recommend removing any rule that still fills them before the new build goes
  live ([Privacy](../privacy.md)).
- **No longer needed:** every other variable not listed on this page. It keeps its history; the
  new portal simply stops sending to it.

## To confirm

1. **MOAA Launch Prod:** is it a virtual report suite on top of GMAD Launch Prod (the same
   variables), or a report suite of its own?
2. **Platform (eVar6):** the MOAA Device Type panel splits on it. Keep it (filled by Adobe Launch
   from the browser), or switch the panel to Adobe's own Mobile Device Type ([decision 5](../open-questions.md))?
3. **What else your reports use:** the January 2026 Executive report also shows a User Type quick
   segment (eVar58), and "QD Portal App Completion" and "advisor" segments. We will read them in
   Workspace to confirm which variables they need.
4. **Before reusing a variable,** we list the segments and calculated metrics that use it, so
   nothing you rely on changes meaning.
