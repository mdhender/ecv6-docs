---
title: Cluster
weight: 3
---

The **cluster** is the map a game is played on: a grid of hexes centered on the
[origin]({{< relref "/reference/glossary.md" >}}).
Each occupied hex holds a **system** addressed by its axial coordinates `(q, r)`.
The cluster belongs to the game and is generated once, at setup, from two settings
the GM chooses.

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

## Systems

A **system** is the contents of a single hex.
Each system is addressed by its own coordinates `(q, r)`, and its contents are
drawn from a stream keyed by those coordinates, independently of every other
system.
See [Determinism]({{< relref "/reference/determinism.md" >}}).

{{< callout type="info" >}}
The composition of a system — stars, planets, and other features — will be
described here (or on its own page) once the generation rules are settled.
{{< /callout >}}

## Generation

The GM generates the cluster once, at setup. Generation is controlled by five
settings: two that shape the map, and one for each natural resource.

### Settings

| Setting                 | Default   | Allowed values                                                 |
| ----------------------- | --------- | -------------------------------------------------------------- |
| Number of systems       | `100`     | an integer from `10` to `1,000`                                |
| Stellar density         | `average` | `extremely dense`, `dense`, `average`, `sparse`, `very sparse` |
| Fuel abundance (`fuel`) | `average` | `poor`, `average`, `rich`                                      |
| Metals abundance (`mtls`) | `average` | `poor`, `average`, `rich`                                    |
| Non-metals abundance (`nmtl`) | `average` | `poor`, `average`, `rich`                                |

- **Number of systems** is how many systems the finished cluster contains.
- **Stellar density** is how tightly those systems are packed, from
  `extremely dense` (closest together) to `very sparse` (farthest apart).
- **Fuel**, **metals**, and **non-metals abundance** each set how favorable that
  natural resource is across the cluster, from `poor` to `rich`. The generator uses
  them when determining the quantity, quality, and type of natural resource
  deposits on planets.

### From the settings to a map

The generator uses the **number of systems** and **stellar density** to:

1. calculate the cluster **radius** — how far from the origin a system may lie;
2. determine the **minimum system spacing** — the smallest distance, in hexes,
   allowed between any two systems; and
3. populate the map with systems.

{{< callout type="info" >}}
The exact radius and minimum spacing that each combination of settings produces
are still to be documented.
{{< /callout >}}

### Placing the systems

Systems are placed by drawing hexes at random and keeping only those that are far
enough from every system already placed:

1. Build a list of every hex coordinate within the radius.
2. Shuffle the list.
3. Draw the next hex from the list:
   - If it is no closer than the minimum system spacing to every system already
     placed, add it to the cluster as a system.
   - Otherwise, discard it.
4. Repeat step 3 until the target number of systems has been placed, or the list
   is exhausted.

If the list is exhausted before enough systems are placed, **generation fails and
no cluster is created** — the requested number of systems does not fit at the
chosen density. The GM must change the settings and generate again.

{{< callout type="info" >}}
What happens once enough systems have been placed — how each system is then
populated, including the natural resource deposits governed by the three abundance
settings — is still to be documented.
{{< /callout >}}

### Reproducibility

The cluster derives its own master seeds from the game's master seeds when it is
generated, and stores them with the cluster's data.
The shuffle and every draw come from those seeds, so the same seeds always
reproduce the same placement — the same systems in the same hexes.
And because each system's contents are keyed by its coordinates rather than by the
order it was generated in, the cluster is reproducible and order-independent.
See [Games]({{< relref "/reference/games.md" >}}) and
[Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Games]({{< relref "/reference/games.md" >}})
- [Determinism]({{< relref "/reference/determinism.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
