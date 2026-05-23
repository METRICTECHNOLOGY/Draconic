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
