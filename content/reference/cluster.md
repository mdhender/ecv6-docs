---
title: Cluster
weight: 3
---

The **cluster** is the map a game is played on: a grid of hexes centered on the
[origin]({{< relref "/reference/glossary.md" >}}).
Each occupied hex holds a **system** addressed by its axial coordinates `(q, r)`.
The cluster belongs to the game and is generated once, at setup.

This page defines what a cluster **is**: the vocabulary and the shape every
cluster shares, whichever generators built it — its coordinates, its systems, and
the [jump routes](#routes) that connect them. The values that fill one in — how
many systems there are, how far apart they sit, which planet occupies which orbit,
and what each planet carries — come from the
[generators]({{< relref "/reference/generators" >}}) the GM chose for the game.

## Coordinates

Hex positions are reported in **axial coordinates**, written `(q, r)`, with the
origin at `(0, 0)` at the center of the map.
The grid uses **flat-top** hexes.
See the [Glossary]({{< relref "/reference/glossary.md" >}}) for both terms.

A cluster is a star map, so there are no compass bearings — north, south, east,
west — to navigate by. Positions are given relative to a hex instead. The hex
directly **above** a given hex is at offset `(0, -1)`.

### Directions

The six hexes adjacent to any hex are reached by adding a **direction vector** to
its coordinates. Starting from the hex above and going clockwise:

| Direction   | Offset `(Δq, Δr)` |
| ----------- | ----------------- |
| Up          | `( 0, -1)`        |
| Upper-right | `(-1,  0)`        |
| Lower-right | `(-1, +1)`        |
| Down        | `( 0, +1)`        |
| Lower-left  | `(+1,  0)`        |
| Upper-left  | `(+1, -1)`        |

Opposite directions negate one another: up and down, upper-right and lower-left,
lower-right and upper-left.

### Measuring distance

Distances are **hex distances** on the axial grid. For two hexes `(q₁, r₁)` and
`(q₂, r₂)`:

```
dist = (|q1 - q2| + |r1 - r2| + |(q1 + r1) - (q2 + r2)|) / 2
```

Every distance in the rules is measured with this metric.

## Systems

A **system** is the contents of a single hex, addressed by its coordinates
`(q, r)`. A cluster holds `N` systems, a count the GM chooses when the game is set
up; the allowed range and the default belong to the game's
[placement generator]({{< relref "/reference/generators" >}}).

Each system holds ten **orbits**, numbered `1` to `10` from the innermost outward.
Each orbit holds one **planet**, or is **empty**. Which planet occupies which orbit
is decided by the game's
[system-contents generator]({{< relref "/reference/generators" >}}).

### Planets

A planet is one of three **types**:

| Type              | Description                           |
| ----------------- | ------------------------------------- |
| **Rocky**         | A solid body.                         |
| **Asteroid belt** | A belt of debris occupying the orbit. |
| **Gas giant**     | A large gaseous body.                 |

Every planet has a **habitability**: a per-planet number rating how habitable it
is, where higher values are more habitable. The habitability of each planet is set
by the game's
[system-contents generator]({{< relref "/reference/generators" >}}).

### Deposits

Every planet carries **natural-resource deposits** of three resources: **fuel**
(`fuel`), **metals** (`mtls`), and **non-metals** (`nmtl`).

Each deposit has two values:

| Value        | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| **Quantity** | The amount of the resource the deposit starts with.          |
| **Yield**    | A percentage governing how much of the deposit is recovered. |

How many deposits a planet carries, and how their quantities and yields are set,
is decided by the game's
[deposit generator]({{< relref "/reference/generators" >}}).

## Routes

Ships travel between systems only along **jump routes** — undirected edges that fix
which systems are directly reachable from which. A route carries a single value, the
[hex distance](#measuring-distance) between its two endpoints; the network is set
once when the cluster is generated and never changes during play.

The route network is a **fixed rule of the cluster, not a value a generator fills
in**: once placement has given the systems their coordinates, the routes follow from
the same procedure in every game, whichever generators built the map. See
[Routes]({{< relref "/reference/routes.md" >}}) for the full definition — what a
route is, how the network is derived (backbone, adjacency, and texture passes, tuned
by the GM's route-density tier), the six-route ceiling, and how to reconstruct it by
hand.

## Generation

The GM generates the cluster once, at setup. Generation runs in three **stages**,
and **the GM chooses a generator for each stage independently**:

| Stage               | Decides                                                    |
| ------------------- | ---------------------------------------------------------- |
| **Placement**       | How many systems there are, and which hexes they occupy.    |
| **System contents** | Which planet occupies each orbit, and its habitability.     |
| **Deposits**        | The deposits each planet carries, and their quantities and yields. |

Each stage's generator carries its own settings and its own version, and a game
records all three. The generators a game can run — and how to find the ones your
game ran — are published under
[Generators]({{< relref "/reference/generators" >}}).

Placement also fixes the cluster's
[route network]({{< relref "/reference/routes.md" >}}). The routes are **not** a
chosen generator: once the systems have their coordinates, the routes follow from a
fixed rule, so every placement generator produces the same routes for the same
systems and route-density tier.

### Reproducibility

The cluster derives its own master seeds from the game's master seeds when it is
generated, and stores them with the cluster's data. Everything random in
generation comes from those seeds, so the same seeds and the same generators
always reproduce the same cluster.

Because each system's contents are keyed by its coordinates rather than by the
order it was generated in, the cluster is also order-independent.
See [Games]({{< relref "/reference/games.md" >}}) and
[Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Generators]({{< relref "/reference/generators" >}}) — how a cluster is filled in
- [Games]({{< relref "/reference/games.md" >}})
- [Determinism]({{< relref "/reference/determinism.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
