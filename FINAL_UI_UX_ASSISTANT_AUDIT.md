# Final UI/UX + Assistant Audit and Repair

## Diagnosis
The generated client UI could look poor because every builder layout ID was not rendered as a polished, stable app shell. Some layouts created cramped/scattered navigation when many modules were selected. The assistant also had useful answers, but it was not exhaustive enough for every generated page.

## Corrective actions

### UI/UX repairs
- Rebuilt the critical app-shell CSS inside generated pages.
- Stabilised sidebar and top navigation for large module lists.
- Added fixed icon boxes so icons no longer scatter or misalign.
- Added proper wrapping/truncation for long module names.
- Added professional card shadows, spacing, sticky topbar and mobile drawer behavior.
- Mapped all builder layouts (`layout0` through `layout16`) to safe professional variants instead of broken experimental layouts.

### Assistant repairs
- Added `assets/js/site-help.js`, a deterministic no-AI help library.
- Added comprehensive explanations for core pages and generic explanations for every generated page.
- Integrated the help library with `Super.chatbot`.
- The topbar Help button and chat questions now return detailed page purpose, users, usage steps and free-tool notes.

### Build/deployment repairs
- Ensured `site-help.js` is included in generated ZIPs.
- Added `site-help.js` to service-worker cache.
- Rebuilt final upload folder `gen/` and archive `gen.zip`.

## Free-tool policy
No AI API was added. The assistant is a rules-based deterministic helper running in browser JavaScript.

## Deployment steps
1. Upload all files in `gen/` to GitHub repo, Netlify, Vercel or Cloudflare Pages.
2. Create a Supabase project.
3. Run SQL files in this order: `schema.sql`, `voting-schema.sql`, `cbt-schema.sql`, `reportcard-schema.sql`, `enterprise-schema.sql`, `enhancements-schema.sql`, `update-v1-schema.sql`, `update-v2-schema.sql`, `update-v4-schema.sql`.
4. Paste Supabase URL and anon key into `assets/js/config.js` of generated client sites.
5. Create first user, then approve/promote admin in Supabase `profiles`.
6. Configure Academic Setup before entering subjects, results and CBT exams.
