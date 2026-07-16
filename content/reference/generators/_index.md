---
title: Generators
weight: 4
sidebar:
  open: true
---

A **generator** builds one part of the [cluster]({{< relref "/reference/cluster.md" >}})
when the GM generates it at setup. The cluster reference defines what a cluster
*is* — hexes, systems, orbits, planet types, habitability, deposits. A generator
decides what a particular cluster actually *contains*: how many systems, where
they sit, which planet occupies each orbit, and what each planet carries.

The rules on these pages are as much a part of the game as the orders you issue.
The generator your game ran determines where the best planets are and how far you
must travel to reach them.

## Generation is staged

Generation runs in three **stages**. Each stage takes its own generator, and
**the GM chooses each one independently**:

| Stage               | Decides                                                            | Hands on                             |
| ------------------- | ------------------------------------------------------------------ | ------------------------------------ |
| **Placement**       | How many systems there are, and which hexes they occupy.            | The systems, at their coordinates.   |
| **System contents** | Which planet occupies each orbit, and its habitability.             | A planet type and habitability per orbit. |
| **Deposits**        | The deposits each planet carries, and their quantities and yields.  | The finished planets.                |

The stages run in that order, and each consumes what the one before it produced:
the deposit generator is given a planet's type and orbit, and nothing else about
the system.

Because the stages are chosen separately, a game can mix them — one family's
placement generator with another family's deposits.

## Families and versions

Generators are grouped into **families**. A family is a name shared by a set of
generators built together, one per stage; it is **not** a version. Every generator
in a family versions **independently**, so a game may run one stage at v2 while
another is still at v1.

A game therefore records **three** `(generator, version)` pairs — one per stage —
never a single version number for the whole cluster.

### Status

Each generator page carries a **status** and a **version**:

| Status     | Meaning                                                                                       |
| ---------- | --------------------------------------------------------------------------------------------- |
| **Draft**  | No game depends on this generator yet. Its rules may change without a new version.             |
| **Stable** | A game depends on this generator. Its rules are fixed; tuning it means publishing a new version, never editing this one. |

A stable generator's rules never change, because a game's cluster must stay
reproducible for as long as the game runs.

## Finding the generators your game ran

A game records the generator and version it ran for each of the three stages, and
those are published with the game. Read them against the pages below to know which
rules built your cluster.

{{< callout type="info" >}}
Where a game surfaces its generators — in the turn report, on the game record, or
both — is still being settled, and will be documented here once it is. Until then,
ask your GM.
{{< /callout >}}

## Families

{{< cards >}}
  {{< card link="genesis" title="Genesis" subtitle="The v1 testbed family: placement, system contents, and deposits." >}}
{{< /cards >}}
