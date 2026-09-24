# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2026-09-24

First versioned release. It covers all work since the initial commit on
2025-07-17.

### Added

- Vue 3 single-page client scaffolded with Vite, Vue Router, Pinia, and a
  Tailwind CSS, PostCSS and Autoprefixer build pipeline.
- Home, Products and About pages with a shared site layout, artwork card and
  artwork modal components.
- Express API backed by MongoDB through Mongoose, starting with an `Artwork`
  model and the `GET /api/artworks` endpoint.
- Product catalogue driven by a Pinia artwork store that loads artworks from the
  API, with category filtering and sorting by date, price and name.
- Craft imagery served as WebP files from `client/public/images`.
- Product detail page at `/products/:id` with an image gallery, related pieces,
  a WhatsApp enquiry link and Product structured data (JSON-LD).
- Inquiry list drawer that keeps selected pieces in the browser and submits a
  commission inquiry to `POST /api/inquiries`.
- Inquiry emails through Nodemailer: a notification to the studio, a
  confirmation to the customer and a quote email when an inquiry is marked
  as quoted. Email sending is skipped cleanly when SMTP is not configured.
- Admin dashboard at `/admin`, unlocked with a shared admin key:
  - Overview with inquiry totals, a 30-day activity series, popular pieces and
    categories, ageing inquiries and a quote pipeline (`GET /api/stats`).
  - Inquiry management with status changes, quotes, internal notes, order
    status and ETA updates, and CSV export.
  - Artwork management with create, edit and delete, gallery images and image
    upload by file picker or drag and drop (`POST /api/uploads`).
- Public order tracking page at `/orders/:token` backed by
  `GET /api/orders/:token`, using an unguessable tracking token issued when an
  order is first updated.
- Search engine and sharing support: per-page title and meta tags, Open Graph
  and Twitter card tags, JSON-LD, `sitemap.xml` and `robots.txt` endpoints, a
  web app manifest, favicon and Open Graph card image.
- Optional privacy-first analytics that loads Plausible or Google Analytics 4
  only when the matching `VITE_*` variable is set.
- Floating WhatsApp and inquiry list buttons, a site footer, an application error
  boundary and a not-found page. `/portfolio` redirects to the home page.
- Responsive layouts for small screens across the public and admin pages.
- Vercel deployment for the whole repository: a root `package.json` whose
  `postinstall` installs the API and whose `build` builds the client, a
  serverless function in `api/` that wraps the Express app, and a
  `vercel.json` fallback that serves the SPA.
- Passwordless sign-in: `/login` page, auth store, `User` model and the
  `POST /api/auth/request-otp`, `POST /api/auth/verify-otp` and
  `GET /api/auth/me` endpoints. A six-digit code is emailed and exchanged for
  a JSON Web Token valid for seven days.
- `GET /api/health` liveness endpoint.
- Project documentation and repository hygiene: README, CHANGELOG, LICENSE,
  SECURITY and CONTRIBUTING guides, code owners, issue and pull request
  templates, a GitHub Actions CI workflow, `.editorconfig` and `.nvmrc`.

### Changed

- Redesigned and finalised the home page.
- Set the Vite `base` to `./` for deployment.
- The API `start` script now runs `server.js`.
- The client falls back to same-origin `/api/*` requests when `VITE_API_URL`
  is empty.
- Package names are now `arthmala-platform` (root), `arthmala-client` and
  `arthmala-api`, all at version 1.0.0 with proprietary licence metadata,
  repository and homepage fields, and a Node.js 22 or later engine
  requirement.
- Removed the placeholder `test` script from the API package.

### Fixed

- Vercel SPA routing for the nested client build.
- Vercel API rewrite destination.
- API routing on Vercel now uses the zero-config catch-all function
  `api/[...slug].js` instead of rewrites.

### Security

- Stopped tracking `.env` files and `node_modules`, and added `.env.example`
  templates with placeholder values only.
- CORS restricted to an allow-list set by `CLIENT_ORIGIN`, Helmet security
  headers and a 1 MB JSON body limit.
- Per-IP rate limits on inquiry submission, the admin inquiry and statistics
  routes, and uploads.
- Admin routes require the `x-admin-token` header, compared in constant time.
- Uploads accept only JPEG, PNG, WebP, GIF and AVIF images up to 8 MB and are
  stored under random file names.
- Inquiry form spam protection with a honeypot field and a minimum fill time.
- The public order tracking endpoint returns only the customer's first name,
  items, quoted price and order progress.

[Unreleased]: https://github.com/DivisionCode/arthmala-platform/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/DivisionCode/arthmala-platform/releases/tag/v1.0.0
