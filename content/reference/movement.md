---
title: Movement
weight: 5
---

Ships move over the [cluster]({{< relref "/reference/cluster.md" >}}) in two ways:
**jumps** between systems and **maneuvers** within a system. This page defines the
observable mechanics — turns taken, fuel burned, and what other players can see.
Exact numbers (fuel rates, and how a ship's mass sets its engine count) are settled
elsewhere; this page fixes the *shape* of the rules.

## Two kinds of movement

| Movement     | Scope                          | Travels                                             | Drive               |
| ------------ | ------------------------------ | --------------------------------------------------- | ------------------- |
| **Jump**     | Interstellar, system → system  | along a [jump route]({{< relref "/reference/routes.md" >}}) | **HDRV** (hyperdrive) |
| **Maneuver** | Intra-system, orbit → orbit    | within one system                                   | **SDRV** (system drive) |

Each drive has its **own tech level (TL)**, and the two are independent: a ship may
carry a strong hyperdrive and a weak system drive, or the reverse.

## Engine count

Both drives charge fuel per **drive unit powered**, and the number of units powered
is set by the **ship's mass** — enough units to move the mass, and no more. A light
or empty ship powers fewer units and burns less fuel than a heavy one.

- You are charged for units **powered, not installed**. Extra drives a ship is not
  using cost nothing to carry.
- Engine count is a function of **mass only — never distance**. A longer trip does
  not power more units; it burns the same per-hex rate over more hexes (and, for
  jumps, through more [bands](#fuel-and-bands)).

{{< callout type="info" >}}
The exact `mass → units powered` function and the per-unit fuel rate for each drive
are not yet fixed, and will be documented here once they are.
{{< /callout >}}

## Jumps

A jump moves a ship between two systems along a
[jump route]({{< relref "/reference/routes.md" >}}) that links them.

### Reach and turns

A route's **distance** `d` is its
[hex distance]({{< relref "/reference/cluster.md" >}}#measuring-distance) — the
fewest hexes between its two endpoint systems.

The hyperdrive's tech level `t` is its **reach: `t` hexes per turn** at normal
drive. A jump of distance `d` therefore takes

```
turns = ⌈d / t⌉
```

A ship with a TL-`t` drive crosses up to `t` hexes each turn. When a route is longer
than `t`, the jump spans several turns and the drive runs into **overdrive**.

### Fuel and bands

Fuel for a jump is the per-hex rate applied hex by hex, with an **overdrive penalty**
that grows the farther a ship pushes past its reach on one route without stopping.

The route is sliced into **bands of `t` hexes**, and the band boundaries scale with
the drive's TL:

| Drive TL | Band 1     | Band 2      | Band 3      | …   |
| -------- | ---------- | ----------- | ----------- | --- |
| TL-1     | hex 1      | hex 2       | hex 3       | …   |
| TL-3     | hexes 1–3  | hexes 4–6   | hexes 7–9   | …   |
| TL-10    | hexes 1–10 | hexes 11–20 | hexes 21–30 | …   |

The hex at position `p` sits in band `⌈p / t⌉` and carries a fuel multiplier of
**`×2^(band − 1)`** — ×1 in band 1, ×2 in band 2, ×4 in band 3, and so on. This
doubling is **baked into the drive; it is not configurable.**

Putting it together:

- **Per-hex rate** = `(units powered, from mass) × (per-unit rate)`.
- **Total fuel** = the sum over bands of `hexes in band × per-hex rate × band multiplier`.

**Mass sets the rate, distance sets the bands, and the two multiply.** Overdrive is
paid **once per spatial segment**, not re-paid each turn, and distance never
increases the engine count.

**Worked example.** A TL-10 drive (reach 10), a per-hex rate of 2, on a jump of 11
hexes:

| Turn | Hexes crossed | Band | Multiplier | Fuel              |
| ---- | ------------- | ---- | ---------- | ----------------- |
| 1    | 1–10          | 1    | ×1         | `10 × 2 × 1 = 20` |
| 2    | 11            | 2    | ×2         | `1 × 2 × 2 = 4`   |

**Total: 24 fuel over 2 turns.**

### Stopping resets the band

The band multiplier counts **hexes crossed since the ship last stopped at a system**,
across however many routes that covers. Stopping at a system **resets** the count to
band 1; passing *through* a system without stopping keeps it climbing.

So a long trip made as a series of **short hops** — stopping at each intermediate
system — keeps every leg in band 1 and never pays overdrive. A **single route longer
than the drive's reach** forces overdrive: there is nowhere to stop mid-route.

### While in transit

A jump that spans more than one turn leaves the ship **in transit between systems**,
flagged **`in-hyperspace`**:

- It **does not appear on scans and is not seen by other players** while in transit.
- It **does not stop at or interact with** the systems it passes, even a real system
  lying directly on its path.

The engine tracks where the ship sits between turns; a player does not need to work
that out.

### Waypoints are yours to choose

Ships travel only along existing routes, so a journey is a **walk over the route
graph**. To go from `A` to `C` when the only routes are `A–B` and `B–C`, a ship goes
`A → B → C` — traversing route `A–B`, then route `B–C`. The path bends at each
system; it is **not** a straight line from start to finish.

**You specify the waypoints** — the full ordered list of systems to pass through, and
where to stop. The engine **executes that intent; it does not invent it.** It checks
that each consecutive pair is a real route and that the ship can complete the whole
journey (the [pre-flight check](#pre-flight-fuel-check)), then runs the fuel, turn,
and visibility mechanics. There is **no authoritative route-finder**: the engine
never picks a path for you, so which route you take is always your own decision.

{{< callout type="info" >}}
A convenience that **suggests** a cheap path may be offered later, but only as an
advisory aid — never an authority — in the same category as the optional
[route-map exports]({{< relref "/reference/routes.md" >}}#deriving-the-network-by-hand).
{{< /callout >}}

### Pre-flight fuel check

The drive **refuses to begin a jump it cannot complete.** A ship never strands
mid-route, and there is no fuel-confiscation penalty for trying: a journey it cannot
finish simply does not start.

### Stop through, or punch through

For any leg longer than the drive's reach, a player chooses between two supported
tactics:

| Tactic                            | Visibility               | Band            | Cost             |
| --------------------------------- | ------------------------ | --------------- | ---------------- |
| **Stop at an intermediate system**| Visible at the stop      | Resets to band 1| Slightly cheaper |
| **Punch through in hyperspace**   | Invisible (`in-hyperspace`) | Keeps climbing | Pays the overdrive |

## Maneuvers

A maneuver moves a ship between **orbits within a single system**, using the system
drive (SDRV).

- A ship may move between **any two orbits in one turn**, however many orbits
  separate them. There is **no band or overdrive structure** inside a system.
- **Fuel** = `(units needed, from mass) × (per-unit rate)`, modified by the SDRV
  tech level.

## A known consequence

{{< callout type="warning" >}}
Because fuel scales with the drive units a ship **powers** (set by its mass), not the
units it **installs**, an over-engined but lightly loaded hull jumps just as cheaply
as a lean one — carrying idle drives costs nothing. This is the current rule, and is
implemented as written. It is on the record as a candidate for a future redesign if
it plays badly.
{{< /callout >}}

## See also

- [Routes]({{< relref "/reference/routes.md" >}}) — the network jumps travel along
- [Cluster]({{< relref "/reference/cluster.md" >}}) — systems, orbits, and the hex-distance metric
- [Determinism]({{< relref "/reference/determinism.md" >}}) — why the same journey always costs the same
- [Glossary]({{< relref "/reference/glossary.md" >}})
