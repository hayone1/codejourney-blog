# Instructions for AI agents

This repository holds only the posts of the CodeJourney blog. The site lives in `hayone1/codejourney`, and its handoff note `.notes/handoff.md` describes how posts reach the site. Read that note before you change a workflow here.

Never commit `.env`: the repository is public, and `.env` holds `SITE_DISPATCH_TOKEN`.

## Expiring credentials

At the start of every session, compare today's date with the dates in this table. When a date is less than 30 days away, or already past, tell the owner at the start of your first reply, before other work: name the credential, the date, and the renewal steps. Then update the date in this table and in the other two repositories (`hayone1/codejourney`, `hayone1/codejourney-platform`, `hayone1/codejourney-blog`) after the owner renews the credential.

| Credential | Where it lives | Expires | Renewal steps |
|------------|----------------|---------|---------------|
| `SITE_DISPATCH_TOKEN` | Secret of the environment `site-dispatch` in `hayone1/codejourney-blog`, copy in `.env` of `codejourney-blog` | 2027-11-08 23:00 UTC | "Renew SITE_DISPATCH_TOKEN" in `.notes/handoff.md` of `hayone1/codejourney` |

GitHub shows the end date of the token in the response header `github-authentication-token-expiration` of every API call with the token.
