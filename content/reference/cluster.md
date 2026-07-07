---
title: Cluster
weight: 3
---

The **cluster** is the map a game is played on: a grid of hexes centered on the
[origin]({{< relref "/reference/glossary.md" >}}).
Each occupied hex holds a **system** addressed by its axial coordinates `(q, r)`.
The cluster belongs to the game and is generated once, at setup.

{{< callout type="info" >}}
This page is a stub. It fixes the vocabulary and structure of the cluster; the
generation rules — cluster size, how many systems, and what each system contains
— are still to be documented.
{{< /callout >}}

## Coordinates

Hex positions are reported in **axial coordinates**, written `(q, r)`, with the
origin at `(0, 0)`.
The grid uses **flat-top** hexes with north toward the top of the map.
See the [Glossary]({{< relref "/reference/glossary.md" >}}) for both terms.

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

## Generation and seeds

The cluster derives its own master seeds from the game's master seeds when it is
generated, and stores them with the cluster's data.
Because each system is addressed by its coordinates rather than by the order it
was generated in, the cluster is reproducible and order-independent.
See [Games]({{< relref "/reference/games.md" >}}) and
[Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Games]({{< relref "/reference/games.md" >}})
- [Determinism]({{< relref "/reference/determinism.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
