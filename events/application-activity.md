# Application Activity

**Event name:** `Application Activity` · Needed for your reports

[Back to the overview](../README.md)

## What it answers

- How many visitors see products and add them to the cart.
- How many reach each application step, and where they drop off.
- Which products are viewed, added and removed.
- How many are stopped at Membership, how many save, how many resume, how many leave to sign.
- How many advisor-prepared applications are sent.

The whole journey, page by page, is on [The application journey](../application-journey.md).

## When to fire it

- **Fire this event when the visitor makes progress in the quote or the application** (the
  [actions below](#actions)), when the result happens, not on the click: products viewed when the
  results arrive.
- **Fire it on every experience the same way**, with or without the instant quote panel
  ([the rule](../how-it-works.md#which-event-fires-where-the-two-landing-experiences)).
- **Fire the page-view actions** (`step-viewed`, `resumed`, `advisor-confirmation-sent`) between
  User Detected and Page Load Completed, so they travel with the page view. Fire the others when
  they happen, each as its own hit.
- **Never repeat a once-only action on a reload:** remember `saved` for the application.
- **Do not fire it for:** the Begin application click, a Next that fails validation (that is a
  [Form Activity](form-activity.md) error), messages the system shows by itself, a screen
  refreshing, answers on the profile, health, payment or beneficiary pages, or the clicks listed
  under [Interaction](interaction.md).
- **Never send PII or an applicant's answers** — no names, contact details, SSNs, dates of birth,
  health responses, or personal financial figures. Product-level values (a product's price and the
  selected coverage amount) are product information, not PII, and are sent. See [Privacy](../privacy.md).

## Actions

`application.action` says what happened. These are all the actions expected:

| `application.action` | Fire it when | With the page view or its own hit | Use |
|---|---|---|---|
| `products-viewed` | "See my quote" (drawer or instant quote panel) or "See my coverage options" returns results. Once per search. Holds every product returned | its own hit | Your reports: Product |
| `product-added` | A product enters the cart after Add or Add coverage. Holds the added product | its own hit | Your reports: Product |
| `product-removed` | Remove, in the coverage cart. Holds the removed product | its own hit | Recommended |
| `step-viewed` | Every view of an application page, Membership to Signature, revisits included (marked as a repeat) | with the page view | Your reports: Eligibility, Coverage, Preview (Proposed: every view counts) |
| `step-completed` | Next passes validation, Membership to Payment, just before the page changes | data layer only, not sent yet ([decision 9](../open-questions.md)) | Recommended, Proposed |
| `saved` | The first successful autosave, after Membership. Once per application | its own hit | Recommended |
| `knocked-out` | The first Membership answer is "not a member": the message shows and Next stays disabled. An eligibility rule is a Proposed second reason ([decision 17](../open-questions.md)) | its own hit | Recommended |
| `resumed` | The first application page after a resume (code accepted, or Continue Application on the welcome-back page) | with the page view | Recommended |
| `esign-started` | Next on the Signature page, as the visitor leaves for DocuSign | its own hit | Your reports: Signature, possibly ([decision 4](../open-questions.md)) |
| `advisor-confirmation-sent` | The advisor's "Application sent successfully" page ([decision 7](../open-questions.md)) | with the page view | Your reports: Advisor Confirmation |

## Example

One application page, Coverage:

```mermaid
sequenceDiagram
  participant UI as Portal
  participant DL as Data layer
  participant AA as Adobe Analytics
  rect rgba(80,140,255,0.12)
    Note over UI,AA: Coverage page view
    UI->>DL: Page Load Started, User Detected
    UI->>DL: Application Activity step viewed (coverage, 3)
    UI->>DL: Page Load Completed
    DL->>AA: page view, carrying the step
  end
  UI->>DL: products viewed (See my coverage options)
  DL->>AA: separate hit
  UI->>DL: product added (Add coverage)
  DL->>AA: separate hit
  UI->>DL: step completed (Next passes validation)
  Note over DL,AA: data layer only
```

The step travels with the page view. Each product action is its own hit when it happens. The
completed step stays in the data layer for now.

## Javascript code

```js
window.appEventData = window.appEventData || [];
window.appEventData.push({
  "event": "Application Activity",
  "application": {
    "action": "<action>",
    "channel": "<channel>",
    "applicationName": "<applicationName>",
    "step": {
      "name": "<stepName>",
      "group": "<stepGroup>",
      "number": "<stepNumber>",
      "repeat": "<stepRepeat>"
    },
    "knockoutReason": "<knockoutReason>",
    "product": [
      {
        "productInfo": {
          "productID": "<productID>",
          "name": "<productName>",
          "category": "<productCategory>",
          "underwritingClass": "<underwritingClass>",
          "benefitAmount": "<benefitAmount>",
          "price": {
            "basePrice": "<basePrice>",
            "sellingPrice": "<sellingPrice>"
          }
        },
        "quantity": "<quantity>",
        "quickDecisionEligible": "<quickDecisionEligible>"
      }
    ]
  }
});
```

## Populated example

The coverage page, a search that returns two products:

```js
window.appEventData = window.appEventData || [];
window.appEventData.push({
  "event": "Application Activity",
  "application": {
    "action": "products-viewed",
    "channel": "self-service",
    "applicationName": "waepa-term-life",
    "step": {
      "name": null,
      "group": null,
      "number": null,
      "repeat": null
    },
    "knockoutReason": null,
    "product": [
      {
        "productInfo": {
          "productID": "G-4120-1",
          "name": "Group Term Life",
          "category": "term-life",
          "underwritingClass": "si",
          "benefitAmount": 100000,
          "price": {
            "basePrice": 45.00,
            "sellingPrice": 39.99
          }
        },
        "quantity": 1,
        "quickDecisionEligible": true
      },
      {
        "productInfo": {
          "productID": "G-5210-1",
          "name": "Group Disability Income",
          "category": "disability",
          "underwritingClass": "di",
          "benefitAmount": 50000,
          "price": {
            "basePrice": 60.00,
            "sellingPrice": 52.50
          }
        },
        "quantity": 1,
        "quickDecisionEligible": false
      }
    ]
  }
});
```

## Variable definitions

All details are sent every time; a detail that does not apply is `null`, and `product` is `[]`
when no product is involved.

| Path | Type | Description | Example |
|---|---|---|---|
| `application.action` | string | Required. For your reports. What happened ([Actions](#actions)) | `products-viewed`, `product-added`, `step-viewed` |
| `application.channel` | string | Required. Recommended. Who is completing the application | `self-service`, `advisor-assisted` |
| `application.applicationName` | string | Required. For your reports. The application the visitor is completing | `waepa-term-life` |
| `application.step.name` | string or null | Required on step actions. For your reports. The application page | `membership`, `eligibility`, `coverage`, `review`, `payment`, `docusign` |
| `application.step.group` | string or null | Required with a step. Recommended. The progress stage | `getting-started`, `coverage-options`, `profile`, `application-review`, `esign-submit` |
| `application.step.number` | integer or null | Required with a step. Recommended. The step's position in this application's flow | `1`, `2`, `3` |
| `application.step.repeat` | boolean or null | Required with a step. Recommended. `true` when this step was already viewed for this application in this visit | `true`, `false` |
| `application.knockoutReason` | string or null | Required on `knocked-out`. Recommended. Why the applicant was stopped | `not-member`; `eligibility-not-met` (Proposed) |
| `application.product` | array | Required. For your reports. The products this event is about: the results, the product added or removed, or the cart on a step. See [Products](../reference/products.md) | a list, or `[]` |
| `application.product[n].productInfo.productID` | string | Required. For your reports. A stable product id. Proposed: the G-number ([decision 14](../open-questions.md)) | `G-4120-1` |
| `application.product[n].productInfo.name` | string | Required. Recommended. The product name as configured | `Group Term Life` |
| `application.product[n].productInfo.category` | string | Required. Recommended. The product category | `term-life`, `disability` |
| `application.product[n].productInfo.underwritingClass` | string or null | Required. Recommended. The product's underwriting class | `si`, `gi` |
| `application.product[n].productInfo.benefitAmount` | integer or null | Required. Recommended. The selected coverage / benefit amount for this product (product-level, not PII) | `100000` |
| `application.product[n].productInfo.price.basePrice` | number or null | Required. Recommended. The product's base price; carried as a currency event | `45.00` |
| `application.product[n].productInfo.price.sellingPrice` | number or null | Required. Recommended. The price charged; drives revenue attribution | `39.99` |
| `application.product[n].quantity` | integer | Required. Recommended. Always `1`: it never counts people | `1` |
| `application.product[n].quickDecisionEligible` | boolean | Required. Recommended. Whether the product offers a quick decision | `true`, `false` |

## How to check it

1. Walk one application with test details. Each step shows a step viewed between Page Load Started
   and Page Load Completed.
2. Reload the Eligibility page: step viewed again with `repeat: true`.
3. Run a coverage search: a products viewed hit fires on its own, holding every product returned;
   add one and a product added hit fires.
4. Each push has `event` and `application` only, and `application.product` is always a list.
