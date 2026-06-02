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

Subclass actions for the Occultist Wizard (homebrew).

| Level | Feature | File | Activation |
| ----- | ------- | ---- | ---------- |
| 3 | Forbidden Knowledge | *passive — learn a language, always have a chosen Warlock 1st-level spell prepared* | — |
| 3 | Intrusion | `actions/wiz-03-occultist-intrusion.json` | No Action |

### Intrusion notes

- The Intrusion Die size is tracked by a single counter — **Intrusion Step** — whose value indexes the progression `[d2, d3, d4, d6, d8, d10, d12]` (step 1 = d2, step 7 = d12).
- The counter's max is the **starting size** for the character's Wizard level: step 4 (d6) at L3–4, step 5 (d8) at L5–10, step 6 (d10) at L11–16, step 7 (d12) at L17+. Raise the max manually when you level up.
- On a non-Intrusion result (2+), the action decrements the counter (die shrinks). On a 1, it increments the counter (die grows, capped at max) and rolls on the Intrusion Table.
- **Long Rest** resets the counter to max automatically. **Short Rest:** the die grows one step (run `!cc "Intrusion Step" -1`).
- Damage rolls in table entries 3, 5, and 8 use the Intrusion Die *at its starting size*, computed from the counter's max.

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

## Druid — Circle of Entropy

Subclass actions for the Circle of Entropy Druid (homebrew) — fatalistic Druids
who act as agents of ruin, hastening the inevitable end of all things.

| Level | Feature | File | Activation |
| ----- | ------- | ---- | ---------- |
| 3 | Catastrophic Power — Elemental Cataclysm | `actions/dru-03-entropy-elemental-cataclysm.json` | Action |
| 3 | Catastrophic Power — Ruinous Smite | `actions/dru-03-entropy-ruinous-smite.json` | No Action |
| 3 | Catastrophic Power — Weapon Mastery | *passive* — use one weapon kind's mastery property | — |
| 3 | Ruin Incarnate | `actions/dru-03-entropy-ruin-incarnate.json` | Bonus Action |
| 6 | Many Roads to Ruin | folded into `dru-03-entropy-ruin-incarnate.json` (L6+ text) | — |
| 10 | Shake the Earth | `actions/dru-10-entropy-shake-the-earth.json` | Action |
| 14 | Entropy's Apex — Enhanced Ruinous Smite | folded into `dru-03-entropy-ruinous-smite.json` (L14+ text) | — |
| 14 | Entropy's Apex — Improved Inexorable Onslaught | folded into `dru-03-entropy-ruin-incarnate.json` (L14+ text) | — |
| 14 | Entropy's Apex — World Breaker | *recharge note* — Shake the Earth also resets on a Short Rest | — |

See [`actions/setup.md`](actions/setup.md) for the `Wild Shape`, `Shake the
Earth`, and `Slot Level` counters these actions expect.

### Design notes

- **Slot scaling**: set the **Slot Level** counter to the slot you're spending;
  the action then consumes a real spell slot of that level via the `counter`
  node's spell-slot reference (`{"slot": ...}`) and scales the dice (Elemental
  Cataclysm `Slot Level`d6, Ruinous Smite `Slot Level + 1`d8). The counter only
  *selects* the level — the slots themselves are tracked by Avrae's spellbook.
- **Spell save DC** is computed inline as `8 + proficiencyBonus + wisdomMod`.
- **Ruin Incarnate** is a 10-minute `ieffect2` that sets base AC to
  `17 + max(1, wisdomMod)` via `ac_value`; the description notes it only applies
  if higher than your current AC, since Avrae can't read live AC to guard it.
- **Vulnerability** (Elemental Cataclysm) uses the native `vulnerabilities` effect.
- Things Avrae can't apply per-attack or per-condition are reminders that point at
  real mechanisms: Advantage vs Bloodied → `-adv`, Increased Might → `-b`, Enhanced
  Ruinous Smite crit range → `-criton 19` / `!csettings criton 19`. Size change and
  the Prone condition are plain text reminders.
