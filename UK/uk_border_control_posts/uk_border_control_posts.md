# UK Border Control Posts

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 169](https://img.shields.io/badge/records-169-lightgrey) ![updated 2026-11-06](https://img.shields.io/badge/updated-2026--11--06-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

169 UK border control posts and plant control points from official Animal and Plant Health Agency and Department for Environment, Food and Rural Affairs (Defra) lists on GOV.UK: 131 border control posts at airports, ports and inland sites, and 38 plant control points. Each record has the official name, postcode, nation, post type, location type, commodity group, designation codes and contact details where published. Animal and high-risk food lists are as at 1 May 2026. Plant lists are as at 6 November 2026. The lists are updated as designations change. Personal mailboxes and UK mobile numbers are not included. Official business names and landlines were kept. No field is required, so you can filter by post type, location, nation, commodity group, official BCP code, name or postcode.

Coverage is the United Kingdom (England, Scotland, Wales and Northern Ireland). There are no map points, because the official lists have none and the linked maps are third party. There is no official key that is unique across all five lists. Plant lists have no official BCP or control-point codes, some inspection centres share a BCP code, and some names appear on both an animal list and a plant list. The API returns a generated `__id` that is unique on every row and is the stable record key. Callers can also use `bcp_code` with the service context, or `name`, `postcode` and `commodity_group`, to identify a post.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_border_control_posts` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per post. All matches are returned. |

## Query parameters

Required: none. Optional: 20 fields. Disallowed: 2 fields.

A call with no filters returns all 169 records. Partial filters accept `*`, such as `name=*Heathrow*`, `commodity_group=*plants*` or `postcode=TW6*`. `post_type`, `location_type` and `nation` are exact matches. Text filters ignore case. There are no range filters. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Official facility heading, such as Dover Harbour (Deep Sea Terminal), Heathrow Airport - Heathrow Perishable Centre (HPC) or Barfoots of Botley (Chichester). Never blank. A few names appear on both an animal list and a plant list as separate rows. Partial wildcards allowed, such as `*Heathrow*` (27), `*Felixstowe*` (6) or `*Sevington*` (2). |
| `bcp_code` | Optional | Yes | No | Official BCP code as text, such as GBDOV2P or GBEMA4, with no added leading zeros. Present on 52 rows from the animal and high-risk food lists (44 distinct codes). Some inspection centres share a code, so it is not a unique key. Blank (`null`) on all 116 plant rows, because those official lists have no BCP or control-point codes, and on 1 animal-list row (Felixstowe - 72 Shed). Partial wildcards allowed, such as `bcp_code=GBDOV2P` (1) or `GBSEV*` (1). |
| `post_type` | Optional | No | No | Kind of post. Exact match. Values: `bcp` (131 border control posts) or `control_point` (38 plant control points). Never blank. |
| `location_type` | Optional | No | No | Kind of site. Exact match. Values: `port` (72), `airport` (57) or `inland` (40, including Sevington IBF and the plant control points). Never blank. |
| `commodity_group` | Optional | Yes | No | Goods the post is designated to check, as one or more of `live_animals`, `animal_products`, `pnao` (high-risk food and feed of non-animal origin) and `plants`, separated by a semicolon and a space. Derived from the official category codes. Partial wildcards allowed, such as `*plants*` (116), `*live_animals*` (9), `*animal_products*` (36) or `*pnao*` (32). |
| `categories` | Optional | Yes | No | Official designation codes as published, separated by a semicolon and a space, such as `P; PP; PP(WP); OO` for plants, or `POA-HC-T(CH)` and `PNAO-HC (food)` on the animal lists. Partial wildcards allowed, such as `*T(CH)*` (14) or `*PP(WP)*` (44). |
| `additional_specifications` | Optional | Yes | No | Official notes on the scope of the designation, such as plastic kitchenware or packed products only. Blank (`null`) on 123 rows when the source had none. Partial wildcards allowed. |
| `address_line1` | Optional | Yes | No | First address line, usually the operator, building or street. Never blank. Partial wildcards allowed. |
| `address_line2` | Optional | Yes | No | Second address line. Never blank. Partial wildcards allowed. |
| `address_line3` | Optional | Yes | No | Remaining official address lines, comma separated. Blank (`null`) on 10 rows. Partial wildcards allowed. |
| `town` | Optional | Yes | No | Town or locality taken from the official address when it was clearly present. Blank (`null`) on 1 row (the Glasgow Airport animal listing, whose official address has no town). Partial wildcards allowed, such as `town=Belfast` (1) or `*Heathrow*` (2). |
| `postcode` | Optional | Yes | No | Postcode as text, usually with one space, such as CT17 9FJ. Blank (`null`) on 1 row (Grangemouth Container Terminal, which has no official postcode). Three official values are defective: AB42 (Peterhead, outward code only), SO15 OLJ (Southampton City Depot; the inspection centre has SO15 0LJ) and SL3 BAQ (Langley ILC). Partial wildcards allowed, such as `postcode=TW6*` (5). |
| `nation` | Optional | No | No | UK nation, from the official postcode area or, where the postcode was missing or defective, from the official town. Exact match. Values: England (143), Scotland (22), Wales (3) and Northern Ireland (1). Never blank. |
| `phone` | Optional | Yes | No | Official landline in UK format, such as 01304 872 216. Present on 157 rows. Blank (`null`) on 12 rows whose official numbers were UK mobiles only, or where the source listed no landline at the main contact. UK mobile numbers were omitted. Where a cell mixed a landline and a mobile, only the landline was kept. Fax numbers were not used. Some rows still have a landline on `inspection_centre_phone`. Partial wildcards allowed, such as `028*` (1) or `020 8*` (12). |
| `email` | Optional | Yes | No | Official organization or role mailbox. Present on 148 rows. Personal mailboxes were omitted. Several official emails may appear, separated by a semicolon and a space. Partial wildcards allowed, such as `*gov.uk*` (46). |
| `website` | Optional | Yes | No | Official website linked from the list, when present. Blank (`null`) on 46 rows. Map links from the lists were not used. Partial wildcards allowed. |
| `opening_hours` | Optional | Yes | No | Official opening hours as published. Blank (`null`) on 35 rows where the source had no hours. Partial wildcards allowed, such as `*24 hours*` (19). |
| `inspection_centre` | Optional | Yes | No | Official inspection centre address, when the list gave one. Present on 78 rows. Where one heading had two centres (Stansted Airport, live animals and products of animal origin), both addresses are labelled in this field. Partial wildcards allowed. |
| `inspection_centre_phone` | Optional | Yes | No | Landline for the inspection centre, when published. Present on 52 rows. UK mobile numbers were omitted. Partial wildcards allowed. |
| `inspection_centre_email` | Optional | Yes | No | Organization mailbox for the inspection centre, when published. Present on 51 rows. Personal mailboxes were omitted. Partial wildcards allowed. |
| `source` | Disallowed | No | No | Attribution for the official Animal and Plant Health Agency and Defra list that the row comes from, on every row, under the Open Government Licence v3.0, including the list title and GOV.UK URL. Not filterable. |
| `source_as_of` | Disallowed | No | No | As-at date of that official list, as text YYYY-MM-DD. Animal and high-risk food rows are `2026-05-01` (53). Plant border control post and control point rows are `2026-11-06` (116). Not filterable. |

## Response fields

All 22 data fields are strings. `__id` is an integer. Blank cells are returned as `null`. Every row has a unique `__id`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `name` | string | Official facility heading. |
| `bcp_code` | string | Official BCP code, or `null` when the source list has no code. |
| `post_type` | string | Kind of post: `bcp` or `control_point`. |
| `location_type` | string | Kind of site: `port`, `airport` or `inland`. |
| `commodity_group` | string | Designated commodity groups, separated by a semicolon and a space. |
| `categories` | string | Official designation codes, as published. |
| `additional_specifications` | string | Official notes on the designation, or `null` when the source had none. |
| `address_line1` | string | First address line. |
| `address_line2` | string | Second address line. |
| `address_line3` | string | Remaining official address lines, or `null` when the source had none. |
| `town` | string | Town or locality, or `null` when the official address has no town. |
| `postcode` | string | Postcode as text, or `null` when the official list has no postcode. |
| `nation` | string | UK nation: England, Scotland, Wales or Northern Ireland. |
| `phone` | string | Official landline, or `null` when none was kept. |
| `email` | string | Official organization mailbox, or `null` when none was kept. |
| `website` | string | Official website, or `null` when the source had none. |
| `opening_hours` | string | Official opening hours, or `null` when the source had none. |
| `inspection_centre` | string | Official inspection centre address, or `null` when the source had none. |
| `inspection_centre_phone` | string | Inspection centre landline, or `null` when none was kept. |
| `inspection_centre_email` | string | Inspection centre organization mailbox, or `null` when none was kept. |
| `source` | string | Attribution for the official list that the row comes from, including the list title and GOV.UK URL. |
| `source_as_of` | string | As-at date of that official list, YYYY-MM-DD. |

## Examples

Dover Harbour (Deep Sea Terminal), looked up by official BCP code GBDOV2P. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'bcp_code=GBDOV2P'
```

```json
[
  {
    "__id": 21,
    "name": "Dover Harbour (Deep Sea Terminal)",
    "bcp_code": "GBDOV2P",
    "post_type": "bcp",
    "location_type": "port",
    "commodity_group": "pnao",
    "categories": "PNAO-HC (food)-NT (1); PNAO-NHC (other) (1A)",
    "additional_specifications": "(1) Plastic kitchenware",
    "address_line1": "Port of Dover, Dover Cargo Terminal (deep sea terminal)",
    "address_line2": "Dover Cargo Terminal (Western Docks)",
    "address_line3": "Union Street, Dover, Kent",
    "town": "Dover",
    "postcode": "CT17 9FJ",
    "nation": "England",
    "phone": "01304 872 216",
    "email": "porthealth@dover.gov.uk",
    "website": "https://www.dover.gov.uk/Port-Health-and-Public-Protection/Port-Health/Port-Health.aspx",
    "opening_hours": "Monday to Friday 8am to 5pm; Out of hours service available",
    "inspection_centre": "Port of Dover, Dover Cargo Terminal (deep sea terminal), Dover Cargo Terminal (Western Docks), Union Street, Dover, Kent, CT17 9FJ",
    "inspection_centre_phone": null,
    "inspection_centre_email": null,
    "source": "Contains public sector information from the Animal and Plant Health Agency and the Department for Environment, Food and Rural Affairs, published on GOV.UK, licensed under the Open Government Licence v3.0. Live animals, animal products and food and feed of non-animal origin border control posts at ports in the UK (https://www.gov.uk/government/publications/animals-animal-products-and-hrfnao-imports-authorised-border-control-posts-in-the-uk/live-animals-animal-products-and-food-and-feed-of-non-animal-origin-border-control-posts-at-ports-in-the-uk).",
    "source_as_of": "2026-05-01"
  }
]
```

All 169 posts. A call with no filters returns every record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts'
```

Border control posts. This call returns 131 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'post_type=bcp'
```

Airport sites. This call returns 57 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'location_type=airport'
```

Posts in Scotland. This call returns 22 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'nation=Scotland'
```

Posts designated for plants. This call returns 116 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'commodity_group=*plants*'
```

Posts whose official name includes Heathrow. This call returns 27 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'name=*Heathrow*'
```

Posts whose postcode starts with TW6. This call returns 5 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_border_control_posts' --data-urlencode 'postcode=TW6*'
```

## Data source

Source: Animal and Plant Health Agency and the Department for Environment, Food and Rural Affairs, official lists on GOV.UK. The five HTML publications, rechecked on 15 November 2026, are:

1. [Live animals, animal products and food and feed of non-animal origin border control posts at airports in the UK](https://www.gov.uk/government/publications/animals-animal-products-and-hrfnao-imports-authorised-border-control-posts-in-the-uk/live-animals-animal-products-and-food-and-feed-of-non-animal-origin-border-control-posts-at-airports-in-the-uk). Last updated 1 May 2026 at 12:29:37 pm BST (7:29 pm on 1 May 2026, Asia/Singapore time).
2. [Live animals, animal products and food and feed of non-animal origin border control posts at ports in the UK](https://www.gov.uk/government/publications/animals-animal-products-and-hrfnao-imports-authorised-border-control-posts-in-the-uk/live-animals-animal-products-and-food-and-feed-of-non-animal-origin-border-control-posts-at-ports-in-the-uk), including Sevington IBF, same as-at date. The [parent animal and high-risk food publication](https://www.gov.uk/government/publications/animals-animal-products-and-hrfnao-imports-authorised-border-control-posts-in-the-uk) has the same date.
3. [Plant imports: border control posts at UK airports](https://www.gov.uk/government/publications/plant-imports-authorised-border-control-posts-in-the-uk/plant-imports-border-control-posts-at-uk-airports). Last updated 6 November 2026 at 12:57:32 pm UTC (8:57 pm on 6 November 2026, Asia/Singapore time).
4. [Plant imports: border control posts at UK ports](https://www.gov.uk/government/publications/plant-imports-authorised-border-control-posts-in-the-uk/plant-imports-border-control-posts-at-uk-ports), same as-at date.
5. [Plant imports: control points in the UK](https://www.gov.uk/government/publications/plant-imports-authorised-border-control-posts-in-the-uk/plant-imports-control-points-in-the-uk), same as-at date. The [parent plant publication](https://www.gov.uk/government/publications/plant-imports-authorised-border-control-posts-in-the-uk) was last updated on 6 November 2026.

The five lists were combined, not collapsed, so a site that appears on both an animal list and a plant list has two rows, with the designations and as-at date from each list. The lists are updated as designations change.

Contains public sector information from the Animal and Plant Health Agency and the Department for Environment, Food and Rural Affairs, published on GOV.UK, licensed under the Open Government Licence v3.0.

The data is used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). The information is Crown copyright. You may copy, share, adapt and use it for any purpose, including commercial use, if you use that attribution and link to the licence. If those conditions are not met, the rights granted under the licence end automatically. The licence does not cover personal data, or logos and other listed exemptions. No APHA, Defra or departmental logos or emblems are used. The data comes with no warranty. Use of this data does not mean the Animal and Plant Health Agency or the Department for Environment, Food and Rural Affairs endorses this API or its maker.

Personal mailboxes and UK mobile numbers were omitted. Official business names and landlines were kept. The official lists have no map points. The parent pages link to third-party maps, which were not used.

Plant lists have no official BCP or control-point codes. Official wording and defects were left as published, including the heading The Port of Kircaldy (for Kirkcaldy), whose contact address is the Forth Ports office in Dundee; the incomplete postcode AB42 at Peterhead; and the typo postcodes SO15 OLJ at Southampton City Depot and SL3 BAQ at Langley ILC. The Grangemouth Container Terminal animal entry has no official postcode, and the Glasgow Airport animal address has no town.

This is a snapshot of the official lists as at 1 May 2026 (animal and high-risk food) and 6 November 2026 (plants). For the latest lists, visit the [animal and high-risk food publication](https://www.gov.uk/government/publications/animals-animal-products-and-hrfnao-imports-authorised-border-control-posts-in-the-uk) and the [plant publication](https://www.gov.uk/government/publications/plant-imports-authorised-border-control-posts-in-the-uk).

[Back to the API list](../../README.md)
