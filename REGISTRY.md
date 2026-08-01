# Package registry

All known `strata-*` packages: the official ecosystem packages and registered community packages.
For most uses you want a language's **resolver** crate (the plain `strata-<lang>` name) or its
**serde** crate; the lower layers are listed per language for parser authors building on them.

To add or update an entry, see [CONTRIBUTING.md](CONTRIBUTING.md#registering-a-package). Naming
questions go to the [issue tracker](CONTRIBUTING.md#issues-and-discussions);
[NAMING.md](NAMING.md) has the guidelines.

> **Sketch note:** package links in this document are placeholders; not every listed package
> exists yet.

## Languages

| Language | Flavour | Resolver | Serde glue | Status |
| --- | --- | --- | --- | --- |
| [JSON](#json) | — | [`strata-json`](https://crates.io/crates/strata-json) | [`strata-json-serde`](https://crates.io/crates/strata-json-serde) | Released |
| [JSONC](#jsonc) | — | `strata-jsonc` | `strata-jsonc-serde` | Planned |
| [JSON5](#json5) | — | `strata-json5` | `strata-json5-serde` | Planned |
| [TOML](#toml-example-user) (@example-user) | — | [`strata-ext-toml`](https://crates.io/crates/strata-ext-toml) | — | Community |

## Infrastructure

Shared, language-independent crates. These are useful outside Strata as well.

| Package | Description | Links |
| --- | --- | --- |
| `strata-reader` | Dynamically buffered reader with peeking, retained lookbehind, and explicit memory control | [repo](https://codeberg.org/Devious-Concepts/strata-reader) · [crates.io](https://crates.io/crates/strata-reader) |
| `strata-lease` | Revocable zero-copy sharing of retained byte buffers through promotable leases | [repo](https://codeberg.org/Devious-Concepts/strata-lease) · [crates.io](https://crates.io/crates/strata-lease) |
| `strata-lexer` | Lexer primitives: composable token rules, a maximal-munch scanner, streaming input | [repo](https://codeberg.org/Devious-Concepts/strata-lexer) · [crates.io](https://crates.io/crates/strata-lexer) |

## Official packages

### JSON

Spec: [json.org](https://www.json.org/) / [RFC 8259](https://datatracker.ietf.org/doc/html/rfc8259)

| Package | Layer | Description | Links |
| --- | --- | --- | --- |
| `strata-json-rules` | Token rules | JSON token rules, reusable by superset languages | [repo](https://codeberg.org/Devious-Concepts/strata-json-rules) · [crates.io](https://crates.io/crates/strata-json-rules) |
| `strata-json-parser` | Parser | JSON CST parser with full trivia retention | [repo](https://codeberg.org/Devious-Concepts/strata-json-parser) · [crates.io](https://crates.io/crates/strata-json-parser) |
| `strata-json` | Resolver | Dynamic JSON document model with owned values and fidelity info | [repo](https://codeberg.org/Devious-Concepts/strata-json) · [crates.io](https://crates.io/crates/strata-json) |
| `strata-json-serde` | Serde glue | Deserialize JSON directly onto your structs, parser → Serde | [repo](https://codeberg.org/Devious-Concepts/strata-json-serde) · [crates.io](https://crates.io/crates/strata-json-serde) |

### JSONC

Status: planned. JSON with comments, as used by VS Code settings files. Spec:
[jsonc.org](https://jsonc.org/) — a well-defined grammar with formal ABNF, describing the syntax
of Microsoft's [`jsonc-parser`](https://github.com/microsoft/node-jsonc-parser). A second,
Codeberg-hosted JSONC spec exists but does not constitute a JSON superset in its current state;
if it is ever supported, its packages would carry a [flavour](NAMING.md#flavours)
(`strata-jsonc-codeberg`).

| Package | Layer | Description | Links |
| --- | --- | --- | --- |
| `strata-jsonc-rules` | Token rules | JSON rules plus comment (and trailing-comma) rules | _planned_ |
| `strata-jsonc-parser` | Parser | JSONC parser reusing the JSON parser parts | _planned_ |
| `strata-jsonc` | Resolver | Dynamic JSONC document model | _planned_ |
| `strata-jsonc-serde` | Serde glue | Serde integration for JSONC | _planned_ |

### JSON5

Status: planned. Spec: [json5.org](https://json5.org/) — a single authoritative spec, so no
flavour segment.

| Package | Layer | Description | Links |
| --- | --- | --- | --- |
| `strata-json5-rules` | Token rules | JSON5 token rules, extending the JSON rules | _planned_ |
| `strata-json5-parser` | Parser | JSON5 parser reusing the JSON parser parts | _planned_ |
| `strata-json5` | Resolver | Dynamic JSON5 document model | _planned_ |
| `strata-json5-serde` | Serde glue | Serde integration for JSON5 | _planned_ |

## Community packages

Packages maintained outside the official ecosystem. Entries carry the author's username (or
preferred name) after the language to disambiguate independent packages for the same language.
Community packages are recommended to use the [`strata-ext-*` prefix](NAMING.md#community-packages-strata-ext-);
listing here does not imply endorsement.

### TOML (@example-user)

> Placeholder entry illustrating the format.

| Package | Layer | Description | Links |
| --- | --- | --- | --- |
| `strata-ext-toml` | Resolver | TOML document model built on the Strata pipeline | [repo](https://example.com/example-user/strata-ext-toml) · [crates.io](https://crates.io/crates/strata-ext-toml) |
