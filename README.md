# plugin-example-kind

The reference **`kind`-class** plugin (`kind:examplekind`) — a plugin that serves
a whole entity kind, recognized and connected before config decode.

A `kind: examplekind` entity whose plugin is *not* compiled in is recognized at
parse time, the plugin is connected before decode, and the authored body lands in
the parsed kinds. The plugin also serves a deep `OpValidate` check: an authored
marker of `INVALID` is rejected with an error diagnostic.

## What it provides

| Capability | Surface |
|---|---|
| `kind:examplekind` | the `examplekind:` entity kind — parse-time prescan + pre-load connect + `OpLoad`/`OpValidate` |

The plugin is **out-of-process only** (not listed in `compiled_plugins:`), which
is exactly the point: it is the witness that the loader can recognize and connect
a kind plugin it was not built with.

## How to use it

Compose the plugin candy, then author the kind:

```yaml
- '@github.com/opencharly/plugin-example-kind/candy/plugin-example-kind:<tag>'
```

```yaml
examplekind:
  marker: hello
```

## Layout

- `candy/plugin-example-kind/` — the plugin module: `plugin.go` (the provider +
  `NewProvider()`/`NewMeta()` + `OpLoad`/`OpValidate`),
  `schema/examplekind.cue` (the self-contained `#ExamplekindInput`),
  `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-internals:plugin` — the plugin/provider model (incl. the
  `kind` class and parse-time prescan). This candy carries no `skill:` entity of
  its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
