# CLAUDE.md

## Project

landfall is a Go project for a code-navigation-focused MCP server: repository/codebase navigation, symbol- and structure-aware retrieval, with an MCP-native interface from day one. It is a **separate product contract from dir2mcp** — it shares the Dirstral ecosystem but **no direct imports from `dir2mcp` are allowed** (see `README.md`).

> Status: early-stage scaffold. The binary currently prints `not yet implemented` and exits non-zero (`cmd/landfall/main.go`). Most subsystems described in the product scope are not built yet. Verify against the actual tree before assuming a package exists.

## Repository layout

- `cmd/landfall`: binary entrypoint (currently a stub)
- `go.mod`: module `github.com/Dirstral/landfall`, Go 1.24, no third-party dependencies yet
- `.github/workflows/go.yml`: CI (build + test on push/PR to `main`)
- `.goreleaser.yml`: release build/archive config
- `docs/landfall-prd.md`: referenced from `cmd/landfall/main.go` as the product scope/architecture source (not yet committed)

## Build and test

There is no `Makefile`; use the Go toolchain directly. CI (`.github/workflows/go.yml`) runs only the build and test steps:

- Build: `go build ./...` — run by CI
- Test: `go test ./...` — run by CI
- Vet: `go vet ./...` — not in CI; run locally before opening a PR
- Format: `gofmt -l .` — not in CI; run locally (should print nothing)

## Releasing

Releases use GoReleaser (`.goreleaser.yml`): it builds `./cmd/landfall` and publishes `tar.gz` archives (with `LICENSE*` and `README.md`). `release.prerelease` is `auto`, so a tag like `v0.1.0-rc1` is marked pre-release automatically.

## Working conventions

- Keep changes scoped to the issue.
- Do not import from `dir2mcp` — the no-direct-imports boundary is a hard product constraint.
- Keep dependency additions minimal and justified; the module currently has zero third-party deps.
- Do not log secrets or raw sensitive payloads.
- If behavior changes, update tests and docs in the same PR.
- Prefer deterministic behavior and explicit error handling.

## PR checklist

- [ ] `go build ./...` and `go test ./...` pass locally
- [ ] `go vet ./...` clean and `gofmt` applied
- [ ] No imports from `dir2mcp`
- [ ] New/changed behavior has test coverage
- [ ] `README.md` and `docs/` remain truthful
- [ ] No unrelated files changed

## Known gotchas

- The binary is a stub: `landfall` writes `not yet implemented` to stderr and exits `1`. Don't mistake this for a runtime bug.
- Module path casing is `github.com/Dirstral/landfall` (capital `D`) while the GitHub repo is `dirstral/landfall`; keep import paths matching `go.mod`.
- No `Makefile` exists — commands referencing `make ...` do not apply here; use the `go` toolchain.
- CI workflow permissions are restricted to `contents: read`; new CI steps must not assume broader GITHUB_TOKEN scope.
