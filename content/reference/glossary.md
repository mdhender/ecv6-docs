---
title: Glossary
weight: 9999
---

Definitions of terms used across the reference.

**Account**
: A person's global identity across all of EC: the email and password used to log in.
  Globally unique by email; the same account whether the person joins one game or many.
  In each game, an account takes a [player]({{< relref "/reference/players.md" >}}) seat.
  See [Account]({{< relref "/reference/account.md" >}}).

**Average nearest neighbor**
: The expected distance, in hexes, from a system to its closest neighbor. A *local* density metric — the "crowdedness" a player feels while exploring.
  See [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}).

**Axial coordinates**
: The coordinate system used to report hex positions, written `(q, r)`.
  The origin is `(0, 0)`.

**Cluster radius**
: How far from the [origin]({{< relref "/reference/cluster.md" >}}) a system may lie.
  A [Genesis Placement]({{< relref "/reference/generators/genesis/placement.md" >}}) value, derived from the number of systems and the stellar density. Another placement generator need not have one.
  See [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}).

<a id="dice-notation"></a>
**Dice notation**
: `XdY` means roll `X` independent `Y`-sided dice and add the results. Each die produces a whole number from `1` through `Y`, inclusive. Apply any addition or subtraction after totaling the dice. For example, `3d4 - 2` produces a value from `1` through `10`.

**Deposit**
: A body of one natural resource on a planet, with a **quantity** (its starting amount) and a **yield** (the percentage recovered).
  A planet carries deposits of fuel, metals, and non-metals; how many, and with what quantity and yield, is set by the game's [deposit generator]({{< relref "/reference/generators" >}}).
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Direction vector**
: An offset `(Δq, Δr)` added to a hex's coordinates to reach one of its six neighbors.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Faction**
: The in-game entity a player commands in a game — its systems, the orders issued for it, and its turn report.
  Founded by a player during setup and commanded by at most one player; a faction with no commanding player is **independent**.
  A founded faction persists — it may be reduced to nothing and still remain, and it outlives the departure of its player.
  See [Faction]({{< relref "/reference/faction.md" >}}).

**Flat-top**
: The hex orientation used by the grid: each hex has a flat edge at its top and bottom and a point at its left and right.

**Game**
: The top-level unit of play; the cluster, the players, and their factions belong to it.

**Generator**
: The rules that build one stage of the [cluster]({{< relref "/reference/cluster.md" >}}) at setup — placement, system contents, or deposits.
  The GM chooses one generator per stage, each with its own settings and its own version; a game records all three. Generators are grouped into **families**, such as [Genesis]({{< relref "/reference/generators/genesis" >}}); a family name is not a version.
  See [Generators]({{< relref "/reference/generators" >}}).

**GM**
: The game master: a player whose GM flag is set, who runs the game and commands no faction.

**Habitability**
: A per-planet number rating how habitable a planet is; higher values are more habitable.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Hexes per System**
: The average number of map hexes per star system, `Hexes ÷ Number of systems`. A *global* density metric — how much of the map is occupied overall.
  See [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}).

**Independent**
: A [faction]({{< relref "/reference/faction.md" >}}) with no commanding player.
  A faction becomes independent when the player commanding it leaves the game; other players still encounter and contend with it.
  See [Faction]({{< relref "/reference/faction.md" >}}).

**Jump route**
: An undirected, fixed route linking two systems, weighted by the [hex distance]({{< relref "/reference/cluster.md" >}}#measuring-distance) between them, along which ships travel between systems.
  Symmetric — the same route both ways — with no intermediate waypoints, and at most one per pair. Built when the cluster is generated and unchanging during play.
  The act of moving along a route (a **jump**) is defined by the movement rules, not here.
  See [Routes]({{< relref "/reference/routes.md" >}}).

**Master seed**
: One of the two `uint64` values (`seed1`, `seed2`) saved for a game.
  Together they are the root of the game's randomness; each subsystem derives its own seeds from them.
  See [Determinism]({{< relref "/reference/determinism.md" >}}).

**Minimum system spacing**
: The smallest distance, in hexes, allowed between any two systems in a cluster.
  An independent [Genesis Placement]({{< relref "/reference/generators/genesis/placement.md" >}}) setting (`S`): an integer of at least `1`, default `2`, with no maximum. Applied as a hard threshold when placing systems — a candidate hex closer than this to an existing system is rejected — and *not* derived from the stellar density.
  See [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}).

**Natural resources**
: The raw resources deposited on planets: **fuel** (`fuel`), **metals** (`mtls`), and **non-metals** (`nmtl`).
  A planet carries [deposits]({{< relref "/reference/glossary.md" >}}) of each; how many, and in what quantity and yield, is set by the game's [deposit generator]({{< relref "/reference/generators" >}}).
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Number of systems**
: How many systems the finished cluster contains (`N`), chosen by the GM.
  A [Genesis Placement]({{< relref "/reference/generators/genesis/placement.md" >}}) setting: an integer from `10` to `1,000` (default `100`). With the stellar density it sets the cluster radius. The allowed range and default belong to the placement generator, not to the game.
  See [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}).

**Orbit**
: One of the ten positions in a system that holds a planet, numbered `1` to `10` from the innermost outward. An orbit may be empty.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Origin**
: The center hex of the cluster, at axial coordinates `(0, 0)`.

**Planet**
: A body occupying an [orbit]({{< relref "/reference/glossary.md" >}}) in a system — rocky, gas giant, or asteroid belt — with a habitability and natural-resource deposits.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Player**
: A person's seat in a single game: the per-game id, active state, GM flag, and the faction commanded (none for the GM).
  A person's global identity — email and password — is their [account]({{< relref "/reference/account.md" >}}).
  See [Players]({{< relref "/reference/players.md" >}}).

**Route network**
: The complete set of [jump routes]({{< relref "/reference/glossary.md" >}}#jump-route) in a cluster — the graph along which ships travel between systems.
  A fixed rule of the cluster, not a generator's output: it is derived from the finished system positions and the GM's route-density tier once placement is done, the same way in every game and with no randomness.
  See [Routes]({{< relref "/reference/routes.md" >}}).

**Stellar density**
: A [Genesis Placement]({{< relref "/reference/generators/genesis/placement.md" >}}) setting for how large an area systems are spread across, from `extremely dense` to `very sparse`.
  With the number of systems it sets the cluster radius — a denser tier spreads the same systems across a smaller map — and so governs the average distance between systems. It does not set the minimum system spacing.
  See [Placement]({{< relref "/reference/generators/genesis/placement.md" >}}).

**System**
: The contents of a single occupied hex in the [cluster]({{< relref "/reference/cluster.md" >}}), addressed by its axial coordinates `(q, r)`.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Turn**
: The unit of play a game advances by.
  Turn `0` is setup (no turn); play begins at turn `1` and counts up.
  See [Turns]({{< relref "/reference/turns.md" >}}).

**Yield**
: The percentage of a [deposit]({{< relref "/reference/glossary.md" >}}) that is recovered when it is worked; higher is better.
  Set by the game's [deposit generator]({{< relref "/reference/generators" >}}).
  See [Cluster]({{< relref "/reference/cluster.md" >}}).
