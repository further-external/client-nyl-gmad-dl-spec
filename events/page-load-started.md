# Page Load Started

[Back to the overview](../README.md) · Needed for your reports

**What it records:** a page view begins. It describes the page: which page or step, which
association, the address and the campaign. It is the first event of every page view.

## When to add it

- **Once per page view, first:** on the first page of a visit, and on every change of page or
  step inside the portal, even though no new web page loads.
- **After the router has settled on the final page.** If one address redirects to another, only
  the final page counts.
- **Not a page view:** a change to the address's `?` part or `#` part alone, or a drawer or window
  opening.

Then come [User Detected](user-detected.md), any events that describe the page, and
[Page Load Completed](page-load-completed.md) ([the sequence](../how-it-works.md#every-page-view-step-by-step)).

## Example

The visitor arrives at the WAEPA portal from an email campaign:

```js
window.appEventData = window.appEventData || [];
window.appEventData.push({
  "event": "Page Load Started",
  "page": {
    "pageName": "home|landing",
    "pageId": "landing",
    "pageType": "home",
    "association": "waepa",
    "experience": "hero-image|multi",
    "url": "https://waepa.nylinsure.com/?association=waepa&campaign=waepa-oe-2026-email&variant=hero-image",
    "host": "waepa.nylinsure.com",
    "campaign": "waepa-oe-2026-email",
    "stage": null,
    "loadType": "initial",
    "language": "en-us",
    "environment": "production"
  }
});
```

On the next step, the same object is sent again, complete: `loadType` becomes `virtual`, and
`campaign` becomes `null`.

## Properties

All 12 details are sent every time; a detail that does not apply is `null`.

| Property | Required | What it holds | Example | Use |
|---|---|---|---|---|
| `page.pageName` | yes | The page's reporting name: page type, then page id. **Proposed** ([decisions 5 and 16](../open-questions.md)) | `application\|eligibility` | Reports |
| `page.pageId` | yes | The page's id, from the route table ([decision 16](../open-questions.md)) | `eligibility`, `receipt` | Reports |
| `page.pageType` | yes | The page group (table below) | `application` | Reports |
| `page.association` | yes | The association whose portal is shown. Set by the site, never guessed from the address ([decisions 2 and 13](../open-questions.md)) | `waepa`, `abe` | Reports |
| `page.experience` | yes | The landing version and layout, kept for the whole visit | `hero-image\|multi` | Recommended |
| `page.url` | yes | The page's address, with every unlisted address parameter removed ([below](#how-the-address-is-cleaned)) | `https://waepa.nylinsure.com/eligibility` | Reports |
| `page.host` | yes | The portal's web address, lower case. Kept separate from the association, because existing segments rely on it | `waepa.nylinsure.com` | Reports |
| `page.campaign` | no | The `campaign` address parameter, on the first page of a visit only, at most 30 characters ([decision 6](../open-questions.md)) | `waepa-oe-2026-email` | Reports |
| `page.stage` | no | The application stage (table below); `null` outside the application | `getting-started` | Recommended |
| `page.loadType` | yes | `initial` for the first page of a page load (including a reload and the return from DocuSign); `virtual` for a change of step | `initial` | Recommended |
| `page.language` | no | The page's language | `en-us` | Recommended |
| `page.environment` | yes | Which build: production, staging or development | `production` | Recommended |

**Page type and stage by page.** Proposed until the production route table is confirmed
([decision 16](../open-questions.md)).

| Page id | Page type | Stage |
|---|---|---|
| `landing` | `home` | none |
| `membership`, `eligibility` | `application` | `getting-started` |
| `coverage` | `application` | `coverage-options` |
| `beneficiary`, `contact`, `profile` | `application` | `profile` |
| `review`, `health-si`, `health-li`, `health-qd`, `health-di`, `health-cir`, `payment` | `application` | `application-review` |
| `docusign` | `application` | `esign-submit` |
| `receipt` | `confirmation` | none |
| `resume`, `resume-method`, `resume-code` | `resume` | none |
| `advisor-login`, `advisor-send-confirmation`, `application-edit-confirmation` | `advisor` | none |
| `not-found` | `error` | none |

## How the address is cleaned

An address can carry personal details (a benefit amount, a token, an ad click id), so the site
cleans it before adding it:

- **Keep** the scheme, the host and the page's path, plus only these parameters: `association`,
  `campaign`, `URLClickedFrom`, `form`, `flow`, `variant`, `template`, `apply`, `prods`,
  `featured_prod`.
- **Remove** every other parameter and the `#` part. For an unknown page, the path becomes
  `/not-found`.

| Address in the browser | `page.url` |
|---|---|
| `https://waepa.nylinsure.com/?association=waepa&campaign=waepa-oe-2026-email&gclid=abc123&amt=250000` | `https://waepa.nylinsure.com/?association=waepa&campaign=waepa-oe-2026-email` |
| `https://waepa.nylinsure.com/?URLClickedFrom=email&token=4f9c2a7b` | `https://waepa.nylinsure.com/?URLClickedFrom=email` |
| `https://waepa.nylinsure.com/membershp` (a mistyped page) | `https://waepa.nylinsure.com/not-found` |

## Key rules

1. **One per page view**, and always followed by exactly one Page Load Completed.
2. **The page object is sent whole every time**, so nothing from the previous page survives.
3. **The campaign code comes from the address the visitor arrived on**, even if the site removes it
   from the address bar afterwards.
4. **The page id follows the route**, never what happens to be on the screen.

## How to check it

1. In the browser console, `appEventData.map(e => e.event)` shows Page Load Started first on every
   page and step.
2. Open the portal with `?campaign=test-campaign&amt=250000`. `page.url` keeps `campaign` and drops
   `amt`, and `page.campaign` is `test-campaign` on the first page only.
3. Move to another step: a new Page Load Started with `loadType: "virtual"`.
