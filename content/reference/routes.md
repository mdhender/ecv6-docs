---
title: Routes
weight: 4
---

Ships travel between systems only along **jump routes**. The routes are the edges
of the [cluster]({{< relref "/reference/cluster.md" >}})'s map — they fix which
systems are directly reachable from which — and, like the systems themselves, they
are set once when the cluster is generated and never change during play.

This page defines the network: what a route is, and how the whole network is
derived. How a ship actually *moves* along a route — jump range, cost, multi-turn
jumps — belongs to the movement rules, not here.

## What a jump route is

A jump route is an **undirected edge between two systems**:

- **Symmetric and bidirectional.** The route from `A` to `B` is the same route as
  `B` to `A`, with the same distance. There are no one-way routes.
- **A pure graph edge.** A route links its two endpoint systems directly, with no
  intermediate waypoints. It carries a single value — its **distance**, the
  [hex distance]({{< relref "/reference/cluster.md" >}}#measuring-distance) between
  the endpoints: the fewest hexes between the two systems.
- **Canonical.** There is at most one route between any given pair of systems.
- **Fixed at generation.** The whole network is built once, when the cluster is
  generated, and never changes during play.

## The route network

The route network is a **fixed rule of the cluster, not a generator choice.** Once
placement has given every system its coordinates, the routes follow from the
procedure below — the same procedure in every game, whichever
[generators]({{< relref "/reference/generators" >}}) built the map. Deriving the
network consumes **no randomness**; it is a pure function of the system positions
and one GM setting, the **route-density tier**.

The network is built in three passes, in order:

1. **Backbone — connectivity.** The backbone is the **minimum spanning tree (MST)**
   over all systems, weighting each candidate route by the
   [hex distance]({{< relref "/reference/cluster.md" >}}#measuring-distance) between
   its endpoints. The MST is the cheapest set of routes that still leaves every
   system reachable from every other, so the backbone alone makes the map fully
   connected.

   Hex distances tie often, so the MST is not unique by distance alone. Ties break
   by **coordinate order**, which pins down a single backbone: order systems by
   their coordinates `(q, r)` lexicographically — by `q`, then by `r` — and key
   each candidate route by its two endpoints in that order, lower endpoint first;
   when two candidates are the same distance, take the one with the smaller key.
   This tie-break is a **frozen surface** — changing it re-derives every existing
   map — so once a game is generated it is never changed.

2. **Adjacency completion — logistics.** Every pair of systems in **adjacent hexes**
   (hex distance `1`) is linked by a route, wherever the backbone has not already
   linked them. A system one hex away is always directly reachable; there is never
   a longer way around to a next-door neighbor.

3. **Texture — density.** The backbone and adjacency passes leave a connected but
   skeletal map. The texture pass adds further routes to give the cluster **hubs
   and backwaters**, preferring systems whose spacing — the distance to their
   nearest neighbor — is near the cluster's **median**, neither the most isolated
   nor the most crowded. How many it adds is set by the GM's route-density tier:

   | Tier                    | Extra routes per system (approx.) |
   | ----------------------- | --------------------------------- |
   | Extra sparse            | +0 (backbone + adjacency only)    |
   | Sparse                  | +0.5                              |
   | **Average** *(default)* | +1                                |
   | Dense                   | +2                                |
   | Extra dense             | +3                                |

   Extra sparse is the sparsest legal map — backbone and adjacency only. Toward the
   dense end the map self-limits as systems fill their six routes (see
   [Route limits](#route-limits)).

{{< callout type="info" >}}
The three passes and their order — MST backbone, adjacency completion, texture —
and the backbone's coordinate-order tie-break are the fixed part of this rule. The
**exact per-tier route counts** and the precise texture-selection rule are still
being tuned, and become frozen the day the first game is generated (as with the
[placement radii]({{< relref "/reference/generators/genesis/placement.md" >}}#stability)).
{{< /callout >}}

## Route limits

- **At most six routes per system — a ceiling that falls out of the geometry, not a
  limit we impose.** A hex has only six neighbors, so the adjacency pass can give a
  system at most six routes, and neither other pass ever adds a seventh. The
  backbone never does: to attach a system ringed by six occupied neighbors, the MST
  always takes one of those adjacent neighbors — the cheapest edge there is — rather
  than a longer edge to a distant system, so every backbone route such a system gets
  is one adjacency would have added anyway. The texture pass never does either: it
  favors systems of middling spacing and skips the most crowded, and a system boxed
  in on all six sides is as crowded as they come. So no system ends up with more than
  six routes — the number describes the map, it does not constrain it.
- **Always fully connected.** From any system you can reach every other by hopping
  routes. The backbone guarantees this before adjacency or texture adds anything.

## Deriving the network by hand

Route derivation uses **no randomness** — it is a pure function of the system
positions and the route-density tier — so the published map fully determines the
network. Given both, a player can reconstruct every route by hand: build the MST
(breaking ties by coordinate order), link every adjacent pair, then add texture
routes for the tier. A game may also publish convenience exports — a route diagram,
or the edges as CSV/JSON — but those are conveniences, not part of the definition.
See [Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Cluster]({{< relref "/reference/cluster.md" >}}) — the systems the routes connect, and the distance metric
- [Generators]({{< relref "/reference/generators" >}}) — how the systems a route links are placed
- [Determinism]({{< relref "/reference/determinism.md" >}}) — why the same map always yields the same routes
