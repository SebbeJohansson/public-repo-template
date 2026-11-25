# AGENTS.md

Instructions for AI agents working in this public repository. Everything committed here is visible to the world, forever. Git history counts.

## Never commit

- Secrets: API keys, tokens, passwords, private keys, `.env` files, cloud credentials.
- Internal references: company names, internal hostnames, internal URLs, ticket IDs (such as Linear or Jira keys), employer email addresses, internal chat links.
- Personal data: real names, emails or phone numbers of other people. Fixtures and examples use fake people.
- Customer or production data, even sampled or "anonymised".
- Machine-specific paths such as `/home/<user>/...`.

If you find any of these already in the tree, stop and tell the maintainer. Removing it in a new commit is not enough: the secret must be rotated, and history may need rewriting.

## What may be committed

- Source, tests, docs, examples with fake data, and build config.
- Lockfiles (`go.sum`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`).
- `.env.example` with placeholder values only. Real `.env` files stay in `.gitignore`.

Before every commit, read `git diff --staged` and check it against "Never commit".

## Commits

- One commit per feature or fix.
- Single-line message in the imperative, matching the style of recent commits.
- Do not push unless asked. Never force-push `main`.

## Issues

- Search existing issues before opening one.
- Describe the problem, the expected behaviour and how to reproduce it. No internal context.
- Security problems are not issues. Follow `SECURITY.md`.

## Pull requests

- Branch from `main`. Keep each PR to one change.
- Describe what changed and why. Link the public issue it closes, if any.
- Tests and lint pass before the PR is opened.
- Do not merge your own PR unless the maintainer said to.

## Releases

- Versions follow [Semantic Versioning](https://semver.org). Tags are `vMAJOR.MINOR.PATCH`.
- Update the changelog (or release notes) in the same change that bumps the version.
- Tag only a commit on `main` whose CI is green.
- Publishing steps depend on the package type:
  - Go: [docs/go-packages.md](docs/go-packages.md)
  - npm: [docs/npm-packages.md](docs/npm-packages.md)
- Ask the maintainer before publishing. Publishing cannot be undone.

## Licensing

Default is MIT. See [docs/licenses.md](docs/licenses.md) for when to pick another.

## Pre-publish checklist

1. `git grep -nIiE 'secret|token|password|api[_-]?key'` shows only placeholders.
2. No internal names or URLs remain (`git grep` for the company name and internal domains).
3. `LICENSE`, `README.md` and `SECURITY.md` are filled in, with no `TODO` placeholders left.
4. CI is green on `main`.
