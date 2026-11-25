# Go packages

## Module path

- The module path is the repository URL without the scheme: `github.com/SebbeJohansson/<repo>`.
- Set it once: `go mod init github.com/SebbeJohansson/<repo>`.
- Several modules in one repo: each lives in a subdirectory with its own `go.mod`, tagged `<subdir>/vX.Y.Z`.
- Major version 2 or higher: the path ends in `/v2` (`github.com/SebbeJohansson/<repo>/v2`), and so do all imports.

## Versioning with tags

- Tags are `vMAJOR.MINOR.PATCH`, created on `main`. The `v` is required.
- `v0.x` means no stability promise. Move to `v1.0.0` when the API is settled.
- Breaking change after `v1`: new major version (see above).
- Never move or delete a published tag. The proxy caches it permanently. Publish a new patch version instead.
- Retract a bad version with a `retract` directive in `go.mod`, released in a newer version.

```sh
git tag v0.1.0
git push origin v0.1.0
```

## go.mod hygiene

- Set `go` to the oldest Go version you support, not the newest installed.
- Run `go mod tidy` before every commit. Commit `go.mod` and `go.sum` together.
- No `replace` directives pointing at local paths in a released version.
- No private modules in `require`. A public package must build for anyone.
- Run `go vet ./...` and `go test ./...` before tagging.

## Publishing via the Go module proxy

There is no publish step. Pushing a tag is enough. The proxy fetches it on first request.

1. Push the tag.
2. Warm the proxy: `GOPROXY=https://proxy.golang.org go list -m github.com/SebbeJohansson/<repo>@v0.1.0`
3. Check that the version shows up at `https://pkg.go.dev/github.com/SebbeJohansson/<repo>`.

Package docs come from doc comments. Every exported identifier gets one, starting with its name.
