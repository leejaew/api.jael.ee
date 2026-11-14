# US NCI Cancer Centers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 74](https://img.shields.io/badge/records-74-lightgrey) ![updated 2026-11-14](https://img.shields.io/badge/updated-2026--11--14-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

74 NCI-Designated Cancer Centers in 36 U.S. states and the District of Columbia, as listed by the National Cancer Institute on 14 November 2026. Each record has the center name, designation type (Comprehensive, Clinical or Basic Laboratory), host institution, address, city, state, phone, website, NCI profile link and a map point. No field is required, so you can filter by type, state, city, a center or institution name, or a latitude and longitude box.

The largest states are CA (10), NY (8), PA (5) and TX (4), then FL, IL and NC (3 each). 58 centers are Comprehensive, 8 are Clinical and 8 are Basic Laboratory.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_nci_cancer_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per center. All matches are returned. |

## Query parameters

Required: none. Optional: 12 fields. Disallowed: 2 fields.

A call with no filters returns all 74 records. Partial filters accept `*`, such as `center_name=*MD Anderson*` or `city=*La Jolla*`. `lat` and `lng` accept a range with two dots, such as `lat=40..41&lng=-75..-73`. `type` and `state` are exact match. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `center_name` | Optional | Yes | No | Center name as the NCI Find a Cancer Center page lists it, such as The University of Texas MD Anderson Cancer Center or Mayo Clinic Cancer Center. 74 distinct names, never blank. Partial wildcards allowed, such as `*MD Anderson*` (1) or `*Kimmel*` (2). |
| `institution` | Optional | Yes | No | Host university, hospital or institute, when the NCI list or the Find a Cancer Center page gives a name distinct from the center, such as Johns Hopkins University or University of California, San Diego. Blank on 26 rows. Partial wildcards allowed, such as `*University of California*` (5). |
| `type` | Optional | No | No | NCI designation: Comprehensive (58), Clinical (8) or Basic Laboratory (8). Exact match. |
| `address` | Optional | Yes | No | Address as the official NCI list published it, such as 1515 Holcombe Boulevard or 1 Bungtown Road. Some rows are a building name, a room code or a PO Box when that is all the list gave, such as Lokey Research Building MC 5456 or P.O. Box 19024. Partial wildcards allowed. |
| `city` | Optional | Yes | No | City from the NCI Find a Cancer Center page, such as Houston, La Jolla or New York. Never blank. Mayo Clinic is the Rochester, Minnesota campus. Partial wildcards allowed, such as `*New York*` (4) or `*La Jolla*` (3). |
| `state` | Optional | No | No | Two letter state or district code, in 37 values (36 states plus DC). The largest are CA (10), NY (8), PA (5), TX (4), FL (3), IL (3) and NC (3). Exact match. |
| `zip` | Optional | Yes | No | 5 digit ZIP as text, so leading zeros remain, such as 06510 or 02139. Blank on 17 rows that had no street-level Census match. Partial wildcards allowed, such as `0*` (5). |
| `phone` | Optional | Yes | No | Phone as the official list published it, such as (713)745-4495 or (800) KARMANOS. Blank on 1 row (Wilmot Cancer Institute). Partial wildcards allowed. |
| `profile_url` | Optional | Yes | No | NCI Find a Cancer Center profile URL, such as https://www.cancer.gov/research/infrastructure/cancer-centers/find/mdanderson. Never blank. Partial wildcards allowed, such as `*mdanderson*` (1). |
| `website` | Optional | Yes | No | Center website from the Visit Website link on the NCI profile, such as http://www.mdanderson.org/. Never blank. Partial wildcards allowed. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 21.296677 (Honolulu) to 47.619335 (Seattle). Use a range, for example `lat=40..41&lng=-75..-73` for the New York area (7). A hyphen returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -157.863288 (Honolulu) to -68.198131 (Bar Harbor). Use a range with `lat` to search a box, or `lng=-125..-114` for the West Coast (12). A hyphen returns HTTP 400. |
| `source` | Disallowed | No | No | National Cancer Institute, NCI-Designated Cancer Centers list, in all 74 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date the official NCI list was last updated, YYYY-MM-DD (`2026-11-14`). Text. Not filterable. |

## Response fields

`__id` is an integer. `lat` and `lng` are numbers. All other values are strings, or `null` when the source has no value.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. `center_name` is also unique on every row. |
| `center_name` | string | Center name as the NCI Find a Cancer Center page lists it. |
| `institution` | string | Host university, hospital or institute, or `null` on 26 rows. |
| `type` | string | `Comprehensive`, `Clinical` or `Basic Laboratory`. |
| `address` | string | Address as the official NCI list published it. |
| `city` | string | City. |
| `state` | string | Two letter state or district code. |
| `zip` | string | 5 digit ZIP as text, or `null` on 17 rows. |
| `phone` | string | Phone number, or `null` on 1 row. |
| `profile_url` | string | NCI Find a Cancer Center profile URL. |
| `website` | string | Center website. |
| `lat` | number | Latitude in decimal degrees (WGS 84). |
| `lng` | number | Longitude in decimal degrees (WGS 84). |
| `source` | string | National Cancer Institute, NCI-Designated Cancer Centers list. |
| `source_as_of` | string | Date the official NCI list was last updated, `2026-11-14`. |

## Data notes

* Designation type comes from the official NCI list dated 14 November 2026. The public center name, city, profile URL and website come from the Find a Cancer Center page (last updated 10 June 2026). Mount Sinai Tisch Cancer Center is Comprehensive on the November list; the older Find page still lists it as Clinical.
* Mayo Clinic Cancer Center is the Rochester, Minnesota campus. The Find page also notes facilities in Arizona and Florida, but those are not separate rows.
* ZIP codes keep their leading zeros. 17 rows have no ZIP because the map point is the city, not a street match.
* Addresses are kept as the official list published them. Director names and titles were left out.
* This table shows designation status only. It is not medical advice.

## Examples

A center whose name contains MD Anderson. This call returns 1 record, The University of Texas MD Anderson Cancer Center in Houston:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_nci_cancer_centers' --data-urlencode 'center_name=*MD Anderson*'
```

```json
[
  {
    "__id": 69,
    "center_name": "The University of Texas MD Anderson Cancer Center",
    "institution": null,
    "type": "Comprehensive",
    "address": "1515 Holcombe Boulevard",
    "city": "Houston",
    "state": "TX",
    "zip": "77030",
    "phone": "(713)745-4495",
    "profile_url": "https://www.cancer.gov/research/infrastructure/cancer-centers/find/mdanderson",
    "website": "http://www.mdanderson.org/",
    "lat": 29.706286,
    "lng": -95.397177,
    "source": "National Cancer Institute, NCI-Designated Cancer Centers list",
    "source_as_of": "2026-11-14"
  }
]
```

Comprehensive centers in Texas. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_nci_cancer_centers' --data-urlencode 'type=Comprehensive' --data-urlencode 'state=TX'
```

Centers in a map box around New York. This call returns 7 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_nci_cancer_centers' --data-urlencode 'lat=40..41' --data-urlencode 'lng=-75..-73'
```

## Data source

Source: [National Cancer Institute, NCI-Designated Cancer Centers list](https://cancercenters.cancer.gov/cancer-centers/cclist), dated 14 November 2026, and the [Find a Cancer Center](https://www.cancer.gov/research/infrastructure/cancer-centers/find) page. This is a United States government work and is in the public domain ([17 U.S.C. 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)). NCI's [reuse policy](https://www.cancer.gov/policies/copyright-reuse) says its text may be reused without permission and asks that you credit the National Cancer Institute as the source.

Map points come from the [US Census Bureau geocoder](https://geocoding.geo.census.gov). 57 rows use a street-level match. The other 17, mostly campus buildings and PO Boxes, use the city's Census point, and their ZIP codes are blank. La Jolla is not a Census place, so Moores uses the San Diego city point.

This table shows designation status only. It is not medical advice. This use does not imply endorsement by the National Cancer Institute. No NCI logo.
