# AGENTS.md

Guidance for AI coding agents (Claude Code, Codex, Cursor, and others that
read this file) working in this repository.

## What this is

YantrikDB is the Rust cognitive-memory engine behind
[yantrikdb-server](https://github.com/yantrikos/yantrikdb-server) and
[yantrikdb-mcp](https://github.com/yantrikos/yantrikdb-mcp). Apache-2.0,
no CLA, no copyright assignment.

## Setup

```sh
git clone https://github.com/yantrikos/yantrikdb
cd yantrikdb
cargo test --workspace
```

A default `cargo build` has no network code path: the `embedder-download`
feature is off, so `with_default()` uses the bundled 64-dim
`potion-base-2M`. The Python wheel turns that feature on, which is why pip
users get the 256-dim `potion-base-8M` default instead. If you're touching
embedder code, build both configurations:

```sh
cargo build --features embedder-download
cargo build --no-default-features        # slim: no bundled embedder at all
```

Python bindings live in `crates/yantrikdb-python` (pyo3 + maturin). The
wasm build used by the in-browser demo is `crates/yantrikdb-wasm`.

## Before opening a PR

Both of these must pass — they're also what CI runs:

```sh
cargo fmt --all -- --check
cargo clippy --all-targets --all-features --workspace --exclude yantrikdb-python -- --cap-lints warn
cargo test --workspace
```

`cargo fmt --all` (no `--check`) and the clippy line above are the two
that fail most often locally; run them before testing.

## Conventions

- One logical change per PR, squash-merged into `main` (branch-protected).
- Add or extend a test that fails without your change — concurrency and
  temporal-decay behavior is easy to break by accident; see
  `CONCURRENCY.md` for the invariants.
- Describe what you observed, not only what you changed.
- If picking up an existing issue, say so on the issue first.

Full contributor guide: [CONTRIBUTING.md](CONTRIBUTING.md).
