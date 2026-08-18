# Open Apps Studio Domain Migration Design

## Objective

Migrate the Open Apps Studio web presence from `opengrounds.org` to
`openappsstudio.com` without downtime, broken product links, loss of search
equity, or an irreversible DNS cutover.

## Public URL map

| Current URL | New canonical URL | Vercel project |
| --- | --- | --- |
| `opengrounds.org` | `openappsstudio.com` | `opengrounds` (rename to `openappsstudio`) |
| `www.opengrounds.org` | `www.openappsstudio.com` | `opengrounds` |
| `sync.opengrounds.org` | `sync.openappsstudio.com` | `sync-contacts-for-google` |
| `periodic.opengrounds.org` | `periodic.openappsstudio.com` | `periodic` |
| `lingo.opengrounds.org` | `lingo.openappsstudio.com` | `lingo` |
| `mentalmath.opengrounds.org` | `mentalmath.openappsstudio.com` | `mentalmath` |
| `better-browsing-history.opengrounds.org` | `better-browsing-history.openappsstudio.com` | `better-browsing-history` |
| `coldstorage.opengrounds.org` | `coldstorage.openappsstudio.com` | `coldstorage` |

The `www` hostname will redirect to the apex. Every old hostname will remain
attached to Vercel and redirect to the matching new hostname while preserving
the path and query string.

## Migration order

1. Add `openappsstudio.com` to Vercel without changing the old domain.
2. Add all new hostnames to their existing Vercel projects.
3. Change GoDaddy DNS from the parking records to Vercel's required records.
4. Wait for Vercel to issue TLS certificates and verify every new hostname.
5. Update source, metadata, canonical URLs, structured data, sitemaps, robots
   files, legal/support pages, and in-product links.
6. Deploy and verify each project on its new canonical hostname.
7. Configure path-preserving permanent redirects on every old hostname.
8. Update GitHub organization metadata, repository names/descriptions/homepage
   URLs, and all repositories that contain old-brand or old-domain references.
9. Recheck public surfaces and leave the old domain active for at least twelve
   months.

At no point is an old hostname removed before its replacement has valid DNS,
TLS, and content.

## Repository changes

### Studio website

Rename `Open-Apps-Studio/opengrounds.org` to
`Open-Apps-Studio/openappsstudio.com`. Update the brand to “Open Apps Studio”
and replace old canonical, Open Graph, Twitter, JSON-LD, sitemap, robots,
product, privacy, support, and GitHub URLs. Add host-based redirects in
`vercel.json` so old root-domain requests retain their path and query string.

### Product repositories

Update domain and brand references in:

- `Open-Apps-Studio/sync-contacts-for-google`
- `arhxam/better-browser-history`
- `arhxam/cold-storage-social-media-backup`

The periodic-table, lingo-lessons, mental-math, and public support repositories
currently contain no indexed old-domain references, but their repository
metadata and deployed domains must still be verified.

## External metadata

Update the GitHub organization website from `https://opengrounds.org` to
`https://openappsstudio.com`. Update repository homepage URLs for all affected
repositories. Where App Store Connect, Chrome Web Store, Google Search Console,
or OAuth settings expose old URLs, retain the old redirect and update the public
URL only when authenticated access is available.

No email migration is included because neither domain currently has MX records
and the existing public support addresses are Gmail accounts. This avoids
inventing a mail provider or breaking support.

## DNS and hosting

GoDaddy remains the registrar and authoritative DNS provider. The new apex
currently uses GoDaddy parking A records and `www` points back to the parked
apex. Replace only the web records Vercel explicitly requests. Preserve any
unrelated TXT or future mail records.

The old domain already uses GoDaddy nameservers with Vercel web records. It will
remain configured so Vercel can serve redirects and renew certificates.

## Verification

For every new hostname:

- DNS resolves to Vercel.
- HTTPS succeeds with a certificate covering the hostname.
- The expected page returns `200` before redirect activation.
- Canonical, Open Graph, structured-data, sitemap, robots, privacy, and support
  URLs use `openappsstudio.com`.
- Internal links do not reference `opengrounds.org` or “Open Grounds Studio.”

For every old hostname:

- HTTP and HTTPS return a permanent redirect.
- The destination hostname is correct.
- The path and query string are preserved.
- There is a single redirect hop where practical.

Repository checks include clean diffs, existing project validators/builds, and
a final organization-wide old-reference scan.

## Rollback

Rollback never deletes records. If a new hostname fails, keep the old hostname
serving content, restore its previous Vercel production assignment if changed,
and remove only the newly added redirect rule. GoDaddy's original new-domain
parking values are recorded before replacement. Repository and Vercel project
renames may be reversed because their prior names are known, while GitHub's
automatic repository redirects protect existing clones and links.
