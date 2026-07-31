<div align="center">

# Enola Labs

### Architectural regression testing for AI-assisted development.

[![GitHub Stars](https://img.shields.io/github/stars/enola-labs/enola?style=social)](https://github.com/enola-labs/enola)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://github.com/enola-labs/enola/blob/main/LICENSE)
[![MCP Protocol](https://img.shields.io/badge/MCP-Compatible-green.svg)](https://modelcontextprotocol.io)

<p align="center">
  <a href="https://github.com/enola-labs/enola">enola</a> •
  <a href="https://github.com/enola-labs/enola#set-it-up">Set it up</a> •
  <a href="https://github.com/enola-labs/enola/blob/main/docs/CLI.md">CLI</a> •
  <a href="https://github.com/enola-labs/enola/blob/main/docs/BENCHMARKS.md">Benchmarks</a> •
  <a href="https://github.com/enola-labs/enola/blob/main/ARCHITECTURE.md">Architecture</a>
</p>

</div>

---

### Your agent can't tell you it's finished while it has introduced a dependency cycle

An AI agent can write more code in an hour than you can carefully review. Your tests tell you the behaviour still works. Your linter tells you the style is fine. Nothing tells you the *structure* still makes sense — that the change didn't couple two modules that had no business knowing about each other, or quietly close a dependency loop.

**Enola Labs** builds the tooling that answers that question at the moment it's cheap to fix. Local, deterministic parsers extract a typed dependency graph straight from multi-language source, served to agents over the **Model Context Protocol** — and graded again after the change, so a structural regression is caught in the editing loop rather than in review.

---

### The loop

**Before a change**, your agent gets the real structure — modules, symbols, routes, storage and how they depend on each other, extracted from source rather than inferred. It can ask what actually depends on the thing it's about to touch, instead of guessing from a grep.

**After a change**, enola grades what the change *did*. It pins the architecture beforehand, compares afterwards, and reports the delta. It's a **delta, not a linter** — completely silent about everything that was already there, so when it does speak, something moved:

```
FAIL — 1 structural regression introduced.

Regressions (fail):
  - [cycles] 1.00 — Cyclic dependency detected (2 modules)
      module "billing" is part of the cycle

New coupling (5):
  billing                                      --imports--> invoice
  invoice                                      --imports--> billing
```

Runs in your agent (as a hook), in your shell (`enola check`, exit `1` on regression), and in CI — the same command and the same exit code in all three.

Only a newly introduced **dependency cycle** fails the build. It's the one finding computed with certainty rather than inferred — Tarjan's SCC over the real import graph — and a gate that fails on exactly one thing is a gate people leave switched on. Everything else (god classes, hotspots, layer violations, complexity outliers) is reported with a confidence score and never breaks your build.

Not a language model, and not embeddings: tree-sitter plus language-specific extractors, a typed fact model, and real graph algorithms. The same commit yields the same answer — across 38 open-source repositories indexed three times each, all 38 produced a byte-identical snapshot ID, over 4.2 million facts with zero parse errors. Nothing leaves your machine.

---

### Ecosystem

* **[`enola-labs/enola`](https://github.com/enola-labs/enola)** — the engine: extractors, cross-repo linker, MCP server, CLI and the `check` gate. Written in Go, Apache 2.0, and it is the *whole* engine — nothing gated, metered or degraded behind a key.

#### Supported languages & frameworks
**Go** · **TypeScript / JS** · **Python** · **Java** · **Kotlin** · **Swift** · **Rust** · **C/C++** · **PHP** · **Ruby** · **Svelte** · **Vue** · **OpenAPI** · **gRPC**
*(Route, DI and storage awareness for Next.js, FastAPI, Django, Spring, Rails, Laravel, SvelteKit, Axum, Compose, SwiftUI, and more)*

Point it at a second repository and it links them into one graph — a web client's `fetch()` to the backend route that serves it, a mobile endpoint enum to that same route, a gRPC call site to the `.proto` service behind it — so an agent can answer *if I change this endpoint, which screens break?* by traversal instead of inference.

---

### Latest engineering articles

Read our technical deep-dives on [enola.tech/blog](https://enola.tech/blog):
<!-- BLOG-POST-LIST:START -->
* 📖 [We asked enola. to audit HuggingFace's chat-ui. Here's what a staff engineer would notice](https://enola.tech/blog/enola-chatui-sveltekit-benchmark)
* 📖 [Parsing code is not the same as mapping architecture](https://enola.tech/blog/parsing-code-is-not-mapping-architecture)
* 📖 [The four memory layers every coding agent needs](https://enola.tech/blog/four-memory-layers-coding-agent)
<!-- BLOG-POST-LIST:END -->

---

### Connect & contribute

- 📂 **Primary repository**: [github.com/enola-labs/enola](https://github.com/enola-labs/enola)
- 🐛 **Report issues**: [GitHub Issues](https://github.com/enola-labs/enola/issues)
- 🤝 **Contributing**: [CONTRIBUTING.md](https://github.com/enola-labs/enola/blob/main/CONTRIBUTING.md)
