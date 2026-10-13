# Decisions we need

[Back to the overview](README.md)

These questions can only be answered by New York Life. Until each is answered, the specification
carries our proposal, clearly labelled, so the build does not wait. Each answer is folded into the
next version.

## Business and reporting

| # | Question | Why it matters | Who answers |
|---|---|---|---|
| 1 | **Is the current portal retired for every association when the new one goes live?** Does it run alongside the new one during the rollout? Does any other GMAD site stay on the current setup? | Decides whether reports must combine two sites, and how history compares with the new data | NYL business owners |
| 2 | **Is MOAA moving to the new portal, and under which association id?** MOAA is not among the associations configured in the final design | One of the two reports is the MOAA Monthly Workspace | NYL business owners |
| 3 | **What did "login success" count on the current portals?** On the new portal, consumers do not log in: they resume with their email and a code. Only advisors log in. The candidate is [Login Activity](events/login-activity.md) "succeeded": advisor logins, consumer resume checks, or both | Feeds the login success count and two Executive segments ([Reports](reference/reports.md)) | NYL business owners |
| 4 | **What did Contact, Signature and Order Page each count?** The new design narrows each to candidates. **Contact:** there is no contact form, so contact means Chat now, Schedule a call, phone and email links. **Signature:** reaching the Signature page, leaving for DocuSign, or a completed signature (only visible on the receipt). **Order Page:** no page is called "order"; the candidates are Payment and the receipt | Three of the ten counts in the reports | NYL business owners |
| 5 | **What did Platform hold, and what format did page name, association and server use?** | So the new values line up with the history | NYL, or a read of the current reports |
| 6 | **Is the `campaign` address parameter the production campaign code?** | Feeds the tracking code | NYL marketing |
| 7 | **Who owns the advisor flow?** Three details wait on that owner: what the advisor portal's Continue does; whether the "Application sent" page shows an application id, so a reload is not counted twice; and whether the applicant's review needs the email and code check | Feeds Advisor Confirmation | NYL |
| 8 | **Do you approve the [form field list](reference/form-fields.md)?** | It is the only list of field names forms may report | NYL |
| 9 | **May we send "field completed" and "step completed" to Adobe?** Each adds server calls. Until then they stay in the data layer only | Form drop-off and step completion analysis | NYL |
| 10 | **Does "implemented as specified" in the statement of work cover the Recommended items, or only those your reports need?** | Sets what the build and its testing must include | Further and NYL, at the knowledge transfer |
| 11 | **What did the "All sites except GI and SI" segment filter on: an underwriting class, or a site?** The new design has both. We will read the segment definition in Adobe and confirm it with you | Rebuilding that Executive segment | Further, then NYL to confirm |

## For the developers

| # | Question | Why it matters | Who answers |
|---|---|---|---|
| 12 | **Which data layer helper library will the build use?** We propose a Data Layer Manager style helper. The tag manager setup uses Adobe Launch with the Adobe Analytics extension | How Adobe Launch reads the events | NYL developers |
| 13 | **Where does the production site read the association from**: the `association` address parameter, its configuration, or the host name? The design's `client` parameter is for the prototype only | Feeds the Association dimension | NYL developers |
| 14 | **Is the G-number the product identifier of record?** | Feeds the Product dimension | NYL developers |
| 15 | **What is the production receipt number format?** The design shows `APP-` plus 8 digits | Counts each application once, and deduplicates the purchase | NYL developers |
| 16 | **Is the route table final** (page ids, page types, stages)? Also: the routes of the health pages the design does not show yet (Whole Life, CI, QD CIR, DI SUPP), the single-column layout, where two windows open (`send-to-applicant`, `tpa-member-verification`), and where the landing page's product links go | Page names and steps in every report | NYL developers |
| 17 | **Does the production Eligibility step stop applicants by age or state?** None was seen in the final design | Whether a second knockout reason is needed | NYL |
| 18 | **In production, does DocuSign return the applicant to the receipt page on the portal?** | The receipt is the only sign that signing was completed | NYL developers |
| 19 | **Which error codes do the production resume, advisor login and application services return?** For example, whether resume says "no saved application for this email" | The error codes in Form Activity and the failure reasons in Login Activity | NYL developers |
| 20 | **What user identifiers does the current portal pass** (in its data layer, cookies or Adobe variables)? **Do you approve an anonymous id issued by your own systems** for logged-in advisors and verified consumers? Until then, `user.id` and `login.id` stay empty | Whether logins can be followed across visits, without identifying anyone | NYL, with its privacy owner |
| 21 | **Which id should follow a person across devices** (for example from a computer to a phone) before the application is submitted, if you want cross-device tracking? One candidate is a hashed email, if you collect the email today. It needs a follow-up conversation and your privacy approval, and follows the same rules as decision 20 | Cross-device journeys | NYL |
| 22 | **Does the current template's field-level tracking carry over to the new template?** Today clicks are tagged with data attributes. For the new portal we recommend explicit data layer pushes ([Click Interaction](events/click-interaction.md), [Form Activity](events/form-activity.md)), so nothing depends on carried-over attributes | Avoids gaps or double tracking | NYL developers |
| 23 | **What is the list of application names?** `applicationName` names the specific application being filled out (for example term life on the WAEPA portal) | Feeds the application funnel breakdowns | NYL developers |
| 24 | **Is a product's price monthly or annual, and what is the base price versus the selling price** (for example a member discount)? | Revenue and price reporting on Application Completed | NYL |
