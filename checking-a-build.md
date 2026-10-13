# Checking a build

[Back to the overview](README.md)

A checklist for testing a build before it goes live. Use test details only, never real ones. Each
event page has its own short checks as well.

**Before you start:** test on a production build. In development, React runs some code twice and
shows duplicate events that do not happen in production.

## The page view sequence

1. Open the browser console. After each page or step, `appEventData.map(e => e.event)` shows Page
   Load Started, User Detected, any events that describe the page, then Page Load Completed, in
   that order, once each.
2. Each event matches the example on its page, detail for detail: nothing missing, nothing extra.
   A data layer helper may add its own `__meta` detail; ignore it.

## Nothing typed is recorded

3. Fill in a form with test details (a test name, email and ZIP code). Search the console output
   for each of them: **none may be found.** Then search for `step-viewed`: it must be found, which
   proves the search works.
4. Open the portal with `?campaign=test-campaign&amt=250000` in the address. In the requests sent to
   Adobe, `test-campaign` is found on the first page view, and `250000` is found nowhere.

## What reaches Adobe

5. In the Adobe debugger:
   - each page view is one hit, carrying the events that describe the page;
   - each click, error, quote or product change is its own hit, sent when it happens;
   - "field completed" and "step completed" send nothing yet;
   - a click on the Privacy Notice or an Application PDF sends exactly one hit.
6. Reload the Eligibility page and the receipt: the step is recorded again as a repeat;
   Application Start and Application Complete are not repeated, and orders and revenue do not
   move (the purchase is deduplicated by the receipt number).

## Both landing experiences

7. On an association with the instant quote panel: the landing page view carries Form Activity
   "viewed"; the first input records "quote started"; a failed "Get an instant quote" records one
   error; results record "products viewed".
8. On the default experience: the landing page view carries no Form Activity; the quote drawer
   records "quote started", "products viewed" and "product added".

## Logins

9. On the advisor login and the resume pages: the first input records Login Activity "started";
   an empty field records one Form Activity error; a wrong code or wrong credentials records one
   Login Activity "failed"; success records one "succeeded", and the next page shows the new
   login status. `login.id` and `user.id` are empty.

## Consent

10. Repeat steps 1 to 5 with cookies accepted and with cookies declined. The data layer is the same
   in both; only what Adobe receives differs.
