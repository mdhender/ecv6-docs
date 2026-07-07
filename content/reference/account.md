---
title: Account
weight: 4
---

An **account** is a person's identity across all of Epimethean Challenge. There
is one account per person: it holds the email and password you log in with, and
it is the same whether you join a single game or many. In each game you join,
your account takes a seat as a [player]({{< relref "/reference/players.md" >}})
commanding a [faction]({{< relref "/reference/faction.md" >}}).

The account is where your *global* identity lives. Anything specific to one game
— a per-game id, active state, the faction you command — belongs to the player
seat, not the account.

## Email

An account is identified by its **email**.

- Stored in lowercase. The email is lowercased when the account is saved, so
  `Alice@Example.com` is stored and matched as `alice@example.com`.
- **Globally unique**, compared after lowercasing: one account per email address,
  across every game. (Contrast this with the [player id]({{< relref "/reference/players.md" >}}#id),
  which is unique only within a single game.)

## Password

Each account has a **password**: the shared secret it uses to authenticate — the
value you present to prove the account is yours.

- Issued when the account is created.
- Contains no characters that require escaping in JSON and none that could be
  confused with an ASCII space.

### Resetting a password

A password can be **reset** — reissued — when the current one has been exposed.
The old password stops working once the new one is issued. See
[Reset your password]({{< relref "/how-to/reset-your-password.md" >}}).

## Accounts, players, and factions

*Your account lets you log in; in each game you join you are a player commanding
a faction.*

- The **account** is global: one per person, reused across every game, carrying
  your email and password.
- In each game, your account takes a [player]({{< relref "/reference/players.md" >}})
  seat — the per-game id, active state, and GM flag live there.
- Each player commands one [faction]({{< relref "/reference/faction.md" >}}), the
  in-game entity that acts within the game.

## See also

- [Players]({{< relref "/reference/players.md" >}})
- [Faction]({{< relref "/reference/faction.md" >}})
- [Games]({{< relref "/reference/games.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
