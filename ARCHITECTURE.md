# Architecture

This document describes the Strata pipeline, the design goals behind it, and the vocabulary the
ecosystem uses. It is the ecosystem-level view; each crate documents its own API and internals.

## Goals

- **Source fidelity.** Retain what the author actually wrote, not just what it evaluates to. This
  began with scalar styles (folded vs. literal vs. plain in YAML) and extends to trivia in
  general: comments, whitespace, and blank-line counts. Field order is preserved as well as
  possible through every stage.
- **Extract human semantics from syntax.** In config files, a few fields, a blank line, then a few
  more fields usually means something to the humans maintaining the file — an implicit grouping no
  data model captures. Surfacing that potential meaning, instead of discarding it, is a primary
  goal.
- **Composable parsers.** Languages are defined by their token rule sets and grammar parts, and
  superset languages reuse their parent's parts. A JSONC parser falls out of the JSON parser's
  parts by essentially adding a comment rule. Because the design is generic, any data or config
  language can join the ecosystem; the JSON family is the starting point.
- **Zero-copy by default.** Stay on reference slices into the source for as long as the caller
  allows; copy only at the ownership boundary, or when the source buffer must be reclaimed (the
  lease system's promotion escape hatch).
- **Built from the ground up.** Minimal dependencies and purpose-built layers. The stack existing
  is not the only success criterion; understanding it is one too.

## The pipeline

The general flow, from bytes to application values:

```text
reader -> lexer -> parser -> resolver -> deserializer
```

- **Reader** — reads streams/files with lookahead, retained lookbehind, and explicit memory
  control. The lexer works against reader traits, so custom readers (for example, a decoding
  reader presenting a non-UTF-8 source as UTF-8) can slot in.
- **Lexer** — applies a language's token rules to the input and produces a token stream, trivia
  included.
- **Parser** — turns tokens into an IR shape: a concrete syntax tree / document object model that
  still references source spans and retains trivia. Still zero-copy.
- **Resolver** — turns the IR into a dynamic object model with real owned values, while keeping
  fidelity information available. This is the typical consumer entry point for callers that want a
  fully dynamic representation of the document.
- **Deserializer** — maps parsed data onto application-facing Rust structs. Serde glue takes the
  parser output directly, making the resolver an optional alternate path rather than a mandatory
  stage.

Zero-copy is preserved as far down the pipeline as possible; ownership begins where the caller
wants format-independent owned values.

## Package anatomy

The infrastructure crates are shared by every language:

- **`strata-reader`** — a dynamically growing buffered reader with peeking and explicit memory
  control, built for tokenizers that need retained lookbehind.
- **`strata-lease`** — revocable zero-copy sharing of retained byte buffers: a store hands out
  promotable leases and reclaims its memory by promoting straggler views to small owned copies.
- **`strata-lexer`** — the generic lexing machinery: composable read-only token rules, a
  maximal-munch scanner, and streaming input built on the reader traits.

Each language then contributes its own small stack (see [NAMING.md](NAMING.md) for how these are
named):

- **`strata-<lang>-rules`** — the language's token rules, standalone so that superset languages
  can depend on just the rules and add their own, without pulling in the parent's full parser.
- **`strata-<lang>-parser`** — the parser producing the language's CST/IR.
- **`strata-<lang>`** — the resolver: the dynamic, owned document model. This is the plain,
  consumer-facing name.
- **`strata-<lang>-serde`** — Serde glue, going parser → your structs directly.

Superset languages compose at the rules and parser-parts level: `strata-jsonc-rules` imports the
JSON rules and adds a comment rule; the JSONC parser reuses the JSON parser's parts. For languages
without a single spec, a shared rules crate (for example, a common-INI rules crate) can hold the
rules every flavour agrees on, and flavour crates pick, choose, and extend.

## Glossary

- **Token rule** — the composable unit that recognizes one token shape from read-only lookahead.
  A language is lexed against a set of token rules; supersets import, replace, and extend a parent
  language's set.
- **Lexeme** — the actual span of source text that a token rule matched: the characters
  themselves, as opposed to the rule that recognized them or the token that records them.
- **Token** — the lexer's output unit: a kind plus the span (and thereby lexeme) it covers.
- **Trivia** — source content with no grammatical role but human significance: comments,
  whitespace, blank lines. Strata retains trivia instead of discarding it.
- **Scanner** — the lexer component that drives token rules over the input and picks the winning
  match, e.g. by maximal munch (longest match wins).
- **IR / CST** — the parser's output: a concrete syntax tree / document object model that still
  references source spans and retains trivia, before any normalization.
- **Resolver** — the stage that converts the IR into a dynamic object model with owned values.
- **Deserializer** — the stage that maps parsed data onto application-facing Rust structs,
  possibly via Serde glue, possibly skipping the resolver.
- **Lease** — a revocable zero-copy view into a retained buffer (`strata-lease`); the owner can
  _promote_ outstanding leases to small owned copies and free the underlying memory.
- **Superset language** — a language defined by extending another's rule set (JSONC over JSON),
  reusing the parent's parser parts.
- **Flavour** — one specific spec or dialect of a language that exists in multiple variants; see
  [NAMING.md](NAMING.md).
