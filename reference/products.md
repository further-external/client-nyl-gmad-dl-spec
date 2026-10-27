# Products

[Back to the overview](../README.md) · Needed for your reports

The products a visitor sees, adds, removes or applies for are described in a list inside the
application object: `application.product`. Only [Application Activity](../events/application-activity.md)
carries it.

## Why a list

An applicant can choose several products in one application, so each event lists every product it
is about. **One item per product, never one per person:** covering a spouse or children adds no
item and changes nothing in an item.

**Every item has the same shape on every event.** That lets the reports compare viewed, added and
submitted products directly.

## Which actions carry products

| Action | `application.product` holds | Sent to Adobe as products |
|---|---|---|
| `products-viewed` | every product shown: quote results, or the coverage product cards | yes |
| `product-added` | the products added since the last commit (a Next on Coverage, or "Apply for coverage" in the quote drawer) | yes |
| `product-removed` | the product whose removal was confirmed | yes |
| `step-started` | the cart at that moment, or `[]` before coverage is chosen | no: for context in the data layer |
| Application Completed | every product in the submitted application | yes, as the purchase (orders, no price) |
| any other action | `[]` | no |

Application Started carries no products.

## One item

```json
{
  "productInfo": {
    "productID": "G-4120-1",
    "name": "Group Term Life",
    "category": "term-life",
    "underwritingClass": "si"
  },
  "quantity": 1,
  "quickDecisionEligible": true
}
```

| Property | Required | What it holds | Example | Use |
|---|---|---|---|---|
| `productInfo.productID` | yes | A stable product id, not the display name. **Proposed: the G-number** ([decision 14](../open-questions.md)) | `G-4120-1` | Reports |
| `productInfo.name` | yes | The product name as configured, the same for every association | `Group Term Life` | Recommended |
| `productInfo.category` | yes | The product category | `term-life`, `disability` | Recommended |
| `productInfo.underwritingClass` | yes, or `null` | The product's underwriting class | `si`, `gi` | Recommended |
| `quantity` | yes | **Always `1`.** It never counts people, which would reveal household size | `1` | Recommended |
| `quickDecisionEligible` | yes | Whether the product offers a quick decision | `true`, `false` | Recommended |

**Never in a product item:** anything about the applicants, including who a product covers or how
many people, and no price or benefit amount: they come from the applicant's own answers
(agreed at the 2026-10-27 office hours, replacing the October 13 agreement;
[Privacy](../privacy.md)).

## How Adobe Launch sends products

For the tag build; developers have nothing to do here.

- Products are written to the Adobe products string only on products viewed, product added,
  product removed and Application Completed, one entry per item.
- **On Application Completed** each entry is `;G-4120-1;1;;eVar25=true` (no category, the product
  id, quantity 1, an empty price slot, then the quick-decision flag). The purchase uses the receipt
  number as its id,
  so it is counted once, however many products it holds.
- The price slot is always empty: no revenue is recorded.
