# Parkway Fabrications — website platform

A dependency-light first complete version of Parkway Fabrications' website: a bespoke public experience, structured service architecture, secure-by-default RFQ prototype and authenticated enquiry workspace.

## Run locally

```bash
npm start
# http://localhost:3000
```

The default build behaves as **staging**: every response sends `X-Robots-Tag: noindex, nofollow`, pages include a matching meta directive, and `robots.txt` disallows all crawling. Only set `DEPLOYMENT_ENV=production` after the launch checklist has been signed off.

## Admin access

Generate a password hash without storing the password:

```bash
node -e "const c=require('crypto'),s=c.randomBytes(16).toString('hex'); console.log(s+':'+c.scryptSync(process.argv[1],s,32).toString('hex'))" 'your-password'
ADMIN_USER=parkway ADMIN_PASSWORD_HASH='<salt>:<hash>' npm start
```

Visit `/admin/`. Local JSON persistence and filesystem uploads demonstrate the workflow only. Production must use the PostgreSQL/object-storage architecture in `docs/TECHNICAL.md`.

## Commands

```bash
npm test
npm run check
```

## Status

This is browser-review software, **not production-ready**. It requires client factual approval, human visual review, photography, email/storage/database configuration, penetration/security review and a production deployment review.
