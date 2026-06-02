# Pugilist Setup

Before importing the actions, set up the custom counters and cvars that the
automation expects.

## Custom Counters (`!cc create`)

Run these from a channel where Avrae is listening, with the Pugilist character
active. The `<X>` values match the Pugilist Features table at your current level.

```
!cc create "Moxie Points" -reset short -max <moxie max> -title "Moxie" -desc "Spent on Brace Up, One-Two Punch, Stick and Move, Haymaker, Swagger Streak, Unbreakable reroll."
!cc create "Bloodied But Unbowed" -reset short -max 1 -title "Bloodied But Unbowed"
!cc create "Dig Deep" -reset long -max 1 -title "Dig Deep"
!cc create "Down But Not Out" -reset long -max 1 -title "Down But Not Out"
!cc create "Shake It Off" -reset long -max 1 -title "Shake It Off"
!cc create "Dig Deeper" -reset long -max 1 -title "Dig Deeper"
!cc create "Pugnacious" -reset long -max 1 -title "Pugnacious"
!cc create "Fighting Spirit" -reset long -max 1 -title "Fighting Spirit"
!cc create "Swagger Streak" -reset short -max 1 -title "Swagger Streak"
```

At level 20, raise the Dig Deeper max to 2.

## Cvars (optional)

The action JSON computes the Fisticuffs die from `levels.pugilist` automatically,
so no cvar is required. If you'd rather hardcode it, set:

```
!cvar fdie 1d8
```

…and replace `<<('2d6' if levels.pugilist >= 17 else '1d12' if levels.pugilist >= 11 else '1d10' if levels.pugilist >= 5 else '1d8')>>` with `<fdie>` inside each action.

## Occultist Wizard Counter

For the Intrusion action, create one counter. Set `-max` to the starting Intrusion Die step for the character's Wizard level: **4** at L3–4 (d6), **5** at L5–10 (d8), **6** at L11–16 (d10), **7** at L17+ (d12).

```
!cc create "Intrusion Step" -reset long -max <starting step> -min 1 -title "Intrusion Die" -desc "Step on progression d2,d3,d4,d6,d8,d10,d12. 1=d2 … 7=d12. Max = starting size."
```

Raise `-max` manually when the starting size grows. On a Short Rest the die grows one step:

```
!cc "Intrusion Step" -1
```

## Druid — Circle of Entropy Counters

For the Circle of Entropy actions, create these counters. `<wild shape max>` is
your Wild Shape uses (2 at L2, 3 at L6, 4 at L17).

```
!cc create "Wild Shape" -reset short -max <wild shape max> -title "Wild Shape" -desc "Druid Wild Shape uses. Ruin Incarnate spends one."
!cc create "Shake the Earth" -reset long -max 1 -title "Shake the Earth" -desc "1/Long Rest. At L14 (World Breaker) recreate with -reset short to also recharge on a Short Rest."
!cc create "Slot Level" -min 1 -max 9 -title "Slot Level" -desc "Scratch value: the level of the spell slot you expend for Elemental Cataclysm / Ruinous Smite."
```

Before using **Elemental Cataclysm** or **Ruinous Smite**, set **Slot Level** to
the slot you're spending — the `=` sets an absolute value, e.g. for a 3rd-level
slot:

```
!cc "Slot Level" =3
```

Elemental Cataclysm then deals `Slot Level`d6; Ruinous Smite deals
`(Slot Level + 1)`d8. If your character already has a **Wild Shape** counter,
reuse it rather than creating a second one.

## Importing

For each `.json` file in this folder, run `!a import <gist url>` or paste it
through the Avrae dashboard (Customization → Actions → Import). The action's
`activation_type` controls whether Avrae files it under Action / Bonus Action /
Reaction / No Action / Special.
