# Global context and metrics

[Back to the overview](../README.md)

Two things in one place: the details Adobe Launch attaches to every hit, and the Adobe metric each
event sets. The detail behind each event is on its own page.

## What every hit carries (enrichment)

Developers send the page and visitor details **once per page view**, on
[Page Load Started](../events/page-load-started.md) and [User Detected](../events/user-detected.md).
Adobe Launch keeps the latest values and attaches them, in one place, to every hit of that page
view: the page view itself and every hit sent after it. They are never repeated on the other
events.

| Detail | Sent on | What it is | Feeds in Adobe | Use |
|---|---|---|---|---|
| `page.pageName` | Page Load Started | Which page or step | Page; pageName (eVar1) | Your reports |
| `page.pageId` | Page Load Started | The page's fixed id | Page; the step and page segments | Your reports |
| `page.association` | Page Load Started | Which association's portal | Association (eVar8) | Your reports |
| `page.url` | Page Load Started | The page address, with personal details removed ([Privacy](../privacy.md)) | Entry Page URL (prop2); the page URL on every hit | Your reports |
| `page.host` | Page Load Started | The portal's web address, kept separate from the association | Server; the browser exclusion segment | Your reports |
| `page.experience` | Page Load Started | The landing version and layout | allocated on review | Recommended |
| `page.environment` | Page Load Started | The build and the site, for example `production \| New Experience Site`, so the new portal is told apart from the legacy site ([decision 25](../open-questions.md)) | allocated on review | Recommended |
| `user.loginStatus`, `user.userType` | User Detected | Consumer or advisor, and whether their identity was checked | the login segments; advisor reporting | Recommended |

**Not on every hit:** the campaign code (`page.campaign`) is recorded on the first page of a visit
only (Tracking Code), and Adobe carries it from there. Repeating it would count it more than once.

## Metrics by event

"With the page view" means the event travels on the page view; "its own hit" means it is sent when
it happens. The details above ride both.

| Event | Action or condition | Sent as | Adobe metric or variable | Use |
|---|---|---|---|---|
| Page Load Started | every page view | with the page view | Page, pageName (eVar1), Association (eVar8), Server, Entry Page URL (prop2); Tracking Code on the first page only | Your reports |
| User Detected | every page view | with the page view | none of your report components | Recommended |
| Page Load Completed | every page view | sends the page view | Page Views, one per page view | Your reports |
| Click Interaction | `contact` | its own hit | Contact (event21), possibly ([decision 4](../open-questions.md)) | Your reports, needs your decision |
| Click Interaction | every other type | its own hit | the click variables (link type, text, position, window, file) | Recommended |
| Form Activity | `viewed`, `step-viewed`, `error` | with the page view (`viewed`, `step-viewed`); its own hit (`error`) | none of your report components | Recommended |
| Form Activity | `field-completed` | data layer only, not sent yet ([decision 9](../open-questions.md)) | none | Recommended |
| Login Activity | `succeeded` | its own hit | Login success (event2), possibly ([decision 3](../open-questions.md)) | Your reports, needs your decision |
| Login Activity | `started`, `failed`, `logged-out` | its own hit | none of your report components | Recommended |
| Application Started | once per application | its own hit | Form Start (event12) | Your reports |
| Application Activity | `step-started` on Eligibility, Coverage, Review | with the page view | Eligibility (event16), Coverage (event19), Preview (event22), every view | Your reports |
| Application Activity | `step-started` on Contact, Signature, Payment | with the page view | Contact (event21), Signature (event39), Order Page (event44), possibly ([decision 4](../open-questions.md)) | Your reports, needs your decision |
| Application Activity | `products-viewed`, `product-added` | its own hit | Product (product views, cart additions) | Your reports |
| Application Activity | `product-removed` | its own hit | Product (cart removals) | Recommended |
| Application Activity | `esign-started` | its own hit | Signature (event39), possibly ([decision 4](../open-questions.md)) | Your reports, needs your decision |
| Application Activity | `advisor-confirmation-sent` | with the page view | Advisor Confirmation (event79) ([decision 7](../open-questions.md)) | Your reports |
| Application Activity | `quote-started`, `saved`, `knocked-out`, `error`, `resumed` | its own hit (`resumed`: with the page view) | none of your report components | Recommended |
| Application Activity | `field-completed`, `step-completed` | data layer only, not sent yet ([decision 9](../open-questions.md)) | none | Recommended |
| Application Completed | once per receipt number | with the page view | Application Submit (event77), Product; orders and revenue | Your reports; orders and revenue Recommended |

The 41 report components, and what feeds each one, are on [Reports](reports.md).
