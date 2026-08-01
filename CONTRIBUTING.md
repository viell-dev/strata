# Contributing

This repository is the Strata ecosystem hub: documentation, the package registry, and the
ecosystem-wide issue portal. It intentionally contains no code — code contributions belong on the
individual crate repositories.

## Issues and discussions

Route issues by scope:

- **Crate-specific bugs and feature requests** → that crate's own repository. Every official crate
  is linked from the [registry](REGISTRY.md).
- **Ecosystem-wide topics** → this repository's issue tracker:
  - cross-crate design questions and suggestions,
  - documentation fixes and improvements for the documents in this repository,
  - registry additions and corrections,
  - **naming questions** — "what should I name my package for language X?" is an explicitly
    welcome issue here; see [NAMING.md](NAMING.md) for the guidelines the answer will follow.

If you are unsure where something belongs, open it here and it will be redirected if needed.

## Registering a package

Community packages are welcome in the [registry](REGISTRY.md). To register one, open a pull
request that:

1. Adds a row to the **Languages** overview table, with the language name suffixed by your
   username in parentheses — e.g. `TOML (@your-name)` — linking to your section below.
2. Adds a section under **Community packages**, following the existing entry format: heading
   `### <Language> (@your-name)`, a short description, and a table of the packages with layer,
   description, and links.

Recommendations for the package itself (see [NAMING.md](NAMING.md) for the full guidelines):

- Use the `strata-ext-*` prefix for community packages.
- Follow the segment pattern `strata-ext-<lang>(-<flavour>)(-<subpackage>)`, including the
  reserved-word blacklist.
- If the language has no authoritative spec, include a flavour segment.

We cannot enforce naming outside the official packages, but registry entries that follow the
guidelines keep the ecosystem predictable for everyone. Ask for naming advice in an issue before
publishing if in doubt — ideally before the name is claimed on crates.io.

## Documentation changes

Pull requests against the documents in this repository are welcome: typo fixes, clarity
improvements, broken links, outdated registry entries. For substantive changes (new guidelines,
restructuring), open an issue first so the direction can be discussed.

Keep Markdown wrapped at 100 columns and match the existing style of the documents.

## Licensing

The contents of this repository are licensed under [MIT-0](LICENSE). By contributing, you agree
that your contributions are licensed under the same terms.
