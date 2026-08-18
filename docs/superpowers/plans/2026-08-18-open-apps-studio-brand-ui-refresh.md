# Open Apps Studio Brand and UI Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (- [ ]) syntax for tracking.

**Goal:** Deliver a cohesive, responsive, accessible Open Apps Studio identity across the studio homepage, product landing pages, and supporting content, then verify and deploy all seven production sites.

**Architecture:** Keep every existing static deployment independent and dependency-free. Express the shared system through equivalent CSS tokens and component conventions in each owning repository, with product-specific accent colors and hero compositions; preserve current metadata, URLs, and redirects.

**Tech Stack:** Semantic HTML5, CSS custom properties, existing vanilla JavaScript, repository-native tests/builds, Playwright browser checks, Vercel CLI.

---

## File map

- The studio repository owns the portfolio homepage, compact product pages, Sync search guides, and the directly deployed Periodic, Lingo, and Mental Math sites.
- Sync site/assets/style.css owns shared presentation for its homepage, guides, privacy, support, 404, and redirect page.
- Better Browser History docs/site.css owns its homepage, support, and privacy presentation.
- Cold Storage website/index.html owns its complete marketing experience.
- .context/ui-refresh-smoke.mjs is an uncommitted workspace browser-QA harness.

### Task 1: Refresh the studio homepage

**Files:**
- Modify: index.html

- [ ] **Step 1: Record the existing public contract**

~~~
grep -q 'https://openappsstudio.com/' index.html
grep -q 'https://sync.openappsstudio.com' index.html
grep -q 'application/ld+json' index.html
~~~

Expected: all commands exit 0.

- [ ] **Step 2: Replace the minimal layout**

Preserve head metadata and JSON-LD. Build a semantic site header, outcome-led hero, five-card product grid, principles section, contact block, and studio footer. Use cobalt #3157e2 on warm paper #f7f5ef, CSS-only grid texture, distinct card accents, 44-pixel controls, visible focus, and reduced-motion handling.

- [ ] **Step 3: Verify the new contract**

~~~
grep -q 'id="apps"' index.html
grep -q 'id="principles"' index.html
grep -q 'prefers-reduced-motion' index.html
grep -q 'focus-visible' index.html
git diff --check
~~~

Expected: all checks pass.

- [ ] **Step 4: Commit**

~~~
git add index.html
git commit -m "Refresh Open Apps Studio homepage"
~~~

### Task 2: Refresh studio-owned product and guide pages

**Files:**
- Modify: better-browser-history/index.html
- Modify: periodic-table/index.html
- Modify: lingo-lessons/index.html
- Modify: mental-math/index.html
- Modify: sync-iphone-contacts-with-google/index.html
- Modify: how-to-sync-google-contacts-to-iphone/index.html

- [ ] **Step 1: Record canonical and heading contracts**

~~~
for page in better-browser-history periodic-table lingo-lessons mental-math sync-iphone-contacts-with-google how-to-sync-google-contacts-to-iphone; do
  grep -q '<link rel="canonical"' "$page/index.html"
  grep -q '<h1' "$page/index.html"
done
~~~

Expected: exit code 0.

- [ ] **Step 2: Apply the shared product shell**

Keep metadata and destinations. Add studio-aware navigation, outcome-led hero, CSS-rendered product visual, scannable benefits, trust strip, and consistent footer. Use violet, emerald, coral, and amber accents respectively, responsive single-column fallbacks, focus-visible styles, and reduced-motion handling.

- [ ] **Step 3: Apply the editorial guide shell**

Preserve article text, structured data, search intent, and heading order. Add studio navigation, warm reading surface, blue callouts, 70-character measure, and consistent footer.

- [ ] **Step 4: Verify and commit**

~~~
for page in better-browser-history periodic-table lingo-lessons mental-math sync-iphone-contacts-with-google how-to-sync-google-contacts-to-iphone; do
  grep -q 'Open Apps Studio' "$page/index.html"
  grep -q 'focus-visible' "$page/index.html"
  grep -q 'prefers-reduced-motion' "$page/index.html"
done
git diff --check
git add better-browser-history periodic-table lingo-lessons mental-math sync-iphone-contacts-with-google how-to-sync-google-contacts-to-iphone
git commit -m "Unify studio product page branding"
~~~

Expected: all checks pass and the commit succeeds.

### Task 3: Refresh Sync Contacts marketing

**Files:**
- Modify: site/assets/style.css
- Modify: site/index.html
- Modify: site/404.html
- Modify: site/appstore.html
- Modify: site/google-contacts-not-syncing-iphone/index.html
- Modify: site/icloud-to-google-contacts/index.html
- Modify: site/merge-duplicate-contacts-iphone/index.html
- Modify: site/privacy/index.html
- Modify: site/support/index.html
- Modify: site/transfer-contacts-android-to-iphone-gmail/index.html

- [ ] **Step 1: Establish test baseline**

~~~
npm test --prefix backend
~~~

Expected: 20 tests pass.

- [ ] **Step 2: Update the shared visual system**

Retain existing class contracts, content, navigation behavior, and dark mode. Replace generic styling with studio tokens using blue #2364e8, warm paper #f7f5ef, dark ink #111827, polished cards, clearer hierarchy, studio maker links, focus-visible outlines, and reduced-motion handling.

- [ ] **Step 3: Verify and commit**

~~~
for page in site/index.html site/404.html site/appstore.html site/*/index.html; do
  grep -q 'Open Apps Studio' "$page"
done
grep -q 'prefers-reduced-motion' site/assets/style.css
grep -q 'focus-visible' site/assets/style.css
git diff --check
npm test --prefix backend
git add site
git commit -m "Refresh Sync Contacts studio branding"
~~~

Expected: checks pass and 20 tests pass.

### Task 4: Refresh Better Browser History marketing

**Files:**
- Modify: docs/site.css
- Modify: docs/index.html
- Modify: docs/privacy.html
- Modify: docs/support.html

- [ ] **Step 1: Establish the extension baseline**

~~~
npm test
npm run typecheck
npm run build
~~~

Expected: 92 tests pass, typecheck passes, and build succeeds.

- [ ] **Step 2: Apply the violet product system**

Keep semantic content and destinations. Use violet #7457e8, warm paper #f7f5ef, studio navigation/footer, a CSS-rendered history search window, polished cards, focus-visible outlines, responsive layouts, and reduced-motion handling.

- [ ] **Step 3: Verify and commit**

~~~
grep -q 'Open Apps Studio' docs/index.html
grep -q 'prefers-reduced-motion' docs/site.css
grep -q 'focus-visible' docs/site.css
git diff --check
npm test
npm run typecheck
npm run build
git add docs
git commit -m "Refresh browser history studio branding"
~~~

Expected: checks pass, 92 tests pass, and build succeeds.

### Task 5: Refresh Cold Storage marketing

**Files:**
- Modify: website/index.html

- [ ] **Step 1: Establish the app baseline**

~~~
npm test --prefix app
~~~

Expected: 251 tests pass.

- [ ] **Step 2: Harmonize the dark identity**

Retain the dark product character and content. Adopt studio spacing, type scale, maker signature, ice-cyan #8ce7f2 accent, visible focus, and reduced-motion handling. Preserve every download and source destination.

- [ ] **Step 3: Verify and commit**

~~~
grep -q 'Open Apps Studio' website/index.html
grep -q 'prefers-reduced-motion' website/index.html
grep -q 'focus-visible' website/index.html
git diff --check
npm test --prefix app
git add website/index.html
git commit -m "Refresh Cold Storage studio branding"
~~~

Expected: checks pass and 251 tests pass.

### Task 6: Browser QA

**Files:**
- Create outside repositories: .context/ui-refresh-smoke.mjs

- [ ] **Step 1: Build the smoke harness**

Create a Playwright script that visits each served page at 320x800, 768x1024, and 1440x1000; fails on console/page errors, horizontal overflow, missing images, invisible h1, missing studio link, missing primary action, and invalid canonical.

- [ ] **Step 2: Run local QA**

Serve the studio root, Sync site, Better docs, and Cold website, then run:

~~~
node .context/ui-refresh-smoke.mjs local
~~~

Expected: every page and viewport passes.

- [ ] **Step 3: Inspect representative screenshots**

Capture all seven homepages at mobile and desktop sizes. Check hierarchy, spacing, contrast, product distinction, focus, and text zoom. Correct every issue and rerun the harness.

### Task 7: Deploy, production-QA, and push

**Files:**
- Create: .context/domain-migration/ui-refresh-final-report.md

- [ ] **Step 1: Confirm intentional diffs**

Run git diff origin/master...HEAD --check and git status --short in every owning repository. Expected: only committed refresh files.

- [ ] **Step 2: Deploy existing Vercel projects**

Deploy the studio root, Sync site, Better docs, Cold website, and the Periodic/Lingo/Mental standalone directories with:

~~~
vercel deploy --prod --yes
~~~

Expected: deployment succeeds without creating replacement projects or changing domains.

- [ ] **Step 3: Run production QA**

~~~
node .context/ui-refresh-smoke.mjs production
~~~

Expected: all seven canonical domains pass all viewports, assets, console, overflow, navigation, canonical, and primary-action checks.

- [ ] **Step 4: Verify legacy redirects**

Use curl -sSI against every old domain with a sample path and query. Expected: permanent redirects preserve target domain, path, and query.

- [ ] **Step 5: Push owning repositories**

~~~
git push origin master
~~~

Push studio, Sync, Better Browser History, and Cold Storage only after their production checks pass.

- [ ] **Step 6: Record evidence**

Write commit hashes, deployment IDs, test totals, browser-QA results, and redirect results to .context/domain-migration/ui-refresh-final-report.md.

