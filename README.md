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
kits/<manufacturer>/<folder>/<kit>.yaml  ← one file per kit
```

- A kit's **id** is its path below `kits/` without the extension, e.g.
  `games-workshop/orks/boyz-2026`. Instances store these ids, so a released
  kit file is never renamed, moved or deleted.
- The **manufacturer** is the first folder, e.g. `games-workshop`.
- The folders below the manufacturer only keep things tidy (a range, a
  faction, a game); they carry no meaning.

## A kit file

```yaml
# kits/games-workshop/orks/lootas-burna-boyz.yaml
# id: games-workshop/orks/lootas-burna-boyz
name: Lootas / Burna Boyz
faction: warhammer-40000/orks
variants:
  - name: Lootas
    slots:
      - count: 1
        buildsAs: [Mek]
        parts: [Kustom Mega-blasta]
      - count: 4
        buildsAs: [Loota]
        parts: [Deffgun]
  - name: Burna Boyz
    slots:
      - count: 1
        buildsAs: [Mek]
        parts: [Kustom Mega-blasta]
      - count: 4
        buildsAs: [Burna Boy]
        parts: [Burna]
```

| Field | Required | Meaning |
|-------|----------|---------|
| `name` | yes | Name of the kit as printed on the box |
| `faction` | no | `<system>/<faction>`, e.g. `warhammer-40000/orks`; left out for kits without a game system (terrain, generic minis) |
| `sku` | no | The manufacturer's product code, e.g. `"50-80"` (quoted) |
| `variants` | yes | The either-or outcomes of the kit; most kits have exactly one |
| `variants[].name` | yes | Name of the outcome, e.g. `Lootas` |
| `variants[].slots` | yes | Groups of minis in this outcome |
| `slots[].count` | yes | How many minis the slot yields |
| `slots[].buildsAs` | yes | What these minis can be built as; the first entry is the default name |
| `slots[].parts` | no | Notable parts that come with these minis (weapons, heads, backpacks) |
| `slots[].faction` | no | Only when it differs from the kit's, e.g. in a starter set with two armies |

Everything is a suggestion. Greyhoarder never checks a user's minis
against the catalog: if someone builds two Nobz from one box, that is
their hoard.

## Releases

Each release bundles all kits into one JSON file with a format version.
Greyhoarder instances check for new releases and import them; a release
only adds new versions of changed kits and never changes a user's hoard.

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
