---
title: Orders
weight: 8
---

**Orders** are the instructions a player issues for a turn.
A player reads their [report]({{< relref "/reference/turns.md" >}}) for turn `N`,
writes orders for turn `N`, and emails them back to the GM, who applies them when
processing the turn.
Orders are plain text.

This page is the authoritative catalogue of order syntax.
For the procedure of composing and sending a turn, see
[Submit your turn by email]({{< relref "/how-to/submit-your-turn.md" >}}).

{{< callout type="info" >}}
This page is a stub. The order grammar and the catalogue of order types below are
placeholders to be filled in as each subsystem is documented.
{{< /callout >}}

## Format

Orders are written as plain text, one order per line.

{{< callout type="info" >}}
The exact grammar — how a line is tokenized, which fields are required, how
comments and blank lines are treated, and how errors are reported back to the
player — will be specified here.
{{< /callout >}}

## Order catalogue

Each order type is described below with its syntax, its parameters, and its
effect when the turn is processed.

{{< callout type="info" >}}
Individual orders will be listed here as their rules are settled. Each entry
gives the order's name, its arguments, and what it does.
{{< /callout >}}

## Processing

Orders take effect when the GM **processes** the turn, not when they are received.
All of a player's orders for a turn are applied together during processing; the
game's [current turn]({{< relref "/reference/turns.md" >}}) does not advance until
the GM advances it.

## See also

- [Turns]({{< relref "/reference/turns.md" >}})
- [Submit your turn by email]({{< relref "/how-to/submit-your-turn.md" >}})
- [Glossary]({{< relref "/reference/glossary.md" >}})
