# design-md

This repository is intended to provide design-system information in Markdown. The previous README claimed a catalog covering 100+ brands, brand-specific specifications, an API, examples, integrations, benchmarks, and case studies. The referenced brand documents and supporting project files were not found at the checked paths, so those claims have been removed pending evidence.

## Current state

| Checked item | Result |
|---|---|
| Root README | Present |
| Root license | MIT license present |
| Brand-specific documents | No local specification content was verified at the checked paths. The recursive tree contains 59 per-brand README stubs under `design-md/`; they redirect to `getdesign.md` pages, whose content and currentness were not reviewed. |
| Package, install, test, and CI configuration | Not found at checked root paths |
| Brand accuracy and rights | Not verified |

The recursive README inventory on this branch lists 61 Markdown paths: the root README, this inventory, and 59 per-brand redirect stubs. These stubs do not contain local design specifications. Their linked pages, accuracy, currentness, and reuse rights were not reviewed. Do not treat the former README's brand list or design details as a verified catalog. Verify any design specification against the brand's own current sources before use.

## Intended contribution format

For each design-system document, include a source URL, date checked, scope, concrete tokens or rules, platform/version context, and known limitations. Separate official source material from interpretation. Avoid implying endorsement or reproducing proprietary assets.

## Use in a design workflow

Markdown notes can provide context to a designer or assistant, but do not establish that a design matches a brand. Validate typography, color, spacing, components, accessibility, and current brand guidance against authoritative sources and rendered output.

## License

See [LICENSE](LICENSE). Verify rights and attribution for any brand-specific material before redistribution.

See [CONTENT_REVIEW.md](CONTENT_REVIEW.md) for paths checked and claims removed.

## README index

Browse the [recursive README inventory](docs/README.md) for README Markdown files in this branch.
