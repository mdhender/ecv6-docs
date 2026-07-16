---
title: Genesis deposits generator design v1
draft: true
---

## Purpose

Genesis is the third stage of world generation. The Cluster Generator
determines where stars are located. The System Generator determines what
planets exist and assigns Habitability. The Deposits Generator
determines the economic geology of those planets.

Genesis generates **nature**, not economics. It determines what
resources exist, where they are found, and their initial quality.
Technology, industry, environmental policy, and extraction rates belong
to the game engine.

## Terminology

-   **Resource** --- Fuel, Metals, or Non-metals.
-   **Endowment** --- Total amount of a resource assigned to a system or
    planet.
-   **Deposit** --- A mineable body containing exactly one resource.
-   **Ore Quantity** --- The amount of raw material remaining in a
    deposit.
-   **Yield** --- The percentage of extracted ore recovered as usable
    product.
-   **Habitability** --- A measure of agricultural potential and
    environmental sensitivity.

Each deposit stores:

-   Resource Type
-   Initial Quantity
-   Current Quantity
-   Initial Yield
-   Current Yield

Current values are initialized from the Initial values and evolve during
play.

## Design Philosophy

The generator follows a fixed pipeline:

1.  Determine system endowment.
2.  Distribute resources among planets.
3.  Apply planet-level quantity variation.
4.  Determine deposit counts.
5.  Assign deposits to resources.
6.  Divide resource quantities among deposits.
7.  Apply deposit quantity variation.
8.  Generate deposit yields.
9.  Apply Habitability yield penalty.

Every stage has a single responsibility.

## Inputs

The generator receives:

-   Complete system description.
-   Planet types.
-   Habitability values.
-   Three independent resource knobs:
    -   Fuel
    -   Metals
    -   Non-metals

Each knob is:

-   Rich
-   Average
-   Poor

## System Endowment

Average ten-planet systems contain configurable baseline amounts:

-   Af (Fuel)
-   Am (Metals)
-   An (Non-metals)

Systems with fewer planets scale proportionally.

Each resource knob modifies only its corresponding resource.

Rich: - +3d20%

Average: - No change to baseline quantity.

Poor: - -3d20%

Each resource rolls independently.

## Planet Distribution

Resources are distributed according to planet affinities.

  Planet Type         Fuel     Metals   Non-metals
  ------------------- -------- -------- ------------
  Rocky terrestrial   High     Normal   Low
  Asteroid belt       Low      High     Normal
  Gas giant           Normal   Low      High

Distribution occurs before any variation.

## Planet Variation

After distribution, each planet's resource quantities receive an
independent modifier.

Rich: - +3d20%

Average: - 2d12-13%

Poor: - -3d20%

No renormalization occurs.

## Deposit Counts

Deposit counts depend on planet type.

  Planet Type         Deposits
  ------------------- ------------------
  Rocky terrestrial   4d10 (4--40)
  Gas giant           20+2d10 (22--40)
  Asteroid belt       30+d10 (31--40)

## Resource Assignment

Every deposit contains exactly one resource.

Deposit slots are assigned proportionally to each resource's share of
the planet's endowment while guaranteeing at least one deposit for every
resource present.

## Deposit Quantities

Within each resource class, quantities are divided among deposits using
weighted random allocation.

Each deposit then receives an independent quantity modifier:

-   Rich: +3d20%
-   Average: 2d12-13%
-   Poor: -3d20%

## Base Yield

Base yields are:

  Resource       Base Yield
  ------------ ------------
  Fuel                   5%
  Metals                12%
  Non-metals             9%

Each deposit rolls an independent yield modifier using the same table:

-   Rich: +3d20%
-   Average: 2d12-13%
-   Poor: -3d20%

## Habitability

Habitability affects agriculture, not geology.

Habitability 20 or less: - No mining penalty.

Habitability 21--25: - Reduce deposit yield by 5% per point above 20.

This models environmental protection on highly habitable worlds,
encouraging specialization between agricultural and industrial colonies.

## Yield

Yield is the recovery efficiency of extracted ore.

Example:

-   Deposit contains 100 ore.
-   Mine extracts 20 ore.
-   Yield is 10%.

Results:

-   2 units of usable product extracted.
-   18 units added to "tailings."
-   Deposit now contains 80 units of ore, 18 units of tailings.

Higher technology may improve Current Yield, allowing future extraction
to recover more usable product from the remaining ore.

## Generator Invariants

Genesis guarantees:

-   Every deposit contains exactly one resource.
-   Every deposit has positive quantity.
-   Every deposit has a yield.
-   Quantities are generated before yields.
-   Geology never changes after generation.
-   Technology changes Current Yield, never Initial Yield.
-   Player actions never regenerate deposits.

## GM Tuning Guide

To make clusters richer: - Increase Af, Am, or An.

To favor one economy: - Set the corresponding resource knob to Rich.

To increase mining fragmentation: - Increase deposit count ranges.

To encourage agriculture: - Increase the Habitability penalty.

To improve mining profitability: - Increase base yields.

Each tuning parameter affects a single aspect of the simulation,
allowing campaign economies to be shaped predictably.
