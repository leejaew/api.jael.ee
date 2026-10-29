# Australia Local Councils

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 541](https://img.shields.io/badge/records-541-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license CC BY and ODbL](https://img.shields.io/badge/license-CC%20BY%20%2B%20ODbL-green)

Every local council in Australia as of October 2026: 541 councils across the six states, the Northern Territory and the Indian Ocean Territories. Each row has the ABS Local Government Area code and name, the council name and type, the street address, suburb and postcode of the main council office, the main phone number, the general email, the website and a map point for the office. Filter by the required `state`, then by name, type, postcode, phone or a coordinate range.

Rows by state: NSW 128, VIC 79, QLD 78, WA 137, SA 70, TAS 29, NT 18 and OT 2 (Christmas Island and Cocos (Keeling) Islands). The ACT is left out because it has no local councils. Unincorporated areas and Norfolk Island are left out too.

> **Use of this data.** Council lists and contact facts come from state and territory government sources and council websites. Area codes come from the Australian Bureau of Statistics and map points from OpenStreetMap. Not affiliated with or endorsed by any council, government agency or local government association. Check with the council before you visit or write.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_local_councils` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 12 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 541 councils. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory: `NSW` (128), `VIC` (79), `QLD` (78), `WA` (137), `SA` (70), `TAS` (29), `NT` (18) or `OT` (2). `OT` is the Indian Ocean Territories. Wildcards work, so `state=*` lists all 541. |
| `lga_code` | Optional | Yes | No | ABS Local Government Area code (ASGS Edition 4, 2026), five digits such as `17200`. One code per row. The first digit is the state code (1 NSW, 2 VIC, 3 QLD, 4 SA, 5 WA, 6 TAS, 7 NT), so `1*` lists the 128 NSW councils. Christmas Island and Cocos (OT) use WA style `5xxxx` codes, so `5*` returns 139 rows. |
| `lga_name` | Optional | Yes | No | ABS name of the area, such as `Sydney` or `Central Highlands (Tas)`. |
| `council_name` | Optional | Yes | No | Name of the council, such as `*Shire*` (237) or `City of*` (52). |
| `council_type` | Optional | Yes | No | Kind of council, taken from its name: `shire` (225), `city` (126), `council` (72), `regional` (51), `district` (20), `town` (14), `aboriginal_shire` (12), `rural_city` (7), `municipality` (6), `borough` (3), `aboriginal_lands_authority` (2), `community_government` (2) or `town_authority` (1). |
| `office_address` | Optional | Yes | No | Street address of the main council office, such as `*Main Street*` (11). |
| `suburb` | Optional | Yes | No | Suburb or town of the main office, such as `Katherine`. |
| `postcode` | Optional | Yes | No | Four digit postcode of the main office, kept as text so leading zeros stay, such as `2000` or `08*` (19). |
| `phone` | Optional | Yes | No | Main phone number with area code, such as `02 9265 9333` or `1300*` (49). |
| `email` | Optional | Yes | No | General council email, such as `*@*.wa.gov.au` (137). Blank for 6 rows. |
| `latitude` | Optional | No | Yes | Latitude of the main office in decimal degrees (WGS 84), from -43.098826 to -10.435979. Example: `latitude=-34..-33` (80). |
| `longitude` | Optional | No | Yes | Longitude of the main office in decimal degrees (WGS 84), from 96.896707 to 153.561675. Example: `longitude=150..152` (68). |
| `geocode_quality` | Optional | Yes | No | How the map point was placed: `street` (288), `address_exact` (194), `council_office_poi` (43) or `locality_centroid` (16). See the notes. |
| `website` | Disallowed | No | No | Council website (https). |
| `source` | Disallowed | No | No | Source of the row's contact facts. |
| `source_as_of` | Disallowed | No | No | Date of the source list the row came from, such as `2026-10-29`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `lga_code` | string | ABS Local Government Area code, five digits. |
| `lga_name` | string | ABS name of the area. |
| `council_name` | string | Name of the council. |
| `council_type` | string | Kind of council. |
| `state` | string | State or territory code. |
| `office_address` | string | Street address of the main council office. |
| `suburb` | string | Suburb or town of the main office. |
| `postcode` | string | Four digit postcode, as text. |
| `phone` | string | Main phone number with area code. |
| `email` | string | General council email. Null for 6 rows. |
| `website` | string | Council website (https). |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `geocode_quality` | string | How the map point was placed. |
| `source` | string | `nsw_office_of_local_government`, `vic_local_government_victoria`, `qld_dept_local_government`, `wa_mycouncil`, `lga_sa`, `lgat_and_tec` or `council_website`. |
| `source_as_of` | string | Date of the source list, `YYYY-MM-DD`. |

Notes:

* `geocode_quality` tells you how close the map point is to the office:
  * `council_office_poi` (43): OpenStreetMap has the council office itself.
  * `address_exact` (194): the street number and street were matched.
  * `street` (288): the street was matched but not the number, so the point is on the right street but may be a short way off.
  * `locality_centroid` (16): only the suburb or town was matched, so the point is its center. Use these with care.
* The first digit of `lga_code` is the ABS state code: 1 NSW, 2 VIC, 3 QLD, 4 SA, 5 WA, 6 TAS, 7 NT. The only exception is OT: Christmas Island (51710) and Cocos (Keeling) Islands (51860) use WA style `5xxxx` codes, because they are run under WA law.
* QLD includes Weipa Town Authority (37300). SA includes the two Aboriginal lands authorities, Anangu Pitjantjatjara Yankunytjatjara (40250) and Maralinga Tjarutja (44000), which the ABS counts as areas.
* Each row is the main office only. Many councils have branch offices, depots and libraries that are not listed.
* Contact facts were taken from each state's list on the date in `source_as_of`. The QLD list is dated 2025-12-02. The 18 NT rows, 6 QLD rows and 1 SA row come from the council's own website (`source=council_website`). Phone numbers, emails and addresses can change.

## Examples

Two filters together. Councils in Tasmania with "hobart" in the name. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/au_local_councils" --data-urlencode "state=TAS" --data-urlencode "council_name=*hobart*"
```

```json
[
  {
    "__id": 1048,
    "lga_code": "62810",
    "lga_name": "Hobart",
    "council_name": "Hobart City Council",
    "council_type": "city",
    "state": "TAS",
    "office_address": "16 Elizabeth Street",
    "suburb": "Hobart",
    "postcode": "7000",
    "phone": "03 6238 2711",
    "email": "coh@hobartcity.com.au",
    "website": "https://www.hobartcity.com.au",
    "latitude": -42.883597,
    "longitude": 147.331275,
    "geocode_quality": "address_exact",
    "source": "lgat_and_tec",
    "source_as_of": "2026-09-07"
  }
]
```

A latitude and longitude range. NSW council offices in a box around central and eastern Sydney. This call returns 9 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/au_local_councils" --data-urlencode "state=NSW" --data-urlencode "latitude=-34..-33.8" --data-urlencode "longitude=151.15..151.3" | jq -r '.[] | [.council_name, .suburb, .postcode, .latitude, .longitude] | @tsv'
```

```text
City of Canada Bay Council	Drummoyne	2047	-33.850566	151.152656
Hunter's Hill Council	Hunters Hill	2110	-33.83482	151.154116
Lane Cove Municipal Council	Lane Cove	2066	-33.812841	151.171256
Mosman Municipal Council	Spit Junction	2088	-33.824482	151.239734
North Sydney Council	North Sydney	2060	-33.83446	151.206919
Randwick City Council	Randwick	2031	-33.910914	151.242464
City of Sydney	Sydney	2000	-33.87296	151.205446
Waverley Council	Bondi Junction	2022	-33.892167	151.246143
Woollahra Municipal Council	Double Bay	2028	-33.872623	151.247632
```

A postcode with a leading zero. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/au_local_councils" --data-urlencode "state=NT" --data-urlencode "postcode=0800" | jq -r '.[] | [.council_name, .postcode, .phone] | @tsv'
```

```text
City of Darwin	0800	08 8930 0300
```

## Data source

The list of councils and their area codes follows the Australian Bureau of Statistics Local Government Areas, ASGS Edition 4, 2026. Contact facts for each council come from its state's official list: the NSW Office of Local Government, Local Government Victoria, the Queensland Department of Local Government, Water and Volunteers, WA MyCouncil, the Local Government Association of South Australia (LGA SA), and for Tasmania the Local Government Association of Tasmania (LGAT) and the Tasmanian Electoral Commission. NT councils and a few others were checked on the council's own website. Only facts were taken (names, addresses, phones, emails and links). Map points come from OpenStreetMap. Council type, geocode quality and the source fields are set by us, and the data can go out of date. Check with the council.

Credit lines:

* Contains Queensland local government data © State of Queensland (Department of Local Government, Water and Volunteers), CC BY 3.0 AU (https://creativecommons.org/licenses/by/3.0/au/).
* Contains Victorian local government data © State Government of Victoria, CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/).
* Local Government Area codes and names: © Australian Bureau of Statistics, ASGS Edition 4, LGA 2026, CC BY 4.0 (https://www.abs.gov.au).
* Coordinates: © OpenStreetMap contributors, ODbL (https://www.openstreetmap.org/copyright).
* Contact facts only from the NSW Office of Local Government, LGA SA, LGAT, the Tasmanian Electoral Commission, WA MyCouncil (© State of Western Australia, facts only) and council websites.
* Changes were made: rows joined across sources, text tidied, council type and map points added.
* Not affiliated with or endorsed by any council, government agency or local government association. Check with the council before you visit or write.

[Back to the API list](../../README.md)
