# UK HGV Test Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 482](https://img.shields.io/badge/records-482-lightgrey) ![updated 2026-11-05](https://img.shields.io/badge/updated-2026--11--05-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

All 482 HGV, bus and trailer MOT test centres in Great Britain found on the DVSA test centre finder on 5 November 2026, with address, phone, website, booking availability, what each centre can and cannot test (HGV, PSV, trailers, LEZ phase 3, ADR category) and a map point from the postcode. Filter by the required `region`, then by postcode area, town, council area, centre name, test type or a latitude and longitude range.

Rows by region: East of England (68), Scotland (57), South East (52), Yorkshire and The Humber (51), West Midlands (50), East Midlands (45), North West (45), South West (44), Wales (37), North East (18) and London (15). England (388), Scotland (57), Wales (37). Northern Ireland is not covered; its HGV tests are run by DVA, not DVSA. The list was collected through postcode searches on the DVSA finder; there is no official download to check it against.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_hgv_test_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 21 fields. Disallowed: 5 fields.

`region` must be in every call. Use `region=*` to list all 482 centres. Disallowed means the field is returned in the output but cannot be used as a filter. Without `region` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | Region of England, or Scotland or Wales: East of England (68), Scotland (57), South East (52), Yorkshire and The Humber (51), West Midlands (50), East Midlands (45), North West (45), South West (44), Wales (37), North East (18) or London (15). Wildcards work, so `region=*` lists all 482 rows and `region=*East*` gives the 4 regions with East in the name (183). |
| `centre_name` | Optional | Yes | No | Test centre name as listed by DVSA, such as `*Volvo*` (17) or `BLS Truck & Van Limited - Aberdeen`. Wildcards work. Not unique: a few companies use the same name at more than one site. |
| `address_line_1` | Optional | Yes | No | First address line. Wildcards work. Blank on 2 rows. |
| `address_line_2` | Optional | Yes | No | Second address line, often the estate or district. Wildcards work. Blank on 163 rows. |
| `town` | Optional | Yes | No | Town as DVSA gives it, such as `Bristol` (6), `Birmingham` (6) or `Glasgow` (5). Wildcards work. For council areas use `admin_district`. |
| `postcode` | Optional | Yes | No | Full postcode with a single space, such as `AB12 3LE`, or a start like `PE11*`. Wildcards work. A few postcodes are shared by two centres on the same estate. |
| `phone` | Optional | Yes | No | Phone number as DVSA gives it, as text; formats vary and a few cells hold two numbers or opening notes. Blank on 1 row. Wildcards work. |
| `website` | Optional | Yes | No | Centre website from the DVSA listing, switched to https where the site answers on https (224); 73 stay http because https did not respond. Blank on 185 rows. Wildcards work. |
| `availability` | Optional | No | No | Exact booking status the centre last gave DVSA: `Tests available` (122), `Fully booked` (54) or `No information` (306). |
| `hgv` | Optional | No | No | `Y` if the centre lists goods vehicles (HGV) as tested (415). Blank where the centre describes what it tests in its own words instead of the standard DVSA wording; see `tested_at_centre`. |
| `psv` | Optional | No | No | `Y` if the centre lists public service vehicles (PSV) or diesel PSVs as tested (418). Blank where the centre uses its own wording. |
| `trailers` | Optional | No | No | `Y` if the centre tests trailers (410), `N` if it lists trailers as not tested (4). Blank where the centre uses its own wording. |
| `lez_phase_3` | Optional | No | No | `Y` if the centre can do London Low Emission Zone phase 3 checks (345), `N` if it says it cannot (99). Blank where the centre uses its own wording. |
| `adr_category` | Optional | No | No | Highest DVSA category of ADR and dangerous goods vehicles the centre tests: `green` (269, purged vehicles only), `amber` (158) or `red` (12, including restricted vehicles). Blank where the centre uses its own wording. |
| `tested_at_centre` | Optional | Yes | No | Everything the centre says it can test, in DVSA or the centre's own words, joined with semicolons; includes limits such as maximum door height. Use wildcards, such as `*low floor*`. |
| `not_tested_at_centre` | Optional | Yes | No | Everything the centre says it cannot test, joined with semicolons, such as petrol public service vehicles. Use wildcards, such as `*petrol*` (164). Blank on 222 rows. |
| `outcode` | Optional | No | No | Exact first half of the postcode, such as `PE11` or `IV1`. |
| `postcode_area` | Optional | No | No | Exact postcode area letters, such as `IP` (Ipswich, 15), `PE` (Peterborough, 13) or `B` (Birmingham, 12); 106 areas. |
| `admin_district` | Optional | Yes | No | Council area from the postcode (ONS via postcodes.io), such as `East Suffolk` (8), `Thurrock` (7) or `Highland` (6). Wildcards work. |
| `country` | Optional | No | No | Exact country: `England` (388), `Scotland` (57) or `Wales` (37). |
| `latitude` | Optional | No | Yes | Latitude of the postcode in decimal degrees (WGS 84), from 50.244283 to 60.169353. Use a range, for example `latitude=57..61` for the north of Scotland and the islands (11 centres). |
| `longitude` | Optional | No | Yes | Longitude of the postcode in decimal degrees (WGS 84), from -6.343682 to 1.717374. Use a range, for example `latitude=51.3..51.7&longitude=-0.5..0.3` for Greater London and its edge (26 centres). |
| `available_before` | Disallowed | No | No | For centres showing Tests available, the date before which they say they can test, `YYYY-MM-DD`. Blank on 306 rows. |
| `availability_updated` | Disallowed | No | No | Date the centre last updated its availability on the DVSA service, `YYYY-MM-DD`. Blank on 46 rows. |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from: `postcode` (475) or `postcode_terminated` (7). |
| `source` | Disallowed | No | No | Data source page. |
| `updated` | Disallowed | No | No | Date the list was read from the DVSA service, `YYYY-MM-DD` (`2026-11-05`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `region` returns HTTP 400 with `Your request is missing the following required parameters: region. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.
* Availability changes daily; check with the centre before you book.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `centre_name` | string | Test centre name. |
| `address_line_1` | string | First address line. Null on 2 rows. |
| `address_line_2` | string | Second address line. Null on 163 rows. |
| `town` | string | Town. |
| `postcode` | string | Full postcode. |
| `phone` | string | Phone number. Null on 1 row. |
| `website` | string | Centre website. Null on 185 rows. |
| `availability` | string | Booking status. |
| `available_before` | string | Earliest available date when given. Null on most rows. |
| `availability_updated` | string | Date availability was last updated. Null on some rows. |
| `hgv` | string | `Y` if HGV tested, else blank. |
| `psv` | string | `Y` if PSV tested, else blank. |
| `trailers` | string | `Y`, `N` or blank. |
| `lez_phase_3` | string | `Y`, `N` or blank. |
| `adr_category` | string | `green`, `amber`, `red` or blank. |
| `tested_at_centre` | string | What the centre can test. |
| `not_tested_at_centre` | string | What the centre cannot test. Null on 222 rows. |
| `outcode` | string | First half of the postcode. |
| `postcode_area` | string | Postcode area letters. |
| `admin_district` | string | Council area. |
| `region` | string | Region of England, or Scotland or Wales. |
| `country` | string | England, Scotland or Wales. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the DVSA finder. |
| `updated` | string | Date the list was read, `YYYY-MM-DD`. |

## Examples

Volvo centres. This call returns 17 rows (one shown):

```bash
curl -sG "https://api.jael.ee/JLEE/uk_hgv_test_centres" --data-urlencode "region=*" --data-urlencode "centre_name=*Volvo*"
```

```json
[
  {
    "__id": 22,
    "centre_name": "Volvo Trucks - Leicester",
    "address_line_1": "Interlink Way East Interlink Business Park",
    "address_line_2": "Bardon Hill Coalville",
    "town": "Leicester",
    "postcode": "LE67 1LD",
    "phone": "01530 277277",
    "website": null,
    "availability": "No information",
    "available_before": null,
    "availability_updated": "2021-05-14",
    "hgv": "Y",
    "psv": "Y",
    "trailers": "Y",
    "lez_phase_3": "Y",
    "adr_category": "green",
    "tested_at_centre": "goods vehicles (HGV); diesel public service vehicles (PSV); trailers; LEZ phase 3; purged ADR and dangerous goods vehicles - green category",
    "not_tested_at_centre": "petrol public service vehicles",
    "outcode": "LE67",
    "postcode_area": "LE",
    "admin_district": "North West Leicestershire",
    "region": "East Midlands",
    "country": "England",
    "latitude": 52.697987,
    "longitude": -1.339435,
    "geocode_quality": "postcode",
    "source": "https://find-test-centre-hgv-bus-trailer-mot.service.gov.uk/",
    "updated": "2026-11-05"
  }
]
```

Scotland. This call returns 57 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_hgv_test_centres?region=Scotland" | jq length
```

Tests available. This call returns 122 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_hgv_test_centres?region=*&availability=Tests%20available" | jq length
```

Map box over Greater London and its edge. This call returns 26 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_hgv_test_centres" --data-urlencode "region=*" --data-urlencode "latitude=51.3..51.7" --data-urlencode "longitude=-0.5..0.3" | jq length
```

## Data source

The centres come from the Driver and Vehicle Standards Agency (DVSA), [Find a test centre for an HGV, bus or trailer MOT](https://find-test-centre-hgv-bus-trailer-mot.service.gov.uk/), read on 5 November 2026. Map points and council areas come from postcodes.io (ONS Postcode Directory). Published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited.

Changes: columns were renamed, emails were left out, websites were switched to https where available, dates were reformatted, test flags were derived from the standard DVSA wording, and outcode, postcode area, council area, region, country and map points were added from the postcode. Availability changes daily; check with the centre. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: Driver and Vehicle Standards Agency (DVSA), Find a test centre for an HGV, bus or trailer MOT (https://find-test-centre-hgv-bus-trailer-mot.service.gov.uk/), read 5 November 2026.
* Map points and council areas from postcodes.io (ONS Postcode Directory). Contains OS data (c) Crown copyright and database right; contains Royal Mail data (c) Royal Mail copyright and database right; source: Office for National Statistics licensed under the Open Government Licence v3.0.
* Not endorsed by DVSA or ONS.

[Back to the API list](../../README.md)
