---
title: Set up a new game
weight: 4
---

This guide shows how to stand up a new game of Epimethean Challenge (EC) and take
it from an empty record through to the first turn reports. It spans two roles: an
**administrator**, who creates the game, and its **GM**, who configures it,
generates the map, recruits players, and starts play.

It assumes you already know the vocabulary — [games]({{< relref "/reference/games.md" >}}),
the [cluster]({{< relref "/reference/cluster.md" >}}),
[players]({{< relref "/reference/players.md" >}}), and
[factions]({{< relref "/reference/faction.md" >}}). If you don't, read those
reference pages first.

{{< callout type="info" >}}
There are no client tools for this yet, so the steps below describe *what* has to
happen, not which command or screen does it. The exact tooling — how you enter the
seeds, generate the cluster, or send the reports — is a placeholder to be filled in
as it lands.
{{< /callout >}}

## Create the game (administrator)

1. **Create the game record with its master seeds.**
   A new game is fixed by its [master seeds]({{< relref "/reference/games.md" >}}#master-seeds)
   — the pair of `uint64` values (`seed1`, `seed2`) that are the root of every
   random outcome in it. Choose them once, at creation. Everything the game
   generates later — the cluster and each player's private outcomes — derives from
   these, so the same seeds always reproduce the same game (see
   [Determinism]({{< relref "/reference/determinism.md" >}})).

   The game starts at **turn 0**: the setup phase, before play begins (see
   [Turns]({{< relref "/reference/turns.md" >}}#turn-numbering)).

2. **Assign a GM.**
   Assign one [player]({{< relref "/reference/players.md" >}}#game-master) as the
   game's **game master**. The GM runs the game and commands no
   [faction]({{< relref "/reference/faction.md" >}}) of their own. Everything that
   follows is the GM's work.

## Configure and launch the game (GM)

3. **Configure the game's settings.**
   Set the options that govern this game before anything is generated from the
   seeds.

   {{< callout type="info" >}}
   The list of settings — map size, player count, turn cadence, and the rest — will
   be documented here as those rules are settled.
   {{< /callout >}}

4. **Generate the cluster.**
   Generate the [cluster]({{< relref "/reference/cluster.md" >}}) — the hex map the
   game is played on, together with the **systems** it holds and each system's
   **stars, planets, and natural resources**. The cluster is generated once, at
   setup, and derives its own seeds from the game's, so it is reproducible and
   order-independent (see [Determinism]({{< relref "/reference/determinism.md" >}})).

   Generation runs in three stages — placement, system contents, and deposits — and
   you choose a **generator** for each, along with its settings. This version ships
   the [Genesis]({{< relref "/reference/generators/genesis" >}}) family; its
   settings, and the map they produce, are documented under
   [Generators]({{< relref "/reference/generators" >}}). Record which generators and
   versions you ran: players need them to know which rules built their cluster.

5. **Prepare home systems.**
   Decide where the players will start:

   - **Build the home-system template** — the standard makeup a starting system is
     given, so that every player begins on comparable footing.
   - **Choose the potential home systems** — the systems in the cluster you will
     hand out to arriving players as their starting positions.

   {{< callout type="info" >}}
   What a home-system template specifies, and how many potential home systems a game
   needs, will be documented here once those rules are settled.
   {{< /callout >}}

6. **Recruit the players.**
   Invite the people who will play, and email each of them instructions to:

   - **Create an [account]({{< relref "/reference/account.md" >}})** if they don't
     already have one — their global login, reused across every game.
   - **Join this game and found their [faction]({{< relref "/reference/faction.md" >}})**
     — taking a [player]({{< relref "/reference/players.md" >}}) seat and founding
     the faction they will command. A newly joined player commands no faction until
     they found one.

   See [Getting started]({{< relref "/tutorials/getting-started.md" >}}) for the
   player's side of this step.

7. **Start play.**
   Once enough players have joined and founded their factions, launch the game:

   - **Advance the turn counter** from turn 0 to **turn 1**, the first turn of play
     (see [Turns]({{< relref "/reference/turns.md" >}}#per-turn-lifecycle)).
   - **Send the first turn reports** — each faction's report for turn 1, which
     describes the state at the start of the turn and is what each player reads to
     decide their orders.

## Verify

The game is live once every joined player has received their **turn 1** report and
the game's current turn reads **1**. From here play proceeds one turn at a time:
players submit orders, and the GM processes and advances the game (see
[Turns]({{< relref "/reference/turns.md" >}})).

## See also

- [Games]({{< relref "/reference/games.md" >}}) — master seeds and current turn
- [Cluster]({{< relref "/reference/cluster.md" >}}) — the map you generate at setup
- [Turns]({{< relref "/reference/turns.md" >}}) — turn 0 setup and advancing to turn 1
- [Getting started]({{< relref "/tutorials/getting-started.md" >}}) — how a recruited player joins
