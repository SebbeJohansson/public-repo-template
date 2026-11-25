# npm packages

## package.json fields

Required for a public package:

- `name`: see "Names".
- `version`: semver, see "Versioning".
- `description`: one line.
- `license`: SPDX identifier, for example `MIT`.
- `repository`: `{ "type": "git", "url": "git+https://github.com/SebbeJohansson/<repo>.git" }`
- `bugs`, `homepage`: link to the repo's issues and README.
- `type`: `module` for ESM packages.
- `exports` (and `types` for TypeScript): the public entry points. Anything not listed is private.
- `engines`: the supported Node version range.
- `files`: see "What gets published".
- `publishConfig`: `{ "access": "public", "provenance": true }`

## Names

- Prefer a scope: `@sebbejohansson/<name>`. It avoids name clashes and typosquatting.
- Scoped packages are private by default. Publish with `--access public` (or `publishConfig.access`).
- Check the name is free first: `npm view <name>`.

## Versioning

- Semantic Versioning. `npm version patch|minor|major` bumps `package.json`, commits and tags `vX.Y.Z`.
- `0.x` means no stability promise.
- Prereleases use a tag: `1.2.0-beta.1`, published with `npm publish --tag beta`.
- A published version is permanent. Fix mistakes with a new version, and mark bad ones with `npm deprecate`.

## What gets published: `files` and `.npmignore`

- Prefer an allowlist: `"files": ["dist", "README.md", "LICENSE"]`. `package.json`, README and LICENSE are always included.
- Use `.npmignore` only when an allowlist does not fit. It replaces `.gitignore` for publishing.
- Check before every publish: `npm pack --dry-run`. Look for `.env`, tests, source maps with secrets, and internal docs.

## Provenance

Provenance links the package to the exact commit and workflow that built it.

1. Publish from GitHub Actions, not a laptop.
2. Give the job `permissions: { contents: read, id-token: write }`.
3. Publish with `npm publish --provenance --access public`.

```yaml
on:
  push:
    tags: ['v*']
jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          registry-url: https://registry.npmjs.org
      - run: npm ci
      - run: npm test
      - run: npm publish --provenance --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Publishing checklist

1. Tests and build pass. CI is green on `main`.
2. `npm pack --dry-run` lists only intended files.
3. Version bumped and changelog updated.
4. Tag pushed, workflow publishes with provenance.
5. `npm view <name> version` shows the new version.

Use a granular automation token stored as a repository secret, with 2FA on the account. Never commit an `.npmrc` that contains a token.
