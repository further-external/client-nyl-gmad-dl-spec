# Privacy

[Back to the overview](README.md)

**Nothing a person types, selects or uploads is ever recorded as a value.** This applies to every
event and every detail in this specification.

## Never recorded

- names, including beneficiary names; email addresses; phone numbers;
- dates of birth or ages; ZIP codes, cities, states or addresses;
- Social Security numbers or any government id;
- health answers, height, weight, tobacco use;
- bank, card or payment details;
- income or debt, including what is entered in the Needs Calculator;
- marital status or gender;
- household make-up: whether a spouse or child is added, or how many people a product covers;
- error message text, which can repeat what was entered;
- the visitor's IP address;
- any identifier of the person: name, email, phone, customer or policy number, or a scrambled
  version of one.

## Product details that are recorded

A product's **price** and the **benefit amount** selected for it describe the product, not the
person, and are recorded with the product (agreed at the October 13 review). They are never
combined with anything that identifies the applicant.

## One proposed exception

A **proposed anonymous id** (`user.id`, `login.id`) for a logged-in advisor or a verified consumer,
issued by New York Life's own systems and never derived from a name, email or number. It stays
empty until New York Life confirms what the current portal passes and approves it
([decision 20](open-questions.md)).

## How the design keeps it that way

- **No field-level tracking** (agreed on 2026-10-23). No event names a field, says that a field
  was completed, or carries a value. Forms are recorded as a whole: shown, started, completed, or
  blocked.
- **Clicks report fixed labels.** A click is recorded only on a tracked control, never on a form
  field, and its label is written in the code, never read from the screen.
- **The health pages report no error detail.** An error there records only that a step was blocked.
- **Addresses are cleaned before they are recorded.** Only listed address parameters are kept; a
  benefit amount, a token or an ad click id in the address is removed
  ([details](events/page-load-started.md#how-the-address-is-cleaned)). Links record their host and
  path only.
- **Products are one per line, never per person.** Quantity is always 1.

## In Adobe Launch and Adobe Analytics

**Proposed** settings for the tag build, no site work:
- **The page address Adobe receives is the cleaned one**, on every hit. The referring address is
  cut to its host and path.
- **Adobe's automatic link and download tracking is off.** The Click Interaction event replaces it, so a
  click is never sent twice with its full address.
- **Activity Map is off.** It would otherwise send the visible text of every clicked link or
  button.
- **We recommend turning on IP obfuscation** in the report suite, and that no rule copies the IP
  address into a report. Your decision.

## Consent

The data layer records events whatever the visitor's cookie choice. Adobe Launch applies the
consent choice and decides what is sent.
