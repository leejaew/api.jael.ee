# US CDL Training Locations

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 5782](https://img.shields.io/badge/records-5782-lightgrey) ![updated 2026-11-03](https://img.shields.io/badge/updated-2026--11--03-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

All 5,782 active training locations open to the public in the FMCSA Training Provider Registry, where schools and trainers sign up to give entry level driver training for a commercial driver's license (CDL). Each row has how the training is given, the provider and location name, phone, website, address, city, state, ZIP code, the date the provider last updated it, Yes or No flags for 13 kinds of training (Class A, Class B, passenger, school bus and hazmat) and a map point for in person sites. Filter by the required `state`, then by location type, provider, city, ZIP code, last update date, training flags or a coordinate range.

Rows by location type: In-Person 4,293 (a fixed site), Traveling 782 (the trainer comes to the student inside one state) and Online 707 (theory only). Most rows: Online 707, California 544, Texas 425, New Jersey 217, Indiana 196, New York 191, Pennsylvania 159 and Florida 156. The `state` field holds 63 values: 50 states, DC, territories, two Armed Forces areas and `Online`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_cdl_training_locations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 24 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 5,782 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `state` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

ZIP codes, phones and dates are text, so leading zeros stay, such as `zip=06001`. Only `latitude` and `longitude` take ranges. For dates use a wildcard, such as `last_updated=2026-10*`.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory where the training is given, full name, such as `California` (544), `Texas` (425) or `North*` (158: North Carolina, North Dakota and Northern Mariana Islands). Online courses use `Online` (707). Wildcards work, so `state=*` lists all 5,782 rows. |
| `provider_name` | Optional | Yes | No | Training provider name as registered with FMCSA, such as `*college*` (602), `*school district*` (333) or `*Truck Driving*` (248). Null on 49 rows: 6 are blank in the source and 43 held the name of a private person, which was removed. |
| `city` | Optional | Yes | No | City of an in person location, such as `Houston` (31 in Texas). Spelling and capital letters are as the source gives them. Null on 1,489 rows (all traveling and online rows). |
| `zip` | Optional | Yes | No | ZIP code of an in person location, 5 digits or ZIP+4, such as `77*` (82, Houston area) or `06001` (1). Text, so leading zeros stay. Null on 1,489 rows. |
| `last_updated` | Optional | Yes | No | Date the provider last updated this location in the registry, `YYYY-MM-DD`, from `2021-07-01` to `2026-11-03`. Text, so use a wildcard, such as `2026*` (2,120) or `2026-10*` (413). |
| `location_type` | Optional | No | No | Exact: `In-Person` (4,293), `Traveling` (782) or `Online` (707). |
| `location_name` | Optional | No | No | Name of the training location as the provider listed it. Null on 23 rows where the source gave a private person's name instead of a place. |
| `phone` | Optional | No | No | Phone number as the source gives it, such as `+1 860-357-3776`. Text. Null on 715 rows. |
| `website` | Optional | No | No | Provider website as listed in the source. Null on 2,910 rows. |
| `address` | Optional | No | No | Street address of an in person location. Null on 2,081 rows: 1,489 traveling and online rows and 592 in person rows where the source gives only a city and ZIP code. |
| `class_a_theory` | Optional | No | No | `Yes` if the location teaches Class A theory (classroom), else `No`. `Yes` on 3,812 rows. |
| `class_a_range` | Optional | No | No | `Yes` if the location gives Class A behind the wheel training on a range, else `No`. `Yes` on 3,553 rows. |
| `class_a_road` | Optional | No | No | `Yes` if the location gives Class A behind the wheel training on public roads, else `No`. `Yes` on 3,557 rows, 309 of them in California. |
| `class_b_theory` | Optional | No | No | `Yes` if the location teaches Class B theory, else `No`. `Yes` on 4,175 rows. |
| `class_b_range` | Optional | No | No | `Yes` if the location gives Class B range training, else `No`. `Yes` on 4,009 rows. |
| `class_b_road` | Optional | No | No | `Yes` if the location gives Class B public road training, else `No`. `Yes` on 4,025 rows. |
| `passenger_theory` | Optional | No | No | `Yes` if the location teaches passenger (P) endorsement theory, else `No`. `Yes` on 3,219 rows. |
| `passenger_range` | Optional | No | No | `Yes` if the location gives passenger endorsement range training, else `No`. `Yes` on 2,944 rows. |
| `passenger_road` | Optional | No | No | `Yes` if the location gives passenger endorsement public road training, else `No`. `Yes` on 2,976 rows. |
| `school_bus_theory` | Optional | No | No | `Yes` if the location teaches school bus (S) endorsement theory, else `No`. `Yes` on 2,726 rows. |
| `school_bus_range` | Optional | No | No | `Yes` if the location gives school bus endorsement range training, else `No`. `Yes` on 2,438 rows. |
| `school_bus_road` | Optional | No | No | `Yes` if the location gives school bus endorsement public road training, else `No`. `Yes` on 2,472 rows. |
| `hazmat_theory` | Optional | No | No | `Yes` if the location teaches hazardous materials (H) endorsement theory, else `No`. `Yes` on 2,691 rows. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 19.537563 to 67.375751. Example: `latitude=41.6..42.1&longitude=-88.0..-87.5` for the Chicago area (40). Null on 1,505 rows. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -159.667851 to -67.217529. Use it with `latitude` to search a box. Null on 1,505 rows. |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from. See the notes. |
| `source` | Disallowed | No | No | Data source for the row, `https://tpr.fmcsa.dot.gov/content/Locations/Location-List-20261103_1901.xlsx` (FMCSA Training Provider Registry, registered locations list). US public domain. |
| `updated` | Disallowed | No | No | Date of the source list, `2026-11-03` on every row. Text. |

* Parameters are combined with AND. Text matches ignore case, so `state=texas` works the same as `state=Texas`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on `geocode_quality`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source`.
* Wildcards work only on `state`, `provider_name`, `city`, `zip` and `last_updated`. A wildcard on another field, such as `location_type=Trav*`, returns `[]`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row number, unique for each row. Use it as the key. |
| `location_type` | string | `In-Person`, `Traveling` or `Online`. |
| `provider_name` | string | Training provider name. Null for 49 rows. |
| `location_name` | string | Training location name. Null for 23 rows. |
| `phone` | string | Phone number. Null for 715 rows. |
| `website` | string | Provider website. Null for 2,910 rows. |
| `address` | string | Street address. Null for 2,081 rows. |
| `city` | string | City. Null for 1,489 rows. |
| `state` | string | State or territory full name, or `Online`. |
| `zip` | string | ZIP code, 5 digits or ZIP+4. Null for 1,489 rows. |
| `last_updated` | string | Date the provider last updated the location, `YYYY-MM-DD`. |
| `class_a_theory` to `hazmat_theory` | string | 13 training flags, each `Yes` or `No`. |
| `latitude` | number | Latitude (WGS 84). Null for 1,505 rows. |
| `longitude` | number | Longitude (WGS 84). Null for 1,505 rows. |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the source list. |
| `updated` | string | Date of the source list, `YYYY-MM-DD`. |

Notes:

* `__id` is included because no source field is unique. One provider can list the same site many times, and a traveling trainer has one row for each state it serves.
* Map points: `geocode_quality` shows where each point comes from, best first: `census_exact` (2,412, exact address match in the US Census geocoder), `census_nonexact` (620, close Census match), `zip_centroid` (1,245, middle of the ZIP code area from the Census ZCTA gazetteer, used when there is no street or no address match), `none` (16, ZIP code not found, so no point) and `no_fixed_address` (1,489, traveling and online rows, no point). That makes 1,505 rows with null `latitude` and `longitude`.
* Privacy: the source lists some private people who train on their own. 88 rows for such trainers with no business name were dropped, 66 personal names were blanked (43 provider names and 23 location names), and email addresses were removed.
* Only active locations open to the public are kept.

## Examples

Range on latitude and longitude with the required wildcard. Locations in the Chicago area. This call returns 40 rows; the first 4:

```bash
curl -sG "https://api.jael.ee/JLEE/us_cdl_training_locations" --data-urlencode "state=*" --data-urlencode "latitude=41.6..42.1" --data-urlencode "longitude=-88.0..-87.5" | jq -r '.[] | [.provider_name, .city, .latitude, .longitude] | @tsv'
```

```text
Experior Truck Driving School	Alsip	41.672676	-87.758573
Division Truck Driving School	BLUE ISLAND	41.677355	-87.668678
Echo Windy City Transportation	Broadview	41.852733	-87.862279
ALAMO TRUCK DRIVING SCHOOL iNC	CHICAGO	41.807525	-87.754838
```

One ZIP code, with the leading zero. This call returns 1 row:

```bash
curl -s "https://api.jael.ee/JLEE/us_cdl_training_locations?state=*&zip=06001"
```

```json
[
  {
    "__id": 923,
    "location_type": "In-Person",
    "provider_name": "Specialty Transportation, Inc",
    "location_name": "Specialty Transportation, Inc",
    "phone": "+1 860-357-3776",
    "website": null,
    "address": "15 Industrial Road",
    "city": "Avon",
    "state": "Connecticut",
    "zip": "06001",
    "last_updated": "2022-01-14",
    "class_a_theory": "No",
    "class_a_range": "No",
    "class_a_road": "No",
    "class_b_theory": "Yes",
    "class_b_range": "No",
    "class_b_road": "Yes",
    "passenger_theory": "Yes",
    "passenger_range": "No",
    "passenger_road": "Yes",
    "school_bus_theory": "Yes",
    "school_bus_range": "No",
    "school_bus_road": "Yes",
    "hazmat_theory": "No",
    "latitude": 41.796138,
    "longitude": -72.837073,
    "geocode_quality": "census_nonexact",
    "source": "https://tpr.fmcsa.dot.gov/content/Locations/Location-List-20261103_1901.xlsx",
    "updated": "2026-11-03"
  }
]
```

A city inside a state. Locations in Houston, Texas. This call returns 31 rows; the first 5:

```bash
curl -sG "https://api.jael.ee/JLEE/us_cdl_training_locations" --data-urlencode "state=Texas" --data-urlencode "city=Houston" | jq -r '.[] | [.location_type, .provider_name, .zip, .phone] | @tsv'
```

```text
In-Person	CDL COMMERCIAL TRUCKS TRAINING LLC	77057	+1 832-867-8087
In-Person	KCD TRAINING CDL	77530	+1 832-944-1906
In-Person	AAA CDL SCHOOL	77072-2507	+1 713-858-0044
In-Person	ACE Trucking Academy LLC	77043	+1 832-769-4523
In-Person	Alpha Trucking Academy LLC	77036-8271	+1 281-817-0750
```

A training flag. Class A public road training in California. This call returns 309 rows:

```bash
curl -s "https://api.jael.ee/JLEE/us_cdl_training_locations?state=California&class_a_road=Yes" | jq length
```

```text
309
```

## Data source

The locations, providers, phones, websites, addresses, dates and training flags come from the FMCSA [Training Provider Registry](https://tpr.fmcsa.dot.gov/), registered training locations list as of 2026-11-03, fetched on 2026-11-03. Only active locations open to the public are kept, email addresses and private person names were removed, columns were renamed, dates were changed to `YYYY-MM-DD` and map points were added. The registry can go out of date, so check with the provider before you sign up.

Credit lines:

* Source: Federal Motor Carrier Safety Administration (FMCSA), U.S. Department of Transportation, Training Provider Registry (https://tpr.fmcsa.dot.gov/), registered training locations list as of 3 November 2026. A work of the US federal government, in the US public domain under 17 U.S.C. 105.
* Coordinates: US Census Bureau geocoder (https://geocoding.geo.census.gov) and Census 2024 ZCTA Gazetteer, public domain.
* FMCSA does not endorse this API. Not affiliated with FMCSA, the US Department of Transportation or the US government.

[Back to the API list](../../README.md)
