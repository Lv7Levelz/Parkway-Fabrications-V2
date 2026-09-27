# Technical architecture and operations

## Environments

- **Development:** native Node server, watched files, local JSON records and filesystem uploads; synthetic test enquiries only.
- **Staging:** production-like infrastructure with anonymised data, HTTP authentication/IP restriction, and three noindex safeguards (header, meta and restrictive robots). Never connect the staging hostname in Search Console.
- **Production:** two stateless application instances behind a managed WAF/load balancer; managed PostgreSQL; private UK/EU object storage with encryption and short-lived signed downloads; transactional email; managed secrets; central logs, alerts and backups.

The dependency-light prototype is intentionally easy to review. Before production, replace the local sequence with a PostgreSQL transaction/sequence and unique reference constraint; persist enquiries and file metadata in the database; stream uploads directly to quarantined object storage; scan with an antivirus service; use a queue/outbox for customer and internal emails; and replace Basic authentication with an identity provider offering MFA, role-based access and auditable sessions.

## RFQ flow

1. Browser validates required contact and requirement fields, file extensions and per-file size.
2. Server independently validates required fields, email shape, allowlisted extension, MIME type, per-file 10 MB and total 20 MB limits.
3. A honeypot and IP rate limit deter automation; production adds Turnstile or equivalent after privacy review and a shared Redis/WAF rate limit.
4. Files receive cryptographically random storage names, are written with private permissions and are never served by the public static handler.
5. The atomic development store records contact, specification, file metadata, timestamp and status, and produces `PF-YYYY-000000` references.
6. Production commits through a database transaction, publishes an outbox event, scans files and sends both a restrained customer acknowledgement and an internal notification. Delivery failures alert operations without exposing drawing contents in logs.

## Admin and planned domain model

The protected workspace supplies an enquiry list, search, status filter and sorting-ready timestamps. The data model already separates contact, requirement, files and status. Production extends this with enquiry detail routes, authorised signed file access, optimistic status transitions, notes, assignee, quote value, follow-up date and immutable audit events. Later additions can expose CSV export, CRM webhooks and aggregate reporting without changing the public form contract.

## Security and privacy

Current controls include output encoding, parameter length caps, upload allowlists, MIME and size checks, random names, non-public storage, atomic writes, rate limiting, honeypot, password hashing support, timing-safe comparison, generic errors, CSP, frame denial, MIME sniffing prevention, restrictive permissions policy and noindex admin responses.

Production gates: threat model; independent penetration test; CSRF protection for cookie-authenticated admin mutations; MFA/RBAC; malware scanning; database and object-store least privilege; KMS rotation; TLS/HSTS; WAF limits; dependency and container scanning; immutable audit log; daily encrypted backups plus quarterly restore tests; 30-day operational log retention; approved enquiry/file retention and deletion jobs; DSAR process; processor agreements; incident response; and redaction of personal data from observability.

## Performance and accessibility

The platform uses server-rendered HTML, system fonts, no external trackers, no framework runtime and one small deferred script. Geometry is inline SVG/CSS, so there is no LCP image request or layout shift. Production photography must use fixed dimensions, AVIF/WebP `srcset`, an eagerly loaded hero candidate only where needed and lazy loading below the fold. CDN compression and immutable fingerprinted assets are deployment tasks.

Semantic landmarks, a skip link, visible focus, labelled form controls, live status messaging, logical DOM order, keyboard navigation, high contrast and reduced-motion handling target WCAG 2.2 AA. Manual screen-reader, 200% zoom, reflow, keyboard and mobile-device tests remain launch gates.

## Monthly retainer

Weekly form-delivery probes and uptime checks; monthly dependency/image rebuilds, security review, backup verification, crawl/indexation review, Search Console/Bing and conversion reporting; quarterly restore, accessibility and Core Web Vitals reviews; and an editorial cadence for approved projects, reviews and service improvements. Alert on elevated API failures, email bounces, upload scan failures, database saturation and quote-volume anomalies.
