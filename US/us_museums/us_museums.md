# US Museums

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 30176](https://img.shields.io/badge/records-30176-lightgrey) ![updated 2018](https://img.shields.io/badge/updated-2018-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

Museums and related groups in the 50 states and DC, from the Institute of Museum and Library Services (IMLS) Museum Data Files, 2018 release. There are 30,176 rows in 51 state codes, from California (2,414), New York (2,028) and Texas (1,673) down to the smallest states. Each row has the museum name and legal name, type of museum, street and mailing address, phone, website, map point, county, locale, region, EIN, nonprofit and industry codes, parent college if any, and IRS income figures. Filter by the required `state`, then by type, city, ZIP, county, income or a coordinate range.

> **Data age.** IMLS retired this data in 2018 and has no plans to update it. Some museums have closed or moved since, and some phone numbers and websites may no longer work. The IRS figures come from the May 2015 IRS Business Master File.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_museums` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 31 fields. Disallowed: 0 fields.

`state` must be in every call. Wildcards work on `state`, so `state=*` lists all 30,176 rows. That is a large reply (about 30 MB), so add at least one more filter when you can.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | 2 letter state code, 51 codes including `DC`. Most common: `CA` (2414), `NY` (2028), `TX` (1673), `PA` (1507), `OH` (1246) and `IL` (1179). `state=*` lists all rows, and `state=C*` matches CA, CO and CT. |
| `museum_id` | Optional | Yes | No | IMLS Museum Data File id (MID), such as `8400100026`. Text, not a number. One id, `8400602963X`, is used for 2 different places, so use `__id` when you need a key that is always unique. |
| `name` | Optional | Yes | No | Common name of the museum, in upper case as IMLS wrote it, such as `*ZOO*` (232 rows) or `*HISTORICAL SOCIETY*`. |
| `legal_name` | Optional | Yes | No | Legal name of the group that runs the museum, such as `*UNIVERSITY*` (917). Blank for 3 rows. |
| `alt_name` | Optional | Yes | No | Another name for the museum (also known as, or doing business as), such as `*SMITHSONIAN*` (6). Blank for 28618 rows. |
| `discipline_code` | Optional | No | No | IMLS 3 letter type code: `HSC` (14785), `GMU` (7961), `ART` (2620), `HST` (1776), `BOT` (1029), `SCI` (833), `ZAW` (465), `CMU` (438) or `NAT` (269). Exact match only. |
| `discipline` | Optional | Yes | No | Type of museum in words, such as `Art Museums`, `History Museums` or `*Zoos*`. Use wildcards for names with commas or the `&` sign. See the list below. |
| `co_located_resources` | Optional | Yes | No | Extra resources IMLS noted at the site, such as `PLANETARIUM` (110). Filled for 286 rows. |
| `address` | Optional | Yes | No | Street address IMLS used for the map point (the physical address when known, otherwise the mailing address), such as `*MAIN ST*`. Blank for 10 rows. |
| `city` | Optional | Yes | No | City in upper case, such as `CHICAGO` (162) or `CHIC*`. |
| `zip` | Optional | Yes | No | 5 digit ZIP code, stored as text so leading zeros stay, such as `02138` or `021*`. |
| `admin_address` | Optional | Yes | No | Mailing address in one line (street, city, state and ZIP), such as `*PO BOX*`. |
| `phone` | Optional | Yes | No | Phone number written as `123-456-7890`, such as `617*`. Blank for 9285 rows. |
| `website` | Optional | Yes | No | Website as listed in 2018, such as `*nps.gov*` (23). Most links are not https, and some may no longer work. Blank for 13942 rows. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, from 19.35445 to 70.66976. Example: `lat=40..41`. Blank for 52 rows. |
| `long` | Optional | No | Yes | Longitude in decimal degrees, from -166.54126 to -66.98344. Example: `long=-75..-73`. Blank for 52 rows. |
| `geocode_precision` | Optional | No | No | Where the map point came from: `imls_2018` (30107, the point in the IMLS file), `census_address` (17, a US Census Geocoder street match added for rows IMLS left without a point) or `none` (52, no point). |
| `locale` | Optional | No | No | NCES locale type of the site: `Rural` (9766), `City` (8453), `Suburb` (7007) or `Town` (4873). Blank for 77 rows. |
| `county_fips` | Optional | Yes | No | 5 digit county FIPS code (Census 2010), stored as text, such as `06037` (Los Angeles County, 438) or `060*`. Blank for 2370 rows. |
| `county` | Optional | Yes | No | County name from the Census 2010 county list, such as `Cook County`. Blank for 2370 rows with no valid county code. |
| `aam_region` | Optional | No | No | American Alliance of Museums region: `Midwest` (6763), `Southeastern` (6103), `Mid-Atlantic` (5113), `Western` (4780), `Mountain Plains` (4694) or `New England` (2720). Blank for 3 rows. |
| `bea_region` | Optional | No | No | Bureau of Economic Analysis region: `Southeast` (6102), `Mid-East` (5110), `Great Lakes` (4828), `Far West` (4022), `Plains` (3227), `Southwest` (2798), `New England` (2720) or `Rocky Mountains` (1362). Blank for 7 rows. |
| `ein` | Optional | Yes | No | 9 digit IRS Employer Identification Number, stored as text, such as `52*`. Many museums share the EIN of a parent body. Blank for 5021 rows. |
| `ntee_code` | Optional | Yes | No | NTEE nonprofit class code, such as `A51` (art museums) or `A8*` (history). Blank for 8969 rows. |
| `naics_code` | Optional | Yes | No | NAICS industry code, stored as text, such as `712110` (museums) or `7121*`. Blank for 16786 rows. |
| `ipeds_id` | Optional | Yes | No | IPEDS id of a parent college or university, stored as text, such as `166027`. Filled for 2566 rows. |
| `parent_institution` | Optional | Yes | No | Name of the parent college or university, such as `*HARVARD*` (19). Filled for 2564 rows. |
| `income_range` | Optional | Yes | No | IRS income band, from `$0` and `$1 to $9,999` up to `$50,000,000 to greater`. Use wildcards, such as `*1,000,000*` for the `$1,000,000 to $4,999,999` band (1141). Blank for 8675 rows with no IRS match. |
| `income` | Optional | No | Yes | Gross receipts in US dollars, from 0 to 83181439574. Example: `income=10000000..49999999`. Large values often belong to a parent body. Blank for 9217 rows. |
| `revenue` | Optional | No | Yes | Total revenue in US dollars from the latest IRS Form 990, from -2127393 to 5840349457. Example: `revenue=100000..500000`. Blank for 9825 rows. |
| `tax_period` | Optional | Yes | No | Tax period of that IRS return as `YYYY-MM` text, from 1999-06 to 2015-04. Use a wildcard, for example `tax_period=2014*` for all of 2014. Blank for 8943 rows. |
| `ruling_date` | Optional | Yes | No | Year and month the IRS granted tax exempt status, as `YYYY-MM` text, from 1903-10 to 2015-04. Use a wildcard, for example `ruling_date=199*` for the 1990s. Blank for 8693 rows. |

* Parameters are combined with AND. Text matches ignore case, so `state=ca` works the same as `state=CA`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* `tax_period` and `ruling_date` are text, so a range such as `tax_period=2014-01..2014-12` returns `[]`. Use a wildcard instead.
* Number fields (`lat`, `long`, `income`, `revenue`) take an exact value or a range, not a wildcard. `income=3828*` returns HTTP 400 with `Value '3828*' is not a integer`.

Discipline codes:

| Code | Discipline | Rows |
|---|---|---|
| `HSC` | Historical Societies / Historic Preservation | 14785 |
| `GMU` | Uncategorized or General Museums | 7961 |
| `ART` | Art Museums | 2620 |
| `HST` | History Museums | 1776 |
| `BOT` | Arboretums, Botanical Gardens, & Nature Centers | 1029 |
| `SCI` | Science & Technology Museums & Planetariums | 833 |
| `ZAW` | Zoos, Aquariums, & Wildlife Conservation | 465 |
| `CMU` | Children's Museums | 438 |
| `NAT` | Natural History & Natural Science Museums | 269 |

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `museum_id` | string | IMLS Museum Data File id (MID). |
| `name` | string | Common name of the museum. |
| `legal_name` | string | Legal name of the group that runs it. Null for 3 rows. |
| `alt_name` | string | Another name. Null for 28618 rows. |
| `discipline_code` | string | IMLS 3 letter type code. |
| `discipline` | string | Type of museum in words. |
| `co_located_resources` | string | Extra resources at the site. Null for 29890 rows. |
| `address` | string | Street address used for the map point. Null for 10 rows. |
| `city` | string | City, upper case. |
| `state` | string | 2 letter state code. |
| `zip` | string | 5 digit ZIP code. |
| `admin_address` | string | Mailing address in one line. |
| `phone` | string | Phone number. Null for 9285 rows. |
| `website` | string | Website as listed in 2018. Null for 13942 rows. |
| `lat` | number | Latitude in decimal degrees. Null for 52 rows. |
| `long` | number | Longitude in decimal degrees. Null for 52 rows. |
| `geocode_precision` | string | `imls_2018`, `census_address` or `none`. |
| `locale` | string | `Rural`, `City`, `Suburb` or `Town`. Null for 77 rows. |
| `county_fips` | string | 5 digit county FIPS code. Null for 2370 rows. |
| `county` | string | County name. Null for 2370 rows. |
| `aam_region` | string | American Alliance of Museums region. Null for 3 rows. |
| `bea_region` | string | Bureau of Economic Analysis region. Null for 7 rows. |
| `ein` | string | IRS Employer Identification Number. Null for 5021 rows. |
| `ntee_code` | string | NTEE nonprofit class code. Null for 8969 rows. |
| `naics_code` | string | NAICS industry code. Null for 16786 rows. |
| `ipeds_id` | string | IPEDS id of a parent college. Null for 27610 rows. |
| `parent_institution` | string | Name of the parent college. Null for 27612 rows. |
| `income_range` | string | IRS income band. Null for 8675 rows. |
| `income` | integer | Gross receipts in US dollars. Null for 9217 rows. |
| `revenue` | integer | Total revenue in US dollars. Null for 9825 rows. |
| `tax_period` | string | Tax period of the IRS return, `YYYY-MM`. Null for 8943 rows. |
| `ruling_date` | string | IRS tax exempt ruling date, `YYYY-MM`. Null for 8693 rows. |

Notes:

* IRS figures (`ein`, `income_range`, `income`, `revenue`, `tax_period`, `ruling_date`) are from 2015 and are often for a parent body, such as a university or a city, not the museum alone.
* Names, cities and addresses are in upper case, as IMLS wrote them.

## Examples

Range and exact filters together. Zoos and aquariums in California with gross receipts from $10 million to $1 billion. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=CA" --data-urlencode "discipline_code=ZAW" --data-urlencode "income=10000000..1000000000"
```

```json
[
  {
    "__id": 403,
    "museum_id": "8400601268",
    "name": "AQUARIUM OF THE PACIFIC",
    "legal_name": "AQUARIUM OF THE PACIFIC",
    "alt_name": null,
    "discipline_code": "ZAW",
    "discipline": "Zoos, Aquariums, & Wildlife Conservation",
    "co_located_resources": null,
    "address": "100 AQUARIUM WAY",
    "city": "LONG BEACH",
    "state": "CA",
    "zip": "90802",
    ...
    "lat": 33.76322,
    "long": -118.19669,
    "locale": "City",
    "county_fips": "06037",
    "county": "Los Angeles County",
    ...
    "income_range": "$10,000,000 to $49,999,999",
    "income": 38286191,
    "revenue": 33641453,
    "tax_period": "2013-12",
    "ruling_date": "1992-11"
  },
  ...
]
```

The same call as a table:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=CA" --data-urlencode "discipline_code=ZAW" --data-urlencode "income=10000000..1000000000" | jq -r '.[] | [.name, .city, .income] | @tsv'
```

```text
AQUARIUM OF THE PACIFIC	LONG BEACH	38286191
MARINE MAMMAL CENTER	SAUSALITO	10278218
MONTEREY BAY AQUARIUM	MONTEREY	137491679
SANTA BARBARA ZOO	SANTA BARBAR	19450300
```

ZIP wildcard. Museums in Massachusetts with a ZIP code that starts with `021` (Boston and nearby). This call returns 163 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=MA" --data-urlencode "zip=021*" | jq length
```

Wildcard on a text date. Museums in Massachusetts with an IRS tax period in 2014. This call returns 281 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=MA" --data-urlencode "tax_period=2014*" | jq length
```

Art museums in Illinois. This call returns 92 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=IL" --data-urlencode "discipline_code=ART" | jq length
```

All states with one more filter. Zoos, aquariums and wildlife groups in every state. This call returns 465 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=*" --data-urlencode "discipline_code=ZAW" | jq length
```

Coordinate box. Art museums in Lower Manhattan and nearby Brooklyn. This call returns 41 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_museums" --data-urlencode "state=NY" --data-urlencode "lat=40.70..40.80" --data-urlencode "long=-74.02..-73.93" --data-urlencode "discipline_code=ART" | jq length
```

## Data source

Institute of Museum and Library Services (IMLS), Museum Data Files, 2018 release. IMLS retired this data in 2018 and has no plans to update it. It is a U.S. government work in the public domain. Columns were renamed, codes were decoded to words from the IMLS documentation, one exact duplicate row was removed, county names were added from the Census 2010 county list, map points were added from the US Census Geocoder for rows IMLS left without one, and internal flags and IRS address fields were dropped.

Credit lines:

* Source: Institute of Museum and Library Services, Museum Data Files, 2018 release (https://www.imls.gov/research-evaluation/data-collection/museum-data-files). U.S. government work, public domain.
* Please cite: Institute of Museum and Library Services, Museum Data Files, 2018.
* Added map points: US Census Geocoder (https://geocoding.geo.census.gov).
* Not endorsed by IMLS.

[Back to the API list](../../README.md)
