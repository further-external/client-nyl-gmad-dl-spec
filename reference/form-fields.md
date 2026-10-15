# Form fields

[Back to the overview](../README.md) · Proposed, needs your approval ([decision 8](../open-questions.md))

These are **the only field names the data layer may report**:
- in the application and the instant quote panel, in `application.field.name` and
  `application.error.field` ([Application Activity](../events/application-activity.md));
- on the resume and advisor login forms, in `form.field.name` and `form.error.field`
  ([Form Activity](../events/form-activity.md)).

 A name says a field was completed or was in error.
**It never carries what was entered:** `zip-code` says the ZIP code field was completed, never which
ZIP code.

A field not on this list is never reported. Adding a name is a change to this specification,
reviewed before it ships.

**One name per field:** when a field is made of several controls (each radio button of a choice,
each part of an address, the day, month and year of a date), every part reports the field's one
name. The ZIP code inside the home address reports `home-address`, not `zip-code`.

## The list

**The application** (Application Activity):

| Page | Allowed names |
|---|---|
| Membership | `member-status`, `first-name`, `last-name`, `email`, `phone`, `phone-type` |
| Eligibility | `zip-code`, `state`, `date-of-birth` |
| Coverage | `coverage-category`, `benefit-amount` |
| Contact | `home-address`, `correspondence-preference`, `employer-details`, `same-as-home` |
| Review | `consent` |
| Beneficiary, Profile, the health pages, Payment | none, on purpose (below) |

**The instant quote panel** (Application Activity): `date-of-birth`, `zip-code`, `state`.

**Resuming an application** (Form Activity, `form.name: "resume"`): `email` (email step), `contact-method`
(method step), `verification-code` (code step).

**The advisor login** (Form Activity, `form.name: "advisor-login"`): `advisor-email`, `advisor-phone`,
`access-code`.

## Why some pages report no fields

On some pages even a field's name would describe something sensitive. **These lists are empty on
purpose:**
- **Profile:** height, weight, SSN, marital status, licence, travel, physician, other coverage.
- **The five health pages.** An error there is recorded with no detail at all.
- **Payment.**
- **Beneficiary details.**
- **Spouse and child sections on Eligibility.** Whether a spouse or child is added is household
  make-up, which is never recorded. For the same reason, an error on Eligibility never names the
  field.

**Nothing is reported from** the quote drawer's inputs or the Needs Calculator.

**Checkboxes:** a single checkbox's completion means it was ticked, so only two are listed,
`consent` and `same-as-home`, because ticking them reveals nothing personal.
