# Package naming

Guidelines for naming `strata-*` packages. They exist so that names stay predictable and do not
collide or confuse once multiple specs of the "same" language are supported — two different JSONC
variants, or the dozens of INI dialects.

These are guidelines, not enforcement: crates.io has no namespaces, so anyone can publish under
`strata-*`. Within the official ecosystem the rules below apply; for community packages we
recommend them heavily and are happy to help — if you are unsure what to name your package,
[open an issue](CONTRIBUTING.md#issues-and-discussions) and ask.

## The pattern

```text
strata(-ext)-<lang>(-<flavour>)(-official)(-<subpackage>)
```

Parenthesized segments are optional. Examples:

| Name | Reading |
| --- | --- |
| `strata-json` | JSON resolver — the plain name is the consumer-facing resolver crate |
| `strata-json-rules` | JSON token rules |
| `strata-json-parser` | JSON parser |
| `strata-json-serde` | JSON Serde glue |
| `strata-ini-acme-rules` | Token rules for the (illustrative) `acme` flavour of INI |
| `strata-toml-official` | Ecosystem TOML resolver, if an outside `strata-toml` already exists |
| `strata-ext-cddl` | A community-made CDDL package |

Infrastructure crates that are useful outside Strata keep simple `strata-<single-word>` names:
`strata-reader`, `strata-lease`, `strata-lexer`.

## Segment rules

- **Every segment is a lowercase alphanumeric slug.** Symbols in language names are transformed
  to words — for example, a language named `C#` would become `csharp` — so segments never contain
  extra hyphens. The only exception is working around outside package claims (see
  [`-official`](#the--official-marker)).
- **Reserved words.** Subpackage and marker words are blacklisted as language and flavour slugs:
  `rules`, `parser`, `serde`, `resolver`, `ext`, and `official` can never be a `<lang>` or
  `<flavour>` value. This is what keeps the pattern unambiguous to read.

## Subpackages

Each language's stack splits into subpackages by pipeline layer (see
[ARCHITECTURE.md](ARCHITECTURE.md#package-anatomy)):

- `-rules` — the token rules crate.
- `-parser` — the parser crate.
- _(no subpackage)_ — the plain name is the resolver, the normal consumer target.
- `-serde` — the Serde glue crate.

`-resolver` is available as an explicit subpackage name where the plain name is unavailable or
would be ambiguous.

## Languages: first come, first served

Which language gets a plain name like `strata-foo` is decided by arrival order in the ecosystem,
not by historical precedence research. If CUE (the configuration language) is added first, `cue`
means CUE and cue sheets later become `cuesheet`. There is no obligation to hunt for other
languages with the same name before naming a package.

Where a confusion is _already known_, disambiguate proactively — pick `cuelang` up front if the
clash with cue sheets is on the table. But nobody is required to do the research; given that the
namespace is open, first-come-first-serve is what would happen naturally anyway.

## Flavours

Many languages have no single spec: JSONC has two nearly identical ones, and INI has dozens of
dialects — practically every INI parser ever written defines its own. The `<flavour>` segment
names one specific spec or dialect.

- The flavour comes **after** the language name, and is used **only when needed**.
- A language with one real standard needs no flavour: `strata-json`, `strata-json5`.
- A language with **no authoritative spec must not have a flavour-less package name**: no package
  can be "the" `strata-ini`, so every INI package carries a flavour
  (`strata-ini-<flavour>`, `strata-ini-<flavour>-rules`, …). The exception is a shared rules
  crate: `strata-ini-rules` may hold the rules common across flavours, for downstream flavour
  crates to pick and choose from.
- Flavour slugs follow the same alphanumeric rule and should be recognizable to people who use
  that dialect. Naming a flavour is genuinely hard when the dialect has no name of its own; ask
  on the issue tracker if in doubt.

## Language or flavour?

Whether a format gets its own language name or lives as a flavour of its parent is ultimately a
judgement call for the package author. The useful test is whether the format has a **natural
unique identifier** of its own:

- TOML grew out of the INI landscape, but it has its own spec and its own name — `strata-toml`
  is clearly right.
- MySQL's config dialect has no unique name of its own: `strata-ini-mysql` is better than
  inventing an identifier like `strata-mysqlini`.

These guidelines can only recommend; when the call is close, either choice is acceptable, and the
issue tracker is open for a second opinion.

## The `-official` marker

The `strata-*` prefix on crates.io is open, and outside claims are worked around rather than
fought. If someone unaware of the ecosystem publishes, say, a perfectly fine TOML parser as
`strata-toml` — or an entirely unrelated package under that name — the ecosystem's own package
becomes `strata-toml-official`.

- `-official` is used **only** to resolve a collision with an outside claim, never by default.
- Subpackages inherit it: `strata-toml-official-serde`.
- Flavours do **not** inherit it: a claimed `strata-toml` pushes `-official` onto that collided
  name and its subpackages only, not onto `strata-toml-<flavour>` names.

## Community packages: `strata-ext-*`

Community packages are recommended to use the `strata-ext-` prefix: `strata-ext-<lang>(-…)`, with
the rest of the pattern applying unchanged. This keeps the plain `strata-<lang>` names readable as
official ecosystem packages while giving community packages a recognizable shared home.

This is a recommendation, not a rule — the registry lists community packages under whatever name
they actually ship with. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to register a package,
and open an issue for naming advice any time.
