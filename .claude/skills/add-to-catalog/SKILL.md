---
name: add-to-catalog
description: Add a kit, a bundle, a starter set or everything for one army to the catalog - research it and write the kit files, faction entries and the army's range file. Use whenever the user asks to add something to the catalog ("füge Boyz hinzu", "füg die Orks hinzu", "add the Combat Patrol").
---

# Add to the catalog

Read `README.md` and `AGENTS.md` first.

## 1. Find out what to add

- Check `kits/`, `factions/` and `ranges/` for what already exists.
- Decide whether the request means one kit, one bundle or starter set, or a
  whole army. If it could mean several things (a kit or a bundle of the
  same name, two armies), ask before researching.
- If it already exists, say so and only propose corrections.

## 2. A whole army

1. Research the manufacturer's current and discontinued kits for the army:
   single kits, bundles (army sets, combat patrols, battleforces) and
   starter sets, including kits the manufacturer also sells for other
   armies. For an army with `partOf`, list only its own kits.
2. Write or update `ranges/<manufacturer>/<faction>.yaml` (format in
   `AGENTS.md`) and add the army to `factions/` if it is missing.
3. Show the list as a table (kit, kind, years, already in the catalog) and
   ask which kits to add. Wait for the answer.
4. Add the chosen kits as in 3, single kits before the bundles that
   contain them.

## 3. One kit, bundle or starter set

1. **Editions:** find every edition over the years and classify it as the
   README's "Editions" section says. If it is unclear whether the sprues
   are the same, treat the editions as different kits and list it as an
   open point. If there are several editions and the user named none, ask
   which to add.
2. **Contents:** research the number of minis, either-or outcomes,
   optional leaders, notable parts and the armies the manufacturer sells
   the box for. Describe what is in the box, not what the rules allow.
3. **Bundles and starter sets:** follow the README's "Bundles" section.
   A contained kit that is not in `kits/` yet is added first, the same
   way. If unsure whether sprues are identical, use own variants and list
   it as an open point. Starter sets with several armies go into
   `kits/<manufacturer>/sets/`.
4. Write each kit file complete, with `draft: true`.
5. If a range file for the army exists, update its entries (`catalog`,
   `state: drafted`).

## 4. Finish

Finish as `AGENTS.md` describes. When adding several kits, report each one
with its path, sources and open points.
