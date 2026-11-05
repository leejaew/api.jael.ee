# UK MOT Test Stations

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 22988](https://img.shields.io/badge/records-22988-lightgrey) ![updated 2026-09-30](https://img.shields.io/badge/updated-2026--09--30-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

All 22,988 active MOT test stations in Great Britain as listed by DVSA on 30 September 2026, with trading name, address, phone, the vehicle classes each garage can test (1, 2, 3, 4, 5 and 7) and a map point from the postcode. Filter by the required `region`, then by postcode area, town, council area, garage name, test class or a latitude and longitude range.

Rows by region: South East (3190), North West (2602), South West (2601), East of England (2367), West Midlands (2202), Yorkshire and The Humber (2180), Scotland (2099), East Midlands (1938), London (1464), Wales (1390) and North East (955). England (19499), Scotland (2099), Wales (1390). Northern Ireland is not covered; its MOTs are run by DVA, not DVSA. Class 4 (cars and small vans): 22,118. Class 7 (light goods vehicles): 6,814. Class 1 and 2 (motorbikes): 3,225 each.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_mot_test_stations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 20 fields. Disallowed: 3 fields.

`region` must be in every call. Use `region=*` to list all 22,988 stations. Disallowed means the field is returned in the output but cannot be used as a filter. Without `region` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges. Site numbers are text, so leading zeros stay.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | Region of England, or Scotland or Wales: South East (3190), North West (2602), South West (2601), East of England (2367), West Midlands (2202), Yorkshire and The Humber (2180), Scotland (2099), East Midlands (1938), London (1464), Wales (1390) or North East (955). Wildcards work, so `region=*` lists all 22988 rows and `region=*East*` gives the 4 regions with East in the name (8450). |
| `site_number` | Optional | No | No | Exact DVSA site number of the test station, such as `0003AW` or `110119`. Text, so leading zeros stay. Not unique: 14 site numbers appear on 2 or 3 rows because DVSA lists more than one trading name at the same site. |
| `trading_name` | Optional | Yes | No | Garage trading name as DVSA gives it, mostly in capitals, such as `HALFORDS*` (488) or `KWIK FIT*` (217). Wildcards work. |
| `address_line_1` | Optional | Yes | No | First address line (building or street), such as `*INDUSTRIAL ESTATE*`. Wildcards work. |
| `address_line_2` | Optional | Yes | No | Second address line. Wildcards work. Blank on 7456 rows. |
| `address_line_3` | Optional | Yes | No | Third address line, often the district. Wildcards work. Blank on 17657 rows. |
| `town` | Optional | Yes | No | Post town as DVSA gives it, mostly in capitals but some in mixed case, such as `LONDON` (593), `BIRMINGHAM` (321) or `Leeds` (215). Wildcards work. For council areas use `admin_district`. |
| `postcode` | Optional | Yes | No | Full postcode with a single space, such as `S17 4QW`, or a start like `S17*`. Wildcards work. On 15 rows an obvious typo in the DVSA postcode was fixed. |
| `phone` | Optional | Yes | No | Phone number as DVSA gives it, as text; formats vary and a few cells hold two numbers. Wildcards work. |
| `class_1` | Optional | No | No | `Y` if the station can test class 1 vehicles (motorbikes up to 200cc), `N` if not. `Y` on 3225 rows. |
| `class_2` | Optional | No | No | `Y` if the station can test class 2 vehicles (all motorbikes), `N` if not. `Y` on 3225 rows. |
| `class_3` | Optional | No | No | `Y` if the station can test class 3 vehicles (3 wheeled vehicles up to 450kg unladen), `N` if not. `Y` on 830 rows. |
| `class_4` | Optional | No | No | `Y` if the station can test class 4 vehicles (cars, small vans and passenger vehicles up to 8 seats), `N` if not. `Y` on 22118 rows. |
| `class_5` | Optional | No | No | `Y` if the station can test class 5 vehicles (private passenger vehicles with more than 12 seats and ambulances), `N` if not. `Y` on 3983 rows. |
| `class_7` | Optional | No | No | `Y` if the station can test class 7 vehicles (goods vehicles over 3,000kg up to 3,500kg design gross weight), `N` if not. `Y` on 6814 rows. |
| `outcode` | Optional | No | No | Exact first half of the postcode, such as `S17` or `BS16` (2430 different outcodes). |
| `postcode_area` | Optional | No | No | Exact postcode area letters, such as `B` (Birmingham, 629), `LS` (Leeds, 254) or `CF` (Cardiff, 386); 120 areas. |
| `admin_district` | Optional | Yes | No | Council area from the postcode (ONS via postcodes.io), such as `Leeds` (243), `Cardiff` (133) or `North Yorkshire` (337). Wildcards work. |
| `country` | Optional | No | No | Exact country: `England` (19499), `Scotland` (2099) or `Wales` (1390). |
| `latitude` | Optional | No | Yes | Latitude of the postcode in decimal degrees (WGS 84), from 49.985306 to 60.387632. Use a range, for example `latitude=58..61` for the far north of Scotland and the islands (53 stations). |
| `longitude` | Optional | No | Yes | Longitude of the postcode in decimal degrees (WGS 84), from -7.371576 to 1.757953. Use a range, for example `latitude=51.49..51.53&longitude=-0.15..-0.07` for central London (7 stations). |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from: `postcode` (22498, the live postcode center), `postcode_terminated` (430, the last known center of a retired postcode), `postcode_corrected` (15, after fixing an obvious typo) or `outcode_centroid` (45, the center of the postcode district). |
| `source` | Disallowed | No | No | Data source page. |
| `updated` | Disallowed | No | No | Date DVSA last updated the list, `YYYY-MM-DD` (`2026-09-30`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `region` returns HTTP 400 with `Your request is missing the following required parameters: region. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `site_number` | string | DVSA site number. Not unique on 14 values. |
| `trading_name` | string | Garage trading name. |
| `address_line_1` | string | First address line. |
| `address_line_2` | string | Second address line. Null on many rows. |
| `address_line_3` | string | Third address line. Null on many rows. |
| `town` | string | Post town. |
| `postcode` | string | Full postcode. |
| `phone` | string | Phone number. |
| `class_1` | string | `Y` or `N`. |
| `class_2` | string | `Y` or `N`. |
| `class_3` | string | `Y` or `N`. |
| `class_4` | string | `Y` or `N`. |
| `class_5` | string | `Y` or `N`. |
| `class_7` | string | `Y` or `N`. |
| `outcode` | string | First half of the postcode. |
| `postcode_area` | string | Postcode area letters. |
| `admin_district` | string | Council area. |
| `region` | string | Region of England, or Scotland or Wales. |
| `country` | string | England, Scotland or Wales. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the DVSA list. |
| `updated` | string | List date, `YYYY-MM-DD`. |

## Examples

Halfords Autocentres. This call returns 488 rows (one shown):

```bash
curl -sG "https://api.jael.ee/JLEE/uk_mot_test_stations" --data-urlencode "region=*" --data-urlencode "trading_name=HALFORDS*"
```

```json
[
  {
    "__id": 14,
    "site_number": "0026BB",
    "trading_name": "HALFORDS AUTOCENTRE",
    "address_line_1": "16-18 PEEBLES STREET",
    "address_line_2": null,
    "address_line_3": null,
    "town": "AYR",
    "postcode": "KA8 8DR",
    "phone": "01292281415",
    "class_1": "N",
    "class_2": "N",
    "class_3": "N",
    "class_4": "Y",
    "class_5": "N",
    "class_7": "N",
    "outcode": "KA8",
    "postcode_area": "KA",
    "admin_district": "South Ayrshire",
    "region": "Scotland",
    "country": "Scotland",
    "latitude": 55.469888,
    "longitude": -4.629738,
    "geocode_quality": "postcode_terminated",
    "source": "https://www.gov.uk/government/publications/active-mot-test-stations",
    "updated": "2026-09-30"
  }
]
```

Class 7 stations in Scotland. This call returns 651 stations that can test class 7 vehicles:

```bash
curl -s "https://api.jael.ee/JLEE/uk_mot_test_stations?region=Scotland&class_7=Y" | jq length
```

Exact site number. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_mot_test_stations" --data-urlencode "region=*" --data-urlencode "site_number=0003AW" | jq -r '.[].trading_name'
```

Map box over central London. This call returns 7 stations:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_mot_test_stations" --data-urlencode "region=*" --data-urlencode "latitude=51.49..51.53" --data-urlencode "longitude=-0.15..-0.07" | jq length
```

## Data source

The stations come from the Driver and Vehicle Standards Agency (DVSA), [Active MOT test stations](https://www.gov.uk/government/publications/active-mot-test-stations), list updated 30 September 2026, fetched on 5 November 2026. Map points and council areas come from postcodes.io (ONS Postcode Directory). Published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited.

Changes: columns were renamed, spaces in postcodes were normalized, 15 postcode typos were fixed, and outcode, postcode area, council area, region, country and map points were added from the postcode. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: Driver and Vehicle Standards Agency (DVSA), Active MOT test stations (https://www.gov.uk/government/publications/active-mot-test-stations), list updated 30 September 2026, fetched 5 November 2026.
* Map points and council areas from postcodes.io (ONS Postcode Directory). Contains OS data (c) Crown copyright and database right; contains Royal Mail data (c) Royal Mail copyright and database right; source: Office for National Statistics licensed under the Open Government Licence v3.0.
* Not endorsed by DVSA or ONS.

[Back to the API list](../../README.md)
