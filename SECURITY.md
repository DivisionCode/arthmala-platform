# Security Policy

Arthmala is owned and operated by Sushraj Ventures. We take the security of the
site, the API and the people who use them seriously, and we welcome reports from
anyone who finds a vulnerability.

## Supported versions

Only the latest release on the `main` branch, which is what runs at
https://arthmala.vercel.app, receives security fixes.

| Version | Supported |
| ------- | --------- |
| 1.0.x   | Yes       |
| < 1.0   | No        |

## Reporting a vulnerability

Please report vulnerabilities privately by email to
**sushrajventures@gmail.com** with the subject line `Security: Arthmala`.

Do **not** open a public GitHub issue, pull request or discussion for a
security problem.

Include as much of the following as you can:

- A description of the issue and its likely impact.
- The affected URL, API endpoint (for example `POST /api/auth/verify-otp`) or
  file and line in this repository.
- Steps to reproduce, or a minimal proof of concept.
- Any logs, screenshots or request and response samples, with personal data
  removed.

## What to expect

- We will acknowledge your report as soon as we can.
- We will keep you informed while we investigate and fix the issue.
- We will credit you in the release notes if you would like to be credited.

## Scope

In scope:

- The web client in `client/` and the site at https://arthmala.vercel.app.
- The API in `server/` and `api/`, served under `/api/*`.
- Build and deployment configuration in this repository (`vercel.json`,
  `package.json` files and `.github/workflows/`).

Out of scope:

- Denial of service through volumetric traffic.
- Social engineering of Sushraj Ventures staff or customers.
- Vulnerabilities in third-party services (Vercel, MongoDB hosting, SMTP
  providers, Google Fonts, analytics providers) that should be reported to
  those vendors directly.

## Safe harbour

We will not pursue action against researchers who act in good faith, avoid
privacy violations and service disruption, do not access or modify other
people's data beyond what is needed to demonstrate the issue, and give us
reasonable time to fix the problem before any disclosure.

## Handling secrets

- Real `.env` files must never be committed. Only `client/.env.example` and
  `server/.env.example`, which hold placeholder values, belong in the repository.
- Production secrets (`MONGO_URI`, `JWT_SECRET`, `ADMIN_TOKEN`, `SMTP_*`) are
  configured as environment variables in the Vercel project settings.
- If a secret is ever committed, treat it as compromised: rotate it first, then
  remove it from the repository.
