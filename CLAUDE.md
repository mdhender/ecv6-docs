# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this project is

The documentation site for **Epimethean Challenge (EC)** — a 4X, play-by-email,
science-fantasy game. This is a **documentation-only** project. There is no game
engine or application source here; the deliverable is the prose, organized as a
website.

- Production site: <https://ecv6.pbbgaming.com>
- Stack: [Hugo](https://gohugo.io) (extended) + [Hextra](https://imfing.github.io/hextra/)
  theme, pulled in as a Hugo Module.

## Scope: rules first, implementation last

The focus is **the rules of the game** — what players and referees need to know to
play. Write about behavior a player can observe and reason about, not how the code
produces it.

- **Do** document rules, orders, costs, formulas, turn resolution, and outcomes.
- **Don't** document code structure, function names, database schemas, file
  layouts, or framework choices of the game engine.
- **Unavoidable exceptions:** some rules cannot be explained without touching
  implementation — e.g. **determinism, PRNG streams, seeding, and turn-processing
  order**. When implementation detail is genuinely necessary to explain a rule,
  include it, but keep it minimal and frame it in terms of what it means for the
  player, not how the engine is built.

When unsure whether something belongs, ask: *"Does a player need this to make
decisions or predict outcomes?"* If yes, it's in scope. If it only matters to
someone maintaining the engine, leave it out.

## Naming

- Full name: **Epimethean Challenge**. Abbreviation: **EC**. Introduce the full
  name first on a page, then EC.
- Genre framing: "a 4X, play-by-email, science-fantasy game."

## Content organization — Diátaxis

Content is organized with the [Diátaxis](https://diataxis.fr) framework. Put each
page in the section that matches its *purpose*, and keep the types unmixed — a
tutorial should not turn into reference, a how-to should not become an essay.

| Directory             | Type        | Purpose                                        |
| --------------------- | ----------- | ---------------------------------------------- |
| `content/tutorials`   | Tutorial    | Learning-oriented, guided lessons (learn by doing) |
| `content/how-to`      | How-to      | Task-oriented recipes for one specific goal    |
| `content/reference`   | Reference   | Information-oriented technical specs of the rules |
| `content/explanation` | Explanation | Understanding-oriented discussion of design/setting |

- The **rules of the game** live primarily in **Reference** (the authoritative,
  neutral spec) and are made usable through **Tutorials** and **How-to Guides**.
- **Explanation** is where the *why* goes — design intent, setting, and rationale.
- Cross-link between types instead of duplicating: a how-to should link to the
  reference for exact formats rather than restating them.
- When writing or restructuring docs, use the `diataxis` skill.

## Authoring conventions

- Each page is a Markdown file under the appropriate `content/` section. Order
  pages within a section with `weight:` in the front matter.
- Hextra shortcodes are available (`{{< callout >}}`, `{{< cards >}}`,
  `{{< card >}}`). Raw HTML in Markdown is enabled.
- Use plain, precise language. Prefer tables for orders, costs, and stats.

## Commands

```sh
hugo mod get        # fetch/update the Hextra theme module
hugo server         # live-reload preview at http://localhost:1313
hugo --gc --minify  # production build into ./public
```

Always confirm a change builds with `hugo --gc --minify` before considering it
done. Build output (`./public`, `./resources/_gen`) is generated and gitignored —
do not edit or commit it.

## Workflow

Tracked changes follow an **issue → branch → PR → merge → close** cycle by default.
Work on `main` directly only for throwaway edits the maintainer explicitly asks to
keep local.

1. **Issue.** Create (or identify) a GitHub issue for the work, assigned to the
   maintainer: `gh issue create --assignee "@me" …` (the owner is **mdhender**).
2. **Branch.** Cut a `docs/<topic>` branch off `main`.
3. **Build.** Confirm `hugo --gc --minify` is clean before committing.
4. **Commit.** Write a descriptive message; reference the issue (e.g. `Closes #N`).
5. **PR.** Push and open a pull request, also assigned to the maintainer
   (`gh pr create --assignee "@me" …`), summarizing the change and linking the issue.
6. **Merge.** Squash-merge and delete the branch
   (`gh pr merge <n> --squash --delete-branch`).
7. **Sync & prune.** Return to `main`, `git pull --ff-only`, and
   `git remote prune origin` so no stale branches linger.
8. **Close.** Ensure the issue is closed (a `Closes #N` commit auto-closes it),
   leaving a comment that links the PR and merge commit.

"Sync" means syncing the local repository with GitHub — it does **not** deploy.
Publishing the live site is a separate rsync step the maintainer runs.
