# Deep Audit and Repair Report

## Scope
Audited the live builder repository (`realschoolconnect`) and the generated demo site (`realgosa`). The audit checked JavaScript syntax, missing local assets, generated app runtime logic, page rendering risks, assistant assets, navigation/UI shell, and deployment blockers.

## Key problems found

1. **Generated runtime regex corruption in `assets/js/app.js`**
   - In the generated demo (`realgosa`), regexes were emitted incorrectly as `/s+/` and `/w/g` control-character behavior instead of `/\s+/` and `/\b\w/g`.
   - Impact: role dashboard filtering, role navigation filtering, and role display formatting could fail or behave incorrectly.
   - Fix: repaired `realgosa_repo/assets/js/app.js` directly and repaired `realschoolconnect/assets/js/generator.js` so future generated clients emit correct regexes.

2. **Missing `assets/js/site-help.js` in the generated demo**
   - Many demo pages referenced `assets/js/site-help.js`, but the file was absent.
   - Impact: 404 asset load; assistant exhaustive help library unavailable.
   - Fix: copied/added `assets/js/site-help.js` and added it to the service worker cache.

3. **Broken local references in demo site**
   - `about.html` linked to a missing `contact.html`.
   - `entrance.html` contained a static-scannable broken logo path `assets/img/logo.`.
   - Fix: added `contact.html` and repaired the broken logo reference.

4. **Future generated clients needed contact page support**
   - The generator could produce sites where `about.html` references contact without a generated contact page.
   - Fix: added `Generator.pageContact(cfg)` and added `contact.html` to generated ZIP output.

5. **Auth/public page issue**
   - `apply.html` should be public but was not in the public page list in generated app runtime.
   - Fix: added `apply` to `PUBLIC_PAGES` in both the generator and the current demo.

6. **UI/UX concerns**
   - Existing generated app shell has many modules and can look crowded. The current repository already contains the newer critical app-shell CSS; verified it is present in the demo pages. The runtime fixes above prevent role filtering failure that could make dashboards/nav look wrong.

## Validation performed

- Ran `node --check` on all JS assets in `realschoolconnect/assets/js`.
- Ran `node --check` on all JS assets in `realgosa_repo/assets/js`.
- Performed local href/src reference audit for all HTML files.
- Result after fixes: no missing local references found in either repository copy.

## Files changed in `realschoolconnect`

- `assets/js/generator.js`

## Files changed/added in `realgosa_repo`

- `assets/js/app.js`
- `assets/js/site-help.js`
- `sw.js`
- `entrance.html`
- `contact.html`

## Deployment advice

After applying these changes to GitHub:

1. Clear browser cache/service worker for the demo site.
2. Reopen the site in an incognito window first.
3. If old assets persist, unregister service worker in DevTools → Application → Service Workers.
4. Redeploy to GitHub Pages/Vercel/Cloudflare.
5. For generated client sites, regenerate from the repaired builder so the corrected app runtime and contact page are included automatically.
