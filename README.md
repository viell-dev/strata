# Strata

Strata is an ecosystem of config/data file parsers designed for **human-authored files**. Where
most parsers reduce a file to the data it evaluates to, Strata retains what the author actually
wrote: field order, comments, blank lines, scalar styles, and source spans survive all the way
through the pipeline.

The name draws on archaeological strata, where a find's place among the surrounding layers is part
of its meaning. Strata treats source files similarly, preserving the context that ordinary parsers
discard.

This repository is the ecosystem hub. It contains no code; it is the public documentation for the
ecosystem as a whole, the registry of official and community `strata-*` packages, and the home of
ecosystem-wide issues and discussions.

## Why Strata

- **Source fidelity.** A folded scalar is not the same as a plain one; a comment above a field is
  not noise. Strata keeps trivia — comments, whitespace, blank lines — instead of discarding it.
- **Human semantics.** A few fields, a blank line, then a few more fields usually means something
  to the humans maintaining the file. Strata surfaces that implicit structure instead of erasing
  it.
- **Composable languages.** Languages are defined by token rule sets and parser parts, and
  superset languages reuse their parent's parts: a JSONC parser is essentially the JSON parts plus
  a comment rule.
- **Zero-copy by default.** Values stay as reference slices into the source for as long as the
  caller allows; owned copies begin only at the ownership boundary.

See [ARCHITECTURE.md](ARCHITECTURE.md) for the full pipeline and design goals.

## Finding a parser

The [package registry](REGISTRY.md) maps every supported language to its packages. For normal use
you want a language's **resolver** crate (`strata-<lang>`, a dynamic document model with full
fidelity) or its **serde** crate (`strata-<lang>-serde`, deserialization onto your own structs).

| Language | Resolver | Serde glue |
| --- | --- | --- |
| [JSON](REGISTRY.md#json) | [`strata-json`](https://crates.io/crates/strata-json) | [`strata-json-serde`](https://crates.io/crates/strata-json-serde) |
| [JSONC](REGISTRY.md#jsonc) | _planned_ | _planned_ |
| [JSON5](REGISTRY.md#json5) | _planned_ | _planned_ |

Lower layers (token rules, parsers) and the shared infrastructure crates (`strata-reader`,
`strata-lease`, `strata-lexer`) are listed in the registry as well.

## Documentation

- [ARCHITECTURE.md](ARCHITECTURE.md) — the pipeline, design goals, and glossary.
- [NAMING.md](NAMING.md) — the `strata-*` package naming guidelines.
- [REGISTRY.md](REGISTRY.md) — the package registry: official and community packages.
- [CONTRIBUTING.md](CONTRIBUTING.md) — registering packages, proposing changes, issue routing.

## Issues and discussions

This repository's issue tracker is the ecosystem-wide portal:

- **Bugs and feature requests for a specific crate** belong on that crate's own repository (linked
  from the [registry](REGISTRY.md)).
- **Ecosystem-wide topics** belong here: cross-crate design questions, documentation fixes,
  registry additions, and naming questions ("what should I name my package for language X?").

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## License

The contents of this repository are licensed under [MIT-0](LICENSE) (MIT No Attribution): copy,
adapt, and reuse freely, no attribution required.

This applies to the documentation in this repository only. The `strata-*` crates themselves are
licensed individually — see each crate's repository; official crates follow the
[`strata-license`](https://codeberg.org/Devious-Concepts/strata-license) template (MPL-2.0 with a
Rust-usage clarification and an MIT-0 carveout for example material).
