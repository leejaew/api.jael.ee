# US Hemophilia Treatment Centers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 143](https://img.shields.io/badge/records-143-lightgrey) ![updated 2026-11-04](https://img.shields.io/badge/updated-2026--11--04-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

All 143 hemophilia treatment centers (HTCs) and partner sites in the CDC US Hemophilia Treatment Center Directory, across 46 states, DC, Guam and Puerto Rico in 10 regions. HTCs give care to people with hemophilia and other bleeding and clotting disorders. Each row has the CDC HTC number, center name, address, city, state, ZIP code, region, the Community Counts flag, main phone, fax, after hours phone numbers for adults and children with their notes, outreach sites and a map point. Filter by the required `state`, then by region, city, center name, HTC number, Community Counts flag or a coordinate range.

Most centers: California and Michigan (11 each), Florida, New York and Ohio (8 each), Pennsylvania and Texas (7 each). Rows by region: Great Lakes 20, Great Plains 19, Mid-Atlantic 16, Southeast, Region IV-South 16, Northern States 14, Western States 14, New England, Region II 12, Mountain States 11, Southeast, Region IV-North 11 and New England, Region I 10. The directory lists no center in Kansas, Mississippi, Montana or Wyoming.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_hemophilia_treatment_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 16 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 143 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `state` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

ZIP codes, HTC numbers, phones and dates are text, so leading zeros stay, such as `htc_id=001` or ZIP `06510`. Only `latitude` and `longitude` take ranges. Some region names contain a comma, which `curl --data-urlencode` encodes for you; in a plain URL write it as `%2C`.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | Two letter state or territory code, such as `CA` (11), `MI` (11), `FL` (8), `NY` (8) or `M*` (25). Wildcards work, so `state=*` lists all 143 rows. Includes `DC`, `GU` (Guam) and `PR` (Puerto Rico). |
| `htc_id` | Optional | No | No | Exact CDC HTC number, 3 digits from `001` to `938`, such as `001` (New England Hemophilia Center). Text, so keep the leading zeros. Unique for each row. |
| `center_name` | Optional | Yes | No | Center name as CDC lists it, such as `*Children*` (35) or `*Hemophilia Treatment Center*` (19). |
| `address` | Optional | No | No | Street address as CDC lists it, often with the building, suite or department first. A few rows list more than one clinic site in this field. |
| `city` | Optional | Yes | No | City, such as `St. Louis` (4), `Philadelphia` (4), `Chicago` (3) or `*Rochester*` (2). |
| `zip` | Optional | No | No | ZIP code, 5 digits or ZIP+4, such as `06510`. Text, so leading zeros are kept. |
| `region` | Optional | No | No | Exact region of the US HTC network, one of: `Great Lakes` (20), `Great Plains` (19), `Mid-Atlantic` (16), `Southeast, Region IV-South` (16), `Western States` (14), `Northern States` (14), `New England, Region II` (12), `Mountain States` (11), `Southeast, Region IV-North` (11) or `New England, Region I` (10). Some names contain a comma; encode it as `%2C` in a URL. |
| `community_counts` | Optional | No | No | Exact: `Yes` (138) or `No` (5), as the CDC directory shows for taking part in Community Counts, the CDC data collection project for bleeding disorders. |
| `main_phone` | Optional | No | No | Main phone number, such as `(907) 917-9235`. Text. A few numbers carry an extension note such as `ext. opt.1`. |
| `fax` | Optional | No | No | Fax number. Text. Null on 6 rows. |
| `after_hours_adult_phone` | Optional | No | No | After hours phone for adult patients. Text. Null on 50 rows. |
| `after_hours_adult_note` | Optional | No | No | Instructions that go with the adult after hours number, such as `Ask for the hematologist on call.` Filled on 52 rows. Staff names were removed. |
| `after_hours_pediatric_phone` | Optional | No | No | After hours phone for children. Text. Null on 48 rows. |
| `after_hours_pediatric_note` | Optional | No | No | Instructions that go with the pediatric after hours number. Filled on 55 rows. Staff names were removed. |
| `outreach_locations` | Optional | No | No | Other towns or sites where the center offers outreach clinics, separated by semicolons, as CDC lists them. Filled on 10 rows. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 13.439368 (Guam) to 61.21107 (Anchorage). Example: `latitude=41.8..42.0&longitude=-87.75..-87.55` for central Chicago (3). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -157.834485 (Honolulu) to 144.796191 (Guam). Use it with `latitude` to search a box. |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from. See the notes. |
| `source` | Disallowed | No | No | Data source for the row, `https://dbdgateway.cdc.gov/HTCDirSearch.aspx` (CDC, US Hemophilia Treatment Center Directory). US public domain. |
| `updated` | Disallowed | No | No | Date the directory was fetched from CDC, `2026-11-04` on every row. Text. |

* Parameters are combined with AND. Text matches ignore case, so `state=ca` works the same as `state=CA`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on `geocode_quality`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source`.
* Wildcards work only on `state`, `center_name` and `city`. A wildcard on another text field, such as `zip=0*`, returns `[]`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `htc_id` | string | CDC HTC number, 3 digits. Unique for each row, so use it as the key. |
| `center_name` | string | Center name. |
| `address` | string | Street address as CDC lists it. |
| `city` | string | City. |
| `state` | string | Two letter state or territory code. |
| `zip` | string | ZIP code, 5 digits or ZIP+4. |
| `region` | string | Region of the US HTC network. |
| `community_counts` | string | `Yes` or `No`. |
| `main_phone` | string | Main phone number. |
| `fax` | string | Fax number. Null for 6 rows. |
| `after_hours_adult_phone` | string | After hours phone for adults. Null for 50 rows. |
| `after_hours_adult_note` | string | Note for the adult after hours number. Null for 91 rows. |
| `after_hours_pediatric_phone` | string | After hours phone for children. Null for 48 rows. |
| `after_hours_pediatric_note` | string | Note for the pediatric after hours number. Null for 88 rows. |
| `outreach_locations` | string | Outreach clinic sites, separated by semicolons. Null for 133 rows. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the source directory. |
| `updated` | string | Date the directory was fetched, `YYYY-MM-DD`. |

Notes:

* No `__id` field: `htc_id` is already unique for every row.
* Community Counts: the 5 rows marked `No` are partner sites that CDC lists with the centers: Children's Rehabilitation Services Birmingham Clinic, Hemophilia of Georgia, Hemophilia Foundation of Michigan, Cascade Hemophilia Consortium and Great Lakes Hemophilia Foundation.
* Addresses: CDC often puts the department or building before the street. The Brigham and Women's row lists three sites in one address; its map point is the office at 1620 Tremont St, Boston. One Dallas address reads `1935 Medical Center DR`; its map point is on Medical District Dr.
* After hours notes: three notes in the source named staff members. The names were removed. Notes are kept as CDC wrote them otherwise.
* Map points: every row has a point. `geocode_quality` shows where it comes from, best first: `census_exact` (86, US Census geocoder exact match), `census_nonexact` (38, Census match with small differences such as the ZIP), `arcgis_pointaddress` (15, ArcGIS address point), `arcgis_streetaddress` (1, ArcGIS street segment) and `arcgis_institution` (3, ArcGIS place match on the hospital or university when the address is a mail box: UNC Chapel Hill, University of Nebraska Medical Center and University of Puerto Rico). For 9 Census matches the matched ZIP differs from the listed ZIP, but the street matches.

## Examples

Range on latitude and longitude with the required wildcard. Centers in central Chicago. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_hemophilia_treatment_centers" --data-urlencode "state=*" --data-urlencode "latitude=41.8..42.0" --data-urlencode "longitude=-87.75..-87.55" | jq -r '.[] | [.htc_id, .center_name, .city, .main_phone] | @tsv'
```

```text
569	Rush University Medical Center	Chicago	(312) 942-3034
553	Northwestern Center for Bleeding Disorders	Chicago	(312) 695-0990
555	Ann & Robert H. Lurie Children's Hospital of Chicago	Chicago	(312) 227-4000
```

A wildcard on the center name. Children's centers in Texas. This call returns 2 rows; the first one in full:

```bash
curl -sG "https://api.jael.ee/JLEE/us_hemophilia_treatment_centers" --data-urlencode "state=TX" --data-urlencode "center_name=*Children*"
```

```json
[
  {
    "htc_id": "680",
    "center_name": "Children's Medical Center Bleeding and Thrombosis Program",
    "address": "Center for Cancer and Blood Disorders, 1935 Medical Center DR",
    "city": "Dallas",
    "state": "TX",
    "zip": "75235",
    "region": "Great Plains",
    "community_counts": "Yes",
    "main_phone": "(214) 456-2379",
    "fax": "(214) 456-6133",
    "after_hours_adult_phone": null,
    "after_hours_adult_note": null,
    "after_hours_pediatric_phone": "(214) 456-7000",
    "after_hours_pediatric_note": "Ask for Hem/Onc Fellow on call 214-456-7000 - 4:30PM - 8:30AM (24 HOURS)",
    "outreach_locations": null,
    "latitude": 32.808944,
    "longitude": -96.836126,
    "geocode_quality": "census_nonexact",
    "source": "https://dbdgateway.cdc.gov/HTCDirSearch.aspx",
    "updated": "2026-11-04"
  },
  ...
]
```

One region. Centers in the Southeast, Region IV-South region. This call returns 16 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_hemophilia_treatment_centers" --data-urlencode "state=*" --data-urlencode "region=Southeast, Region IV-South" | jq -r '.[] | [.state, .city, .center_name] | @tsv'
```

One center by its HTC number, with the leading zero:

```bash
curl -s "https://api.jael.ee/JLEE/us_hemophilia_treatment_centers?state=*&htc_id=001" | jq -r '.[0].center_name'
```

```text
New England Hemophilia Center at UMASS Memorial
```

## Data source

The center list, addresses, regions, Community Counts flags, phones, notes and outreach sites come from the CDC [US Hemophilia Treatment Center Directory](https://dbdgateway.cdc.gov/HTCDirSearch.aspx), fetched on 2026-11-04. The address text was split into street, city, state and ZIP, and staff names were removed from three after hours notes. Map points come from the US Census Bureau geocoder and, where it found no match, ArcGIS World Geocoding. The directory can go out of date, so call the center before you visit.

Credit lines:

* Source: CDC, US Hemophilia Treatment Center Directory (https://dbdgateway.cdc.gov/HTCDirSearch.aspx), fetched 4 November 2026. CDC material is in the US public domain under 17 U.S.C. 105 and may be used and copied without permission (https://www.cdc.gov/other/agencymaterials.html).
* Coordinates: US Census Bureau geocoder (https://geocoding.geo.census.gov), public domain, and ArcGIS World Geocoding for 19 rows.
* CDC does not endorse this API. Not affiliated with CDC, HHS or the US government.

[Back to the API list](../../README.md)
