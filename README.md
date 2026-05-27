# Draconic

Aliases and character actions for D&D on Discord, written in Avrae's Draconic
automation language.

## Pugilist

Character actions covering the Pugilist class (homebrew). Each `.json` in
`actions/` is an Avrae Automation v2 action that can be imported on a
character's action list.

| Level | Feature | File | Activation |
| ----- | ------- | ---- | ---------- |
| 1 | Fisticuffs (Unarmed Strike) | `actions/01-fisticuffs.json` | Action |
| 1 | Iron Chin | *passive* — AC = 12 + CON in Light/no armor without a Shield | — |
| 2 | Brace Up | `actions/02-brace-up.json` | Bonus Action |
| 2 | One-Two Punch | `actions/02-one-two-punch.json` | Bonus Action |
| 2 | Stick and Move | `actions/02-stick-and-move.json` | Bonus Action |
| 2 | Bloodied But Unbowed | `actions/02-bloodied-but-unbowed.json` | Reaction |
| 2 | Swagger Streak | `actions/02-swagger-streak.json` | No Action |
| 3 | Heavy Hitter | *passive* — referenced in the Fisticuffs action | — |
| 3 | Pugilist Subclass | *subclass-specific, not included* | — |
| 4 | Dig Deep | `actions/04-dig-deep.json` | Bonus Action |
| 5 | Extra Attack | *passive* | — |
| 5 | Haymaker | `actions/05-haymaker.json` | Action |
| 6 | Moxie-Fueled Fists | *passive* — switch Unarmed/improvised damage to Force | — |
| 7 | Down But Not Out | folded into `02-bloodied-but-unbowed.json` | Special |
| 9 | School of Hard Knocks — Damage | `actions/09-school-of-hard-knocks-damage.json` | No Action |
| 9 | School of Hard Knocks — Endanger | `actions/09-school-of-hard-knocks-endanger.json` | No Action |
| 9 | School of Hard Knocks — Provoke | `actions/09-school-of-hard-knocks-provoke.json` | No Action |
| 10 | Herculean | *passive* — carry x2, jump x2, crit objects | — |
| 10 | Shake It Off | `actions/10-shake-it-off.json` | Special (start of turn) |
| 13 | Dig Deeper | `actions/13-dig-deeper.json` | Bonus Action |
| 14 | Unbreakable | `actions/14-unbreakable-reroll.json` | Special |
| 15 | Pugnacious | `actions/15-pugnacious.json` | Special (on initiative) |
| 18 | Fighting Spirit | `actions/18-fighting-spirit.json` | Reaction |
| 19 | Epic Boon | *feat choice* | — |
| 20 | Peak Physical Condition | *passive* | — |

See [`actions/setup.md`](actions/setup.md) for the counters and cvars these
actions expect.

## Wizard — Occultist

Subclass features for the Occultist Wizard (homebrew). Unlike the Pugilist
features, **Intrusion** is delivered as an Avrae **alias** (a `!intrusion`
command) rather than an imported action — see
[`aliases/intrusion.alias`](aliases/intrusion.alias).

| Level | Feature | Alias | Notes |
| ----- | ------- | ----- | ----- |
| 3 | Forbidden Knowledge | — | *passive — learn a language, always have a chosen Warlock 1st-level spell prepared* |
| 3 | Intrusion | `aliases/intrusion.alias` | `!intrusion` |

### Installing the alias

Paste the contents of `aliases/intrusion.alias` into the Avrae dashboard
(My Aliases → New Alias, name it `intrusion`) or run
`!alias intrusion <code>` in Discord.

### Using it

| Command | Effect |
| ------- | ------ |
| `!intrusion` | Risk an Intrusion: roll the current Intrusion Die. On 2+ the die shrinks one step; on a 1 the die grows one step (capped at starting size) and you roll on the Intrusion Table. |
| `!intrusion sr` | After a **Short Rest** — the die grows one step (up to its starting size). |
| `!intrusion setup` (or `lr`) | Create/reset the tracker to the starting size for your current Wizard level. Run once after install, and again whenever you level up so the max keeps pace. |

### Intrusion notes

- The die size is tracked by a self-managed counter, **Intrusion Step**, whose
  value indexes the progression `[d2, d3, d4, d6, d8, d10, d12]` (step 1 = d2,
  step 7 = d12). The alias creates it on first use.
- The counter's max is the **starting size** for the character's Wizard level:
  d6 at L3–4, d8 at L5–10, d10 at L11–16, d12 at L17+ (read from
  `levels.get("Wizard")`).
- **Long Rest** resets the counter to its starting size automatically
  (`reset="long"`). **Short Rest:** run `!intrusion sr` to grow the die one step.
- Damage references in table entries 3, 5, and 8 use the Intrusion Die *at its
  starting size*; entries needing a save quote your spell save DC from the sheet.

### Design notes

- The **Fisticuffs die** is computed from `levels.pugilist` inline so it scales
  automatically:
  `<<('2d6' if levels.pugilist >= 17 else '1d12' if levels.pugilist >= 11 else '1d10' if levels.pugilist >= 5 else '1d8')>>`
- **Moxie Points** are managed by an Avrae custom counter. Features that *spend*
  a point use a `counter` node with `amount: 1`. Bloodied But Unbowed, Fighting
  Spirit, and Pugnacious restore consumed counters with a negative amount equal
  to `max − current`.
- **Bloodied** is checked as `currentHp <= hp / 2`.
- Effects that affect the world outside Avrae's tracking (Exhaustion, condition
  removal, Dash/Disengage, picking damage type) are emitted as text reminders
  rather than mechanical automation.
