# Release handoff report

Repository label: `synthetic-node-app`

> This report compares a small set of declared metadata. It is not a code review, security audit, build result, deployment check, release guarantee, or certification.

## Findings

### 1. Node.js version declarations disagree

Rule: `NODE_MAJOR_MISMATCH`

Evidence:
- `.nvmrc:1` — `20`
- `.node-version:1` — `22`
- `Dockerfile:1` — `Node base image major 18`
- `package.json:5` — `engines.node = >=20 <22`

Interpretation: The supported pinned declarations name Node.js majors 18, 20, 22. The package.json engines.node range `>=20 <22` excludes these pinned declarations: .node-version (22), Dockerfile (Node base image major 18). This does not prove a build or release failure.

Next check: Ask the maintainer which runtime is intended, then run the documented checks with that version.

Confidence boundary: Directly observed metadata; operational behavior is not verified.

### 2. `npm run release` is referenced but not declared

Rule: `NPM_SCRIPT_NOT_DECLARED`

Evidence:
- `.github/workflows/ci.yml:7` — `npm run release`

Interpretation: No `scripts.release` entry was found in `package.json`; references occur at .github/workflows/ci.yml:7.

Next check: Confirm whether `release` was renamed, intentionally supplied by another tool, or should be added to the package scripts.

Confidence boundary: Directly observed metadata; operational behavior is not verified.

### 3. `npm run test` is referenced but not declared

Rule: `NPM_SCRIPT_NOT_DECLARED`

Evidence:
- `README.md:6` — `npm run test`

Interpretation: No `scripts.test` entry was found in `package.json`; references occur at README.md:6.

Next check: Confirm whether `test` was renamed, intentionally supplied by another tool, or should be added to the package scripts.

Confidence boundary: Directly observed metadata; operational behavior is not verified.

## Files in scope

- `.github/workflows/ci.yml`
- `.node-version`
- `.nvmrc`
- `Dockerfile`
- `README.md`
- `package.json`

### Allowlist inventory

- `package.json`: read
- `.nvmrc`: read
- `.node-version`: read
- `Dockerfile`: read
- `README.md`: read
- `.github/workflows/ci.yml`: read

## Explicit limits

- No source files, lockfile contents, `.git` history, environment files, credentials, or private keys were read.
- No repository commands, package managers, builds, tests, containers, or installation hooks were run.
- No network connection was made and no repository content was uploaded.
- Workflow parsing is deliberately narrow: only recognized `npm run NAME` calls beneath simple inline or indentation-based `run:` keys are considered. Other YAML and shell forms were not validated.
- Version comparison supports pinned declarations and simple major-boundary `engines.node` ranges. Nonzero minor or patch bounds, OR expressions, aliases, templating, and less common formats are not compared.
- Repository behavior and release success remain unverified; a maintainer must confirm the intended setup and run authorized checks.

## Reviewer handoff

Use each finding as a question for the repository maintainer. Verify the intended runtime and scripts in an owner-approved environment before changing or releasing the application.
