---
title: System contents
weight: 2
---

**Genesis System Contents** — status **draft**, version **v1**.

The system-contents generator decides **which planet occupies each of a system's
ten orbits, and how habitable it is**. It runs after
[placement]({{< relref "/reference/generators/genesis/placement.md" >}}), and hands
each planet's type and orbit to the
[deposit generator]({{< relref "/reference/generators/genesis/deposits.md" >}}).

It has **no settings**: the layout below is fixed.

## Every system is identical

This generator gives **every system the same contents** — the same planets in the
same orbits with the same habitability. Systems do not vary from one another at
all, in any game, at any settings. Because
[deposits]({{< relref "/reference/generators/genesis/deposits.md" >}}) are fixed by
planet type and orbit, they do not vary either.

Systems therefore differ only in **where they sit**. Choosing where to expand is a
question of position and distance, never of what a system contains.

{{< callout type="warning" >}}
This is a **testbed** generator, deliberately simplistic. Per-system variation is
expected to arrive in a later version — and, because this generator is **draft**,
its layout may change before any game depends on it.
{{< /callout >}}

## The per-orbit layout

Every system has exactly this layout, from orbit `1` (innermost) to `10`
(outermost). An orbit either holds one planet or is empty.

| Orbit | Planet          | Habitability |
| ----- | --------------- | -----------: |
| 1     | Rocky           |            0 |
| 2     | Rocky           |            1 |
| 3     | Rocky           |           20 |
| 4     | Asteroid belt   |            0 |
| 5     | *(empty)*       |            — |
| 6     | Gas giant       |           10 |
| 7     | Gas giant       |            0 |
| 8     | Gas giant       |            0 |
| 9     | Asteroid belt   |            0 |
| 10    | *(empty)*       |            — |

Each system holds **eight planets**: three rocky, two asteroid belts, and three gas
giants. Orbits `5` and `10` are always empty.

**Habitability** is a per-planet number; higher values are more habitable. Three of
the eight planets have a non-zero habitability:

| Orbit | Planet    | Habitability | Note                        |
| ----- | --------- | -----------: | --------------------------- |
| 3     | Rocky     |           20 | The most habitable planet.  |
| 6     | Gas giant |           10 | The second most habitable.  |
| 2     | Rocky     |            1 | Marginal.                   |

The remaining five planets have habitability `0`.

## Determinism

The layout is fixed, so this generator **draws no randomness**: given an orbit, the
planet and its habitability are the same in every system of every game. See
[Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Cluster]({{< relref "/reference/cluster.md" >}}) — orbits, planet types, and habitability
- [Genesis]({{< relref "/reference/generators/genesis" >}}) — the family this generator belongs to
- [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}) — the stage that runs before
- [Deposits]({{< relref "/reference/generators/genesis/deposits.md" >}}) — the stage that runs next
