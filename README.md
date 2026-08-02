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

Sometimes the values are all you need. In that case, a Strata Serde crate can deserialize a file
straight into your own types. But software that inspects, validates, or explains a file often needs
to know more. Comments can record why a value was chosen, field order and blank lines can reveal
how the author grouped the data, and diagnostics are more useful when they can point back to the
exact source. Strata keeps that information available instead of discarding it during parsing.

Strata aims to provide that same choice across data and configuration languages. Parsers are
strict by default about the standard or named flavour they accept, rather than each quietly
inventing another interpretation of the format. The lower layers also keep source data zero-copy
for as long as practical. That is mostly an implementation detail, but it helps when files or
individual values get large.

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
