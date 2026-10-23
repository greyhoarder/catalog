# Greyhoarder catalog

Unofficial, community-maintained kit data for
[Greyhoarder](https://github.com/greyhoarder/greyhoarder). Not affiliated
with or endorsed by any of the manufacturers or publishers listed here.

The catalog describes what a box of miniatures can become: which minis it
yields, what they can be built as and which parts come with them. It is
open to any manufacturer and any game system, and to kits that belong to
no game system at all. Greyhoarder instances import it so that users can
add a box to their hoard without typing its contents.

It holds facts in our own words only: no images, no shop texts, no prices,
no rules.

> **Status:** draft. The format below may still change until it is fixed
> in Greyhoarder's ADR-0020 and validated in CI.

## Layout

```text
kits/<manufacturer>/<folder>/<kit>.yaml   ← one file per kit
factions/<system>.yaml                    ← the armies of one game system
```

- A kit's **id** is its path below `kits/` without the extension, e.g.
  `games-workshop/orks/boyz-2026`. Instances store these ids, so a released
  kit file is never renamed, moved or deleted.
- The **manufacturer** is the first folder, e.g. `games-workshop`.
- The folders below the manufacturer only keep things tidy (a range, a
  faction, a game); they carry no meaning. Boxes with several armies go
  into `sets/`.
- Ids and file names are lower case with hyphens.

## A kit file

```yaml
# kits/games-workshop/orks/boyz-2026.yaml
# id: games-workshop/orks/boyz-2026
name: Boyz
factions: [warhammer-40000/orks]
releases:
  - year: 2026
    sku: "50-80"
    barcode: "5011921267064"
minis: 11
draft: true
supersedes: games-workshop/orks/boyz-2021
variants:
  - name: Boyz
    slots:
      - count: 9
        buildsAs: [Boy]
        parts: [Slugga, Choppa, Shoota, Stikkbomb, Big Shoota, Rokkit Launcha, Burna]
      - count: 1
        buildsAs: [Nob]
        parts: [Big Choppa, Kustom Choppa, Power Klaw, Kombi-skorcha, Boss Pole]
      - count: 1
        buildsAs: [Ammo Runt]
```

| Field | Required | Meaning |
|-------|----------|---------|
| `name` | yes | Name of the kit as printed on the box |
| `factions` | no | The armies the manufacturer sells the kit for, see [Factions](#factions). The first entry is the army it is mainly sold for. Left out only for kits without a game system (generic minis, bits) |
| `releases` | no | One entry per packaging of the same sprues, see [Editions](#editions) |
| `releases[].year` | no | The year this packaging first went on sale on its own |
| `releases[].sku` | no | The manufacturer's product code, e.g. `"50-80"` (quoted) |
| `releases[].barcode` | no | EAN/GTIN from the box, digits only (quoted) |
| `minis` | yes | Total minis the box yields; for either-or kits the count of the first variant |
| `draft` | no | `true` while the entry is not checked against a physical box |
| `supersedes` | no | Id of the previous edition with other sprues |
| `contains` | no | Bundles only: kits whose sprues are in the box unchanged, see [Bundles](#bundles) |
| `contains[].kit` | yes | Id of the contained kit |
| `contains[].count` | yes | How many of that kit |
| `variants` | no | The either-or outcomes of the minis not covered by `contains`; most kits have exactly one |
| `variants[].name` | yes | Name of the outcome, e.g. `Lootas` |
| `variants[].slots` | yes | Groups of minis in this outcome |
| `slots[].count` | yes | How many minis the slot yields |
| `slots[].buildsAs` | yes | What these minis can be built as, by the model's short name within the kit (`Nob`, not `Beast Snagga Nob`); the full name is put together from kit and model. The first entry is the most common build |
| `slots[].parts` | no | Notable parts that come with these minis (weapons, heads, backpacks) |
| `slots[].factions` | no | Only when it differs from the kit's, e.g. in a starter set with two armies |

- **Either-or kits** have one variant per outcome; minis that are the same
  in every outcome appear in each variant.
- **Optional leaders** are an alternative in `buildsAs`; they get a slot of
  their own only when the box has a dedicated body for them.

Everything is a suggestion. Greyhoarder never checks a user's minis
against the catalog: if someone builds two Nobz from one box, that is
their hoard.

## Editions

A kit file stands for one set of sprues.

- **Same sprues in new packaging** (new box, product code or barcode):
  another entry under `releases` of the same file.
- **New sculpts:** a new file, with the year in its name
  (`boyz-2026.yaml`), that names the previous edition in `supersedes`.

If it is unclear whether two editions share their sprues, they are treated
as different kits.

## Bundles

Army sets, combat patrols and starter sets are kits too.

- Sprues identical to an existing kit go into `contains`. The same name is
  not enough: starter-set Boyz with their own sprues are not the Boyz kit.
- Everything else in the box is described in its own `variants`.
- `minis` counts the whole box: the minis of the contained kits times
  their count, plus those in its own slots.

## Factions

Each game system has one file below `factions/`; its file name is the
**system id**.

```yaml
# factions/warhammer-40000.yaml
name: Warhammer 40,000
groups:
  imperium: Imperium
  xenos: Xenos
factions:
  space-marines:
    name: Space Marines
    group: imperium
  blood-angels:
    name: Blood Angels
    partOf: space-marines
  orks:
    name: Orks
    group: xenos
```

| Field | Required | Meaning |
|-------|----------|---------|
| `name` | yes | Name of the game system |
| `groups` | no | Blocks that armies are sorted into for display, as `<id>: <name>` |
| `factions` | yes | The armies of the system, as `<faction id>: {...}` |
| `factions.*.name` | yes | Name of the army |
| `factions.*.group` | no | Id of its group; left out with `partOf`, the group then comes from that army |
| `factions.*.partOf` | no | Id of an army whose kits all apply to this one as well |

- An entry in a kit's `factions` is either `<system>/<faction>`, e.g.
  `warhammer-40000/orks`, or a whole system, e.g. `warhammer-40000` for
  terrain that every army uses.
- Every faction is an army of its own. `group` only sorts armies; it shares
  no kits. Death Guard are in the group Chaos without being part of Chaos
  Space Marines.
- `partOf` is for armies the manufacturer sells as part of another one,
  such as chapters of the Space Marines. A generic Space Marines kit lists
  only `warhammer-40000/space-marines`; the chapters get it through
  `partOf`. A chapter is listed in a kit only for boxes sold for it.
- `factions` says what the manufacturer sells a kit for, not what the
  rules of a game allow.

## Checks

CI will check, once it exists:

- every file matches the format above;
- every entry in `factions`, `partOf` and `group` exists in `factions/`;
- no kit lists an army twice, also not through `partOf`
  (`[space-marines, blood-angels]` is an error);
- every id in `supersedes` and `contains` exists;
- no released kit file was renamed, moved or deleted.

## Releases

Each release bundles all kits and factions into one JSON file with a
format version. Greyhoarder instances check for new releases and import
them; a release only adds new versions of changed kits and never changes a
user's hoard.

## Kits created in Greyhoarder

Users can create their own kits in Greyhoarder, for anything the catalog
does not have yet. These kits stay in their instance and belong to the
user; nothing is sent to this repository. To share a kit, export it from
Greyhoarder in the format above and open a pull request.

## License

The data in this repository is dedicated to the public domain under
[CC0 1.0](LICENSE); contributions are accepted under the same terms.
Product, faction and game names are trademarks of their owners and are
used here only to identify the products.
