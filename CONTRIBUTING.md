# Contributing to Arthmala

Thank you for helping to improve Arthmala. This repository is proprietary
software owned by Sushraj Ventures (see [LICENSE](LICENSE)); contributions are
accepted from people who have been given access by the owner. By opening a pull
request you agree that your contribution becomes the property of Sushraj
Ventures under the same terms.

## Contents

- [Prerequisites](#prerequisites)
- [Local setup](#local-setup)
- [Running and building](#running-and-building)
- [Branch naming](#branch-naming)
- [Commit messages](#commit-messages)
- [Pull requests](#pull-requests)
- [Continuous integration](#continuous-integration)
- [Releasing](#releasing)
- [Security issues](#security-issues)

## Prerequisites

- Node.js 22 (the version is pinned in [`.nvmrc`](.nvmrc); run `nvm use` in the
  repository root).
- npm (bundled with Node.js).
- A MongoDB connection string for the API.
- Optional: SMTP credentials if you need emails to be sent. Without them the API
  still stores inquiries and prints login codes to the server console.

## Local setup

The client and the API are separate npm packages, each with its own lockfile.

```bash
git clone https://github.com/DivisionCode/arthmala-platform.git
cd arthmala-platform

# API
cd server
cp .env.example .env        # then fill in MONGO_URI, JWT_SECRET, ADMIN_TOKEN, ...
npm ci

# Client
cd ../client
cp .env.example .env.local  # VITE_API_URL=http://localhost:5000 for local work
npm ci
```

Never commit a real `.env` or `.env.local` file. They are ignored by
[`.gitignore`](.gitignore).

## Running and building

| Task | Directory | Command |
| ---- | --------- | ------- |
| Start the API with auto-reload (nodemon) | `server/` | `npm run dev` |
| Start the API without reload | `server/` | `npm start` |
| Syntax-check the API and serverless entry | repository root | `find server api -name '*.js' -not -path '*/node_modules/*' -print0 \| xargs -0 -n1 node --check` |
| Start the client dev server (Vite, port 5173) | `client/` | `npm run dev` |
| Build the client for production | `client/` | `npm run build` |
| Preview the production build | `client/` | `npm run preview` |
| Reproduce the Vercel build | repository root | `npm install && npm run build` |

The API listens on `PORT` (default `5000`). The client calls the API at
`VITE_API_URL`, so set it to `http://localhost:5000` when running both locally.

There is no automated test suite yet. Before opening a pull request, run the
client build and the API syntax check above, and exercise the changed flow by
hand against a development database.

## Branch naming

Create a branch from an up-to-date `main` using one of these prefixes:

| Prefix | Use for | Example |
| ------ | ------- | ------- |
| `feature/` | New functionality | `feature/whatsapp-otp` |
| `fix/` | Bug fixes | `fix/order-tracking-eta` |
| `chore/` | Tooling, dependencies, configuration | `chore/bump-express` |
| `docs/` | Documentation only | `docs/api-overview` |

Use short, lowercase, hyphenated names.

## Commit messages

This repository follows [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):

```
<type>(<optional scope>): <summary in the imperative mood>

<optional body explaining what and why>

<optional footer, e.g. BREAKING CHANGE: ...>
```

Common types: `feat`, `fix`, `docs`, `chore`, `refactor`, `perf`, `style`,
`build`, `ci`, `test`, `revert`. Useful scopes: `client`, `api`, `auth`,
`admin`, `vercel`, `deps`.

Examples:

```
feat(auth): add passwordless email otp authentication
fix(vercel): use zero-config catch-all route for the API
docs: document environment variables
```

## Pull requests

1. Push your branch and open a pull request into `main`.
2. Fill in the pull request template, including how you tested the change.
3. CI must pass before the pull request is merged.
4. Code owners (see [`.github/CODEOWNERS`](.github/CODEOWNERS)) review every
   pull request.
5. Add an entry under `[Unreleased]` in [CHANGELOG.md](CHANGELOG.md) for any
   user-facing or operational change.
6. Keep pull requests focused. Do not mix refactoring with behaviour changes.
7. Do not change `vercel.json`, the root `build` script or the root
   `postinstall` script without testing a Vercel preview deployment, because
   production deploys from this repository.

## Continuous integration

[`.github/workflows/ci.yml`](.github/workflows/ci.yml) runs on every push to
`main` and on every pull request, using the Node.js version from `.nvmrc`:

- **client**: `npm ci` and `npm run build` in `client/`.
- **api**: `npm ci` in `server/`, then `node --check` on every JavaScript file
  in `server/` and `api/`.

CI needs no secrets.

## Releasing

Arthmala uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html). The
root, client and server packages always share one version number.

1. Make sure `main` is green and up to date.
2. Decide the new version `X.Y.Z` (major for breaking changes, minor for new
   features, patch for fixes).
3. Bump the version in all three packages without creating a tag:

   ```bash
   npm version X.Y.Z --no-git-tag-version
   npm version X.Y.Z --no-git-tag-version --prefix client
   npm version X.Y.Z --no-git-tag-version --prefix server
   ```

   This updates `package.json` in the root, `client/` and `server/`, and the
   `client/` and `server/` lockfiles.
4. In [CHANGELOG.md](CHANGELOG.md), move the `[Unreleased]` entries into a new
   `[X.Y.Z] - YYYY-MM-DD` section and update the comparison links at the bottom.
5. Commit with `chore(release): vX.Y.Z` and merge into `main` through a pull
   request.
6. Tag the merge commit and push the tag:

   ```bash
   git tag -a vX.Y.Z -m "vX.Y.Z"
   git push origin vX.Y.Z
   ```

7. Create a GitHub Release for the tag, using the changelog section as the
   release notes.

## Security issues

Do not report security problems in public issues or pull requests. Follow
[SECURITY.md](SECURITY.md) instead.
