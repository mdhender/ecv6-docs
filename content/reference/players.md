---
title: Players
weight: 5
---

A **player** is a person's seat in a single game. When an
[account]({{< relref "/reference/account.md" >}}) joins a game it takes a player
seat there; the player belongs to exactly one game and, unless they are the
game's GM, commands one [faction]({{< relref "/reference/faction.md" >}}). Each
game has its own set of players.

Your *global* identity — your email and password — lives on your
[account]({{< relref "/reference/account.md" >}}), not on the player. What the
player carries is everything specific to one game: its id, its active state, and
the faction it commands.

## Identity

A player is identified within its game by an **id**.

### ID

- A positive integer assigned by the engine when the player is created.
- Unique within the game.
- Assigned sequentially in increasing order.
  The first player created in a game is `1`, the next `2`, and so on.
  IDs are never reused within a game, even after a player is removed.

This is the **player** (seat) id, distinct from the account. It names a seat in
one game; a person's global identity is their [account]({{< relref "/reference/account.md" >}})
email, which is the same across every game.

## Active state

Every player is either **active** or **inactive**.
A player is active when created.

Players are never physically deleted.
**Removing** a player marks them inactive; the record — including its id — is retained in full.
**Reactivating** an inactive player marks them active again.
A player may move between the two states any number of times.

Because the record is retained, the player's id is never freed or reused, matching
the id rule above. A removed player's id stays assigned to that player (see
[Uniqueness and scope](#uniqueness-and-scope)).

## Uniqueness and scope

Within a single game, each player **id** is unique. Uniqueness spans every player
in the game, active or inactive: a removed player still holds its id, so it can
not be reused.

Players in different games are independent. Ids restart at `1` for each game. One
[account]({{< relref "/reference/account.md" >}}) may hold a player seat in many
games at once, each with its own id.

## Faction

Almost every player commands exactly one [faction]({{< relref "/reference/faction.md" >}}):
the in-game entity — its systems, the orders issued for it, and its turn report —
that the player acts through.

## Game master

One player in each game is the **game master (GM)**: a player whose GM flag is
set. The GM runs the game — generating the cluster, processing turns, and managing
the other players — and commands no faction of their own. In every other respect
the GM is an ordinary player: a seat in one game, with its own id, reached through
an [account]({{< relref "/reference/account.md" >}}).

## Randomness

Each player has a **private randomness stream**, used to draw per-player outcomes independently of other players and of cluster generation.
It is derived deterministically from the game's master seeds and the player's id.
See [Determinism]({{< relref "/reference/determinism.md" >}}).

Because a player's id never changes and is never reused (see [ID](#id) above), this stream is fixed for the life of the game.

## See also

- [Account]({{< relref "/reference/account.md" >}})
- [Faction]({{< relref "/reference/faction.md" >}})
- [Games]({{< relref "/reference/games.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
