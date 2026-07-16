# Epimethean Challenge — Documentation

Documentation site for **Epimethean Challenge (EC)**, a 4X, play-by-email, science-fantasy game.
Built with [Hugo](https://gohugo.io) and the [Hextra](https://imfing.github.io/hextra/) theme, and organized with the [Diátaxis](https://diataxis.fr) framework.

Production site: <https://ecv6.pbbgaming.com>

## Prerequisites

- [Hugo **extended**](https://gohugo.io/installation/) (v0.163+)
- [Go](https://go.dev/dl/) (Hextra is a Hugo Module)

## Local development

```sh
hugo mod get          # fetch/update the Hextra theme module
hugo server           # serve at http://localhost:1313 with live reload
```

## Build

```sh
hugo --gc --minify    # output written to ./public
```

## Content structure (Diátaxis)

| Directory            | Type        | Purpose                                   |
| -------------------- | ----------- |-------------------------------------------|
| `content/tutorials`  | Tutorial    | Learning-oriented, guided lessons         |
| `content/how-to`     | How-to      | Task-oriented recipes for a specific goal |
| `content/reference`  | Reference   | Information-oriented technical specs      |
| `content/explanation`| Explanation | Understanding-oriented discussion         |

Add a page by creating a Markdown file in the relevant directory, e.g. `content/how-to/submit-your-turn.md`.
Use `weight:` in the front matter to order pages within a section.

## Rulebook structure: core and supplements

The rulebook is split into a **core** and a set of **generator supplements**.

- **Core** — the schema and vocabulary every generator shares: the hex map and axial `(q, r)` coordinates; that a cluster holds `N` systems; that a system has orbits, each holding a planet or nothing; that planets carry fuel, metals, and non-metals deposits, each with a quantity and a yield; and that **the GM chooses a generator for each stage of generation**.
- **Supplement** — one generator: its settings, its tables, its distributions, and how it turns settings into a map. Supplements live under `content/reference/generators/<family>/`, one page per generation stage. The first family is **Genesis**, the v1 testbed generators: `generators/genesis/{placement,system-contents,deposits}.md`.

The line: **core defines the schema and the vocabulary; supplements define the values and the knobs.** Stellar density, minimum spacing, and the resource abundance knobs are generator settings, not core rules — a different generator may have none of them.

Each stage versions independently, so each gets its own page: "Genesis v1" is shorthand for all three stages at v1, never a single version number.

Supplements are first-class published rules, not appendices. The test: **a player must never need to read the engine's source to play well.** Anything that changes optimal play belongs in a supplement, published here.

Decisions — including decisions about this repository's own structure — are recorded in the engine repository, not here: this repository carries rules, not decisions about rules. See [ADR-0016](https://github.com/mdhender/ecv6-api/blob/main/doc/decisions/adr-0016-core-rulebook-and-generator-supplements.md) for the split, and [ADR-0015](https://github.com/mdhender/ecv6-api/blob/main/doc/decisions/adr-0015-decision-records-in-api-repository.md) for why they live there.

## License

Except where otherwise noted, Michael Henderson’s original documentation contributions in this repository are licensed under the Creative Commons Attribution 4.0 International License, CC BY 4.0.

Original Empyrean Challenge materials and intellectual property are owned by James Colombo and are not licensed by this repository.

See `LICENSE` and `NOTICE.md`.
