# AU University Study Hubs

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 98](https://img.shields.io/badge/records-98-lightgrey) ![updated 2026-09-28](https://img.shields.io/badge/updated-2026--09--28-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

© Commonwealth of Australia, Department of Education, University Study Hubs, CC BY 4.0. Source: [Australian Government Department of Education, University Study Hubs](https://www.education.gov.au/access-and-participation/university-study-hubs), page last modified 28 September 2026. Licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) per the [Department copyright notice](https://www.education.gov.au/copyright). The data was reformatted: one row per site, hub names grouped, program type added from the hub detail pages, two staff email addresses left out and map points added. Map points: Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement.

The 98 sites of the 70 Australian Government University Study Hubs, 55 regional and 15 suburban, from the Department of Education list. A hub can run several sites, so each record is one site, with the name of its hub, the program type (`Regional` or `Suburban`), street address, town, state, postcode, a shared inbox and website for most, and a map point on 92 sites. `state` is required on every call, and `state=*` returns all 98 records.

Not affiliated with or endorsed by the Department of Education. The list is a snapshot, so check the hub website for opening hours and rules before you visit.

Third party note: the hub email addresses and websites are details that each hub or its operator supplied, and the Department copyright notice excludes content supplied by third parties from the CC BY licence. They are included as the Department page publishes them. Ask the Department (copyright@education.gov.au) if you need to be sure before reusing them.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_university_study_hubs` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per site. All matches are returned. |

## Query parameters

Required: 1 field (`state`). Optional: 12 fields. Disallowed: 1 field.

`state` is required. A call without it returns HTTP 400. Use `state=*` to return all 98 records, or a code such as `state=QLD`. Partial filters accept `*`, such as `hub=*Lumen*`, `town=Mount*`, `address=*Street` or `email=*.gov.au`. `geoq` and `source_as_of` are exact matches, and a star there is read as a plain character, so it returns `[]`. All filters ignore case. `lat` and `lng` accept a range with two dots, lower number first, such as `lat=-38..-37`. A hyphen range such as `lng=150-154` returns HTTP 400. `postcode` is text, so use a wildcard for a range, such as `postcode=4*`.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory code, 8 values on every row (none is blank): QLD (24); NSW (22); WA (17); VIC (15); SA (12); TAS (5); NT (2); OT (1). OT is Norfolk Island (the page gives no state for it, and OT is the G-NAF and ABS code for Other Territories). There is no hub in ACT. Every call must include state, and a call without it returns HTTP 400. Partial wildcards allowed, so `state=*` returns all 98 rows, and `N*` returns NSW and NT. Case ignored. |
| `hub` | Optional | Yes | No | Name of the hub as the Department lists it, with a trailing site name in brackets removed, 70 values, never blank. Hubs with several sites repeat the same name on each row, such as CUC RAPAD Central West Queensland (8 sites) or Lumen Wheatbelt Regional University Study Hubs (4). Partial wildcards allowed, such as `*Lumen*` or `CUC*`. |
| `site` | Optional | Yes | No | Name of one site as the Department card prints it, such as Wuyagiba Study Hub or CUC Ovens Murray (Bright), 98 values, unique on every row, never blank. Single site hubs have the same text as hub. Partial wildcards allowed, such as `*Birdsville*` or `*Outreach*`. |
| `hub_type` | Optional | Yes | No | Program the hub belongs to, from the program line on its Department detail page: Regional (81 sites in 55 hubs, Regional University Study Hubs) or Suburban (17 sites in 15 hubs, Suburban University Study Hubs). The overview page lists 55 and 15. Never blank. Partial wildcards allowed, such as `Sub*`. Case ignored. |
| `address` | Optional | Yes | No | Street line as the Department prints it, such as 72 Taylors Road or Shop G40, Fairfield City Central 54 Smart Street, never blank. Many include a building, level or shop before the street number, and the town and state are in their own fields. Kept as published, including one typo (8 Doyle Steet, CUC North West Narrabri). Partial wildcards allowed, such as `*Street` or `*Library*`. |
| `town` | Optional | Yes | No | Town or suburb as printed on the card, such as Birdsville, Liverpool or Burnt Pine, 97 values, never blank. Partial wildcards allowed, such as `Mount*` or `*ton`. |
| `postcode` | Optional | Yes | No | Four digit postcode as text, never blank, 93 values, such as 2170 or 0852. It is text, so leading zeros stay (Northern Territory 08xx) and a range needs a wildcard: `4*` returns every 4xxx postcode. Partial wildcards allowed. |
| `email` | Optional | Yes | No | Shared inbox of the hub or site as the Department prints it, lower case, such as degrees@cucrapadcwq.edu.au or info@armadale.wa.gov.au, never a personal inbox. Present on 90 rows (62 distinct, because hubs share one inbox); the other 8 rows are null. Two addresses that carry a staff member name were left out (Copper Coast University Centre, CUC Hay). Partial wildcards allowed, such as `*@cucrapadcwq.edu.au` or `*.gov.au`. Do not use for bulk mailing. |
| `website` | Optional | Yes | No | Website of the hub or its operator as linked on the Department page, such as https://www.cucrapadcwq.edu.au/, present on 97 rows (59 distinct, shared across the sites of a hub); Study King Island has none, so it is null. One link uses http. Links can go out of date. Partial wildcards allowed, such as `*.edu.au*`. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (GDA2020, 6 decimals) of the matched Geoscape G-NAF address, present on 92 rows, null on 6 (no safe match). Values run from -42.78 to -15.47, all negative (south). A number. Range filter with two dots, lower number first, such as `-38..-37`. A hyphen range returns 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same point, from 114.61 to 167.95 (167.95 is Norfolk Island), null on 6 rows. A number. Range filter with two dots, such as `150..154`. |
| `geoq` | Optional | No | No | How the point was matched, exact value: address (76, a G-NAF address with that street number), near (15, the nearest numbered address on the same street at most 6 numbers away, or the same number on a street with a different type, such as Day Street against Day Avenue) or corner (1, the midpoint of two named streets at most 150 m apart, Northern Study Hub in Epping). Null on 6 rows that have no lat or lng. Filtering on null is not possible. Exact match, such as geoq=address. |
| `source_as_of` | Optional | No | No | Date the Department page was last modified, 2026-09-28 ("Last modified on Monday 28 September 2026" in the page footer), the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: © Commonwealth of Australia, Department of Education, University Study Hubs, CC BY 4.0, with the page link and the Geoscape G-NAF credit for map points. Returned in output only. |

## Response fields

`lat` and `lng` are numbers. All other values are strings, including `postcode`. Blank cells are returned as `null`: `email` on 8 records, `website` on 1, `lat`, `lng` and `geoq` on 6. There is no `__id`: `site` is unique on every record.

| Field | Type | Description |
|---|---|---|
| `hub` | string | Name of the hub. Hubs with several sites repeat it on each record. |
| `site` | string | Name of the site as the Department card prints it. Unique. |
| `hub_type` | string | `Regional` or `Suburban`. |
| `address` | string | Street line as published, which can start with a building, level or shop. |
| `town` | string | Town or suburb. |
| `state` | string | `QLD`, `NSW`, `WA`, `VIC`, `SA`, `TAS`, `NT` or `OT` (Norfolk Island). |
| `postcode` | string | Four digit postcode as text, with any leading zero. |
| `email` | string | Shared inbox of the hub or site, or `null`. |
| `website` | string | Website of the hub or its operator, or `null`. |
| `lat` | number | Latitude of the matched G-NAF address, or `null`. |
| `lng` | number | Longitude of the same point, or `null`. |
| `geoq` | string | `address`, `near` or `corner`, or `null` where there is no point. |
| `source` | string | Credit line for the Department list and the G-NAF credit for map points. |
| `source_as_of` | string | Date the Department page was last modified, `2026-09-28`. |

## Data notes

* The records are the 98 cards of the Department list, one per site. The page says there are 70 hubs: 15 Suburban University Study Hubs and 55 Regional University Study Hubs. Grouping the cards by name gives exactly 70 hubs (55 Regional and 15 Suburban), 15 of which have several sites, such as CUC RAPAD Central West Queensland (8 sites), Gippsland East Study Hub Outreach (5) and Lumen Wheatbelt (4).
* `hub` is the card name with only a trailing bracket removed, for example CUC Ovens Murray (Bright) gives the hub CUC Ovens Murray. Cards with different names stay separate hubs, such as Gippsland East Study Hub (Bairnsdale) and Gippsland East Study Hub Outreach, or Lumen Wheatbelt and Lumen Wheatbelt Northam. `site` keeps the full card name.
* The cards do not show the program. `hub_type` comes from the program line on each hub detail page on the Department site: 17 sites in 15 hubs are Suburban (Armadale, Beenleigh, Ellenbrook, Fairfield, Inala, Kurri Kurri, Liverpool, Mandurah, Macquarie Fields, Melton, Mt Druitt and Emerton, Northern Study Hub (Broadmeadows and Epping), Southeast Tasmania, Strathpine and Playford), and the other 81 sites are Regional. Gippsland East Study Hub has no detail page link and is listed as Regional.
* Two email addresses that carry a staff member name are left out (Copper Coast University Centre and CUC Hay), so `email` is `null` there. Shared inboxes are kept, and several sites of one hub share one. Moree and Narrabri (CUC North West) share `moree@cucnorthwest.edu.au`, as published. No phone numbers are published.
* `address` and `town` are as published. The Narrabri address reads 8 Doyle Steet in the source and is kept, and Armadale reads Upper 145 Jull Street. CUC Norfolk Island has no state on the page, so `state` is `OT` (Other Territories, the G-NAF code), with town Burnt Pine and postcode 2899. Australia Post lists 2899 under NSW.
* `lat` and `lng` come from the Geoscape G-NAF, November 2026 release (GDA2020), not from the Department page. 92 of 98 sites have a point: `address` (76 sites, a G-NAF address with that street number), `near` (15, the nearest numbered address on the same street at most 6 numbers away, or the same number on a street of another type, such as Day Street and Day Avenue) and `corner` (1, the midpoint of two named streets at most 150 m apart). 6 sites have no point: Birdsville, Warwick, Leeton (no street number), Wongan Hills, and Garrthalala and Wuyagiba (no street).
* The data is a snapshot of the Department page as last modified on 28 September 2026, the `source_as_of` value.

## Examples

Western Australia sites of the Lumen hubs, found by a wildcard on the hub name. This call returns 5 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=WA' --data-urlencode 'hub=*Lumen*'
```

```json
{
  "hub": "Lumen Wheatbelt Regional University Study Hubs",
  "site": "Lumen Wheatbelt Regional University Study Hubs (Merredin)",
  "hub_type": "Regional",
  "address": "22 Coronation Street",
  "town": "Merredin",
  "state": "WA",
  "postcode": "6415",
  "email": "merredin@lumenwruc.edu.au",
  "website": "https://lumenwruc.edu.au/",
  "lat": -31.479221,
  "lng": 118.277824,
  "geoq": "address",
  "source": "© Commonwealth of Australia, Department of Education, University Study Hubs, CC BY 4.0 (https://www.education.gov.au/access-and-participation/university-study-hubs). Map points: Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement.",
  "source_as_of": "2026-09-28"
}
```

All sites of the Suburban University Study Hubs. This call returns 17 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=*' --data-urlencode 'hub_type=Suburban'
```

New South Wales sites whose town ends in ton. This call returns 2 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=NSW' --data-urlencode 'town=*ton'
```

Queensland sites between latitude -28 and -27, a range with two dots. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=QLD' --data-urlencode 'lat=-28..-27'
```

Sites in a 4xxx postcode, which is Queensland, as text with a wildcard. This call returns 24 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=*' --data-urlencode 'postcode=4*'
```

Sites whose shared inbox is a government address. This call returns 2 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=*' --data-urlencode 'email=*.gov.au'
```

Sites whose point is the corner of two named streets. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_university_study_hubs' --data-urlencode 'state=*' --data-urlencode 'geoq=corner'
```

## Data source

© Commonwealth of Australia, Department of Education, University Study Hubs, CC BY 4.0. Source: Australian Government Department of Education, [University Study Hubs](https://www.education.gov.au/access-and-participation/university-study-hubs), page last modified 28 September 2026. Material on the Department website is provided under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/), with exceptions that include content supplied by third parties, the Department logo and the Commonwealth Coat of Arms, and the attribution to use is "© Commonwealth of Australia" (see the [Department copyright notice](https://www.education.gov.au/copyright)). This data was changed from the source as described in the notes above. The Department logo and Coat of Arms are not used, and the Department does not endorse this API.

Map points come from the Geoscape Geocoded National Address File (G-NAF), November 2026 release, used under the Open G-NAF End User Licence Agreement. Required credit: "Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement." The licence says you must not use G-NAF to generate an address or a list of addresses for sending mail, unless you check each address against another source. This API is for finding study hubs, not for building a mailing list.

Not affiliated with or endorsed by the Department of Education. The data is as the Department page publishes it, with the changes in the notes above, and it can contain errors.

[Back to the API list](../../README.md)
