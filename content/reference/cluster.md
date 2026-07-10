---
title: Cluster
weight: 3
---

The **cluster** is the map a game is played on: a grid of hexes centered on the
[origin]({{< relref "/reference/glossary.md" >}}).
Each occupied hex holds a **system** addressed by its axial coordinates `(q, r)`.
The cluster belongs to the game and is generated once, at setup, from the settings
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

The GM generates the cluster once, at setup. Generation is controlled by six
settings: three that shape the map, and one for each natural resource.

### Settings

| Setting                 | Default   | Allowed values                                                 |
| ----------------------- | --------- | -------------------------------------------------------------- |
| Number of systems       | `100`     | an integer from `10` to `1,000`                                |
| Stellar density         | `average` | `extremely dense`, `dense`, `average`, `sparse`, `very sparse` |
| Minimum spacing         | `2`       | an integer of at least `1`                                     |
| Fuel abundance (`fuel`) | `average` | `poor`, `average`, `rich`                                      |
| Metals abundance (`mtls`) | `average` | `poor`, `average`, `rich`                                    |
| Non-metals abundance (`nmtl`) | `average` | `poor`, `average`, `rich`                                |

- **Number of systems** is how many systems the finished cluster contains.
- **Stellar density** is how large an area those systems are spread across, from
  `extremely dense` (a small, tightly packed map) to `very sparse` (a large,
  loosely packed one). It governs the *average* distance between systems.
- **Minimum spacing** is the smallest distance, in hexes, allowed between any two
  systems — a hard floor applied when they are placed. It has no maximum, and
  unlike the others it is **not** derived from the density.
- **Fuel**, **metals**, and **non-metals abundance** each set how favorable that
  natural resource is across the cluster, from `poor` to `rich`. The generator uses
  them when determining the quantity, quality, and type of natural resource
  deposits on planets.

### From the settings to a map

Three of the settings shape the map: the **number of systems** (`N`), the
**stellar density** (`D`), and the **minimum spacing** (`S`). They feed two values
the placement algorithm consumes:

1. the cluster **radius** `R` — how far from the origin a system may lie —
   **derived from `N` and `D`** (the rule is below); and
2. the **minimum system spacing** — the smallest distance, in hexes, allowed
   between any two systems — which is the GM's **`S` setting used directly**. It is
   *not* derived from the density.

With the radius and spacing fixed, the generator [places the systems](#placing-the-systems).

The two values come from different places on purpose. `N` and `D` set how large
the map is and how far apart systems sit *on average*; `S` sets the *guaranteed
minimum* gap between any two systems, independently of the density. See
[the minimum-spacing knob](#the-minimum-spacing-knob) for why spacing is its own
setting.

#### Measuring distance

Distances are **hex distances** on the axial grid. For two hexes `(q₁, r₁)` and
`(q₂, r₂)`:

```
dist = (|q1 - q2| + |r1 - r2| + |(q1 + r1) - (q2 + r2)|) / 2
```

The cluster radius, the minimum spacing, and every nearest-neighbor figure below
are measured with this metric.

#### The cluster radius

The radius depends only on `N` and `D` — no randomness, no game seed, nothing else
— so two GMs who choose the same `N` and `D` get the same `R` on any machine.
Density is a **radius knob**: at a fixed `N`, a denser tier spreads the same
systems across a smaller disk and a sparser tier across a larger one.

At the default `N = 100` the tiers use these baseline radii. The first three
columns are pure geometry (they depend only on `N` and `D`); the last is the
average nearest-neighbor distance **measured** over 300 placement trials at the
tightest spacing (`S = 1`), so it isolates the density knob's own effect:

| Density (setting) | Radius `R` | Hexes | Hexes / System | Avg. nearest neighbor |
| ----------------- | ---------: | ----: | -------------: | --------------------: |
| extremely dense   |         37 | 4,219 |           42.2 |                   3.5 |
| dense             |         40 | 4,921 |           49.2 |                   3.8 |
| average           |         43 | 5,677 |           56.8 |                   4.0 |
| sparse            |         47 | 6,769 |           67.7 |                   4.4 |
| very sparse       |         51 | 7,957 |           79.6 |                   4.8 |

- **Radius** — the maximum hex distance from the origin `(0, 0)` to any hex on the
  map. The map contains every hex whose distance from the origin is ≤ `R`.
- **Hexes** — the total number of hexes on the map. For a radius `R` this is
  `1 + 3R(R + 1)`.
- **Hexes per system** — `Hexes ÷ N`, the average map hexes available for each
  system. A **global** density metric: how much of the map is occupied overall.
  Larger means more empty space on average.
- **Average nearest neighbor** — the expected distance from a system to its
  closest neighbor. A **local** density metric: the "crowdedness" a player feels
  while exploring, and the more useful tuning parameter. (Measured here at
  `S = 1`; the default `S = 2` nudges each figure up ~0.25 — see
  [the minimum-spacing knob](#the-minimum-spacing-knob).)

For a fixed `N` the two metrics move together, so you can reason in whichever is
more natural. The tiers are **monotonic**: `extremely dense` is the tightest map
and `very sparse` the loosest, and a denser tier always yields a smaller `R` and
closer systems.

#### Scaling with the number of systems

When `N ≠ 100`, the density is **held constant and the radius adjusted**, so a
larger galaxy is not also a sparser one. Each tier's hexes-per-system is kept fixed
at its baseline, making the target hex count

```
T = round(H_D * N / 100)
```

where `H_D` is the tier's baseline hex count (`4219`, `4921`, `5677`, `6769`,
`7957`). The radius is the integer whose hex count `1 + 3R(R + 1)` is nearest `T`
(on a tie, the smaller `R`). In closed form,

```
R(N, D) = round( ( sqrt((4*T - 1) / 3) - 1 ) / 2 )
```

which reproduces the baseline radii exactly at `N = 100`. The radius grows roughly
as `√N`, and because hexes-per-system is held fixed, **both** the global density
and the average nearest neighbor stay stable as `N` changes. Measured across the
whole range (`S = 1`):

| `N`  | Radius `R` | Hexes / System | Avg. nearest neighbor | Success |
| ---- | ---------: | -------------: | --------------------: | ------: |
| 10   |         13 |           54.7 |                   4.4 |    100% |
| 100  |         43 |           56.8 |                   4.0 |    100% |
| 1000 |        137 |           56.7 |                   3.9 |    100% |

(Average tier shown. The average nearest neighbor drifts down only slightly at
large `N`, as the fixed map boundary matters less.)

#### The minimum-spacing knob

**Minimum spacing `S` is an independent GM setting** — an integer of at least `1`,
default `2`, with no maximum — **not** derived from the density. It does not affect
the radius; it only sets the rejection threshold when
[placing systems](#placing-the-systems): a drawn hex is kept only if it is at least
`S` hexes from every system already placed. The decision and its rationale are
recorded in
[ADR-0014](https://github.com/mdhender/ecv6-api/blob/main/doc/decisions/adr-0014-cluster-minimum-spacing-knob.md).

**Why a separate knob and not just more density?** Density and spacing both push
systems apart, but act on different parts of the distribution. Density (radius)
raises the **average** distance between systems; `S` raises the **guaranteed
minimum**. Measured per-system nearest-neighbor distribution at `N = 100`:

| Setting                        | Mean NN | NN = 1 (adjacent) |  NN ≤ 2 | Map hexes |
| ------------------------------ | ------: | ----------------: | ------: | --------: |
| average, `S = 1` (baseline)    |    4.04 |             9.9%  |  26.7%  |     5,677 |
| very sparse, `S = 1` (crank D) |    4.79 |             7.2%  |  19.7%  |     7,957 |
| average, `S = 3` (crank S)     |    4.71 |          **0%**   | **0%**  |     5,677 |

Both bottom rows reach roughly the same mean (~4.7), by different means. Cranking
the **density** to `very sparse` still leaves about one system in five with a
neighbor two hexes away or closer — and inflates the map ~40% — so a lucky start
can still expand on the cheapest engines. Cranking **`S`** to `3` removes the close
neighbors entirely (0% within two hexes) while the map stays the same size. Because
a faction invests only as much jump technology as its *cheapest* reachable
neighbor demands, `S` is the deliberate lever for tech pressure, while density
governs map size and the typical trip. The three concerns separate cleanly:

| Concern                 | Set by                  |
| ----------------------- | ----------------------- |
| Map size / exploration  | density only            |
| Typical trip length     | both (density stronger) |
| Guaranteed minimum jump | `S` only                |

**Raising `S` trades against feasibility.** Placement is random-sequential, not
optimal packing, so it jams well below the theoretical maximum. Because `S` has no
maximum, a GM can set it high enough that `N` systems cannot fit within the radius;
placement then exhausts the hex list and
**[generation fails](#placing-the-systems) — no cluster is created.** There is no
engine fallback: it does not relax `S`, grow the radius, or build a partial map.
This is expected, GM-resolved behavior, not a bug — the GM changes the seed, lowers
`S`, lowers `N`, or chooses a sparser density and generates again. At the default
`S = 2`, every legal `(N, D)` pair places all `N` systems on the first try in the
overwhelming majority of trials (100% across every tier in testing at `S ≤ 3`); the
radii are sized with generous headroom so this holds even at the corners `N = 10`
and `N = 1000`, densest and sparsest.

#### Worked examples

Each derives `R` from `N` and `D` (the radius is independent of `S`). The `R`
values are golden checkpoints — part of the reproducibility contract described
under [stability](#stability), below.

| # | `N`  | Density         | `T`    | Radius `R` | Hexes / System | Avg. NN at `S = 2` |
| - | ---- | --------------- | -----: | ---------: | -------------: | -----------------: |
| 1 | 100  | average         |  5,677 |         43 |           56.8 |              ~ 4.3 |
| 2 | 10   | extremely dense |    422 |         11 |           39.7 |              ~ 4.1 |
| 3 | 1000 | very sparse     | 79,570 |        162 |           79.2 |              ~ 4.9 |
| 4 | 1000 | extremely dense | 42,190 |        118 |           42.1 |              ~ 3.7 |

1. **Default** (`N = 100`, `average`) → `T = round(5677 × 100 / 100) = 5,677`;
   the nearest hex count is `5,677` exactly, at `R = 43`. The baseline map.
2. **Dense and small** (`N = 10`, `extremely dense`) →
   `T = round(4219 × 10 / 100) = 422`; the nearest hex count is `397` at `R = 11`.
3. **Sparse and large** (`N = 1000`, `very sparse`) →
   `T = round(7957 × 1000 / 100) = 79,570`; the nearest hex count is `79,219` at
   `R = 162`.
4. **Extreme corner** (`N = 1000`, `extremely dense`) →
   `T = round(4219 × 1000 / 100) = 42,190`; the nearest hex count is `42,127` at
   `R = 118`.

The avg-NN figures are measured at `S = 2` and shift with `N` — up somewhat at
small `N` (the map boundary crowds systems inward) and down slightly at large `N`.

**Failure example.** Take `N = 100`, `extremely dense` — so `R = 37` — but set
`S = 40`. Almost no two hexes within a radius-37 disk are 40 apart, so after
placing a handful of systems every remaining hex is rejected, the list is
exhausted, and **generation fails: no cluster is created.** The radius does not
grow and `S` is not relaxed. The GM must lower `S` (or `N`), pick a sparser
density, or try another seed.

#### Stability

Once the first real game is generated, these radii become part of the game's
reproducibility contract: the same `N` and `D` must always yield the same `R`, so
the same seeds reproduce the same map. **Changing the radius formula later would
change every existing map.** The worked examples above are therefore effectively
frozen the day a game is first persisted.

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
