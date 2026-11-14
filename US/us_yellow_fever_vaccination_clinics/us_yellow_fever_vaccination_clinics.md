# US Yellow Fever Vaccination Clinics

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 6,906](https://img.shields.io/badge/records-6%2C906-lightgrey) ![updated 2026-11-14](https://img.shields.io/badge/updated-2026--11--14-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

6,906 clinics, pharmacies and health departments certified to give the yellow fever vaccine in all 50 states, DC, Puerto Rico, Guam and the U.S. Virgin Islands, as listed in the CDC U.S. Yellow Fever Vaccination Center Registry on 14 November 2026. Each record has the site name, address, city, state, ZIP, county, phone, website, whether the site sees patients under 18, whether it serves members only, a pharmacy, health department or clinic tag and a map point. No field is required, so you can filter by state, city, county, ZIP, name, type, under 18 status or a latitude and longitude box.

The largest states are CA (1,189), TX (805), WA (442), IL (413) and NY (357). 3,393 sites are pharmacies, 3,150 are clinics and 363 are health departments. 2,416 sites say they see patients under 18.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_yellow_fever_vaccination_clinics` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per site. All matches are returned. |

## Query parameters

Required: none. Optional: 15 fields. Disallowed: 2 fields.

A call with no filters returns all 6,906 records. Partial filters accept `*`, such as `name=*Walgreens*`, `zip=941*` or `county=Bexar*`. `lat` and `lng` accept a range with two dots, such as `lat=40..41&lng=-75..-73`. `state`, `type`, `sees_under_18`, `members_only` and `geo_match` are exact match. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Site name exactly as the CDC registry lists it, such as WALGREENS #11725, Passport Health or BALDWIN COUNTY HEALTH DEPARTMENT. Never blank. Some clinics are listed under a doctor's name, as CDC shows them. Partial wildcards allowed, such as `*Walgreens*` (788), `*Passport Health*` (314) or `*Concentra*` (61). |
| `address` | Optional | Yes | No | First address line as CDC lists it, such as 22251 PALMER ST. On 456 rows this line does not start with a house number: often a site label, building name or PO Box (for example Airport San Antonio - Urgent Care), with the street in `address_2`. Never blank. Partial wildcards allowed, such as `*Suite*` (62). |
| `address_2` | Optional | Yes | No | Second address line when CDC gives one, usually a suite, floor or the street after a site label, such as STE 102 or 10200 Broadway Street, Suite 200. Blank on 4922 rows. Partial wildcards allowed, such as `STE*` or `*Floor*`. |
| `city` | Optional | Yes | No | City from the CDC city, state and ZIP line, in the case CDC used (SAN ANTONIO or San Antonio). Never blank. Matching is case insensitive, so `city=San Antonio&state=TX` returns 46. Partial wildcards allowed, such as `*New York*` or `Saint*`. |
| `state` | Optional | No | No | Two letter code in 54 values: 50 states plus DC, PR (Puerto Rico), GU (Guam) and VI (U.S. Virgin Islands). The largest are CA (1189), TX (805), WA (442), IL (413) and NY (357). Exact match. |
| `zip` | Optional | Yes | No | 5 digit ZIP as text from the CDC listing, so leading zeros remain, such as 02115 or 00918. Never blank. Partial wildcards allowed, such as `941*` (San Francisco area) or `0*` (523). |
| `county` | Optional | Yes | No | County, parish, borough or independent city as CDC lists it, such as Los Angeles County (249), Cook County (188), Harris County (179), East Baton Rouge Parish or Alexandria City. DC rows read District Of Columbia; PR, GU and VI rows use the municipio or island name (Ponce, Guam, St. Croix). Never blank. Partial wildcards allowed, such as `Bexar*`. |
| `phone` | Optional | Yes | No | Phone as CDC lists it, almost always 999-999-9999, such as 251-947-1910. Two rows carry an extension (847-985-0600, ext- 6) and one lists two numbers joined by OR. Blank on 12 rows. Partial wildcards allowed, such as `212-*` (119). |
| `website` | Optional | Yes | No | Website link from the CDC listing, such as https://www.uintacounty.com/42/Public-Health. Blank on 2520 rows. Partial wildcards allowed, such as `*walgreens.com*` (526). |
| `sees_under_18` | Optional | No | No | Yes when the CDC listing carries the note `SEES PATIENTS <18 Y/O` (2416 rows); No when the listing has no such note (4490 rows). No means CDC does not say the site sees children, not that it refuses them; call ahead. Exact match, Yes or No. |
| `members_only` | Optional | No | No | Yes when the CDC listing carries the note `MEMBERS / AFFILIATES / STAFF / STUDENTS ONLY` (272 rows, such as university health centers and health plan clinics); No otherwise (6634). Exact match, Yes or No. |
| `type` | Optional | No | No | Our tag from the site name, not a CDC field: pharmacy (3393), clinic (3150) or health_department (363). health_department means a county, city, district or state health department or public health office. pharmacy means a pharmacy, drug store or chain pharmacy such as Walgreens, Safeway, Costco, CVS, Rite Aid or Kroger. Everything else, including in store clinics such as The Little Clinic, is clinic. Exact match. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 17.999711 (Ponce, PR) to 66.895116 (Kotzebue, AK). Blank on 38 rows with no Census match. Use a range, for example `lat=40..41&lng=-75..-73` for the New York area (412). A hyphen returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -165.415287 (Nome, AK) to -66.05756 (San Juan, PR). Blank on 38 rows. Use a range with `lat` to search a box, or `lng=-125..-114` for the West Coast (1996). A hyphen returns HTTP 400. |
| `geo_match` | Optional | No | No | How the map point was found: street (6403 rows, a US Census street address match), city (465 rows, the Census point for the city because the street did not match) or blank (38 rows, no point). Exact match, such as `geo_match=street` for the most precise points. |
| `source` | Disallowed | No | No | CDC U.S. Yellow Fever Vaccination Center Registry, in all 6906 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date the CDC registry pages were read, YYYY-MM-DD (`2026-11-14`). Text. Not filterable. |

## Response fields

`__id` is an integer. `lat` and `lng` are numbers. All other values are strings, or `null` when the source has no value.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. CDC gives no site ID and names repeat, so use this as the key. |
| `name` | string | Site name exactly as the CDC registry lists it. |
| `address` | string | First address line as CDC lists it. |
| `address_2` | string | Second address line, or `null` on 4,922 rows. |
| `city` | string | City. |
| `state` | string | Two letter state or territory code. |
| `zip` | string | 5 digit ZIP as text, with leading zeros kept. |
| `county` | string | County, parish, borough, independent city, municipio or island. |
| `phone` | string | Phone as CDC lists it, or `null` on 12 rows. |
| `website` | string | Website link, or `null` on 2,520 rows. |
| `sees_under_18` | string | `Yes` or `No`. |
| `members_only` | string | `Yes` or `No`. |
| `type` | string | `pharmacy`, `clinic` or `health_department`. Our tag. |
| `lat` | number | Latitude in decimal degrees (WGS 84), or `null` on 38 rows. |
| `lng` | number | Longitude in decimal degrees (WGS 84), or `null` on 38 rows. |
| `geo_match` | string | `street` or `city`, or `null` on 38 rows with no point. Our tag. |
| `source` | string | CDC U.S. Yellow Fever Vaccination Center Registry. |
| `source_as_of` | string | Date the registry was read, `2026-11-14`. |

## Data notes

* Most sites give the vaccine by appointment only. Call ahead before you go, and check the age and members only notes.
* Names, addresses and phone numbers are kept exactly as CDC lists them, split into fields. Some clinics are listed under a doctor's name. No other personal data was added.
* `sees_under_18` is `Yes` when the CDC listing says the site sees patients under 18. `No` means the listing says nothing about children, not that the site turns them away.
* `type` and `geo_match` are our tags, not CDC fields. `type` comes from the site name only. In store clinics, such as The Little Clinic, are tagged `clinic`.
* A `geo_match=city` point is the Census point for the city, not the clinic's door. Use `geo_match=street` when you need a precise point.
* 8 listings that matched in every field were removed as duplicates. One CDC test listing filed under Palau was left out.
* ZIP codes keep their leading zeros. 523 start with 0, such as 02115 and 00918.
* This is a snapshot from 14 November 2026, refreshed monthly. It is not medical advice. CDC's registry is the final word on whether a site is certified.

## Examples

Walgreens stores in Bexar County, Texas, which includes San Antonio. This call returns 14 records. The first two:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_yellow_fever_vaccination_clinics' --data-urlencode 'name=*Walgreens*' --data-urlencode 'county=Bexar*'
```

```json
[
  {
    "__id": 5166,
    "name": "WALGREENS #11971",
    "address": "20226 STONE OAK PKWY",
    "address_2": null,
    "city": "SAN ANTONIO",
    "state": "TX",
    "zip": "78258",
    "county": "Bexar County",
    "phone": "210-481-9138",
    "website": null,
    "sees_under_18": "No",
    "members_only": "No",
    "type": "pharmacy",
    "lat": 29.64309,
    "lng": -98.480284,
    "geo_match": "street",
    "source": "CDC U.S. Yellow Fever Vaccination Center Registry",
    "source_as_of": "2026-11-14"
  },
  {
    "__id": 5167,
    "name": "Walgreens #15621",
    "address": "25215 West Interstate 10",
    "address_2": null,
    "city": "San Antonio",
    "state": "TX",
    "zip": "78257",
    "county": "Bexar County",
    "phone": "210-698-2540",
    "website": "https://www.walgreens.com/locator/walgreens-25215+w+interstate+10-san+antonio-tx-78257/id=15621?o=ac",
    "sees_under_18": "No",
    "members_only": "No",
    "type": "pharmacy",
    "lat": 29.676045,
    "lng": -98.636808,
    "geo_match": "street",
    "source": "CDC U.S. Yellow Fever Vaccination Center Registry",
    "source_as_of": "2026-11-14"
  }
]
```

Every site in San Antonio, Texas. This call returns 46 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_yellow_fever_vaccination_clinics' --data-urlencode 'state=TX' --data-urlencode 'city=San Antonio'
```

Sites in the San Francisco area that see patients under 18. This call returns 58 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_yellow_fever_vaccination_clinics' --data-urlencode 'zip=941*' --data-urlencode 'sees_under_18=Yes'
```

Sites in a map box around New York. This call returns 412 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_yellow_fever_vaccination_clinics' --data-urlencode 'lat=40..41' --data-urlencode 'lng=-75..-73'
```

## Data source

Source: [CDC U.S. Yellow Fever Vaccination Center Registry](https://wwwnc.cdc.gov/travel/yellow-fever-vaccination-clinics/search), read on 14 November 2026. The registry is free to search at [wwwnc.cdc.gov](https://wwwnc.cdc.gov/travel/yellow-fever-vaccination-clinics/search). This is a United States government work and is in the public domain ([17 U.S.C. 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)). CDC's [Use of Agency Materials](https://www.cdc.gov/other/agencymaterials.html) page says its material may be freely used or reproduced, with credit to CDC.

Use of this data does not mean CDC, HHS or the U.S. government endorses this API or its maker.

Map points come from the [US Census Bureau geocoder](https://geocoding.geo.census.gov). 6,403 rows use a street level match. 465 use the city's Census point. 38 have no point, including the Guam and U.S. Virgin Islands sites and a few airport sites.
