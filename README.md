# Epimethean Challenge — Documentation

Documentation site for **Epimethean Challenge (EC)**, a 4X, play-by-email,
science-fantasy game. Built with [Hugo](https://gohugo.io) and the
[Hextra](https://imfing.github.io/hextra/) theme, and organized with the
[Diátaxis](https://diataxis.fr) framework.

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
| -------------------- | ----------- | ----------------------------------------- |
| `content/tutorials`  | Tutorial    | Learning-oriented, guided lessons         |
| `content/how-to`     | How-to      | Task-oriented recipes for a specific goal |
| `content/reference`  | Reference   | Information-oriented technical specs       |
| `content/explanation`| Explanation | Understanding-oriented discussion          |

Add a page by creating a Markdown file in the relevant directory, e.g.
`content/how-to/submit-your-turn.md`. Use `weight:` in the front matter to order
pages within a section.
