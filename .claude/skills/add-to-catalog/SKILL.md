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
   - The year of the sprues is the year the kit was announced as new.
     Start with the kit's Scalemates page and its timeline (`/timeline`
     after the kit URL): follow "New box" entries back to the "New tool"
     entry. Scalemates refuses the web fetch tool; read single pages with
     `curl` and a browser user agent instead. For Warhammer kits, also
     check Lexicanum's "Miniatures: <army>" page (linked from
     `https://wh40k.lexicanum.com/wiki/Portal:Miniatures`): it lists new
     models by edition and year, but not new packagings. It may answer
     with a bot check; never work around it, ask the user to look
     instead. If Scalemates and Lexicanum have no year,
     look for the manufacturer's release announcement, news sites'
     release articles, or the oldest archived product page on
     `web.archive.org`. A later year on a retailer page is usually a new
     packaging of the same sprues: another release, not the file name.
   - Manufacturers and shops often add the year of a new packaging in
     brackets to the product name or URL (`<name> (<year>)`,
     `<name>-<year>`). Search for these forms to date each release.
   - Patterns are no proof of a year: kits of the same army need not share
     a year, and neighbouring barcodes or product codes only hint at it.
   - New bases, box art or a new product code do not make new sprues,
     even where Scalemates or shops call the box new. Before taking such
     a year for the file name, check whether the same models were sold
     earlier, e.g. under an older edition of the game.
   - Search product codes and barcodes in every form shops use: the short
     code (`50-xx`), the long code (`99120103xxx`) and combinations such as
     `GW-99120103xxx-50-xx`. European shops often list barcode and long
     code together.
2. **Contents:** research the number of minis, either-or outcomes,
   optional leaders, notable parts and the armies the manufacturer sells
   the box for. Describe what is in the box, not what the rules allow.
   - For either-or kits, put the names of the outcomes that are not in
     `name` into `aliases`, so the kit is found by each of them.
   - Take the model names for `buildsAs` from the manufacturer's own
     product page, announcement or assembly guide. Retailers often use a
     generic or older name for a model; if only retailers name it, list
     the name as an open point.
   - Slots suggest a typical build, they do not list every possible one:
     give parts the box has only once or twice (a special or heavy
     weapon, a leader's weapons) a slot of their own with a matching
     count, never one slot shared by the whole box. If the box has a
     basic part for every mini (a boltgun each), list it in every slot.
     In monopose kits a body comes with its weapon, so each such body
     group is a slot of its own.
   - `buildsAs` lists models the box can build, not roles from the rules:
     a leader that the rules know but the box gives no own parts for
     (head, body or weapon) is left out.
   - Retailer texts sometimes mix in parts or models of other kits. A
     part or model that only one retailer names while the others do not
     is an open point, not part of the file.
   - Announcements and product texts describe parts loosely; the assembly
     guide and the box decide. Where an unboxing review or the guide
     names fewer or other parts than the announcement, list the
     difference as an open point instead of copying the announcement.
   - For Games Workshop kits, Warhammer 40,000 datasheets are a
     cross-check for names of models and parts. In the 8th edition
     (2017–2020) the datasheets give a rule to nearly every part in the
     box of that time, but may also list options no box contains. From
     the 9th edition on, the options were cut down to what is in the box,
     and the 10th edition merged many weapons under generic names. For
     an older kit, use the datasheet of the edition it was on sale in.
     Nothing from the rules goes into the files.
   - Retailer texts list only a selection of the parts; a part they do not
     name may still be in the box. For a kit on sale during the 8th
     edition, add the parts its datasheet lists for the models of the box
     and name them as an open point, unless a source says the box lacks
     them.
   - A datasheet shows which parts a box has, not always what they are
     best called: a part may have had a generic rules name then and a more
     fitting one today. Use the manufacturer's current name for the same
     part; if unsure, list it as an open point instead of correcting the
     file.
   - Name the same part the same way in every kit file: before writing
     `parts`, look at the kit files of the same army and reuse their
     names.
3. **Bundles and starter sets:** follow the README's "Bundles" section.
   A contained kit that is not in `kits/` yet is added first, the same
   way. A bundle contains the edition on sale when the bundle came out:
   if a contained kit has a newer edition today, add the older one. If unsure whether sprues are identical, use own variants and list
   it as an open point. Starter sets with several armies go into
   `kits/<manufacturer>/sets/`.
4. Write each kit file complete, with `draft: true`.
5. If a range file for the army exists, update its entries (`catalog`,
   `state: drafted`).

## 4. Finish

Finish as `AGENTS.md` describes. When adding several kits, report each one
with its path, sources and open points.
