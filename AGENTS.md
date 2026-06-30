# AGENTS.md

## Purpose

Operational guide for coding agents working in this repository.

## Before you start

- Re-check whether the requested plan still matches the current codebase before making changes — this repo is an early-stage scaffold and moves fast.
- Review relevant context first: `README.md` (scope and the dir2mcp boundary) and `docs/landfall-prd.md` (product scope/architecture, referenced from `cmd/landfall/main.go`) when present.
- Preserve existing architecture and conventions unless the issue explicitly requires a refactor.

## Project summary

landfall is a Go, code-navigation-focused MCP server: repository/codebase navigation, symbol- and structure-aware retrieval, and an MCP-native interface from day one. It is a **separate product contract from dir2mcp** with a hard boundary — **no direct imports from `dir2mcp` are permitted**. The binary is currently a scaffold stub (`cmd/landfall/main.go` prints `not yet implemented` and exits non-zero); most subsystems are not built yet.

## Repo map

- `cmd/landfall` - CLI binary entrypoint (stub)
- `go.mod` - module `github.com/Dirstral/landfall`, Go 1.24, no third-party deps
- `.github/workflows/go.yml` - CI: `go build` + `go test` on push/PR to `main`
- `.goreleaser.yml` - GoReleaser build/archive/release config
- `docs/landfall-prd.md` - product requirements doc referenced by the binary (not yet committed)

## Build / test / CI commands

There is no `Makefile`. Use the Go toolchain directly — these are the commands CI runs:

```bash
go build ./...   # build everything
go test ./...    # run tests
go vet ./...     # static checks
gofmt -l .       # formatting (must print nothing)
```

CI (`.github/workflows/go.yml`) runs `go build ./...` then `go test ./...` on Go 1.24 for every push and pull request targeting `main`, with `contents: read` permissions and in-progress-cancel concurrency.

## Conventions

- Work only in this repository unless explicitly instructed otherwise.
- Never import from `dir2mcp`; the no-direct-imports boundary is a product constraint, not a style preference.
- Keep patches minimal and issue-focused.
- Use Conventional Commits for all commit messages: <https://www.conventionalcommits.org/>.
- Do not mention yourself in commit messages.
- Do not push directly to `main`; open a PR from a feature branch.
- Keep dependency additions minimal and justified; the module currently has zero third-party deps.
- Do not silently change public API/CLI/error contracts.
- Never hardcode API keys, auth tokens, or provider base URLs; keep all credentials env-backed.
- Never introduce secret leakage in logs or error payloads.
- Do not add extra markdown files unless explicitly requested for the task.
- Update tests and docs together when behavior changes.

## Review/merge readiness

- `go build ./...` and `go test ./...` are green; `go vet ./...` is clean and `gofmt` is applied.
- No imports from `dir2mcp`.
- `README.md` and `docs/` are aligned with real behavior.
- No unrelated refactors in issue PRs.

## Important behavior notes

- The binary is a stub: it writes `not yet implemented` to stderr and exits `1`. This is expected, not a bug.
- Module path casing is `github.com/Dirstral/landfall` (capital `D`); keep imports consistent with `go.mod` even though the repo slug is lowercase `dirstral/landfall`.
