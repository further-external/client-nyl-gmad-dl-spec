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
   for each of them: **none may be found.** Then search for `step-started`: it must be found, which
   proves the search works.
4. Open the portal with `?campaign=test-campaign&amt=250000` in the address. In the requests sent to
   Adobe, `test-campaign` is found on the first page view, and `250000` is found nowhere.

## What reaches Adobe

5. In the Adobe debugger:
   - each page view is one hit, carrying the events that describe the page;
   - each click, error, quote or product change is its own hit, sent when it happens;
   - "step completed" sends nothing yet;
   - a click on the Privacy Notice or an Application PDF sends exactly one hit.
6. Reload the Eligibility page and the receipt: the step is recorded again as a repeat;
   Application Started and Application Completed are not repeated, and orders and revenue do not
   move (the purchase is deduplicated by the receipt number).

## Both landing experiences

7. On an association with the instant quote panel: the landing page view carries no form event;
   the first input records Application Activity "quote started"; a failed "Get an
   instant quote" records one Application Activity error; results record "products viewed".
8. On the default experience: the quote drawer records "quote started" and "products viewed";
   Add records nothing; "Apply for coverage" records one "product added".
9. In the application: Add coverage on Coverage records nothing; a Next that passes validation
   records one "product added" with the products added; a confirmed removal records "product
   removed". No form event fires anywhere in the application or the quote, and no push names a
   field.

## Logins

10. On the advisor login and the resume pages: the first input records Login Activity "started";
   an empty field records nothing; a wrong code or wrong credentials records one
   Login Activity "failed"; success records one "succeeded", and the next page shows the new
   login status. `login.id` and `user.id` are empty.

## Consent

11. Repeat steps 1 to 5 with cookies accepted and with cookies declined. The data layer is the same
   in both; only what Adobe receives differs.
