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

**Account**
: A person's global identity across all of EC: the email and password used to log in.
  Globally unique by email; the same account whether the person joins one game or many.
  In each game, an account takes a [player]({{< relref "/reference/players.md" >}}) seat.
  See [Account]({{< relref "/reference/account.md" >}}).

**Faction**
: The in-game entity a player commands in a game — its systems, the orders issued for it, and its turn report.
  Each player commands exactly one faction.
  See [Faction]({{< relref "/reference/faction.md" >}}).

**Game**
: The top-level unit of play; the cluster, the players, and their factions belong to it.

**GM**
: The game master: a player whose GM flag is set, who generates and runs the game.

**Master seed**
: One of the two `uint64` values (`seed1`, `seed2`) saved for a game.
  Together they are the root of the game's randomness; each subsystem derives its own seeds from them.
  See [Determinism]({{< relref "/reference/determinism.md" >}}).

**Origin**
: The center hex of the cluster, at axial coordinates `(0, 0)`.

**Player**
: A person's seat in a single game: the per-game id, active state, GM flag, and the faction commanded.
  A person's global identity — email and password — is their [account]({{< relref "/reference/account.md" >}}).
  See [Players]({{< relref "/reference/players.md" >}}).

**Turn**
: The unit of play a game advances by.
  Turn `0` is setup (no turn); play begins at turn `1` and counts up.
  See [Turns]({{< relref "/reference/turns.md" >}}).
