# Click elements

[Back to the overview](../README.md) · Recommended, a general guide to share with your teams

Which links and buttons are worth tracking with [Click Interaction](../events/click-interaction.md),
and how to name them. The approach agreed on October 13 is **targeted**: track the elements that
answer a business question, not every click on the page. The elements already specified for the
new portal are in the [tracked controls list](../events/click-interaction.md#tracked-controls).

## Worth tracking

| Category | Examples | `interaction.type` | `element.type` | Name pattern |
|---|---|---|---|---|
| Contact and help | Chat now, Schedule a call, Call for help, email links | `contact` | `button`, `link` | what it does: `chat-now`, `schedule-call`, `call-for-help` |
| Primary calls to action | the button that ends a quote drawer or popover, such as Continue | `cta` | `button`, `link` | the action: `continue`, `learn-more` |
| Navigation | the buttons and links inside the quote drawers and popovers, the menu button, Continue Application in the menu, the logo, main menu links | `navigation` | `button`, `link` | `menu`, `logo`, `continue-application`, the target page: `nav-about-coverage` |
| Information windows | How Applying Works, About Coverage, the Needs Calculator | `modal-open`, `modal-close` | `button`, `backdrop`, `key` | the window: `how-applying-works` |
| Documents | Application PDF, Payment PDF, a plan brochure | `download` | `button`, `link` | the document: `application-pdf` |
| Print | Print on the Review page | `print` | `button` | the page: `print-review` |
| Sections and tabs | product category tabs, expandable sections | `tab`, `accordion` | `tab`, `accordion` | the group: `product-category-tabs` |
| Links that leave the portal | the Privacy Notice, the association's website | `click` | `link` | the destination: `privacy-notice`, `association-website` |
| Footer links | terms, accessibility, contact us | `click` | `link` | `footer-` plus the topic: `footer-terms` |
| Product links | product cards and product pages | `click` | `link` | `product-` plus the product category: `product-term-life` |
| Helper and eligibility chips | every helper chip on any page, such as the three on Membership; never a chip that adds a spouse or a child | `click` | `button` | `helper-chip`, with the chip's id as the detail: `coverage-options` |

## Not worth tracking

- **Form fields** of any kind: there is no field-level tracking (agreed on 2026-10-23). Forms are
  recorded as a whole, by the [form events](../events/form-viewed.md) and the application events.
- **Controls whose result is already recorded:** Begin application, Next and Back, Add coverage,
  See my quote, Apply for coverage in the quote drawer. The result is the event: a drawer button
  that commits products is recorded as Application Activity `product-added` only.
- **Household controls** (Add Spouse, Add Child): household make-up is never recorded.
- **Messages the system shows by itself:** snackbars, the cookie banner.
- **Anything "just in case":** an element without a question behind it adds load and noise.

## Naming rules

- **`interaction.name`** is a fixed id in lower-case words joined by hyphens, the same on every page
  and every association. The page comes from the page details, so it is never in the name.
- **`element.text`** is the label written in the code, never read from the screen, and empty when
  there is none.
- **`element.region`** is one of `header`, `hero`, `body`, `drawer`, `modal`, `footer`.
- **No personal data in any value:** no names, amounts, receipt or policy numbers.

## Adding an element

1. Name the business question it answers.
2. Pick its `interaction.type`, `element.type` and a name following the rules above.
3. Send it for review, so the list stays consistent; once agreed it joins the tracked controls list.
