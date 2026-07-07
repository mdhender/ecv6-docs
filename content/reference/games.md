---
title: Games
weight: 1
---

A **game** is the top-level unit of play.
Everything else — the cluster, the players, and the factions they command — belongs to a game.

## Identity

Every game has an id and a pair of master seeds.
It also tracks its current turn.

### ID

- A short slug the GM chooses to name the game.
- Quoted text that contains no characters that require escaping in JSON and none that could be confused with an ASCII space.

### Master seeds

- Two `uint64` values, `seed1` and `seed2`, that make the game deterministic.
- They are the root of every random outcome in the game (see [Seeds and subsystems](#seeds-and-subsystems)).

### Current turn

- The turn the game is on now.
- A new game starts at turn `0` (setup — no turn); play begins at turn `1`.
  See [Turns]({{< relref "/reference/turns.md" >}}).

## Manifest

## Seeds and subsystems

The game's master seeds are the root of all randomness.
Each subsystem derives its own master seeds from the game's, deterministically:

- The **cluster** derives its master seeds from the game's when it is generated.
- Each **player** derives private seeds from the game's, keyed by the player's id.

A subsystem's derived seeds are stored with the subsystem's own data, so the subsystem carries everything it needs to reproduce its randomness on its own.
This lets a scenario be exercised without standing up a whole game, which is convenient when writing and testing code.

Because those seeds were derived from one game's master seeds, data belongs to the game that created it.
Sharing data between games is a convenience for development and testing only; production games do not share data.

## See also

- [Glossary]({{< relref "/reference/glossary.md" >}})
