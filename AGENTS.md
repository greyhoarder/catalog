# AGENTS.md — greyhoarder catalog

This repo is the Greyhoarder catalog: an unofficial, community-maintained
list of miniature kits from any manufacturer and any game system. You
research kits and draft files for a human to review and commit.

## Format

- [README.md](README.md) defines the format of kit and faction files, the id
  rules, editions, bundles and factions. Follow it exactly.
- Range files are local working lists, not part of the catalog; their format
  is described below.
- Before every task, read the README and check the existing files in
  `kits/`, `factions/` and `ranges/`.
- If a task needs something the README or this file does not cover, say so
  instead of inventing a field.

## Range files

A range file lists what a manufacturer sells or sold for one army, so we can
see which kits are still missing. It is only a tool for drafting:
`ranges/` is ignored by git, its files are never committed, and Greyhoarder
does not read them.

```yaml
# ranges/games-workshop/warhammer-40000/orks.yaml
manufacturer: games-workshop
faction: warhammer-40000/orks
checked: 2026-10-22
kits:
  - name: Boyz
    kind: kit
    first: 2026
    catalog: [games-workshop/warhammer-40000/orks/boyz-2026]
    state: drafted
```

| Field | Required | Meaning |
|-------|----------|---------|
| `manufacturer` | yes | Manufacturer folder below `kits/` |
| `faction` | yes | The army, `<system>/<faction>` |
| `checked` | yes | Date of the last research, `YYYY-MM-DD` |
| `kits[].name` | yes | Name of the kit |
| `kits[].kind` | yes | `kit`, `bundle` or `starter-set` |
| `kits[].first` | no | First year on sale |
| `kits[].last` | no | Last year on sale; left out while still on sale |
| `kits[].catalog` | yes | Ids of the matching kit files; empty until they are planned |
| `kits[].state` | yes | `open`, `drafted` or `verified` |

- Every id in `catalog` must exist in `kits/`. Nothing checks range files
  automatically, so check the ids yourself before finishing.

## Workflow

To add a kit, a bundle, a starter set or a whole army, use the
`add-to-catalog` skill. If the request could mean several kits or armies,
ask which one before researching.

## Factions

- `factions` in a kit lists the armies the manufacturer sells the box for,
  the main one first. It never says what the rules of a game allow.
- A generic kit of an army lists only that army, never its chapters or
  sub-armies; they get it through `partOf`. Never list an army that
  already gets the kit through `partOf`.
- Set `partOf` only where the manufacturer sells an army as part of
  another one. `group` only sorts armies and shares no kits. If unsure, no
  `partOf`, and list it as an open point.

## Research

- For the history of a kit (years, product codes, new sprues or only a
  new box) use [Scalemates](https://www.scalemates.com/) as the main
  source: each kit page has facts and a timeline, where "New tool" marks
  new sprues and "New box" a new packaging of existing ones.
- For contents and names prefer the manufacturer's own announcements and
  product pages; use two or three retailer pages to confirm. Read the pages themselves, not just
  search snippets; a handful of pages per task, no automated crawling.
- Every number, product code, barcode and year must appear on a page you
  read, be told by the user, or already be in an existing file (the year
  in a file name counts). If sources disagree, use the manufacturer's
  value and list the disagreement as an open point. When the user marks a
  fact as unsure ("I think", "allegedly") or pastes it from elsewhere,
  check it on pages first and report what was confirmed.
- Not finding a value is no proof that it is wrong. Never call existing
  data wrong or remove it only because your search found nothing; list it
  as an open point.
- A retailer's "publish year" or "edition" year usually marks a new
  packaging, not new sprues. Use it for a file name only if a page says
  the kit was new that year.
- Never guess. A fact no source states is left out and listed as an open
  point.
- The year is the exception: every kit needs one in its file name and in
  each release. A kit or release whose year no page states is not written;
  list it as an open point instead.
- Facts only, in your own words. Never copy sentences from shops,
  manufacturer pages or packaging. No images, no prices, no rules (points,
  datasheets, unit sizes, wargear restrictions).
- Researched kits always carry `draft: true`.

## Finishing a task

- Write new or changed files directly into the repo. Change only the files
  the task needs; if something else looks outdated or breaks the README,
  say so instead of changing it.
- In the reply, per kit or range: the path, the sources (links to the pages
  actually read) and the open points (what to check on the physical box,
  or none).
- Don't commit unless asked; then use the `commit` skill. Never push or
  open pull requests.
- Don't change `README.md`, this file or the skills without asking first,
  except as described under "Corrections".

## Corrections

When the user corrects something or supplies a fact you missed, find out
why you missed it and adjust this file or the skills so it does not happen
again: better sources, search terms or rules. Write general advice only,
never data of concrete kits (names, codes, years). Name the change in the
reply.

## Commits

- One line, no body: `<area>: <verb> <what>`
- Area:
  - the folder below the system for kits (`orks`, `sets`);
  - `factions` when only faction files change;
  - `docs` for `README.md`, `AGENTS.md` and skills;
  - `repo` for repo setup (`.gitignore`, CI, templates).
- Verbs for kits and factions:
  - `add`: new kits or factions;
  - `correct`: changes to existing data;
  - `verify`: checked against the physical box (`draft` removed).
- Name the kits, not the files; several kits are listed with commas or
  summed up (`add 5 kits`).
- Imperative mood, no period at the end, at most 72 characters. Lower
  case, except names of kits and armies as printed.
- Examples:
  - `orks: add Beast Snagga Boyz`
  - `orks: correct parts of Boyz 2026`
  - `orks: verify Boyz 2026 against the box`
  - `factions: add Death Guard`
  - `docs: describe bundles`
- Separate commits for different areas and for different verbs. A new
  army in `factions/` goes into the commit of its first kits.
- Files in `ranges/` are never committed.

## Language

- Files, ids, YAML and commit messages are in English.
- Reply to the user in the language they write in.
