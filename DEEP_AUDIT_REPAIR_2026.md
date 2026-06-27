# Deep Audit & Repair Report — School Connect Builder
Date: 2026-06-27

This pass re-audited the builder/source repository patiently after the first repair and fixed additional conflicts found during validation.

## Additional issues fixed in this pass
1. `assets/js/site-help.js` was referenced by generated pages but was not actually being added into new generated ZIPs. Future client builds now include it.
2. Public pages (`index.html`, `about.html`, `contact.html`) now load `site-help.js` and `app.js` consistently so public-page initialization, assistant help, theme handling, and modals do not depend on accidental globals.
3. `about.html` generated link corrected from private `admissions.html` to public `apply.html`.
4. Generator CBT prompt output was made quote-safe: the generated CBT page no longer emits a broken multi-line single-quoted JavaScript string.
5. CBT prompt copy button no longer embeds fragile nested quotes in an inline `onclick`; it binds safely after modal creation.
6. Service-worker cache version bumped to force browsers to discard stale cached broken assets after deployment.
7. Builder canonical/social/sitemap URLs corrected from old `schoolconnectv2` to `realschoolconnect`.
8. Font catalogue corrected for single-weight Google fonts and the `Prompt` font typo, reducing failed font requests.

## Previous fixes retained
- Generated role regex corruption fixed (`/\s+/`, `/\b\w/g`).
- `contact.html` generation added.
- `apply.html` added to public page allow-list.
- Generated `realgosa` missing `site-help.js` fixed.
- Broken `assets/img/logo.` reference fixed.

## Validation run
- Local HTML references: 0 missing in builder and demo.
- JavaScript files: all `node --check` clean.
- Inline executable scripts: 201 checked, 0 syntax errors.
- Control characters in source text: 0.
- Suspicious old bug strings (`/s+/`, broken CBT string, nested print script): 0 active hits in runtime files.
