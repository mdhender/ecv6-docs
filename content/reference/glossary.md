---
title: Glossary
weight: 9999
---

Definitions of terms used across the reference.

**Axial coordinates**
: The coordinate system used to report hex positions, written `(q, r)`.
  The origin is `(0, 0)`.

**Flat-top**
: The hex orientation used by the grid, with a flat edge at the top and bottom of each hex and north toward the top of the map.

**Game**
: The top-level unit of play; the cluster and the players belong to it.

**GM**
: The game master; the operator who generates and runs a game.

**Master seed**
: One of the two `uint64` values (`seed1`, `seed2`) saved for a game.
  Together they are the root of the game's randomness; each subsystem derives its own seeds from them.
  See [Determinism]({{< relref "/reference/determinism.md" >}}).

**Origin**
: The center hex of the cluster, at axial coordinates `(0, 0)`.

**Player**
: A person participating in a game.

**Turn**
: The unit of play a game advances by.
  Turn `0` is setup (no turn); play begins at turn `1` and counts up.
  See [Turns]({{< relref "/reference/turns.md" >}}).
