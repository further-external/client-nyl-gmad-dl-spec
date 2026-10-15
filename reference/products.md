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
submitted products directly, with their benefit amount and price.

## Which actions carry products

| Action | `application.product` holds | Sent to Adobe as products |
|---|---|---|
| `products-viewed` | every product shown: quote results, or the coverage product cards | yes |
| `product-added` | the products added since the last commit (a Next on Coverage, or "Apply for coverage" in the quote drawer) | yes |
| `product-removed` | the product whose removal was confirmed | yes |
| `step-started` | the cart at that moment, or `[]` before coverage is chosen | no: for context in the data layer |
| Application Completed | every product in the submitted application | yes, with its price (revenue) |
| any other action | `[]` | no |

Application Started carries no products.

## One item

```json
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
}
```

| Property | Required | What it holds | Example | Use |
|---|---|---|---|---|
| `productInfo.productID` | yes | A stable product id, not the display name. **Proposed: the G-number** ([decision 14](../open-questions.md)) | `G-4120-1` | Reports |
| `productInfo.name` | yes | The product name as configured, the same for every association | `Group Term Life` | Recommended |
| `productInfo.category` | yes | The product category | `term-life`, `disability` | Recommended |
| `productInfo.underwritingClass` | yes, or `null` | The product's underwriting class | `si`, `gi` | Recommended |
| `productInfo.benefitAmount` | yes, or `null` | The coverage or benefit amount selected for this product | `100000` | Recommended |
| `productInfo.price.basePrice` | yes, or `null` | The product's base price | `45.00` | Recommended |
| `productInfo.price.sellingPrice` | yes, or `null` | The price charged; the revenue on Application Completed | `39.99` | Recommended |
| `quantity` | yes | **Always `1`.** It never counts people, which would reveal household size | `1` | Recommended |
| `quickDecisionEligible` | yes | Whether the product offers a quick decision | `true`, `false` | Recommended |

**Never in a product item:** anything about the applicants, including who a product covers or how
many people. A product's price and the selected benefit amount describe the product, not the
person, and were agreed at the October 13 review.

## How Adobe Launch sends products

For the tag build; developers have nothing to do here.

- Products are written to the Adobe products string only on products viewed, product added,
  product removed and Application Completed, one entry per item.
- **On Application Completed** the entry carries the price: `;G-4120-1;1;39.99;event<n>=45.00;eVar25=true`
  (no category, the product id, quantity 1, the selling price as revenue, the base price as a
  currency event, then the quick-decision flag). The purchase uses the receipt number as its id,
  so it is counted once, however many products it holds.
- On the other events the price slot stays empty.
