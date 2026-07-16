---
title: Deposits
weight: 3
---

**Genesis Deposits** — status **draft**, version **v1**.

The deposit generator decides **how many deposits each planet carries, and each
deposit's quantity and yield**. It runs last, and is given only a planet's
**type** and **orbit** by the
[system-contents generator]({{< relref "/reference/generators/genesis/system-contents.md" >}}).

Deposits are of three resources: **fuel** (`fuel`), **metals** (`mtls`), and
**non-metals** (`nmtl`). Each deposit has a **quantity** (its starting amount) and
a **yield** percentage.

{{< callout type="warning" >}}
**Known gap: this generator ignores its own abundance settings.** The `fuel`,
`mtls`, and `nmtl` settings below have **no effect** on any value on this page —
setting metals to `rich` changes nothing. The values are fixed by planet type and
orbit alone. A revised abundance model is designed and will land as a later
version; until it does, the settings are inert. See
[Abundance settings](#abundance-settings).
{{< /callout >}}

## Deposits by orbit

The deposits every planet carries, orbit by orbit. `n @ y%` means `n` deposits at
yield `y%`; each carries the row's per-deposit quantity. Empty orbits carry none.

Because [every system is identical]({{< relref "/reference/generators/genesis/system-contents.md" >}}),
this is the deposit map of **every system in the cluster**.

| Orbit | Planet        | Qty / deposit |   Fuel |  Metals | Non-metals |
| ----- | ------------- | ------------: | -----: | ------: | ---------: |
| 1     | Rocky         |    33,333,334 | 1 @ 2% |  1 @ 3% |     1 @ 3% |
| 2     | Rocky         |    33,333,334 | 1 @ 2% |  1 @ 3% |     1 @ 3% |
| 3     | Rocky         |    33,333,334 | 1 @ 2% |  1 @ 3% |     1 @ 3% |
| 4     | Asteroid belt |    16,666,667 | 1 @ 2% |  3 @ 3% |     2 @ 3% |
| 5     | *(empty)*     |             — |      — |       — |          — |
| 6     | Gas giant     |    16,666,667 | 3 @ 3% |  1 @ 5% |     2 @ 6% |
| 7     | Gas giant     |    16,666,667 | 3 @ 3% |  1 @ 5% |     2 @ 6% |
| 8     | Gas giant     |    16,666,667 | 3 @ 2% |  1 @ 3% |     2 @ 3% |
| 9     | Asteroid belt |    16,666,667 | 1 @ 1% |  3 @ 2% |     2 @ 2% |
| 10    | *(empty)*     |             — |      — |       — |          — |

**The gas giants in orbits 6 and 7 are the best deposits in the system.** They are
the only planets whose yields are not halved, so they carry the full `5%` metals
and `6%` non-metals — while the identical gas giant in orbit `8` yields `3%` on
both. Orbit `9`'s asteroid belt is the worst, halved twice to `1–2%`.

Note that the most *habitable* planet — the rocky planet in orbit `3`
([habitability `20`]({{< relref "/reference/generators/genesis/system-contents.md" >}})) —
is not the best planet for extraction; its yields are halved, and it carries only
three deposits.

## System-wide totals

Summing every deposit across the ten orbits — identical in every system — gives the
system's starting resource totals. Totals are not round because the per-planet
`100,000,000` is divided with round-up (see [Quantity](#quantity)).

| Resource       | Deposits | Total quantity  |
| -------------- | -------: | --------------: |
| Fuel           |       14 |     283,333,339 |
| Metals         |       12 |     250,000,005 |
| Non-metals     |       13 |     266,666,672 |
| **Total**      |   **39** | **800,000,016** |

## How the values are set

### Deposit counts by planet type

The planet's type fixes how many deposits of each resource it carries.

| Planet type   | Fuel | Metals | Non-metals | Total deposits |
| ------------- | ---: | -----: | ---------: | -------------: |
| Rocky         |    1 |      1 |          1 |              3 |
| Asteroid belt |    1 |      3 |          2 |              6 |
| Gas giant     |    3 |      1 |          2 |              6 |

### Quantity

Every deposit on a planet starts at the same quantity: **100,000,000 divided by the
total number of deposits on that planet, rounded up.**

- Rocky (3 deposits): `⌈100,000,000 / 3⌉` = **33,333,334** each.
- Asteroid belt or gas giant (6 deposits): `⌈100,000,000 / 6⌉` = **16,666,667** each.

Every planet therefore holds about the same total quantity; the type decides how
that total is split, and among which resources.

### Yield

Each deposit's yield is set by its resource, then reduced by orbit and planet type.
All halving rounds up (`⌈x / 2⌉`).

1. **Base yield** by resource:

   | Resource   | Base yield |
   | ---------- | ---------: |
   | Fuel       |         3% |
   | Metals     |         5% |
   | Non-metals |         6% |

2. **If the planet is in orbit `1`, `2`, `3`, `8`, `9`, or `10`, halve the yield.**
   Orbits `4`, `5`, `6`, and `7` are not halved.
3. **If the planet is an asteroid belt, halve the yield again.**

Steps 2 and 3 are cumulative, so an asteroid belt in a halved orbit is halved
twice:

| Case                                        | Halvings | Fuel | Metals | Non-metals |
| ------------------------------------------- | -------: | ---: | -----: | ---------: |
| Gas giant, orbit `6`–`7` (neither applies)  |        0 |   3% |     5% |         6% |
| Rocky, orbit `1`–`3` (halved orbit)         |        1 |   2% |     3% |         3% |
| Gas giant, orbit `8` (halved orbit)         |        1 |   2% |     3% |         3% |
| Asteroid belt, orbit `4` (belt only)        |        1 |   2% |     3% |         3% |
| Asteroid belt, orbit `9` (both apply)       |        2 |   1% |     2% |         2% |

Because the halving rounds up, two halvings of metals give `⌈⌈5/2⌉/2⌉ = 2%`, not
`1%`.

## Abundance settings

| Setting                       | Default   | Allowed values             |
| ----------------------------- | --------- | -------------------------- |
| Fuel abundance (`fuel`)       | `average` | `poor`, `average`, `rich`  |
| Metals abundance (`mtls`)     | `average` | `poor`, `average`, `rich`  |
| Non-metals abundance (`nmtl`) | `average` | `poor`, `average`, `rich`  |

Each setting is intended to set how favorable that resource is across the cluster,
from `poor` to `rich`.

{{< callout type="warning" >}}
**At v1 these settings do nothing.** No value on this page depends on them. A GM
may set them, and a game may record them, but the deposits generated are identical
at `poor`, `average`, and `rich`. This is a known gap, not a rule of the game: the
abundance model is being revised, and a later version of this generator will
consume these settings. Do not plan around them.
{{< /callout >}}

## Determinism

The values draw **no randomness**: given a planet's type and orbit, every value on
this page is fixed. The same deposits appear in every system, in every game, at
every setting. See [Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Cluster]({{< relref "/reference/cluster.md" >}}) — what a deposit is
- [Genesis]({{< relref "/reference/generators/genesis" >}}) — the family this generator belongs to
- [System contents]({{< relref "/reference/generators/genesis/system-contents.md" >}}) — the layout these deposits attach to
