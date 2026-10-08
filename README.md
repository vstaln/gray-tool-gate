<p align="center">
  <img src="assets/gray-logo.svg" alt="gray" width="96">
</p>
<h1 align="center">gray-tool-gate</h1>
<p align="center">A persisted glob deny-list that blocks tool calls before they run.</p>
<p align="center">
  <a href="https://github.com/vstaln/gray-tool-gate/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
  <img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-7aa2f7.svg">
  <img alt="rust" src="https://img.shields.io/badge/built%20with-rust-orange.svg">
</p>

A persisted deny-list for tools. `tool/before` checks every tool call's `name`
against `~/.gray/tool-gate/deny.txt` — one pattern per line, `#` comments,
`*`/`?` globs (`git-*` matches `git-foo`).

A match answers `{"decision":"deny"}` with a reason pointing at
`/gate allow`. This is a guard: it fails closed only on an explicit match —
a missing file denies nothing and a read error is logged to stderr and
allows.

## Commands

- `/gate deny <tool-or-glob>` — add to the deny list
- `/gate allow <tool-or-glob>` — remove (exact entry match)
- `/gate list` — show denied patterns
- `/gate clear` — empty the list

## Wire

`plugin/manifest`, `tool/before`, `command/run`, `plugin/shutdown`.
Protocol 2.0, hook `tool/before`. No capabilities required.

## Install

```sh
gray plugin install tool-gate
```

## Develop

```sh
cargo test
gray account check      # entry point + manifest handshake
gray account publish    # check → build → release → publish to the gray registry
```

Bump `version` in `Cargo.toml` before each `publish`; the registry refuses to
republish a version.

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
