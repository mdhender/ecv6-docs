---
title: System contents
weight: 2
---

**Genesis System Contents** — status **draft**, version **v1**.

The system-contents generator decides **which planets occupy a system's ten
orbits, and how habitable each planet is**. It runs after
[placement]({{< relref "/reference/generators/genesis/placement.md" >}}), and hands
the resulting systems to the
[deposit generator]({{< relref "/reference/generators/genesis/deposits.md" >}}).

It has **no settings**. Every ordinary system uses the same procedure, but its
number of planets, occupied orbits, and habitability values may differ.

## Generating an ordinary system

Each system is generated independently in three steps:

1. Roll the number of planets.
2. Select the orbits they occupy.
3. Determine each planet's type and habitability from its orbit.

### Number of planets

Roll `3d4 - 2`. The result is the system's number of planets, from `1` through
`10`. See [Dice notation]({{< relref "/reference/glossary.md" >}}#dice-notation).

### Occupied orbits

Systems with one, two, or three planets use fixed orbits:

| Planets | Occupied orbits |
| ------: | --------------- |
| 1       | `4`             |
| 2       | `4`, `6`        |
| 3       | `3`, `4`, `8`   |

For a system with four or more planets:

1. Make a list of the ten orbit numbers, `1` through `10`.
2. Shuffle the list.
3. Take one orbit from the start of the shuffled list for each planet.

Each orbit appears only once in the list, so two planets cannot be assigned to the
same orbit. For example, if a four-planet system's shuffled list begins
`[10, 3, 9, 7, …]`, its planets occupy orbits `10`, `3`, `9`, and `7`.

All orbits not selected by these rules are empty.

### Planet type and base habitability

An occupied orbit determines its planet's type and base habitability. Process
occupied orbits in ascending numerical order, from orbit `1` toward orbit `10`.
Roll and clamp each planet's habitability before processing the next occupied
orbit.

| Orbit | Planet          | Habitability |
| ----- | --------------- | -----------: |
| 1     | Rocky           |    `3d2 - 3` |
| 2     | Rocky           |    `3d3 - 3` |
| 3     | Rocky           |    `5d6 - 5` |
| 4     | Asteroid belt   |            0 |
| 5     | Rocky           |    `5d4 - 5` |
| 6     | Gas giant       |    `2d8 - 2` |
| 7     | Gas giant       |    `2d8 - 4` |
| 8     | Gas giant       |    `2d8 - 8` |
| 9     | Rocky           |   `2d8 - 12` |
| 10    | Asteroid belt   |            0 |

Clamp every rolled habitability to the range `0–25`, inclusive. Asteroid belts
have habitability `0` and require no roll. Empty orbits have no planet and no
habitability.

## Minimum habitability for larger systems

After generating all base habitability values, count the system's planets. A
system with fewer than five planets receives no adjustment.

For a system with at least five planets:

1. Add the habitability of all its planets to find the system's **total
   habitability**. Asteroid belts contribute `0`; empty orbits contribute nothing.
2. Set the current orbit to `3`.
3. While total habitability is less than `9`:
   - If the current orbit contains a rocky planet or gas giant, add `2` to that
     planet's habitability when it is in orbit `3` or `6`; otherwise add `1`.
     Increase total habitability by the same amount.
   - Advance to the next orbit.
   - After orbit `10`, continue at orbit `2`.

The orbit sequence is therefore `3, 4, 5, 6, 7, 8, 9, 10, 2, 3, …`. Empty orbits
and asteroid belts receive no increase. An increase of `2` is applied in full, so
the loop may raise a total of `8` to `10`. The resulting total is at least `9`, not
necessarily exactly `9`.

{{< callout type="info" >}}
**Orbit `1` is intentionally skipped.** Even when orbit `1` contains a planet, the
minimum-habitability adjustment never increases it.
{{< /callout >}}

The loop always finishes. A system with at least five planets cannot consist only
of asteroid belts because only orbits `4` and `10` contain belts.

## Home systems

A faction's home system is not one of the ordinary systems above. It is built at
**founding**, on demand, by a **home-system generator** — a system generator the GM
selects and runs against an already-placed system. Founding runs three steps, in
order:

1. The GM **picks** an already-placed system.
2. A home-system generator **rebuilds** it, replacing that system's contents — its
   planets and habitability here, and its deposits through the
   [deposit generator]({{< relref "/reference/generators/genesis/deposits.md" >}}).
3. The GM **assigns** the faction to the rebuilt system.

Which generator builds a given home is the **GM's choice**: she may run the same one
for every faction or vary it from one to the next to balance the game. Home systems
are picked and configured at the GM's discretion.

Genesis provides one such generator. It makes no random rolls and produces the same
home for every system it rebuilds — replacing the planets and habitability with this
fixed layout:

| Orbit | Planet          | Habitability |
| ----- | --------------- | -----------: |
| 1     | Rocky           |            0 |
| 2     | Rocky           |            3 |
| 3     | Rocky           |           25 |
| 4     | Asteroid belt   |            0 |
| 5     | Rocky           |           15 |
| 6     | Gas giant       |           10 |
| 7     | Gas giant       |            0 |
| 8     | Gas giant       |            0 |
| 9     | Rocky           |            4 |
| 10    | Asteroid belt   |            0 |

It sets only the home planets' types, orbits, and habitability; their deposits come
from the
[deposit generator]({{< relref "/reference/generators/genesis/deposits.md" >}}).

## Output

Genesis System Contents returns the planets and habitability values of every
ordinary system. The [home-system generator](#home-systems) runs separately — at
founding, not at cluster generation.

## Determinism

Planet counts, shuffled orbits, and habitability use random dice rolls. As with all
EC generation, the same game seeds reproduce the same results. See
[Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Cluster]({{< relref "/reference/cluster.md" >}}) — orbits, planet types, and habitability
- [Genesis]({{< relref "/reference/generators/genesis" >}}) — the family this generator belongs to
- [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}) — the stage that runs before
- [Deposits]({{< relref "/reference/generators/genesis/deposits.md" >}}) — the stage that runs next
