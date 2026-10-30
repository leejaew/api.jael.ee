# Australia headspace Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 177](https://img.shields.io/badge/records-177-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license CC BY 4.0 and ODbL](https://img.shields.io/badge/license-CC%20BY%204.0%20%2B%20ODbL-green)

All 177 headspace youth mental health centres listed in Australia in October 2026. headspace centres offer free or low cost help for young people aged 12 to 25. Each row has the centre name, suburb, postcode, street address, phone, email, the agency that runs it, the Primary Health Network (PHN) that funds it, the services it lists, its opening hours, satellite and outreach flags, the ABS remoteness area and a checked map point. Filter by the required `state`, then by suburb, service, remoteness or a coordinate range.

Rows by state: NSW 54, VIC 43, QLD 33, WA 22, SA 14, TAS 5, NT 4 and ACT 2. 31 rows are satellites linked to a larger centre and 2 are outreach services. 91 centres are outside the Major Cities remoteness area.

> **Use of this data.** Centre facts come from the headspace website and the headspace network map. Remoteness comes from the Australian Bureau of Statistics and one map point from OpenStreetMap. Not affiliated with or endorsed by headspace. Hours and services can change, so check the centre page or call the centre before you go. This list is not a crisis service. For urgent help call 000.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_headspace_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 23 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 177 centres. Disallowed means the field is returned in the output but cannot be used as a filter.

Yes or no fields hold the text `yes` or `no`. Only `days_open`, `lat` and `long` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory abbreviation: `NSW` (54), `VIC` (43), `QLD` (33), `WA` (22), `SA` (14), `TAS` (5), `NT` (4) or `ACT` (2). Wildcards work, so `state=*` lists all 177. |
| `centre_name` | Optional | Yes | No | Centre name as headspace lists it, such as `headspace Bendigo` (1) or `*Darwin*` (1). |
| `suburb` | Optional | Yes | No | Suburb or town of the centre, such as `Bendigo` or `Sunshine`. |
| `postcode` | Optional | Yes | No | Four digit postcode, kept as text so NT postcodes keep their leading zero, such as `0811` (1) or `3*` (43). Blank for 1 centre (headspace Pilbara), which works as outreach across a region. |
| `address` | Optional | Yes | No | Street address from the centre page, such as `*Main Street*` (4). PO Boxes, Country acknowledgments and parking notes are removed. |
| `phone` | Optional | Yes | No | Centre phone number in Australian form, such as `(03) 5406 1400` (1) or `1800 *` (17). |
| `email` | Optional | Yes | No | Centre contact email from the centre page. |
| `outreach` | Optional | Yes | No | `yes` when the network map marks the centre as an outreach service. `yes` on 2 rows. |
| `satellite` | Optional | Yes | No | `yes` when the centre is a satellite linked to a larger centre, from the network map or the centre page. `yes` on 31 rows. |
| `linked_centre` | Optional | Yes | No | Main centre a satellite is linked to, as the centre page states, such as `headspace Bega` (2). Filled for 22 rows, so `linked_centre=*` returns 22. |
| `early_psychosis` | Optional | Yes | No | `yes` when the centre page lists Early Psychosis among its services. `yes` on 17 rows. |
| `work_and_study_ips` | Optional | Yes | No | `yes` when the network map marks the centre as offering Work and Study Individual Placement and Support (IPS). `yes` on 58 rows. |
| `has_gp` | Optional | Yes | No | `yes` when the centre page lists Doctor (GP) among its services. `yes` on 97 rows. |
| `services` | Optional | Yes | No | Services named on the centre page, separated by semicolons, such as `*Sexual Health*` (109). |
| `lead_agency` | Optional | Yes | No | Organization that runs the centre, as named on the centre page, such as `*EACH*` (10). 68 different agencies. |
| `phn` | Optional | Yes | No | Primary Health Network that funds the centre, as named on the centre page, such as `*Brisbane*` (12). Blank for 2 centres whose page names only WA Primary Health Alliance. |
| `hours` | Optional | Yes | No | Opening hours per day in 24 hour time, as published, such as `*Sat 09*` (2). Closed days read `closed`. |
| `days_open` | Optional | No | Yes | Number of days per week the centre is open, from 2 to 6: 5 (148), 4 (14), 3 (10), 6 (3) or 2 (2). Example: `days_open=5..7` (151). |
| `open_saturday` | Optional | Yes | No | `yes` when the centre opens on Saturday. `yes` on 3 rows. |
| `remoteness_area` | Optional | Yes | No | ABS Remoteness Area (ASGS 2021) of the map point: `Major Cities` (86), `Inner Regional` (53), `Outer Regional` (28), `Remote` (8) or `Very Remote` (2). `*Remote` returns 10. |
| `regional` | Optional | Yes | No | `yes` when the remoteness area is outside Major Cities. `yes` on 91 rows. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from -42.880255 to -12.372352. Example: `lat=-34..-33` (28). |
| `long` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 114.607436 to 153.566041. Example: `long=150..152` (36). |
| `geocode_quality` | Optional | Yes | No | How the map point was checked: `address_checked` (161), `suburb_checked` (14), `street_point` (1) or `region_pin` (1). See the notes. |
| `centre_page` | Disallowed | No | No | Centre page on the headspace website. |
| `source` | Disallowed | No | No | Sources of the row. |
| `source_as_of` | Disallowed | No | No | Date the sources were read, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `centre_name` | string | Centre name as headspace lists it. |
| `state` | string | State or territory abbreviation. |
| `suburb` | string | Suburb or town of the centre. |
| `postcode` | string | Four digit postcode as text. Null for 1 row. |
| `address` | string | Street address. |
| `phone` | string | Centre phone number. |
| `email` | string | Centre contact email. |
| `outreach` | string | `yes` or `no`: outreach service on the network map. |
| `satellite` | string | `yes` or `no`: satellite of a larger centre. |
| `linked_centre` | string | Main centre a satellite is linked to. Null for 155 rows. |
| `early_psychosis` | string | `yes` or `no`: Early Psychosis listed on the centre page. |
| `work_and_study_ips` | string | `yes` or `no`: Work and Study IPS on the network map. |
| `has_gp` | string | `yes` or `no`: Doctor (GP) listed on the centre page. |
| `services` | string | Services on the centre page, separated by semicolons. |
| `lead_agency` | string | Organization that runs the centre. |
| `phn` | string | Primary Health Network that funds the centre. Null for 2 rows. |
| `hours` | string | Hours for Mon to Sun in 24 hour time, separated by semicolons, such as `Mon 09:00-17:00` or `Sun closed`. |
| `days_open` | integer | Days per week the centre is open. |
| `open_saturday` | string | `yes` or `no`: open on Saturday. |
| `remoteness_area` | string | ABS Remoteness Area (ASGS 2021) of the map point. |
| `regional` | string | `yes` or `no`: outside Major Cities. |
| `lat` | number | Latitude (WGS 84). |
| `long` | number | Longitude (WGS 84). |
| `geocode_quality` | string | How the map point was checked. |
| `centre_page` | string | Centre page on headspace.org.au. |
| `source` | string | Sources of the row. |
| `source_as_of` | string | Date the sources were read, `YYYY-MM-DD`. |

## Notes

* headspace is for young people aged 12 to 25. This API is a directory, not a crisis service. For urgent help call 000.
* The list follows the headspace centre list and centre pages read on 2026-10-30, joined with the headspace network map of November 2025. Centres that open, move or close after that date are not reflected.
* `hours` and `days_open` are the regular hours as published. Public holidays and changes are not shown. Some centres also offer phone or online sessions outside these hours.
* A satellite is a smaller site linked to a larger centre. 31 rows are satellites, but only 22 name a `linked_centre`, because the centre page does not always say which centre it is linked to.
* The yes or no service flags (`early_psychosis`, `has_gp`) follow what the centre page lists. A `no` means the page does not list it, not that the centre never offers it.
* `remoteness_area` is read from the map point against the ABS Remoteness Areas (ASGS 2021). `regional` is `yes` for every area other than Major Cities.
* `geocode_quality` tells you how far to trust the map point:
  * `address_checked` (161): the point is within 2 km of the street address in OpenStreetMap, or it was checked by hand.
  * `suburb_checked` (14): the point falls in the right ABS suburb.
  * `street_point` (1): headspace Sarina. The headspace pin was 2.6 km off, so the point is an OpenStreetMap point on Broad Street.
  * `region_pin` (1): headspace Pilbara, an outreach service across a region with no single street address.
* The asterisk is the wildcard and cannot be escaped, so a filter value cannot match a literal `*`.

## Examples

Two filters together. Centres in Victoria that list Early Psychosis among their services. This call returns 5 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/au_headspace_centres" --data-urlencode "state=VIC" --data-urlencode "early_psychosis=yes" | jq -r '.[] | [.centre_name, .suburb, .postcode, .lead_agency, .phone] | @tsv'
```

```text
headspace Bentleigh	Bentleigh	3204	Alfred Health	(03) 9076 9400
headspace Dandenong	Dandenong	3175	EACH	1800 367 968
headspace Frankston	Frankston	3199	YSAS	(03) 9769 6419
headspace Narre Warren	Narre Warren	3805	EACH	1800 367 968
headspace Rosebud	Rosebud	3940	YSAS	(03) 5925 9464
```

A latitude and longitude range. Victorian centres with a GP in a box around greater Melbourne. This call returns 11 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/au_headspace_centres" --data-urlencode "state=VIC" --data-urlencode "lat=-38..-37.5" --data-urlencode "long=144.5..145.5" --data-urlencode "has_gp=yes" | jq -r '.[] | [.centre_name, .suburb, .days_open, .lat, .long] | @tsv'
```

```text
headspace Bentleigh	Bentleigh	5	-37.932754	145.034933
headspace Box Hill	Box Hill	5	-37.818372	145.117803
headspace Collingwood	Abbotsford	5	-37.797236	144.995739
headspace Dandenong	Dandenong	5	-37.986574	145.215231
headspace Elsternwick	Elsternwick	5	-37.884281	145.002451
headspace Glenroy	Glenroy	5	-37.704997	144.918043
headspace Hawthorn	Hawthorn	5	-37.822836	145.03573
headspace Knox	Wantirna South	5	-37.870491	145.243951
headspace Malvern	Malvern	5	-37.856355	145.032138
headspace Melton	Melton	5	-37.702708	144.574417
headspace Syndal	Mount Waverley	5	-37.877587	145.147093
```

A partial match on the name. The NT postcode keeps its leading zero. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/au_headspace_centres" --data-urlencode "state=NT" --data-urlencode "centre_name=*Darwin*"
```

```json
[
  {
    "__id": 57,
    "centre_name": "headspace Darwin",
    "state": "NT",
    "suburb": "Casuarina",
    "postcode": "0811",
    "address": "Ground Level, Stage 4 CASCOM Centre, 13-17 Scaturchio Street",
    "phone": "(08) 8931 5999",
    "email": "headspacedarwin@anglicare-nt.org.au",
    "outreach": "no",
    "satellite": "no",
    "linked_centre": null,
    "early_psychosis": "yes",
    "work_and_study_ips": "yes",
    "has_gp": "yes",
    "services": "Alcohol & Drug Services; Doctor (GP); Early Psychosis; Mental Health Services; Sexual Health Services; Work & Study Services; Youth Programs; Youth Reference Group",
    "lead_agency": "Anglicare NT",
    "phn": "Northern Territory",
    "hours": "Mon 09:00-17:00; Tue 09:00-19:00; Wed 09:00-19:00; Thu 09:00-19:00; Fri 09:00-17:00; Sat closed; Sun closed",
    "days_open": 5,
    "open_saturday": "no",
    "remoteness_area": "Outer Regional",
    "regional": "yes",
    "lat": -12.372352,
    "long": 130.882074,
    "geocode_quality": "address_checked",
    "centre_page": "https://headspace.org.au/headspace-centres/darwin/",
    "source": "headspace centre list and centre page (https://headspace.org.au/headspace-centres/); headspace network map, November 2025; ABS Remoteness Areas, ASGS 2021",
    "source_as_of": "2026-10-30"
  }
]
```

## Data source

The list of centres and their facts come from the headspace centre list and each centre page on the headspace website (https://headspace.org.au/headspace-centres/), joined with the headspace network map (November 2025) for the outreach, satellite and Work and Study IPS flags. Only facts were taken (names, addresses, phones, emails, agencies, services and hours). Remoteness areas come from the Australian Bureau of Statistics. Map points were checked against OpenStreetMap and the ABS suburbs, and one point comes from OpenStreetMap. The yes or no flags, `days_open`, `regional` and `geocode_quality` are set by us, and the data can go out of date. Check with the centre.

Credit lines:

* Source: headspace National Youth Mental Health Foundation Ltd, centre list and centre pages (https://headspace.org.au/headspace-centres/) and headspace network map (November 2025). Facts only.
* Remoteness Areas: © Australian Bureau of Statistics, ASGS 2021, CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/).
* One map point (headspace Sarina): © OpenStreetMap contributors, ODbL (https://www.openstreetmap.org/copyright).
* Changes were made: rows joined across sources, text tidied, flags and remoteness added.
* Not affiliated with or endorsed by headspace. For urgent help call 000.

[Back to the API list](../../README.md)
