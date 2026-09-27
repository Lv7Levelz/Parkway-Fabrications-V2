# Migration and launch plan

## Inventory and redirects

Because the current domain could not be crawled in this environment, export the live CMS URL list, XML sitemap, server logs, GA4 landing pages and Search Console pages/links before freeze. Crawl both HTTP/HTTPS and www/non-www variants. Map every indexable URL one-to-one to the nearest genuinely equivalent new page; do not bulk redirect unrelated content to the homepage. Retain high-value copy/media evidence for factual review. Test redirect chains, loops, parameters, casing, trailing slashes, PDFs and legacy image backlinks.

## Release sequence

1. Approve facts, copy, NAP, privacy text, project evidence and photography.
2. Complete browser/device/accessibility review and independent security review.
3. Provision production database, storage, email, secrets, WAF, backups, monitoring and restore procedures.
4. Run full unit/integration/E2E suite, upload malware tests, authenticated admin tests and email delivery probes.
5. Crawl staging with authentication; validate status codes, canonicals, titles, headings, internal links, schema and redirects while retaining noindex.
6. Configure GA4 or approved alternative, consent mode where required, conversion events, Search Console, Bing Webmaster Tools, Google Business Profile and Bing Places.
7. Lower DNS TTL, freeze legacy changes, take a final backup, deploy, run smoke tests, then set `DEPLOYMENT_ENV=production` and expose the production robots/sitemap only on the canonical host.
8. Change DNS, validate TLS/HSTS and host redirects, submit the XML sitemap, inspect priority URLs and annotate analytics.
9. Crawl production immediately and again after 24 hours, 7 days and 30 days. Monitor 404s, 5xx, indexing, Core Web Vitals, rankings and enquiries daily during the first week.

Never remove staging noindex settings globally: promote a reviewed immutable release and enable indexing only through production environment configuration.
