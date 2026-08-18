# Open Apps Studio Domain Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Move the studio and all product sites from `opengrounds.org` to `openappsstudio.com` with valid TLS, updated branding/metadata, and path-preserving permanent redirects from every old URL.

**Architecture:** Keep GoDaddy authoritative for DNS and keep the existing Vercel projects. Attach each new hostname to its matching project before any redirect is enabled, update the canonical content in source, then use host-conditional Vercel redirects for the old hostnames. Preserve the old domain and its project assignments for at least twelve months.

**Tech Stack:** Static HTML, Vercel projects and domain API/CLI, GoDaddy DNS, GitHub repositories and organization API, shell-based HTTP/DNS verification.

---

### Task 1: Record the production baseline and rollback values

**Files:**
- Create: `.context/domain-migration/baseline.md`

- [ ] **Step 1: Capture GitHub, Vercel, and DNS identifiers**

Record the GitHub organization/repositories, Vercel project IDs, current
GoDaddy nameservers, apex A records, subdomain CNAME/A records, and HTTP status
for every old hostname. Do not record access tokens or cookies.

- [ ] **Step 2: Verify every old site is healthy**

Run:

```bash
for host in opengrounds.org www.opengrounds.org sync.opengrounds.org \
  periodic.opengrounds.org lingo.opengrounds.org mentalmath.opengrounds.org \
  better-browsing-history.opengrounds.org coldstorage.opengrounds.org; do
  curl -fsSIL --max-time 15 "https://$host" | sed -n '1,8p'
done
```

Expected: every canonical old site returns `200`; no TLS failure.

### Task 2: Add the new domain and aliases without switching traffic

**External systems:**
- Vercel team: `arhams-projects-2394572b`
- GoDaddy zone: `openappsstudio.com`

- [ ] **Step 1: Add the parent domain to Vercel**

Run `vercel domains add openappsstudio.com`, then inspect the exact DNS values
Vercel requests. Do not remove `openappsstudio.com`'s parking records until the
requested replacement values are recorded.

- [ ] **Step 2: Attach every new hostname to its existing project**

Attach:

```text
openappsstudio.com, www.openappsstudio.com -> opengrounds
sync.openappsstudio.com                   -> sync-contacts-for-google
periodic.openappsstudio.com               -> periodic
lingo.openappsstudio.com                  -> lingo
mentalmath.openappsstudio.com             -> mentalmath
better-browsing-history.openappsstudio.com -> better-browsing-history
coldstorage.openappsstudio.com             -> coldstorage
```

- [ ] **Step 3: Replace only GoDaddy web records**

Change the new apex and `www` parking records to the values Vercel reports.
Create the six product records using the values Vercel reports. Preserve all
TXT/MX records and keep `opengrounds.org` unchanged.

- [ ] **Step 4: Verify DNS and TLS before source changes**

Run `dig` and `curl -fsSIL` for each new hostname until Vercel reports valid
configuration and HTTPS no longer reports a certificate error.

### Task 3: Rebrand and recanonicalize the studio website

**Files:**
- Modify: `index.html`
- Modify: `periodic-table/index.html`
- Modify: `lingo-lessons/index.html`
- Modify: `mental-math/index.html`
- Modify: `better-browser-history/index.html`
- Modify: `how-to-sync-google-contacts-to-iphone/index.html`
- Modify: `sync-iphone-contacts-with-google/index.html`
- Modify: `logo.svg`
- Modify: `robots.txt`
- Modify: `sitemap.xml`
- Modify: `README.md`
- Modify: `vercel.json`

- [ ] **Step 1: Add a failing reference check**

Run:

```bash
grep -RIn --exclude-dir=.git -E 'Open Grounds Studio|Open-Grounds-Studio|opengrounds\.org' .
```

Expected: matches in the listed source files.

- [ ] **Step 2: Replace the brand and canonical URLs**

Use `Open Apps Studio`, `https://openappsstudio.com`, and the matching new
product subdomains in titles, descriptions, canonical tags, Open Graph/Twitter
metadata, JSON-LD, navigation, footer, sitemap, robots file, and README. Change
GitHub organization links to `https://github.com/Open-Apps-Studio`.

- [ ] **Step 3: Add host-conditional redirects**

In `vercel.json`, add permanent host rules for `opengrounds.org` and
`www.opengrounds.org` to `https://openappsstudio.com/:path*`, plus a canonical
redirect from `www.openappsstudio.com` to the new apex. Leave requests whose
host is `openappsstudio.com` untouched.

- [ ] **Step 4: Validate locally**

Run the reference check again and validate `vercel.json` with `jq empty`. The
only remaining old-domain text may be explicit redirect host conditions and the
migration documentation.

- [ ] **Step 5: Commit the studio source change**

```bash
git add README.md index.html logo.svg robots.txt sitemap.xml vercel.json \
  periodic-table lingo-lessons mental-math better-browser-history \
  how-to-sync-google-contacts-to-iphone sync-iphone-contacts-with-google \
  docs/superpowers
git commit -m "Migrate site to Open Apps Studio domain"
```

### Task 4: Update the Sync Contacts site

**Repository:** `Open-Apps-Studio/sync-contacts-for-google`

**Files:**
- Modify: `site/*.html`
- Modify: `site/*/index.html`
- Modify: `site/robots.txt`
- Modify: `site/sitemap.xml`
- Modify: `site/vercel.json`

- [ ] **Step 1: Confirm old references exist**

Run the old brand/domain reference check in the repository and retain its output
as the failing baseline.

- [ ] **Step 2: Update public content and metadata**

Replace `Open Grounds Studio` with `Open Apps Studio`, and replace
`sync.opengrounds.org` with `sync.openappsstudio.com` in canonical URLs,
structured data, privacy/support pages, sitemap, and robots files.

- [ ] **Step 3: Add the old-host redirect**

Prepend a permanent host-conditional redirect from
`sync.opengrounds.org/:path*` to
`https://sync.openappsstudio.com/:path*` in `site/vercel.json`.

- [ ] **Step 4: Validate and commit**

Run `jq empty site/vercel.json`, ensure only the redirect condition retains the
old hostname, then commit the exact changed files with message
`Migrate Sync Contacts to Open Apps Studio domain`.

### Task 5: Update Better Browser History

**Repository:** `arhxam/better-browser-history`

**Files:**
- Modify: `PRIVACY.md`
- Modify: `README.md`
- Modify: `docs/*.html`
- Modify: `docs/robots.txt`
- Modify: `docs/sitemap.xml`
- Modify: `docs/vercel.json`
- Modify: `docs/chrome-web-store/*.md`
- Modify: `scripts/validate-launch.mjs`
- Modify: `src/ui/app/Options.tsx`

- [ ] **Step 1: Update references and validator expectations**

Use `better-browsing-history.openappsstudio.com` and `Open Apps Studio`
everywhere, including extension settings links and Chrome Web Store submission
documentation. Update `validate-launch.mjs` to require the new canonical origin.

- [ ] **Step 2: Add the old-host redirect**

Add a permanent host-conditional redirect in `docs/vercel.json` from the old
hostname to the new hostname, preserving `:path*`.

- [ ] **Step 3: Run repository verification**

```bash
npm install
npm run validate:launch
npm run typecheck
npm test
npm run build
```

Expected: all commands pass.

- [ ] **Step 4: Commit**

Commit the exact migration files with message
`Migrate site to Open Apps Studio domain`.

### Task 6: Update Cold Storage

**Repository:** `arhxam/cold-storage-social-media-backup`

**Files:**
- Modify: `website/index.html`
- Create or modify: the Vercel config used by the `coldstorage` deployment

- [ ] **Step 1: Update canonical and publisher metadata**

Replace the old Cold Storage hostname, studio brand, publisher URL, social image
URLs, and footer link with their `openappsstudio.com` equivalents.

- [ ] **Step 2: Add the old-host redirect at the deployed root**

Place a permanent host-conditional Vercel redirect in the directory actually
deployed by the `coldstorage` project. Confirm the project root before writing
the config.

- [ ] **Step 3: Preview, scan, and commit**

Serve the static site locally, verify the HTML loads, run the old-reference
scan, and commit with message `Migrate Cold Storage to Open Apps Studio domain`.

### Task 7: Deploy and validate canonical sites

**External system:** Vercel

- [ ] **Step 1: Deploy each changed project to production**

Deploy the studio, Sync Contacts, Better Browser History, and Cold Storage from
their real deployment roots. Record each deployment URL and project name.

- [ ] **Step 2: Validate all new canonical hosts**

For each of the eight new hostnames, verify DNS, TLS, response status, expected
page title/body, canonical URL, and absence of old-brand links.

- [ ] **Step 3: Validate redirects**

Request a representative nested path and query string on each old hostname.
Expected: `301` or `308` to the exact matching new hostname, same path, and same
query string.

### Task 8: Rename GitHub and Vercel identifiers and metadata

**External systems:** GitHub, Vercel

- [ ] **Step 1: Rename the website repository**

Rename `Open-Apps-Studio/opengrounds.org` to
`Open-Apps-Studio/openappsstudio.com`. Verify the old GitHub URL redirects and
update the local `origin` URL.

- [ ] **Step 2: Update organization and repository metadata**

Set the organization blog to `https://openappsstudio.com`. Update affected
repository homepage URLs and descriptions to the new domain/brand. The
organization handle and display name are already correct and must not be
renamed again.

- [ ] **Step 3: Rename the Vercel project**

Rename `opengrounds` to `openappsstudio` only after all aliases and production
content are verified. Reinspect the project to confirm its ID is unchanged.

### Task 9: Final external-surface and rollback audit

**Files:**
- Update: `.context/domain-migration/final-report.md`

- [ ] **Step 1: Scan all repositories**

Run the organization/personal repository old-reference scan. Classify every
remaining match as an intentional redirect/migration record or a defect; fix
all defects.

- [ ] **Step 2: Verify public metadata**

Check GitHub organization/repository pages, Vercel project/domain mappings,
root and product URLs, sitemap and robots URLs, privacy/support pages, and
public store documentation.

- [ ] **Step 3: Record external follow-ups that require separate credentials**

Document any App Store Connect, Chrome Web Store, Google Search Console, or
Google OAuth URL that cannot be changed with the authenticated sessions. Old
redirects must make those URLs safe until the corresponding console is updated.

- [ ] **Step 4: Confirm rollback remains possible**

Verify `opengrounds.org` remains registered, DNS records remain present, and
all old hostnames remain assigned to Vercel. Do not delete the old domain,
aliases, repositories, or deployment history.
