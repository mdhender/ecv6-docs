---
title: Genesis
weight: 1
sidebar:
  open: true
---

**Genesis** is a family of [generators]({{< relref "/reference/generators" >}}) —
one for each of the three generation stages. It is the family this version of the
game ships with, built to exercise the cluster rules end to end while the content
rules are still being settled.

{{< callout type="warning" >}}
**Genesis is a family name, not a version.** Its three generators version
independently, and a game records one `(generator, version)` pair per stage. Read
"Genesis v1" as shorthand for *all three stages at v1* — never as a version number
for the family.
{{< /callout >}}

## The three generators

| Stage                                                                       | Generator               | Status | Version |
| --------------------------------------------------------------------------- | ----------------------- | ------ | ------- |
| [Placement]({{< relref "/reference/generators/genesis/placement.md" >}})       | Genesis Placement       | Draft  | v1      |
| [System contents]({{< relref "/reference/generators/genesis/system-contents.md" >}}) | Genesis System Contents | Draft  | v1      |
| [Deposits]({{< relref "/reference/generators/genesis/deposits.md" >}})         | Genesis Deposits        | Draft  | v1      |

All three are **draft**: no game depends on them yet, so their rules may change
without a new version. Each becomes **stable** on its own schedule, once a game
depends on it.

## Settings

Genesis takes six settings, split across its stages. There is no cluster-wide
settings list — each setting belongs to the generator that reads it.

| Setting                       | Default   | Stage                                                                       |
| ----------------------------- | --------- | --------------------------------------------------------------------------- |
| Number of systems             | `100`     | [Placement]({{< relref "/reference/generators/genesis/placement.md" >}})       |
| Stellar density               | `average` | [Placement]({{< relref "/reference/generators/genesis/placement.md" >}})       |
| Minimum spacing               | `2`       | [Placement]({{< relref "/reference/generators/genesis/placement.md" >}})       |
| Fuel abundance (`fuel`)       | `average` | [Deposits]({{< relref "/reference/generators/genesis/deposits.md" >}})         |
| Metals abundance (`mtls`)     | `average` | [Deposits]({{< relref "/reference/generators/genesis/deposits.md" >}})         |
| Non-metals abundance (`nmtl`) | `average` | [Deposits]({{< relref "/reference/generators/genesis/deposits.md" >}})         |

Genesis System Contents has no settings: its layout is fixed.

## What Genesis produces

At v1, Genesis varies **only where systems sit**. Placement draws the map from the
game's seeds, so the hexes systems occupy differ from game to game. Every system's
contents are then identical — the same planets in the same orbits with the same
habitability, carrying the same deposits — in every system of every game.

So in a Genesis v1 game, a system is worth exactly what its **position** is worth.
Systems differ from one another in nothing else.

{{< cards >}}
  {{< card link="placement" title="Placement" subtitle="How many systems, how large the map, and which hexes they occupy." >}}
  {{< card link="system-contents" title="System contents" subtitle="The planet and habitability in each of the ten orbits." >}}
  {{< card link="deposits" title="Deposits" subtitle="Deposit counts, quantities, and the yield ladder." >}}
{{< /cards >}}
