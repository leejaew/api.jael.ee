# New Zealand sea container hubbing facilities

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 73](https://img.shields.io/badge/records-73-lightgrey) ![updated 2026-05-01](https://img.shields.io/badge/updated-2026--05--01-orange)

All 73 printed rows from the Ministry for Primary Industries (MPI) list of Approved Transitional Facilities to Hub Imported Sea Containers, May 2026, Version 7. Each row has an ATF code, facility name and city, plus a derived region and approximate city coordinates. Filter by the required `city`, then by ATF code, name, region or a coordinate range.

The list covers 14 cities. Coordinates are shared city points from OpenStreetMap/Nominatim, rounded to three decimals, not facility sites. The source PDF has no street addresses. Use these points to group results by city, not to find a yard.

There are 72 unique ATF codes. Code `2853` appears twice in Invercargill, as Freight Haulage and Freight Haulage Limited. Both printed rows are kept.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_container_hubbing_facilities` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row, with 8 fields and no `__id` |
| Record limit | None |

## Query parameters

Required: 1 field (`city`). Optional: 5 fields. Disallowed: 2 fields.

`city` must be in every call. Use `city=*` to list all 73 rows. A call without `city` returns HTTP 400. Disallowed fields are returned in the output but cannot be used as filters.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `city` | Required | Yes | No | City from MPI's list. `city=Auckland` returns 23 rows; `city=*Christchurch*` returns 16. Use `city=*` for all rows. |
| `atf_code` | Optional | No | No | Exact MPI ATF code as text, such as `atf_code=32523`. Code `2853` returns 2 rows with `city=*`. |
| `facility_name` | Optional | Yes | No | Facility name as printed. `facility_name=*Kiwirail*` returns 7 rows with `city=*`. |
| `region` | Optional | Yes | No | New Zealand region derived from city. `region=Canterbury` returns 20 rows with `city=*`. |
| `lat` | Optional | No | Yes | Approximate city latitude. `lat=-44..-43` returns 18 rows with `city=*`. |
| `lng` | Optional | No | Yes | Approximate city longitude. `lng=172..174` returns 17 rows with `city=*`. |
| `source` | Disallowed | No | No | MPI source title and edition. |
| `updated` | Disallowed | No | No | Source edition date as text: `2026-05-01`. |

Partial matching uses `*` wildcards. Filters are combined with AND, and text matches ignore case. Ranges use two dots (`min..max`). Only `lat` and `lng` accept ranges. See [How filters work](../../README.md#how-filters-work).

## Response fields

| Field | Type | Description |
|---|---|---|
| `atf_code` | string | MPI Approved Transitional Facility code. Not a unique key. |
| `facility_name` | string | Facility name from the printed list. |
| `city` | string | City from the printed list. |
| `region` | string | New Zealand region derived from city. |
| `lat` | number | Approximate city latitude, rounded to three decimals. Not a facility site. |
| `lng` | number | Approximate city longitude, rounded to three decimals. Not a facility site. |
| `source` | string | MPI source title and edition. |
| `updated` | string | Source edition date, `2026-05-01` (`YYYY-MM-DD`). |

## Examples

Auckland. This call returns 23 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_container_hubbing_facilities?city=Auckland"
```

Response excerpt, showing the first row:

```json
[
  {
    "atf_code": "31236",
    "facility_name": "Cardinal Logistics Ltd",
    "city": "Auckland",
    "region": "Auckland",
    "lat": -36.852,
    "lng": 174.763,
    "source": "MPI Approved Transitional Facilities to Hub Imported Sea Containers, May 2026",
    "updated": "2026-05-01"
  }
]
```

Facility name wildcard. This call returns 7 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_container_hubbing_facilities?city=*&facility_name=*Kiwirail*"
```

City latitude range. This call returns 18 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_container_hubbing_facilities?city=*&lat=-44..-43"
```

## Data source

Source: Crown and Ministry for Primary Industries (MPI), [Approved Transitional Facilities to Hub Imported Sea Containers](https://www.mpi.govt.nz/dmsdocument/50539-List-of-transitional-facilities-approved-for-hubbing), May 2026, Version 7, dated May 1, 2026 (printed `01/05/2026`).

The [MPI copyright page](https://www.mpi.govt.nz/about-this-site/mpi-copyright) says its material is "licensed under the Creative Commons Attribution 4.0 New Zealand licence". Full name: Creative Commons Attribution 4.0 New Zealand (CC BY 4.0 NZ). Commercial reuse is allowed with credit to the Crown and MPI. The data contains no personal information.

Changes: extracted the 73 printed rows, trimmed whitespace, added region from city, added approximate city coordinates and the `source` and `updated` fields, and retained both printed forms of ATF code `2853`.

City coordinates: © OpenStreetMap contributors, via Nominatim, licensed under the [Open Data Commons Open Database License (ODbL) 1.0](https://opendatacommons.org/licenses/odbl/1-0/). See [OpenStreetMap copyright](https://www.openstreetmap.org/copyright). ODbL allows reuse with credit and requires the same license terms for adapted databases where applicable.

[Back to the API list](../../README.md)
