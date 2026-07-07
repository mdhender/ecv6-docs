---
title: Players
weight: 4
---

A **player** is a person participating in a game.
Players are scoped to a game: each game has its own set of players, and a player belongs to exactly one game.

## Identity

Every player has two unique identifying fields: an id and an email.

### ID

- A positive integer assigned by the engine when the player is created.
- Unique within the game.
- Assigned sequentially in increasing order.
  The first player created in a game is `1`, the next `2`, and so on.
  IDs are never reused within a game, even after a player is removed.

### Email

- The player's email address.
- Stored in lowercase.
  The email is lowercased when the player is saved, so `Alice@Example.com` is stored and matched as `alice@example.com`.
- Unique within the game, compared after lowercasing.

## Active state

Every player is either **active** or **inactive**.
A player is active when created.

Players are never physically deleted.
**Removing** a player marks them inactive; the record — including its id and email — is retained in full.
**Reactivating** an inactive player marks them active again.
A player may move between the two states any number of times.

Because the record is retained:

- The player's id is never freed or reused, matching the id rule above.
  A removed player's id stays assigned to that player.
- The player continues to occupy its email.
  The email can not be taken by a new or existing player while the inactive record holds it — uniqueness is enforced across active and inactive players alike (see [Uniqueness and scope](#uniqueness-and-scope)).

## Uniqueness and scope

Within a single game, each of `id`, `email` is unique.
Email uniqueness is checked after lowercasing.
Uniqueness spans every player in the game, active or inactive: an inactive player still holds its email, so it can not be reused.

Players in different games are independent.
The same email may appear in more than one game, and ids restart at `1` for each game.

## Password

Each player has a **password**: a shared secret used to authenticate the player.

- Generated when the player is created.
- Stored in plain text.
- Contains no characters that require escaping in JSON and none that could be
  confused with an ASCII space.

### Resetting a password

A player's password can be **reset** — reissued — when the current one has been exposed.

## Randomness

Each player has a **private randomness stream**, used to draw per-player outcomes independently of other players and of cluster generation.
It is derived deterministically from the game's master seeds and the player's id.
See [Determinism]({{< relref "/reference/determinism.md" >}}).

Because a player's id never changes and is never reused (see [ID](#id) above), this stream is fixed for the life of the game.

## See also

- [Glossary]({{< relref "/reference/glossary.md" >}})
