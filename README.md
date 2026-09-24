# Strata

Strata is a Rust ecosystem of parsers for files written by humans, built to keep the **human
intent** in them. Most parsers hand you the data a file evaluates to and throw the rest away.
Strata tries to keep, within reason, what the author actually wrote: field order, comments, blank
lines, how a value was written, and where in the source it all came from.

That leftover stuff often means something. In a config file, a few fields, a blank line, and a few
more fields is how people group things. A comment above a value tends to explain why it is that
value. Tools that inspect, validate, or explain a file can use all of that, as long as the parser
has not binned it first.

The name comes from archaeology, where a find's meaning depends partly on the layers around it.
Strip away the context and you are left with a nice pile of pottery. It also happens to describe a
stack of layered crates, and it sounded cool, which honestly came first.

Strata is read-only. It reads, keeps, and explains; it does not edit files or write them back out.

## Status

Early. Very early. This is everything that exists outside of experiments.

### strata-reader

A `BufReader` whose buffer grows as needed, with peeking, retained lookbehind, and manual memory
control.

- **Status:** released, currently 0.1.2
- **Links:** [repository][reader] · [crates.io][reader-crate] · [docs.rs][reader-docs]

### strata-lease

Zero-copy views into a retained buffer that can be promoted to small copies of their own, so the
owner can free the buffer whenever it likes.

- **Status:** unreleased, coming soon
- **Links:** [repository][lease]

Both crates are useful on their own, with or without the rest of Strata.

Everything else, like the lexer and the JSON family parsers, is still an experiment. Those exist to
find out what the design should be, not to be used, so they are not listed until something ships.

## Languages

![This is where I'd put the supported languages - if there were any][meme]

None yet. When a language ships, it gets a table here pointing at its crates. The experiments
started with the JSON family, so that is the likely first candidate, but nothing here is a promise.

## Where this is heading

The rough shape, subject to change:

```text
reader -> lexer -> parser -> resolver
```

- **Reader** gets bytes in. This is `strata-reader`.
- **Lexer** turns bytes into tokens, keeping the comments and whitespace.
- **Parser** builds a syntax tree that still points back into the source.
- **Resolver** turns that tree into owned values you can walk, with the source details attached.
  This is the part you would normally use.

The idea is that languages share this machinery, and a language that extends another (JSONC over
JSON, for example) reuses its parent's parts instead of starting over. All of it is built from the
ground up, with as few dependencies as possible.

Parsers follow their spec and are strict by default. Where a spec allows parsers to offer options,
we are happy to add them; where it does not, options may be limited at first, and more can be added
on request. What we avoid is blending specs together just because they look alike: the JSON parser
will not accept comments. Use the JSONC parser for that, which reads plain JSON too, by definition.

## Issues and discussions

This repository's issue tracker is for anything ecosystem-wide: cross-crate design questions,
documentation fixes, and questions like "what should I name my package for language X?".

Bugs and feature requests for a specific crate belong in that crate's own repository, linked under
[status](#status). If you are not sure where something goes, ask here and we will point you
the right way.

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## License

The contents of this repository are licensed under [MIT-0](LICENSE) (MIT No Attribution): copy,
adapt, and reuse freely, no attribution required.

This only covers the documentation in this repository. The crates are licensed individually; the
official ones follow the [strata-license][license] template (MPL-2.0 with a Rust usage
clarification, and MIT-0 for example code).

[lease]: https://codeberg.org/Devious-Concepts/strata-lease
[license]: https://codeberg.org/Devious-Concepts/strata-license
[meme]: assets/languages-meme.jpg
[reader-crate]: https://crates.io/crates/strata-reader
[reader-docs]: https://docs.rs/strata-reader
[reader]: https://codeberg.org/Devious-Concepts/strata-reader
