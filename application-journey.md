# The application journey

[Back to the overview](README.md)

This page follows one self-service application from the first quote to the receipt, then the
other ways in: knockout, resume and advisor-assisted. All of these are
[Application Activity](events/application-activity.md) unless marked otherwise; the first
Membership input is [Application Started](events/application-started.md), and the receipt is
[Application Completed](events/application-completed.md).

## Self-service, quote to receipt

Each box is one page view. **On view** actions travel with the page view. **As the visitor acts**
actions are separate hits. Dashed boxes appear only when they apply.

```mermaid
flowchart TD
  L["Landing page<br/>as the visitor acts:<br/>quote started<br/>products viewed<br/>product added, on<br/>Apply for coverage"]
  M["Membership<br/>on view: step started<br/>as the visitor acts:<br/>Application Started"]
  KO["knocked out<br/>instead of started<br/>Next stays disabled"]
  E["Eligibility<br/>on view: step started<br/>as the visitor acts:<br/>saved (first autosave)"]
  C["Coverage<br/>on view: step started<br/>as the visitor acts:<br/>products viewed<br/>product removed, if confirmed<br/>on Next: product added"]
  B["Beneficiary<br/>on view: step started"]
  CPR["Contact, Profile, Review<br/>on view: step started,<br/>each page in turn"]
  H["Health pages<br/>on view: step started,<br/>each page in turn"]
  PAY["Payment<br/>on view: step started"]
  DS["Signature (DocuSign)<br/>on view: step started<br/>as the visitor acts:<br/>signature started, on Next"]
  SIGN["Signing at DocuSign<br/>outside the portal<br/>cannot be measured"]
  RC["Receipt<br/>on view: Application Completed<br/>once per receipt number"]

  L -->|"Begin application<br/>nothing on the click"| M
  M -.->|"first answer<br/>not a member"| KO
  M --> E --> C
  C -->|"Life or AD#amp;D only"| B
  B --> CPR
  C --> CPR
  CPR -->|"only the classes<br/>that apply"| H
  H --> PAY
  CPR --> PAY
  PAY --> DS
  DS -->|"the portal unloads"| SIGN
  SIGN -->|"back to the portal"| RC

  classDef conditional stroke-dasharray: 5 5
  classDef offportal stroke-dasharray: 2 4
  class B,H,KO conditional
  class SIGN offportal
```

- **Each validated Next**, from Membership to Payment, also adds "step completed" to the data layer
  just before the page changes. It is not sent to Adobe yet ([decision 9](open-questions.md)).
- **Signing happens outside the portal**, so it cannot be measured. A return to the receipt page is
  the only sign that it was completed ([decision 18](open-questions.md)).
- **Revisits count.** Back, Edit and a reload record the step again, marked as a repeat, so the
  reports count step views.

## Knockout, resume and advisor-assisted

```mermaid
flowchart TB
  subgraph KO["Knockout"]
    direction LR
    K1["Membership<br/>on view: step started"] -->|"first answer<br/>not a member"| K2["knocked out<br/>Next stays disabled"]
  end
  subgraph RC["Resume with a verification code"]
    direction LR
    R1["Resume pages<br/>Form Activity:<br/>resume form"] -->|"code verified"| R2["first restored page<br/>on view: step started,<br/>then resumed"]
  end
  subgraph WB["Resume from the welcome-back landing page"]
    direction LR
    W1["Continue Application<br/>nothing on the click"] --> W2["first restored page<br/>on view: step started,<br/>then resumed"]
  end
  subgraph AD["Advisor-assisted"]
    direction LR
    A1["Advisor login<br/>Login Activity,<br/>plus the login form"] --> A2["Application pages<br/>the same events as<br/>self-service"] --> A3["Confirmation sent<br/>on view: advisor<br/>confirmation sent"] --> A4["Applicant review<br/>on view: step started,<br/>each page, no started"]
  end
  KO ~~~ RC ~~~ WB ~~~ AD
```

- **Application Started and saved never fire on a resumed application:** it was already started and saved.
- **Advisor-assisted applications** carry `application.channel: "advisor-assisted"`. Who owns this
  flow, and three details of it, are [decision 7](open-questions.md).

## One application, at a glance

What a typical application sends to Adobe, in order. PLS, UD and PLC are Page Load Started, User
Detected and Page Load Completed.

| Step | Events, in order | Sent to Adobe as |
|---|---|---|
| Landing page | PLS, UD, PLC | a page view |
| Quote drawer | quote started, products viewed, product added (on Apply for coverage) | 3 separate hits |
| Begin application | nothing | none |
| Membership | PLS, UD, step started (1), PLC; then Application Started, and an error if Next fails | a page view; then separate hits |
| Eligibility | PLS, UD, step started (2), PLC; then saved | a page view; a hit |
| Coverage | PLS, UD, step started (3), PLC; then products viewed; product added on Next | a page view; 2 hits |
| Beneficiary | PLS, UD, step started (4), PLC; Add Beneficiary opens a window | a page view; a hit |
| Contact, Profile, Review | PLS, UD, step started, PLC each; Print on Review | a page view each; a hit |
| Health pages that apply | PLS, UD, step started, PLC each | a page view each |
| Payment | PLS, UD, step started, PLC | a page view |
| Signature | PLS, UD, step started, PLC; then signature started | a page view; a hit |
| DocuSign | nothing: outside the portal | none |
| Receipt | PLS, UD, Application Completed, PLC; Application PDF if clicked | a page view; a hit |
