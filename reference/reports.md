# Reports

[Back to the overview](../README.md)

The scope of this specification is the data behind two Adobe Analytics reports: the **MOAA Monthly
Workspace** ("MOAA" below) and the **Portal Analytics Executive Report | 2026** ("Executive").
Together they use **41 distinct components**. This page lists each one and what will feed it on the
new portal.

The 41 components are what these two reports need. They are not every question the business may
want to answer later.

## Counts (10)

| Component | Report | Fed by |
|---|---|---|
| Login success (event2) | both | **Needs your decision** ([decision 3](../open-questions.md)): consumers do not log in on the new portal. Candidate: [Login Activity](../events/login-activity.md) `succeeded`, for advisor logins, consumer resume checks, or both |
| Form Start (event12) | both | [Application Started](../events/application-started.md), once per application |
| Eligibility (event16) | both | Application Activity `step-viewed` on Eligibility, every view. **Proposed** |
| Coverage (event19) | both | Application Activity `step-viewed` on Coverage, every view. **Proposed** |
| Contact (event21) | both | **Needs your decision** ([decision 4](../open-questions.md)): the Contact page, or contact clicks ([Click Interaction](../events/click-interaction.md) `contact`) |
| Preview (event22) | MOAA | Application Activity `step-viewed` on Review, every view. **Proposed** |
| Signature (event39) | both | **Needs your decision** ([decision 4](../open-questions.md)): reaching the Signature page, leaving for DocuSign, or the receipt |
| Order Page (event44) | both | **Needs your decision** ([decision 4](../open-questions.md)): the Payment page or the receipt; no page is called "order" |
| Application Submit (event77) | both | [Application Completed](../events/application-completed.md), once per receipt number |
| Advisor Confirmation (event79) | Executive | Application Activity `advisor-confirmation-sent` ([decision 7](../open-questions.md)) |

**Also recorded, beyond your two reports (Recommended):** orders, revenue and average order value from
Application Completed, by product and channel.

## Dimensions (9)

| Component | Report | Fed by |
|---|---|---|
| Page name (eVar1) | Executive | `page.pageName` on [Page Load Started](../events/page-load-started.md) ([decision 5](../open-questions.md)) |
| Platform (eVar6) | MOAA | **Needs your decision** ([decision 5](../open-questions.md)): nothing on the new portal holds it; it can be derived by Adobe, or retired |
| New or repeat visitor (eVar7) | MOAA | **Proposed:** Adobe's own Visit Number, a report change with no site work |
| Association (eVar8) | Executive | `page.association` ([decision 13](../open-questions.md)) |
| Page | MOAA | Page Load Started, every page view |
| Product | MOAA | `application.product`, written by Adobe Launch into the products string ([Products](products.md)) |
| Server | Executive | `page.host`, kept separate from the association |
| Entry page address (prop2) | MOAA | `page.url` |
| Tracking code | MOAA | `page.campaign`, on the first page of a visit ([decision 6](../open-questions.md)) |

## Provided by Adobe, no site work (13)

Unique Visitors, Visits, Browser, Browser Type, Day, Month, Quarter, Countries, Regions, Operating
Systems, Referring Domain, Day of Week, Hour of Day.

## Segments, rebuilt in Adobe on the new data (9)

| Segment | Report |
|---|---|
| Thank-you page | MOAA |
| All sites except GI and SI ([decision 11](../open-questions.md)) | Executive |
| Exclude bot traffic from mybarbenefits.com | Executive |
| Exclude outdated browsers | Executive |
| Page Name: Resume OTP Validation | Executive |
| Page Name: Resume Email Confirmation | Executive |
| Page Name: Resume OTP Request | Executive |
| Login Success Users ([decision 3](../open-questions.md)) | Executive |
| App Submit after Login, same visit ([decision 3](../open-questions.md)) | Executive |
