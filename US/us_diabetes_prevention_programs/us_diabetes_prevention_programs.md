# US Diabetes Prevention Programs

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,570](https://img.shields.io/badge/records-1%2C570-lightgrey) ![updated 2026-11-10](https://img.shields.io/badge/updated-2026--11--10-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

1,570 CDC recognized National Diabetes Prevention Program lifestyle change providers in all 50 states, DC, and 8 US territories and freely associated states, on the registry dated November 10, 2026. Each record has the organization name, address, phone, website, recognition status, who classes are open to, delivery mode, an umbrella hub when one is listed, and a map point for 1,373 sites. Choose a state, then narrow by city, ZIP, name, status, delivery mode, umbrella hub or a lat and lng box.

Largest states: California (126), New York (113), Pennsylvania (80), Texas (76) and North Carolina (70). Territories and freely associated states: Puerto Rico (9), Federated States of Micronesia (7), Northern Mariana Islands (3), Guam, Marshall Islands, Palau and the US Virgin Islands (2 each), and American Samoa (1). Recognition status: Full Plus (625), Preliminary (447), Pending (349) and Full (149). Delivery modes, as published: In-person (673), In-Person with a Distance Learning Component (591), Distance Learning (live) (159), Online (non-live) (99) and Combination with an Online Component (48).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_diabetes_prevention_programs` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per program site or delivery mode. All matches are returned. |

## Query parameters

Required: 1 field (`state`). Optional: 11 fields. Disallowed: 5 fields.

`state` must be in every call. Use the two letter code, such as `CA` or `PR`. Partial filters accept `*`, such as `name=*YMCA*` or `zip=995*`. `lat` and `lng` accept ranges with two dots, such as `lat=40.5..40.95`. All other filterable fields are exact match strings. Text filters ignore case, so `city=Chicago` also matches CHICAGO. `address_line2`, `phone`, `web_address`, `source` and `updated` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A call without `state`, or a filter on a disallowed field, returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `state` | Required | No | No | Required two letter state or territory code, 59 values. Largest: CA (126); NY (113); PA (80); TX (76); NC (70); MD (66); GA (65); FL (63); IL (48). Territories and freely associated states: PR (9); FM (7); MP (3); GU (2); MH (2); PW (2); VI (2); AS (1). Text match ignores case. |
| `name` | Optional | Yes | No | Organization name exactly as CDC publishes it, with stray spaces trimmed only. 1,436 distinct names; one organization can have several rows for different sites or delivery modes. Partial wildcards allowed, such as *YMCA* or *Pharmacy*. |
| `address_line1` | Optional | Yes | No | First address line as published, usually the street. Partial wildcards allowed. |
| `city` | Optional | Yes | No | City as published. Case is not always the same in the source (for example Chicago and CHICAGO). Text match ignores case, and a wildcard may also help, such as San *. |
| `zip` | Optional | Yes | No | ZIP code as text, as published: 5 digits (656 rows) or ZIP+4 such as 99503-7174 (914 rows). Partial wildcards allowed, such as 995* or 10001*. |
| `recognition_status` | Optional | No | No | CDC recognition status, exact match (case ignored). Values: Full Plus (625); Preliminary (447); Pending (349); Full (149). |
| `class_open_to` | Optional | Yes | No | Who may join, as published, with the source's trailing space trimmed. Words in any mix of Public, Employees, Members and Other, such as Public (708); Public Employees (234); Public Employees Members (217); also Non-Delivery Site (5). 1,380 rows include Public. Use *Public* to find programs open to anyone. |
| `delivery_mode` | Optional | Yes | No | How the program is delivered, as published. Values: In-person (673); In-Person with a Distance Learning Component (591); Distance Learning (live) (159); Online (non-live) (99); Combination with an Online Component (48). Exact match works on the full value; wildcards such as *Online* or *Distance* group related modes. |
| `umbrella_hub` | Optional | Yes | No | Umbrella hub organization the program delivers through, as published. Blank in 1,407 rows. Largest: HabitNu (32); The Granite YMCA (15); YMCA of Metropolitan Milwaukee (14); Health Promotion Council of Southeastern Pennsylvania, Inc. (13). Partial wildcards allowed. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees from the US Census Bureau Geocoder, matched on the published street, city, state and ZIP. Present in 1,373 rows and blank in 197 (no match, a tie between two places, or a territory the geocoder does not cover). For online programs the point is the organization's address, not where classes happen. Bounds 18.340115..64.831768. Range uses two dots, such as 40.5..40.95. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees from the same geocoder. Present in 1,373 rows. Bounds -158.031307..-66.082517. Range uses two dots, such as -74.3..-73.7. |
| `coord_source` | Optional | No | No | Where the map point came from. Exact match. Values: US Census Geocoder, exact (1,181); US Census Geocoder, non exact (192); blank (197). |
| `address_line2` | Disallowed | No | No | Second address line as published, such as a suite, building or campus name. Blank in 1,166 rows. Not filterable. |
| `phone` | Disallowed | No | No | Organization phone as published, such as (907) 743-7200; 198 rows include an extension such as Ext: 104. Not filterable. |
| `web_address` | Disallowed | No | No | Program web address as published. Blank in 1,149 rows. Not filterable. |
| `source` | Disallowed | No | No | Source name and URL. Not filterable. |
| `updated` | Disallowed | No | No | Registry export date 2026-11-10, as text. Not filterable. |

## Response fields

Blank source cells are returned as `null`. `__id` is an integer. `lat` and `lng` are numbers. All other values are strings.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. The source has no single unique key: one organization can have several rows for different sites or delivery modes. |
| `name` | string | Organization name as CDC publishes it. 1,436 distinct names in 1,570 rows. |
| `address_line1` | string | First address line, usually the street. |
| `address_line2` | string | Second address line, such as a suite, building or campus name. Null in 1,166 rows. |
| `city` | string | City as published. Case is not always the same in the source. |
| `state` | string | Two letter state or territory code. 59 values. |
| `zip` | string | ZIP code as text, 5 digits or ZIP+4. |
| `phone` | string | Organization phone as published. |
| `web_address` | string | Program web address as published. Null in 1,149 rows. |
| `recognition_status` | string | Full Plus, Preliminary, Pending or Full. |
| `class_open_to` | string | Who may join, as published. |
| `delivery_mode` | string | How the program is delivered, as published. |
| `umbrella_hub` | string | Umbrella hub organization, when listed. Null in 1,407 rows. |
| `lat` | number | Latitude in decimal degrees. Null in 197 rows. |
| `lng` | number | Longitude in decimal degrees. Null in 197 rows. |
| `coord_source` | string | US Census Geocoder, exact or US Census Geocoder, non exact. Null in 197 rows. |
| `source` | string | CDC Diabetes Prevention Recognition Program Registry, https://dprp.cdc.gov/Registry. |
| `updated` | string | Registry export date 2026-11-10. |

## Data notes

* One organization can have several rows (1,436 distinct names in 1,570 rows), one per site or delivery mode.
* The email column was dropped (264 rows had one, many of them personal webmail or named staff addresses). One row whose organization name is only a person's name, a solo medical practice in Hackettstown, NJ, was dropped whole rather than renamed, so that no published value is changed. Phones are organization contact numbers as published; US numbers cannot be told apart from mobiles and were kept.
* Map points come only from the US Census Bureau Geocoder (batch address service, Public_AR_Current benchmark, run November 11, 2026, Asia/Singapore), a US government service whose output is in the public domain. Points were matched on the published street, city, state and ZIP; where line 1 was a building name and line 2 a street, line 2 was used. No OpenStreetMap or other Open Database License data was used, and no point was guessed. For online and distance programs the point is the organization's listed address, not where a class meets.
* 1,373 of 1,570 rows (87.5%) have a point: US Census Geocoder, exact (1,181) or US Census Geocoder, non exact (192). 197 rows are blank: 185 the geocoder could not match and 12 ties between two places. In the territories, 3 of 9 Puerto Rico rows have a point; American Samoa, the Federated States of Micronesia, Guam, the Marshall Islands, the Northern Mariana Islands, Palau and the US Virgin Islands have none (19 rows). Coverage by delivery mode: In-person 574 of 673; In-Person with a Distance Learning Component 520 of 591; Distance Learning (live) 145 of 159; Online (non-live) 91 of 99; Combination with an Online Component 43 of 48. Bounds: latitude 18.340115..64.831768; longitude -158.031307..-66.082517. Non exact matches may sit at the street or ZIP level rather than the building.
* Some line 2 values hold a city and ZIP or a building name, as published. City name case is not always the same in the source.
* Changes from the source: shorter field names; leading, trailing and repeated spaces trimmed (the source adds a trailing space to almost every `class_open_to` value and to some names, cities and phones); no other changes to names, statuses or values.

## Examples

Programs in Alaska whose ZIP code starts with 995. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_diabetes_prevention_programs' --data-urlencode 'state=AK' --data-urlencode 'zip=995*'
```

First record:

```json
[
  {
    "__id": 1,
    "name": "Anchorage Neighborhood Health Center",
    "address_line1": "4951 Business Park Blvd",
    "address_line2": null,
    "city": "Anchorage",
    "state": "AK",
    "zip": "99503-7174",
    "phone": "(907) 743-7200",
    "web_address": null,
    "recognition_status": "Preliminary",
    "class_open_to": "Public Employees",
    "delivery_mode": "In-person",
    "umbrella_hub": null,
    "lat": 61.175974,
    "lng": -149.889857,
    "coord_source": "US Census Geocoder, exact",
    "source": "CDC Diabetes Prevention Recognition Program Registry, https://dprp.cdc.gov/Registry",
    "updated": "2026-11-10"
  }
]
```

Programs in a New York City area box. This call returns 54 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_diabetes_prevention_programs' --data-urlencode 'state=NY' --data-urlencode 'lat=40.5..40.95' --data-urlencode 'lng=-74.3..-73.7'
```

Providers in Fresno, California. This call returns 2 records, California Health Collaborative and Shehadey Family Foods:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_diabetes_prevention_programs' --data-urlencode 'state=CA' --data-urlencode 'city=Fresno'
```

## Data source

[CDC Diabetes Prevention Recognition Program Registry](https://dprp.cdc.gov/Registry), data as of November 10, 2026, available free from CDC. The full registry export (DPRP_Results_Full_RegistryNov_10_2026.xlsx, from the Registry page's export button) was downloaded November 11, 2026, Asia/Singapore. Materials developed by CDC. CDC states that most of its information [is in the public domain, and may be freely used or reproduced without obtaining copyright permission](https://www.cdc.gov/other/agencymaterials.html), and as a work of the US government it is not subject to copyright in the US ([17 U.S.C. § 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)). Organization names, statuses and other published values are kept as CDC publishes them, with stray spaces trimmed only.

Map points come from the [US Census Bureau Geocoder](https://geocoding.geo.census.gov/geocoder/), a US government service whose output is in the public domain.

Credit: CDC Diabetes Prevention Recognition Program Registry, https://dprp.cdc.gov/Registry, data as of November 10, 2026. Map points: US Census Bureau Geocoder. Use of this data, including any links to the CDC website, does not imply endorsement by CDC, ATSDR, HHS or the United States Government of this API or its publisher. Reference to specific commercial products, manufacturers, companies, or trademarks does not constitute its endorsement or recommendation by the U.S. Government, Department of Health and Human Services, or Centers for Disease Control and Prevention. No CDC, ATSDR or HHS logo or mark is used. A program's listing here is not a referral; check the CDC registry for current status, as CDC updates it often.

