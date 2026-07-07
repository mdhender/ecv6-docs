---
title: Determinism
weight: 6
---

EC is deterministic: a game is fixed by its master seeds. The same seeds
always reproduce the same game — the same cluster, the same outcomes — on any
machine, no matter when it is run or in what order things are processed.

## Reproducible from the seeds

A game records its master seeds when it is created (see
[Games]({{< relref "/reference/games.md" >}})). Everything random in the game
— the cluster, and each player's private outcomes — is derived from those
seeds. Because that derivation is fixed, regenerating a game from the same
seeds yields an identical result.

## Independent of order

An outcome depends on *what* it belongs to, not on the order things happen. A
system's contents are fixed by the system's location; a player's private
outcomes are fixed by that player. So the result does not change with the
order players are handled, the order the GM processes a turn, or who submitted
first.

## See also

- [Games]({{< relref "/reference/games.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
