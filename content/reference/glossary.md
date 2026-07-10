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
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Axial coordinates**
: The coordinate system used to report hex positions, written `(q, r)`.
  The origin is `(0, 0)`.

**Cluster radius**
: How far from the [origin]({{< relref "/reference/cluster.md" >}}) a system may lie.
  Calculated during cluster generation from the number of systems and the stellar density.
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

**GM**
: The game master: a player whose GM flag is set, who runs the game and commands no faction.

**Habitability**
: A per-planet number rating how habitable a planet is; higher values are more habitable.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Hexes per System**
: The average number of map hexes per star system, `Hexes ÷ Number of systems`. A *global* density metric — how much of the map is occupied overall.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Independent**
: A [faction]({{< relref "/reference/faction.md" >}}) with no commanding player.
  A faction becomes independent when the player commanding it leaves the game; other players still encounter and contend with it.
  See [Faction]({{< relref "/reference/faction.md" >}}).

**Master seed**
: One of the two `uint64` values (`seed1`, `seed2`) saved for a game.
  Together they are the root of the game's randomness; each subsystem derives its own seeds from them.
  See [Determinism]({{< relref "/reference/determinism.md" >}}).

**Minimum system spacing**
: The smallest distance, in hexes, allowed between any two systems in a cluster.
  An independent cluster-generation setting (`S`): an integer of at least `1`, default `2`, with no maximum. Applied as a hard threshold when placing systems — a candidate hex closer than this to an existing system is rejected — and *not* derived from the stellar density.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Natural resources**
: The raw resources deposited on planets: **fuel** (`fuel`), **metals** (`mtls`), and **non-metals** (`nmtl`).
  Each has a cluster-generation abundance setting — `poor`, `average`, or `rich` — that shapes the deposits placed during generation.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Number of systems**
: A cluster-generation setting: how many systems the finished cluster contains, an integer from `10` to `1,000` (default `100`). With the stellar density it sets the cluster radius.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

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

**Stellar density**
: A cluster-generation setting for how large an area systems are spread across, from `extremely dense` to `very sparse`.
  With the number of systems it sets the cluster radius — a denser tier spreads the same systems across a smaller map — and so governs the average distance between systems. It does not set the minimum system spacing.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**System**
: The contents of a single occupied hex in the [cluster]({{< relref "/reference/cluster.md" >}}), addressed by its axial coordinates `(q, r)`.
  See [Cluster]({{< relref "/reference/cluster.md" >}}).

**Turn**
: The unit of play a game advances by.
  Turn `0` is setup (no turn); play begins at turn `1` and counts up.
  See [Turns]({{< relref "/reference/turns.md" >}}).
