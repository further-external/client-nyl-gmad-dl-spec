# Application Started

**Event name:** `Application Started` · Needed for your reports

[Back to the overview](../README.md)

> **Relationship to [Application Activity](application-activity.md).** Application Started is a
> dedicated milestone that marks the moment an application begins. The detailed step-by-step
> journey stays on Application Activity; this event is the **front bookend** for every completion
> rate, so "how many applications began" is a clean, deduped metric on its own.

## What it answers

- How many visitors begin an application: the denominator for every downstream completion rate.
- Where starts come from: self-service vs. advisor-assisted (`channel`), and which association.
- The start-to-submit conversion rate, paired with [Application Completed](application-completed.md).
- Whether a page or experience change moved the start rate, over time.

The whole journey, page by page, is on [The application journey](../application-journey.md).

**Feeds your reports:** Form Start (event12), once per application.

## When to fire it

- **Fire this once per application, on the first real input on the Membership form.** Not on the
  Begin application click, and not on the page view.
- **Values carried from the quote are not an input.** A date of birth, ZIP or state the instant
  quote filled in starts nothing; wait for the applicant's own first input (agreed on 2026-10-23).
- **Fire it as its own hit.**
- **Fire it on every experience the same way**, with or without the instant quote panel
  ([the rule](../how-it-works.md#which-event-fires-where-the-two-landing-experiences)).
- **Never repeat it on a reload or revisit.** Remember the start for the application: one start
  per application.
- **Do not fire it for:** the Begin application click, a quote start, or a resumed application
  (that is [Application Activity](application-activity.md) `resumed`).
- **Never send PII or an applicant's answers**: no names, contact details, SSNs, dates of birth,
  health responses, or personal financial figures. Product-level values (a product's price and the
  selected coverage amount) are product information, not PII, and are sent. See [Privacy](../privacy.md).

## Example

The Membership page view, then the first input:

```mermaid
sequenceDiagram
  participant UI as Portal
  participant DL as Data layer
  participant AA as Adobe Analytics
  rect rgba(80,140,255,0.12)
    Note over UI,AA: Membership page view
    UI->>DL: Page Load Started, User Detected
    UI->>DL: Page Load Completed
    DL->>AA: page view
  end
  UI->>DL: Application Started (first Membership input)
  DL->>AA: separate hit: counted once per application
```

The start is its own hit, fired once, and remembered so a reload cannot count it twice.

## Javascript code

```js
window.appEventData = window.appEventData || [];
window.appEventData.push({
  "event": "Application Started",
  "application": {
    "channel": "<channel>",
    "applicationName": "<applicationName>",
    "id": "<applicationId>"
  }
});
```

## Populated example

```js
window.appEventData = window.appEventData || [];
window.appEventData.push({
  "event": "Application Started",
  "application": {
    "channel": "self-service",
    "applicationName": "waepa-term-life",
    "id": "4f9c2a7e1b83"
  }
});
```

## Variable definitions

| Path | Type | Description | Example |
|---|---|---|---|
| `application.channel` | string | Required. For your reports. Who is completing the application | `self-service`, `advisor-assisted` |
| `application.applicationName` | string | Required. For your reports. The specific application being filled out (for example term life on the WAEPA portal); `channel` says who completes it. The list of values is [decision 23](../open-questions.md) | `waepa-term-life` |
| `application.id` | string or null | Required. Recommended. The masked id of this application, the same on every application event from the moment it is created (agreed on 2026-10-23). When it is created and how it is masked is [decision 27](../open-questions.md) | `4f9c2a7e1b83` |

## How to check it

1. Begin one application with test details. **Application Started** fires once, as its own hit, at
   the first Membership input, not on the Begin application click.
2. Reload the Membership page and type again: **no second start**.
3. The start-to-submit funnel using Application Started and
   [Application Completed](application-completed.md) returns a sane conversion rate.
