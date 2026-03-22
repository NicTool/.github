# NicTool Monorepo Copilot Instructions

## Purpose

This repository is a multi-package NicTool workspace that includes:

- A Node.js API service (`api`) that talks to MySQL and uses JWT auth.
- Shared DNS/domain libraries (`dns-resource-record`, `dns-zone`, `dns-nameserver`, `validate`).
- A web client (`client`).
- A legacy Perl NicTool stack (`NicTool`).

When making changes, treat this as a set of related but independently runnable packages.

## How The Pieces Fit Together

High-level dependency flow:

1. `dns-resource-record`: record-level parsing/validation/conversion.
2. `dns-zone`: zone-level import/export and zone-rule validation, built on `dns-resource-record`.
3. `dns-nameserver`: nameserver config parsers, built on `dns-zone`.
4. `validate`: NicTool object schema validation (group/user/zone/etc).
5. `api`: HTTP API service using `@nictool/validate` and `@nictool/dns-resource-record`, plus MySQL.
6. `client`: browser UI that consumes API behavior.
7. `NicTool`: legacy server/client implementation (Perl stack).

## Monorepo Working Rules

- ## Commands

- All commands are run from within the relevant package directory (e.g., `cd NicTool` or `cd dns-resource-record`).
- There is no single root `package.json` for all packages. Run install/test/lint inside the package you are changing.
- Keep changes scoped. If you need to change API behavior, update the API package and its tests, but do not make sweeping changes to shared libraries unless necessary.
- Respect each package's existing module system:
  - ESM packages exist (`api`, `dns-resource-record`, `dns-zone`, `dns-nameserver`, `client`).
  - Convert CommonJS in (`validate`) to ESM
- Preserve package-local formatting/lint style. Do not run sweeping repo-wide formatting.

## API Package Guidance (`api`)

- Service entry: `server.js` calls `routes/index.js` startup.
- Routing lives in `routes/*.js`; data access and business logic live in `lib/*.js`.
- Route validation schemas come from `@nictool/validate`.
- Config is YAML-driven via `conf.d/*.yml` and loaded through `lib/config.js`.
- Tests are `node --test`, orchestrated by `test.sh`.
- `test.sh` sets up and tears down DB fixtures; keep tests compatible with that flow.

If you change an API route or payload shape:

1. Update route handler in `routes/`.
2. Update related logic in `lib/` as needed.
3. Update route/lib tests (`routes/*.test.js`, `lib/*.test.js`).
4. Keep validation contracts in sync with `@nictool/validate`.

## Database And Config Safety

- API tests expect a local MySQL instance and schema initialization from `api/sql`.
- Never introduce real secrets.
- Prefer environment overrides or test/dev config blocks instead of hardcoding credentials.
- Do not commit local machine paths, ad hoc cert material, or one-off debug config changes.

## Shared Library Guidance

### `validate`

- Contains object/schema validation used by API routes.
- CommonJS exports (`module.exports` pattern).
- Tests run with `node --test`.

### `dns-resource-record`

- Record-level parser/validator/import/export library.
- Focus on RFC-compliant RR behavior and conversion fidelity.
- Tests use Mocha.

### `dns-zone`

- Zone-level import/export and coexistence rules for records.
- Uses `dns-resource-record` underneath.
- Tests use Mocha.

### `dns-nameserver`

- Parses nameserver config formats (bind, knot, maradns, nsd, tinydns).
- Uses `dns-zone`.
- Tests use Mocha.

## Client Guidance

- `client`: Vite-based web UI with Bootstrap/Sass.
- Keep API contract assumptions aligned with current API responses.
- Avoid changing API payload formats only to satisfy UI code unless requested.

## Legacy NicTool (`NicTool`)

- This is the legacy Perl implementation and has different conventions/tooling.
- Do not apply Node package assumptions in this tree.
- Make minimal, isolated changes when touching this area.

## Test And Lint Commands By Package

- `api`
  - `npm test`
  - `npm run lint`
  - `npm run prettier`
- `validate`
  - `npm test`
  - `npm run lint`
  - `npm run prettier`
- `dns-resource-record`
  - `npm test`
  - `npm run lint`
  - `npm run prettier`
- `dns-zone`
  - `npm test`
  - `npm run lint`
  - `npm run prettier`
- `dns-nameserver`
  - `npm test`
  - `npm run lint`
- `client`
  - `npm run develop` (or `npm run start`)
  - `npm run build`
  - `npm run lint`
  - `npm run prettier`

Run the smallest relevant test/lint set for changed files first, then broaden if needed.

## Change Style Expectations

- Keep patches small and targeted.
- Preserve existing naming and file layout.
- Add or update tests for behavior changes.
- Avoid dependency churn unless required for the task.
- Prefer explicit error handling and clear return shapes in API code.

## Commit Hygiene

- Keep commits focused by package or concern.
- Include a brief rationale in commit messages when behavior changes.
- Mention cross-package impact explicitly when a change in one package affects another.
