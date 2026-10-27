# Australia TAFE Campuses

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 402](https://img.shields.io/badge/records-402-lightgrey)

TAFE and public vocational college campuses across Australia, from 29 providers in all 8 states and territories. Each row is one campus with its provider, provider type, state, suburb, street address, postcode, phone, website, campus type and map coordinates. Dual sector providers are universities that also teach vocational courses, such as RMIT University and CQUniversity. Online and virtual campuses, sites with no street address and closed sites are not included, and some small Victorian sites are missing.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_tafe_campuses` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 9 fields. Disallowed: 4 fields.

The `state` parameter is required on every call. Use `state=*` to list all campuses. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory code: `NSW` (155 campuses), `VIC` (88), `QLD` (59), `WA` (55), `SA` (24), `TAS` (10), `NT` (6) or `ACT` (5). `state=*` lists all 402 campuses. |
| `name` | Optional | Yes | No | Campus name, for example `*Bruce*` or `*Sydney*`. |
| `provider` | Optional | Yes | No | TAFE or institute that runs the campus, such as `TAFE NSW`, `TasTAFE` or `*Metropolitan*`. 29 providers. |
| `provider_type` | Optional | No | No | `TAFE` (376 campuses) or `Dual sector` (26 campuses of universities that also teach vocational courses). |
| `campus_type` | Optional | No | No | `Campus` (356), `Specialist centre` (24) or `Community learning centre` (22). |
| `city` | Optional | Yes | No | Suburb or town, such as `Bruce`, `Darwin City` or `*Brisbane*`. |
| `address` | Optional | Yes | No | Street address, for example `*Vowels*` or `*Highway*`. |
| `postcode` | Optional | Yes | No | Australian postcode as text, such as `2617`, `08*` or `30*`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees, from -42.877814 to -10.576594. Use a range to search a box, for example `latitude=-34..-33`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees, from 113.655998 to 153.58233. Use a range to search a box, for example `longitude=150..152`. |
| `phone` | Disallowed | No | No | Campus or provider phone number as text. Blank for 1 campus. |
| `website` | Disallowed | No | No | Provider page for the campus. |
| `source` | Disallowed | No | No | Pages the row was built from and how the coordinates were found. |
| `updated` | Disallowed | No | No | Date the row was last checked, such as `2026-10-27`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Campus name. |
| `provider` | string | TAFE or institute that runs the campus. |
| `provider_type` | string | `TAFE` or `Dual sector`. |
| `state` | string | State or territory code. |
| `city` | string | Suburb or town. |
| `address` | string | Street address. |
| `postcode` | string | Australian postcode, with the leading zero kept for the Northern Territory. |
| `phone` | string | Campus or provider phone number. Many rows carry the provider's general line. |
| `website` | string | Provider page for the campus. |
| `campus_type` | string | `Campus`, `Specialist centre` or `Community learning centre`. |
| `latitude` | number | Latitude in decimal degrees. |
| `longitude` | number | Longitude in decimal degrees. |
| `source` | string | Pages the row was built from and how the coordinates were found. |
| `updated` | string | Date the row was last checked. |

Coordinate accuracy: 115 rows are address or building matches, 235 are street level points on the road (on long roads they can be up to about 2 km from the building), 20 are street corners or street matches without a house number, and 32 are suburb centres. Use the coordinates for area searches, not for exact building location. The `source` column says how each point was found.

## Examples

TAFE Queensland campuses in Brisbane, with wildcards on `provider` and `city`.

```bash
curl -s "https://api.jael.ee/JLEE/au_tafe_campuses?state=QLD&provider=*Queensland*&city=*Brisbane*"
```

Response (1 row):

```json
[
  {
    "__id": 214,
    "name": "South Bank",
    "provider": "TAFE Queensland",
    "provider_type": "TAFE",
    "state": "QLD",
    "city": "South Brisbane",
    "address": "66 Ernest Street",
    "postcode": "4101",
    "phone": "1300 308 233",
    "website": "https://tafeqld.edu.au/about/campus-locations/greater-brisbane/south-bank",
    "campus_type": "Campus",
    "latitude": -27.479079,
    "longitude": 153.020102,
    "source": "TAFE Queensland campus locations https://tafeqld.edu.au/about/campus-locations; coordinates OSM Nominatim (ODbL), address or building match",
    "updated": "2026-10-27"
  }
]
```

All 6 Northern Territory campuses, with name, postcode and phone:

```bash
curl -s "https://api.jael.ee/JLEE/au_tafe_campuses?state=NT" | jq -r '.[] | "\(.name) | \(.postcode) | \(.phone)"'
```

```text
Alice Springs | 0871 | 1800 061 963
Casuarina | 0810 | 1800 061 963
Darwin Waterfront | 0800 | 1800 061 963
Katherine | 0850 | 1800 061 963
Nhulunbuy | 0881 | 1800 061 963
Palmerston | 0830 | 1800 061 963
```

Dual sector campuses in inner and western Melbourne, with ranges on latitude and longitude. This call returns 8 campuses:

```bash
curl -s "https://api.jael.ee/JLEE/au_tafe_campuses?state=VIC&provider_type=Dual%20sector&latitude=-37.9..-37.7&longitude=144.8..145.1" | jq -r '.[].name'
```

```text
Brunswick
Bundoora
Melbourne City
Hawthorn
Footscray Nicholson
Footscray Park
Melbourne City
Sunshine
```

## Data source

Campus lists and addresses come from each provider's own campus pages. Victorian rows come from the Victorian Government region pages on [vic.gov.au](https://www.vic.gov.au) under CC BY 4.0, credit "© Copyright State Government of Victoria". Addresses and phone numbers for the other providers are plain facts, written in plain words, with the page named in `source`. TAFE Directors Australia and the 2018 AURIN point set were used only to cross check coverage. Coordinates come from single address lookups on OpenStreetMap data, © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), under the Open Database Licence (ODbL). Opening details and phone lines change, so check the provider page before you visit.

[Back to the API list](../../README.md)
