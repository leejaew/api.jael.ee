# New South Wales Licensed Venues

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 18,986](https://img.shields.io/badge/records-18%2C986-lightgrey) ![updated 2026-11-01](https://img.shields.io/badge/updated-2026--11--01-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

18,986 current and suspended liquor licences in New South Wales, as listed in the Liquor and Gaming NSW premises list as of 1 November 2026. Each record has the licence type, trading status, venue name, address, council, late trading flags, business type, electronic gaming machine count and a map point. No field is required, so you can filter by council, suburb, licence type, late hours or a latitude and longitude box.

The official types are Liquor - on-premises licence (9,223), Liquor - packaged liquor licence (3,142), Liquor - hotel licence (2,104), Liquor - producer wholesaler licence (1,708), Liquor - limited licence (1,253), Liquor - club licence (1,247), Liquor - small bar licence (293) and Liquor - pop-up licence (16). 18,976 licences are current and 10 are suspended. 3,257 venues may trade after midnight, 2,534 after 3am and 1,850 after 5am. 2,100 venues have at least one electronic gaming machine.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_nsw_licensed_venues` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per licence. All matches are returned. |

## Query parameters

Required: none. Optional: 32 fields. Disallowed: 2 fields.

A call with no filters returns all 18,986 records. Partial filters accept `*`, such as `licence_type=*small bar*`, `licence_name=*BWS*` or `postcode=200*`. `lat`, `lng` and `egms` accept a range with two dots, such as `lat=-33.88..-33.86&lng=151.20..151.22` or `egms=1..30`. `status`, `trading_status`, `region`, `licensee_type`, `band` and the Yes or No flags (`extended_trading`, `reduced_trading`, `primary_service`, `right_to_keep_gaming`, `unrestricted`, `after_12am`, `after_3am` and `after_5am`) are exact match. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `licence_number` | Optional | Yes | No | Official licence number, 13 characters, unique on every row, such as LIQH400100078 or LIQS220000001. The prefix marks the type: LIQO on-premises (9223), LIQP packaged liquor (3142), LIQH hotel (2104), LIQW producer wholesaler (1708), LIQL limited (1253), LIQC club (1247), LIQS small bar (293), LIQT pop-up (16). Never blank. Partial wildcards allowed, such as `LIQH*` (2104) or `LIQS*` (293). |
| `licence_type` | Optional | Yes | No | Official type in 8 values: Liquor - on-premises licence (9223), Liquor - packaged liquor licence (3142), Liquor - hotel licence (2104), Liquor - producer wholesaler licence (1708), Liquor - limited licence (1253), Liquor - club licence (1247), Liquor - small bar licence (293), Liquor - pop-up licence (16). Never blank. Partial wildcards allowed, such as `*small bar*` (293) or `*hotel*` (2104). |
| `start_date` | Optional | Yes | No | Date the licence started, YYYY-MM-DD text, from 1905-02-06 (Broadway Cellars, as the source lists it) to 2026-11-01. Never blank. Partial wildcards allowed, such as `2026*` (847) or `2026-11*` (2). |
| `status` | Optional | No | No | Current (18976) or Suspended (10). The monthly list includes only these two statuses. Exact match. |
| `status_date` | Optional | Yes | No | Date of the last status change, YYYY-MM-DD text. Never blank. Partial wildcards allowed, such as `2026-10*`. |
| `trading_status` | Optional | No | No | Trading (18481), Temporarily Ceased Trading (495) when the venue has been closed for 6 weeks or more, or Suspended (10, the same 10 rows as `status=Suspended`). Exact match. |
| `licence_name` | Optional | Yes | No | Venue name as Liquor and Gaming NSW lists it, such as Royal Hotel, BWS - Beer Wine Spirits or Mode By YHA Sydney. Never blank. Names repeat (17,406 distinct names for 18,986 rows); BWS - Beer Wine Spirits appears 374 times. Partial wildcards allowed, such as `*BWS*` (379), `*RSL*` (123) or `*hotel*` (1636). |
| `address` | Optional | Yes | No | Street address as the source lists it, such as 8-10 LEE ST or Shop 3/78-80 Railway Cr. Never blank. Case follows the source, often capitals. Matching is case insensitive. Partial wildcards allowed, such as `*GEORGE ST*` (282). |
| `suburb` | Optional | Yes | No | Suburb in capitals as the source lists it, such as SURRY HILLS (235) or HAYMARKET. Never blank. 2,049 distinct suburbs. Matching is case insensitive, so `suburb=Surry Hills` also returns 235. Partial wildcards allowed, such as `*BEACH*`. |
| `postcode` | Optional | Yes | No | 4 digit NSW postcode as text, such as 2000 (1138) or 2026. Never blank. No leading zeros occur in this file, but the column stays text. Partial wildcards allowed, such as `200*` (1368). |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, from -37.249714 (southern NSW) to -28.164221 (Tweed). Never blank. Use a range, for example `lat=-33.88..-33.86&lng=151.20..151.22` for central Sydney (1098). A hyphen range returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, from 141.167268 (western NSW) to 153.632705 (eastern coast). Never blank. Use a range with `lat` to search a box, or `lng=153.5..153.63` with `lat=-28.70..-28.60` for the Byron area (152). A hyphen range returns HTTP 400. |
| `lga` | Optional | Yes | No | Local government area (council) as the source lists it, 129 values, such as Sydney (2529), Inner West (671), Northern Beaches (668), Central Coast (578) or Byron (228). Never blank. Use the exact name for a single council (`lga=Sydney`). A wildcard `*Sydney*` also matches North Sydney (2887 in total). Partial wildcards allowed, such as `*Western*`. |
| `precinct` | Optional | Yes | No | Sydney night time precinct when the source sets one: Sydney CBD Precinct (1228) or Kings Cross Precinct (97). Blank on 17661 rows. Partial wildcards allowed, such as `*Kings Cross*` (97). |
| `region` | Optional | No | No | Metropolitan (11954) or Country (7032), as classified for clubs and hotels with a right to keep gaming, and applied here to every row. Never blank. Exact match. |
| `licensee` | Optional | Yes | No | Organisation name of the licensee, such as YHA LTD or MERITON PROPERTY SERVICES PTY LIMITED, when `licensee_type` is Organisation (8288). Blank on all 10698 Person rows; personal names were removed. Partial wildcards allowed, such as `*Meriton*` (2) or `*PTY*` (6595). |
| `licensee_type` | Optional | No | No | Organisation (8288) or Person (10698). Never blank. Exact match. |
| `licensee_abn` | Optional | Yes | No | Australian Business Number as text, usually 11 digits, such as 94008387791. Blank on 11965 rows, including every Person row. Two source values are shorter than 11 digits and were kept as listed. Partial wildcards allowed. |
| `licensee_acn` | Optional | Yes | No | Australian Company Number as text, usually 9 digits, with leading zeros kept, such as 008387791 (1,330 values start with 0). Blank on 12415 rows, including every Person row. One source value is 6 digits and was kept as listed. Partial wildcards allowed, such as `0*` (1330). |
| `business_type` | Optional | Yes | No | Business type or types as the source lists them, often joined by commas, such as Restaurant (6244 rows with that exact value; 7369 contain Restaurant), Full hotel (1992) or Bottle shops & delivery (2343). Blank on 3248 rows: every club, producer wholesaler and small bar licence, which is how the source file leaves them. Partial wildcards allowed, such as `*Restaurant*` (7369) or `*Wine bar*` (26). |
| `authorisations` | Optional | Yes | No | Official authorisations and restrictions, comma separated, such as Reduced trading hours or Extended trading authorisation,Minors area authorisation,Right to keep gaming machines. Blank on 3499 rows. Partial wildcards allowed, such as `*gaming*` (3234). |
| `extended_trading` | Optional | No | No | Yes (2232) when the venue has an Extended Trading Authorisation, otherwise No (16754). Never blank. Exact match, Yes or No. |
| `reduced_trading` | Optional | No | No | Yes (13191) when the venue has chosen shorter hours than the legal maximum, otherwise No (5795). Never blank. Exact match, Yes or No. |
| `primary_service` | Optional | No | No | Yes (1730) when the venue has a Primary Service Authorisation, otherwise No (17256). Never blank. Exact match, Yes or No. |
| `right_to_keep_gaming` | Optional | No | No | Yes (3234) when the venue may keep and operate electronic gaming machines, otherwise No (15752). Never blank. Exact match, Yes or No. |
| `unrestricted` | Optional | No | No | Yes (1187) when the venue may trade with unrestricted hours, otherwise No (17799). Never blank. Exact match, Yes or No. |
| `after_12am` | Optional | No | No | Yes (3257) when the venue may trade after midnight on one or more days, otherwise No (15729). Never blank. Exact match, Yes or No. |
| `after_3am` | Optional | No | No | Yes (2534) when the venue may trade after 3am on one or more days, otherwise No (16452). Never blank. Exact match, Yes or No. |
| `after_5am` | Optional | No | No | Yes (1850) when the venue may trade after 5am on one or more days, otherwise No (17136). Never blank. Exact match, Yes or No. |
| `sa2` | Optional | Yes | No | Australian Bureau of Statistics Statistical Area 2 name, such as Sydney (South) - Haymarket or Surry Hills. Never blank in this edition of the list (633 distinct names). Partial wildcards allowed, such as `*Haymarket*`. |
| `band` | Optional | No | No | SA2 gaming risk band 1 (1186, lower risk), 2 (1123) or 3 (930, higher risk), for venues with a right to keep gaming. Blank on 15747 rows. Stored as text. Exact match, 1, 2 or 3. |
| `egms` | Optional | No | Yes | Number of authorised electronic gaming machines at the venue, from 0 to 745. Never blank; 2100 venues have at least one. Use a range, for example `egms=1..30` (1597) or `egms=100..745` (190). |
| `source` | Disallowed | No | No | Liquor & Gaming NSW licensed premises list, in all 18986 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date the official list is current to, YYYY-MM-DD (`2026-11-01`). Text. Not filterable. |

## Response fields

`lat` and `lng` are numbers. `egms` is an integer. All other values are strings, or `null` when the source has no value. There is no `__id`; `licence_number` is unique and is the stable key.

| Field | Type | Description |
|---|---|---|
| `licence_number` | string | Official 13 character licence number, unique on every row. |
| `licence_type` | string | Official licence type, such as Liquor - hotel licence. |
| `start_date` | string | Date the licence started, `YYYY-MM-DD`. |
| `status` | string | `Current` or `Suspended`. |
| `status_date` | string | Date of the last status change, `YYYY-MM-DD`. |
| `trading_status` | string | `Trading`, `Temporarily Ceased Trading` or `Suspended`. |
| `licence_name` | string | Venue name as Liquor and Gaming NSW lists it. |
| `address` | string | Street address as the source lists it. |
| `suburb` | string | Suburb, in capitals. |
| `postcode` | string | 4 digit NSW postcode as text. |
| `lat` | number | Latitude in decimal degrees. |
| `lng` | number | Longitude in decimal degrees. |
| `lga` | string | Local government area (council). |
| `precinct` | string | Sydney CBD Precinct or Kings Cross Precinct, or `null` on 17,661 rows. |
| `region` | string | `Metropolitan` or `Country`. |
| `licensee` | string | Organisation name of the licensee, or `null` on all 10,698 Person rows. |
| `licensee_type` | string | `Organisation` or `Person`. |
| `licensee_abn` | string | Australian Business Number as text, or `null` on 11,965 rows. |
| `licensee_acn` | string | Australian Company Number as text, with leading zeros kept, or `null` on 12,415 rows. |
| `business_type` | string | Business type or types, or `null` on 3,248 rows. |
| `authorisations` | string | Official authorisations and restrictions, or `null` on 3,499 rows. |
| `extended_trading` | string | `Yes` or `No`. |
| `reduced_trading` | string | `Yes` or `No`. |
| `primary_service` | string | `Yes` or `No`. |
| `right_to_keep_gaming` | string | `Yes` or `No`. |
| `unrestricted` | string | `Yes` or `No`. |
| `after_12am` | string | `Yes` or `No`. |
| `after_3am` | string | `Yes` or `No`. |
| `after_5am` | string | `Yes` or `No`. |
| `sa2` | string | Australian Bureau of Statistics Statistical Area 2 name. |
| `band` | string | SA2 gaming risk band `1`, `2` or `3`, or `null` on 15,747 rows. |
| `egms` | integer | Number of authorised electronic gaming machines, from 0 to 745. |
| `source` | string | Liquor & Gaming NSW licensed premises list. |
| `source_as_of` | string | Date the official list is current to, `2026-11-01`. |

## Data notes

* Personal licensee names are omitted. `licensee`, `licensee_abn` and `licensee_acn` are blank on all 10,698 Person rows. Organisation names, ABN and ACN were kept.
* Two source latitudes were missing a minus sign (Bottle Keg Can in Jannali, and HLWJV Worker Accommodation Facilities - Tarcutta). Those signs were flipped so the points sit in NSW. The same two rows have unlikely council names in the source (Wollongong for Jannali, Northern Beaches for Tarcutta); those LGA values were left as listed. Every other map point comes from the source.
* Late hours flags (`after_12am`, `after_3am`, `after_5am` and `unrestricted`) mean the licence allows trade after that time on one or more days, not that the venue is open every night.
* `business_type` is blank on every club, producer wholesaler and small bar row (3,248), as in the source.
* `band` is the SA2 gaming risk band and is filled mainly for venues with a right to keep gaming. Five other rows also have a band, as in the source.
* ABN and ACN values stay as listed, including two short ABNs and one short ACN. 1,330 ACN values keep a leading zero.
* This is a snapshot at 1 November 2026. A new monthly file is published on the official list page. This API is not legal advice.

## Examples

Small bars across New South Wales. This call returns 293 records. The first two:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_nsw_licensed_venues' --data-urlencode 'licence_type=*small bar*'
```

```json
[
  {
    "licence_number": "LIQS220000380",
    "licence_type": "Liquor - small bar licence",
    "start_date": "2026-10-02",
    "status": "Current",
    "status_date": "2026-10-02",
    "trading_status": "Trading",
    "licence_name": "Stage Door Productions",
    "address": "UNIT 7 33 MADDOX ST",
    "suburb": "ALEXANDRIA",
    "postcode": "2015",
    "lat": -33.90721,
    "lng": 151.194993,
    "lga": "Sydney",
    "precinct": null,
    "region": "Metropolitan",
    "licensee": "STAGE DOOR GC PTY LTD",
    "licensee_type": "Organisation",
    "licensee_abn": "59683962012",
    "licensee_acn": "683962012",
    "business_type": null,
    "authorisations": "Minors authorisation,Reduced trading hours",
    "extended_trading": "No",
    "reduced_trading": "Yes",
    "primary_service": "No",
    "right_to_keep_gaming": "No",
    "unrestricted": "No",
    "after_12am": "No",
    "after_3am": "No",
    "after_5am": "No",
    "sa2": "Erskineville - Alexandria",
    "band": null,
    "egms": 0,
    "source": "Liquor & Gaming NSW licensed premises list",
    "source_as_of": "2026-11-01"
  },
  {
    "licence_number": "LIQS220000379",
    "licence_type": "Liquor - small bar licence",
    "start_date": "2026-09-15",
    "status": "Current",
    "status_date": "2026-09-15",
    "trading_status": "Trading",
    "licence_name": "Easy Peasy",
    "address": "207 CLARENCE ST",
    "suburb": "SYDNEY",
    "postcode": "2000",
    "lat": -33.86992,
    "lng": 151.205199,
    "lga": "Sydney",
    "precinct": "Sydney CBD Precinct",
    "region": "Metropolitan",
    "licensee": "REDTOWN COMPANY PTY LTD",
    "licensee_type": "Organisation",
    "licensee_abn": "63662158492",
    "licensee_acn": "662158492",
    "business_type": null,
    "authorisations": "Extended trading authorisation,Reduced trading hours",
    "extended_trading": "Yes",
    "reduced_trading": "Yes",
    "primary_service": "No",
    "right_to_keep_gaming": "No",
    "unrestricted": "No",
    "after_12am": "Yes",
    "after_3am": "No",
    "after_5am": "No",
    "sa2": "Sydney (North) - Millers Point",
    "band": null,
    "egms": 0,
    "source": "Liquor & Gaming NSW licensed premises list",
    "source_as_of": "2026-11-01"
  }
]
```

Hotels in the City of Sydney that may trade after 3am. This call returns 192 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_nsw_licensed_venues' --data-urlencode 'lga=Sydney' --data-urlencode 'after_3am=Yes' --data-urlencode 'licence_type=Liquor - hotel licence'
```

Venues with 1 to 30 electronic gaming machines. This call returns 1,597 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_nsw_licensed_venues' --data-urlencode 'egms=1..30'
```

Licences in a central Sydney box. This call returns 1,098 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_nsw_licensed_venues' --data-urlencode 'lat=-33.88..-33.86' --data-urlencode 'lng=151.20..151.22'
```

## Data source

© State of New South Wales (Department of Creative Industries, Tourism, Hospitality and Sport) 2026. Source: [Liquor and Gaming NSW licensed premises list](https://www.nsw.gov.au/business-and-economy/liquor-and-gaming/gaming/gaming-machine-data-reports/licensed-premises-data), as at 1 November 2026. Licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license, which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited and changes are noted.

This API omits personal licensee names, drops detailed gaming entitlement columns from the official file, rounds coordinates to 6 decimal places, and corrects a missing minus on two source latitudes. Two council names that look wrong were left as listed. For the latest list visit the [official premises list page](https://www.nsw.gov.au/business-and-economy/liquor-and-gaming/gaming/gaming-machine-data-reports/licensed-premises-data).

Use of this data does not mean Liquor and Gaming NSW or the State of New South Wales endorses this API or its maker.

[Back to the API list](../../README.md)
