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

A **system** is the contents of a single hex, addressed by its coordinates
`(q, r)`. Each system's contents are drawn from a stream keyed by those
coordinates, independently of every other system. A system holds ten **orbits**,
numbered `1` to `10` from the innermost outward; each orbit holds one **planet**,
or is **empty**. See [Determinism]({{< relref "/reference/determinism.md" >}}).

{{< callout type="warning" >}}
This version of the game is a **testbed**, and its system generator is deliberately
simplistic: **every system is identical**. All systems have the same planets in the
same orbits with the same habitability, described below. Only the natural-resource
deposits (still to be documented) are expected to vary from system to system.
{{< /callout >}}

{{< callout type="info" >}}
Because every system is identical in this testbed, the generator does not draw from
a system's stream. The stream is still created and keyed by the system's
coordinates — it is simply not used yet. It is reserved for when system contents
vary from system to system.
{{< /callout >}}

### Planets and habitability

Every system contains the following, orbit by orbit:

| Orbit | Planet          | Habitability |
| ----- | --------------- | ------------ |
| 1     | Rocky           | 0            |
| 2     | Rocky           | 1            |
| 3     | Rocky           | 20           |
| 4     | Asteroid belt   | 0            |
| 5     | *(empty)*       | 0            |
| 6     | Gas giant       | 10           |
| 7     | Gas giant       | 0            |
| 8     | Gas giant       | 0            |
| 9     | Asteroid belt   | 0            |
| 10    | *(empty)*       | 0            |

**Habitability** is a per-planet number; higher values are more habitable. In every
system, the rocky planet in orbit `3` (habitability `20`) is the most habitable.

### Deposits

Each planet also carries **natural-resource deposits** — fuel, metals, and
non-metals — shaped by the cluster's [abundance settings](#settings).

{{< callout type="info" >}}
The quantity, quality, and type of deposits a planet carries are still to be
documented.
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

Once enough systems have been placed, each is populated with its planets. In this
testbed version every system gets the same fixed layout — see
[Systems](#systems) — so this step adds only the natural-resource deposits, which
are still to be documented.

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
