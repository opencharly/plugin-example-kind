# AGENTS.md — plugin-example-kind

Standalone plugin repo for the `examplekind` capability (`kind:examplekind`) —
the reference out-of-tree kind-class plugin (F4). The plugin is a Go module at
`candy/plugin-example-kind/` (module path
`github.com/opencharly/plugin-example-kind/candy/plugin-example-kind`); the root
`charly.yml` only declares `discover: candy` so the repo is a project and its
candy is scanned.

Canonical files:

- `candy/plugin-example-kind/charly.yml` — the `plugin-example-kind:` candy entity
  (`plugin:` block, `plan:` check).
- `candy/plugin-example-kind/plugin.go` — the provider (`NewProvider()` +
  `NewMeta()`) and the `OpLoad` / `OpValidate` dispatch.
- `candy/plugin-example-kind/schema/examplekind.cue` — the self-contained
  `#ExamplekindInput`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model (incl. the `kind` class), the per-plugin
  CUE-schema contract, placement. Load before touching the provider or schema.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-example-kind/` — compile the plugin module.
- `go test ./...` in `candy/plugin-example-kind/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.

## Modify this repo

- Edit the `plugin-example-kind:` candy entity, the Go source, and
  `schema/examplekind.cue` **together**.
- The plugin is **out-of-process only** (deliberately not in
  `compiled_plugins:`); do not add it to the compiled set — it exists to witness
  the parse-time prescan of a plugin the host was not built with.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
