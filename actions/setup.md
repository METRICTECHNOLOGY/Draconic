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

## Importing

For each `.json` file in this folder, run `!a import <gist url>` or paste it
through the Avrae dashboard (Customization → Actions → Import). The action's
`activation_type` controls whether Avrae files it under Action / Bonus Action /
Reaction / No Action / Special.
