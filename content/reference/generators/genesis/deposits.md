---
title: Deposits
weight: 3
---

**Genesis Deposits** — status **draft**, version **v1**.

The deposit generator decides **which natural resources occur on each planet and
the quantity and yield of every deposit**. It runs after
[system contents]({{< relref "/reference/generators/genesis/system-contents.md" >}})
and processes both the ordinary systems and the home-system template.

Deposits contain one of three resources: **fuel** (`fuel`), **metals** (`mtls`), or
**non-metals** (`nmtl`). Each deposit has a quantity and a yield:

- **Quantity** is the amount of raw material initially available.
- **Yield** is the percentage of extracted material recovered as usable product.

For both values, a deposit's initial and current values are identical when it is
generated. Current values may change during play; initial values do not.

## Settings

Genesis Deposits takes two GM-provided settings for each resource: an
**abundance** knob and a **baseline endowment**.

| Setting                       | Default         | Allowed values            |
| ----------------------------- | --------------- | ------------------------- |
| Fuel abundance (`fuel`)       | `average`       | `poor`, `average`, `rich` |
| Metals abundance (`mtls`)     | `average`       | `poor`, `average`, `rich` |
| Non-metals abundance (`nmtl`) | `average`       | `poor`, `average`, `rich` |
| Fuel endowment (`Af`)         | `4,891,250,000` | any positive amount       |
| Metals endowment (`Am`)       | `4,891,250,000` | any positive amount       |
| Non-metals endowment (`An`)   | `4,891,250,000` | any positive amount       |

Each **abundance** setting affects only deposits of its resource. It adjusts both
the final quantity and final yield of each deposit, using a separate roll for each
value. It does not change the system endowment, planet shares, number of deposits,
or deposit weights.

Each **endowment** setting is the resource's ten-planet baseline amount, described
under [System endowments](#system-endowments). The default is the same for all
three resources; the factored form carries the intended meaning:

```text
Af = Am = An = 6.5 × 17.5 × 43,000,000 = 4,891,250,000
```

## Exact processing order

Within a system, process planets in ascending orbit order. Whenever resources are
processed for a planet, use the order for that planet's type:

| Planet type   | First      | Second     | Third      |
| ------------- | ---------- | ---------- | ---------- |
| Rocky         | Fuel       | Metals     | Non-metals |
| Asteroid belt | Metals     | Non-metals | Fuel       |
| Gas giant     | Non-metals | Fuel       | Metals     |

Within a resource, process deposits in their creation order. Complete each phase
for the entire system before starting the next:

1. Determine system endowments.
2. Roll planet shares and distribute each endowment among the planets.
3. Roll each planet's deposit count.
4. Assign its deposit slots to resources and create the deposits.
5. Roll deposit weights and calculate base quantities.
6. Roll and apply final quantity adjustments.
7. Roll and apply yield adjustments and habitability penalties.

## System endowments

The ten-planet baseline endowments are the GM-provided settings `Af`, `Am`, and
`An`:

| Resource   | Ten-planet baseline |
| ---------- | ------------------: |
| Fuel       |                `Af` |
| Metals     |                `Am` |
| Non-metals |                `An` |

The names mean **Amount of Fuel**, **Amount of Metals**, and **Amount of
Non-metals**. For a system containing `P` planets, each resource's system
endowment is:

```text
system endowment = ten-planet baseline × P ÷ 10
```

A ten-planet system therefore receives the complete baseline; a five-planet
system receives half. The abundance settings do not change these endowments.

`Af`, `Am`, and `An` are supplied per game, exactly like the abundance knobs, the
number of systems, stellar density, and minimum spacing. A GM may raise or lower
any of them to make a game more or less resource-rich. Left unset, each takes the
default endowment from [Settings](#settings).

Keep endowments and every quantity derived from them as fractional values. Do not
round during system, planet, or deposit allocation. Quantity is rounded only after
the final deposit adjustment.

## Distributing resources among planets

Every planet rolls **shares** of all three resources. The planet's affinity for a
resource determines the roll:

| Affinity | Shares roll |
| -------- | ----------: |
| High     |   `1d3 + 4` |
| Normal   |   `1d3 + 1` |
| Low      |   `1d3 - 1` |

Clamp every result to `0–7` shares. Process planets and resources in the order
defined under [Exact processing order](#exact-processing-order).

Planet type determines affinity:

| Planet type   | Fuel   | Metals | Non-metals |
| ------------- | ------ | ------ | ---------- |
| Rocky         | High   | Normal | Low        |
| Asteroid belt | Low    | High   | Normal     |
| Gas giant     | Normal | Low    | High       |

After rolling every planet's shares, total the shares separately for fuel, metals,
and non-metals.

- If a resource has **zero total shares**, none of that resource occurs in the
  system. Its endowment is not reassigned, and no deposits of that resource are
  created.
- Otherwise, each planet receives this fractional amount:

```text
planet resource amount =
    system endowment × planet shares ÷ total system shares
```

A planet that rolled zero shares of a resource receives none of it.

## Deposit counts

After distributing all three resources, roll the total number of deposits on each
planet in ascending orbit order:

| Planet type   | Deposit count |
| ------------- | ------------: |
| Rocky         |        `4d10` |
| Gas giant     |   `20 + 2d10` |
| Asteroid belt |     `30 + d10` |

This count covers all resources on the planet. It is not a separate count for
each resource.

## Assigning deposits to resources

Process each planet's resources in its type-specific order:

1. A resource is **present** when its planet resource amount is greater than zero.
2. Reserve one deposit slot for every present resource.
3. Distribute the remaining slots among the present resources in proportion to
   their planet resource amounts.

For the proportional distribution, calculate each resource's exact share of the
remaining slots. Give it the whole-number portion first, then assign leftover
slots in descending order of fractional remainder. Equal remainders favor the
planet's resource order:

| Planet type   | Equal-remainder priority        |
| ------------- | ------------------------------- |
| Rocky         | Fuel, Metals, Non-metals        |
| Asteroid belt | Metals, Non-metals, Fuel        |
| Gas giant     | Non-metals, Fuel, Metals        |

Create the resulting deposits in that same resource order. Every deposit contains
exactly one resource, and every resource present on the planet receives at least
one deposit.

## Deposit quantities

### Base quantity

For each resource present on a planet:

1. Roll `2d6` as the weight of each of its deposits, in creation order.
2. Add the weights of all deposits of that resource on the planet.
3. Calculate each deposit's fractional base quantity:

```text
deposit base quantity =
    planet resource amount × deposit weight ÷ total deposit weights
```

Do not round the base quantity.

### Final quantity

Each deposit makes an independent quantity-adjustment roll using the setting for
its resource:

| Setting   | Quantity adjustment |
| --------- | ------------------: |
| `rich`    |           `+3d20%` |
| `average` |    `(2d12 - 13)%` |
| `poor`    |           `-3d20%` |

Percentage adjustments are relative: `+10%` multiplies a value by `1.10`, rather
than adding ten units. Apply the adjustment to the fractional base quantity, then
round down to a whole number:

```text
adjusted quantity = deposit base quantity × (1 + quantity adjustment)

final quantity = max(1, floor(adjusted quantity))
```

The minimum of `1` guarantees that every created deposit has a positive quantity.
Adjustments are not renormalized, so the sum of the final deposit quantities may
differ from the system endowment.

## Deposit yields

### Base yield

The deposit's resource determines its base yield:

| Resource   | Base yield |
| ---------- | ---------: |
| Fuel       |         5% |
| Metals     |        12% |
| Non-metals |         9% |

### Yield adjustment

Each deposit makes a new, independent adjustment roll using the same resource
setting that governs its quantity:

| Setting   | Yield adjustment |
| --------- | ---------------: |
| `rich`    |        `+3d20%` |
| `average` | `(2d12 - 13)%` |
| `poor`    |        `-3d20%` |

The quantity and yield rolls are independent: a deposit that rolls a high quantity
adjustment does not necessarily roll a high yield adjustment.

### Habitability penalty

A planet with habitability `20` or less has no yield penalty. Above `20`, the
penalty is `5%` for each additional point:

```text
habitability penalty = max(0, habitability - 20) × 5%
```

Combine the rolled adjustment and the penalty before applying them to base yield:

```text
net yield adjustment = yield adjustment - habitability penalty

unrounded yield = base yield × (1 + net yield adjustment)
```

Keep the calculation fractional until this point. Round the result down to the
nearest tenth of a percentage point, with a minimum yield of `0.1%`.

For example, a metals deposit has a base yield of `12%`. If it rolls a `+10%`
yield adjustment on a planet with habitability `22`, its habitability penalty is
`10%`:

```text
12% × (1 + 10% - 10%) = 12%
```

## Worked example: a three-planet system

This example uses the default `average` abundance setting and the default
endowment for all three resources:

```text
Af = Am = An = 4,891,250,000
```

The system has the three planets assigned by Genesis System Contents:

| Orbit | Planet          | Habitability |
| ----: | --------------- | -----------: |
| 3     | Rocky           |           24 |
| 4     | Asteroid belt   |            0 |
| 8     | Gas giant       |            8 |

### Endowments and planet shares

Because the system has three planets, each resource receives `3 ÷ 10` of its
ten-planet baseline:

```text
Fuel endowment       = 4,891,250,000 × 3 ÷ 10 = 1,467,375,000
Metals endowment     = 4,891,250,000 × 3 ÷ 10 = 1,467,375,000
Non-metals endowment = 4,891,250,000 × 3 ÷ 10 = 1,467,375,000
```

The share rolls, shown in each planet's processing order, are:

| Planet          | Share rolls in processing order                                      |
| --------------- | -------------------------------------------------------------------- |
| Rocky           | Fuel: `2 + 4 = 6`; Metals: `2 + 1 = 3`; Non-metals: `2 - 1 = 1` |
| Asteroid belt   | Metals: `3 + 4 = 7`; Non-metals: `2 + 1 = 3`; Fuel: `2 - 1 = 1` |
| Gas giant       | Non-metals: `1 + 4 = 5`; Fuel: `3 + 1 = 4`; Metals: `1 - 1 = 0` |

The system totals are `11` fuel shares, `10` metals shares, and `9` non-metals
shares. Applying those shares gives:

| Planet          | Fuel                         | Metals                   | Non-metals                    |
| --------------- | ---------------------------: | -----------------------: | -----------------------------: |
| Rocky           | `1,467,375,000 × 6/11` = `800,386,363.63…` | `1,467,375,000 × 3/10` = `440,212,500`   | `1,467,375,000 × 1/9` = `163,041,666.66…` |
| Asteroid belt   | `1,467,375,000 × 1/11` = `133,397,727.27…` | `1,467,375,000 × 7/10` = `1,027,162,500` | `1,467,375,000 × 3/9` = `489,125,000`     |
| Gas giant       | `1,467,375,000 × 4/11` = `533,590,909.09…` | `0`                                      | `1,467,375,000 × 5/9` = `815,208,333.33…` |

The displayed decimals are abbreviated. Generation retains the exact fractional
values.

### Deposit counts and resource slots

Suppose the deposit-count rolls and resulting slot assignments are:

| Planet          | Deposit-count roll       | Deposits | Slots by resource                         |
| --------------- | ------------------------ | -------: | ----------------------------------------- |
| Rocky           | `5 + 6 + 7 + 8`          |       26 | 14 Fuel, 8 Metals, 4 Non-metals           |
| Asteroid belt   | `30 + 5`                 |       35 | 21 Metals, 10 Non-metals, 4 Fuel          |
| Gas giant       | `20 + 6 + 7`             |       33 | 20 Non-metals, 13 Fuel, 0 Metals          |

The gas giant receives no metals deposits because it rolled zero metals shares.
The other slot counts follow the proportional and largest-remainder rules above.

### One deposit from each planet

The generator creates every deposit, but this example follows only one deposit of
the highest-affinity resource on each planet: fuel on the rocky planet, metals on
the asteroid belt, and non-metals on the gas giant.

After all deposits roll their `2d6` weights, suppose the selected deposits have
these weights and resource-weight totals:

| Planet          | Selected resource | Deposit weight | Total weight for that resource |
| --------------- | ----------------- | -------------: | -----------------------------: |
| Rocky           | Fuel              |              8 |                             98 |
| Asteroid belt   | Metals            |             10 |                            147 |
| Gas giant       | Non-metals        |              6 |                            140 |

Their base quantities and independent `average` quantity adjustments are:

| Planet          | Base quantity calculation                 | Adjustment roll       | Final quantity |
| --------------- | ----------------------------------------- | --------------------- | -------------: |
| Rocky           | `800,386,363.63… × 8/98 = 65,337,662.33…` | `8 + 9 - 13 = +4%`    |     `67,951,168` |
| Asteroid belt   | `1,027,162,500 × 10/147 = 69,875,000`     | `5 + 6 - 13 = -2%`    |     `68,477,500` |
| Gas giant       | `815,208,333.33… × 6/140 = 34,937,500`    | `7 + 7 - 13 = +1%`    |     `35,286,875` |

For example, the rocky planet's selected fuel deposit finishes at:

```text
floor(65,337,662.33… × 1.04) = 67,951,168
```

Finally, each selected deposit makes a separate `average` yield-adjustment roll:

| Planet          | Resource   | Base yield | Yield roll            | Habitability penalty | Final yield |
| --------------- | ---------- | ---------: | --------------------- | -------------------: | ----------: |
| Rocky           | Fuel       |         5% | `10 + 8 - 13 = +5%`   |                  20% |        `4.2%` |
| Asteroid belt   | Metals     |        12% | `8 + 8 - 13 = +3%`    |                   0% |       `12.3%` |
| Gas giant       | Non-metals |         9% | `6 + 6 - 13 = -1%`    |                   0% |        `8.9%` |

The rocky planet's habitability `24` gives a `20%` penalty, so its fuel yield is:

```text
5% × (1 + 5% - 20%) = 4.25%
round down to 0.1% = 4.2%
```

## Home-system template

The fixed home-system template from
[Genesis System Contents]({{< relref "/reference/generators/genesis/system-contents.md" >}}#home-system-template)
passes through all the rules above once. The completed template, including its
deposits, is then copied unchanged whenever a player's home system is created.
Deposit counts, quantities, and yields are not rerolled separately for each
player.

## Output guarantees

Genesis Deposits guarantees that:

- every deposit contains exactly one resource;
- every resource present on a planet has at least one deposit;
- every deposit has a positive whole-number initial quantity;
- every deposit has an initial yield of at least `0.1%`, in increments of `0.1%`;
- current quantity and yield initially equal their corresponding initial values;
- quantities are completed before yields are rolled; and
- every player receives the same completed home-system template.

## Determinism

Planet shares, deposit counts, deposit weights, quantities, and yields use random
dice rolls. As with all EC generation, the same game seeds reproduce the same
results. See [Determinism]({{< relref "/reference/determinism.md" >}}).

## See also

- [Cluster]({{< relref "/reference/cluster.md" >}}) — planets, resources, deposits, quantities, and yields
- [Genesis]({{< relref "/reference/generators/genesis" >}}) — the family this generator belongs to
- [System contents]({{< relref "/reference/generators/genesis/system-contents.md" >}}) — the systems and template processed here
