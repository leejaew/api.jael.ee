# UAE International Schools

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 489](https://img.shields.io/badge/records-489-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange)

International private schools in Dubai, Abu Dhabi and Sharjah, 489 schools that teach at least one non UAE national curriculum from Grade 1 up. Each row has the school name in English and Arabic, emirate, area, curriculum, grades, yearly fees in AED where published, the latest inspection rating, phone, website and map coordinates. Dubai data is from October 2026, Sharjah from October 2026, and Abu Dhabi is a Q4 2024 snapshot, so check `source_as_of` on each row.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uae_international_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`emirate`). Optional: 19 fields. Disallowed: 5 fields.

`emirate` must be in every call. Use `emirate=*` to list all 489 schools. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `emirate` | Required | Yes | No | `Dubai` (215), `Abu Dhabi` (173) or `Sharjah` (101). Wildcards work, so `emirate=*` lists all 489 schools. |
| `school_name` | Optional | Yes | No | School name in English, such as `*Dubai British*` or `Al*`. 480 distinct names for 489 rows, so add `emirate` or `area` to tell branches apart. |
| `school_name_ar` | Optional | Yes | No | School name in Arabic, such as `*الأمريكية*`. |
| `region` | Optional | Yes | No | Area group: `Dubai` (215), `Abu Dhabi` (115), `Sharjah` (93), `Al Ain` (48), `Al Dhafra` (10), `Central Region` (4) or `Eastern Region` (4). The last four are all inside the emirate of Abu Dhabi. |
| `area` | Optional | Yes | No | Neighbourhood text from the source, such as `Muwaileh`, `Khalifa City` or `*Qusais*`. 154 distinct values. |
| `curriculum` | Optional | No | No | Main non national curriculum: `British` (169), `American` (139), `Indian` (90), `IB` (33), `French` (10), `SABIS` (10), `Philippine` (9), `Australian` (8), `Pakistani` (7), `German`, `Canadian`, `Japanese`, `Russian`, `Chinese`, `Italian`, `Bangladeshi` or `Spanish`. |
| `curriculum_other` | Optional | No | No | Second curriculum, only filled for 27 schools: `IB` (21), `British` (3), `MoE` (1), `American` (1) or `Kerala syllabus` (1). |
| `phase` | Optional | No | No | Grade span group: `All through` (341), `Primary and middle` (94), `Primary` (22), `Secondary` (20) or `Middle` (1). Blank for 11 rows. |
| `gender` | Optional | No | No | `Mixed` (269), `Girls` (3) or `Boys` (3). Blank for 214 rows, mostly Dubai, where the source does not say. |
| `grades` | Optional | Yes | No | Grade range as text, such as `KG1 to Grade 12` (179) or `FS1 to Year 13` (50). Use a wildcard such as `*Year 13`. Blank for 11 rows. |
| `fee_min_aed` | Optional | No | Yes | Lowest yearly tuition fee in UAE dirhams, a whole number from 2,700 to 93,220. Filled for 274 schools (Dubai 2024-25 and Sharjah), blank for Abu Dhabi. Use a range, for example `fee_min_aed=10000..30000`. |
| `fee_max_aed` | Optional | No | Yes | Highest yearly tuition fee in UAE dirhams, a whole number from 4,040 to 140,382. Filled for 274 schools. Use a range, for example `fee_max_aed=20000..50000`. |
| `fee_average_aed` | Optional | No | Yes | Average yearly tuition in UAE dirhams, a whole number from 3,824 to 78,540. Filled for 173 schools, all in Abu Dhabi. Use a range, for example `fee_average_aed=15000..40000`. |
| `fee_year` | Optional | No | No | School year of the fee, `2024-25` for 348 rows (Dubai and Abu Dhabi). Blank for 141 rows, including Sharjah, which has no fee year. |
| `inspection_body` | Optional | No | No | Who rated the school: `KHDA DSIB` (193), `ADEK Irtiqaa` (160) or `SPEA` (92). Blank for 44 rows. The three rating systems are not strictly the same scale. |
| `inspection_rating` | Optional | No | No | Latest published rating: `Good` (209), `Very good` (107), `Acceptable` (97), `Outstanding` (30) or `Weak` (2). Blank for 44 rows. |
| `rating_year` | Optional | No | No | School year of the rating where the source gives it: `2023-2024` (183), `2025-2026` (5), `2024-2025` (4) or `2019-2020` (1). Blank for 296 rows. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 23.644041 to 25.387423. No blanks. Use a range to search a box, for example `latitude=25.0..25.3`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 52.655178 to 56.357461. No blanks. Use a range to search a box, for example `longitude=55.1..55.5`. |
| `geocode_quality` | Optional | No | No | `exact` (470) is the source's own point. `approximate` (19) is an area or place level point that can be off by hundreds of metres. |
| `phone` | Disallowed | No | No | Published school phone number as text, with country code as given. Blank for 48 rows. |
| `website` | Disallowed | No | No | Published school website. Blank for 47 rows. Links were not tested. |
| `source` | Disallowed | No | No | Data source and licence credit for the row (KHDA, ADEK or SPEA). |
| `source_as_of` | Disallowed | No | No | Date of the source data for the row: `2026-10-27` (Dubai), `2024-12-31` (Abu Dhabi) or `2026-10-28` (Sharjah). |
| `updated` | Disallowed | No | No | Date the data was pulled, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Chinese and Arabic text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `school_name` | string | School name in English, tidied to one style. |
| `school_name_ar` | string | School name in Arabic, as the source gives it. |
| `emirate` | string | `Dubai`, `Abu Dhabi` or `Sharjah`. |
| `region` | string | Area group, including `Al Ain`, `Al Dhafra`, `Central Region` and `Eastern Region` for Abu Dhabi. |
| `area` | string | Neighbourhood text from the source. |
| `curriculum` | string | Main non national curriculum. |
| `curriculum_other` | string | Second curriculum. Null for most schools. |
| `phase` | string | Grade span group. Null for 11 rows. |
| `gender` | string | `Mixed`, `Girls` or `Boys`. Null when the source does not say. |
| `grades` | string | Lowest to highest grade as text. |
| `fee_min_aed` | integer | Lowest yearly tuition fee in AED. Null for Abu Dhabi and schools with no published fee. |
| `fee_max_aed` | integer | Highest yearly tuition fee in AED. |
| `fee_average_aed` | integer | Average yearly tuition in AED. Abu Dhabi only. |
| `fee_year` | string | School year of the fee. |
| `inspection_body` | string | Who rated the school. |
| `inspection_rating` | string | Latest published rating. |
| `rating_year` | string | School year of the rating. |
| `phone` | string | School phone number, as text. |
| `website` | string | School website. |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `geocode_quality` | string | `exact` or `approximate`. |
| `source` | string | Where the row came from. |
| `source_as_of` | string | Date of the source data for the row. |
| `updated` | string | Date the data was pulled. |

Notes: a school is included when it is private, teaches at least one non UAE national curriculum and has at least Grade 1. 95 listed schools are left out: 92 teach only the UAE national curriculum and 3 are kindergarten only. Three schools in the Sharjah list have no public page and are not included. Fees are in UAE dirhams per year and can change. Dubai gives a lowest and highest fee, and Abu Dhabi gives only an average. Sharjah fees have no stated year. Ratings come from three systems (KHDA, ADEK and SPEA), and most Dubai ratings are from 2023-2024. No source publishes a street address, so there is no address column, and some area labels differ from OpenStreetMap names. Names were tidied by rule, and Arabic names are as published. Emails are not included. Rows marked `approximate` should not be used for exact map pins.

## Examples

Indian curriculum schools in Sharjah, listing the names. This call returns 27 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uae_international_schools?emirate=Sharjah&curriculum=Indian" | jq -r '.[0:3][].school_name'
```

```text
Al Sabah Indian Private School
Ambassador School LLC
Amity Private School LLC
```

Outstanding IB schools in Abu Dhabi:

```bash
curl -s "https://api.jael.ee/JLEE/uae_international_schools?emirate=Abu%20Dhabi&curriculum=IB&inspection_rating=Outstanding"
```

Response (1 row):

```json
[
  {
    "__id": 371,
    "school_name": "The British International AD",
    "school_name_ar": "المدرسة البريطانية العالمية",
    "emirate": "Abu Dhabi",
    "region": "Abu Dhabi",
    "area": "Zayed City",
    "curriculum": "IB",
    "curriculum_other": null,
    "phase": "Secondary",
    "gender": "Mixed",
    "grades": "Grade 11 to Grade 12",
    "fee_min_aed": null,
    "fee_max_aed": null,
    "fee_average_aed": 74980,
    "fee_year": "2024-25",
    "inspection_body": "ADEK Irtiqaa",
    "inspection_rating": "Outstanding",
    "rating_year": null,
    "phone": "+97125100100",
    "website": "https://www.nordangliaeducation.com/our-schools/abu-dhabi",
    "latitude": 24.369009,
    "longitude": 54.563618,
    "geocode_quality": "exact",
    "source": "ADEK private schools open data (Abu Dhabi Open Data Platform), snapshot Q4/2024",
    "source_as_of": "2024-12-31",
    "updated": "2026-10-28"
  }
]
```

A wildcard on the school name. This call returns 11 rows, and the first three are Al Maaref American School L.L.C, American Academy for Girls and American International School:

```bash
curl -s "https://api.jael.ee/JLEE/uae_international_schools?emirate=Dubai&school_name=*American*" | jq -r '.[0:3][].school_name'
```

British schools with a lowest yearly fee between 10,000 and 30,000 AED, using a range. This call returns 43 rows, and the first three are Al Salam Community School L.L.C, Al Salam Private School and Cambridge International School:

```bash
curl -s "https://api.jael.ee/JLEE/uae_international_schools?emirate=*&curriculum=British&fee_min_aed=10000..30000" | jq -r '.[0:3][].school_name'
```

Outstanding schools in a box around Dubai, with ranges on latitude and longitude. This call returns 23 rows, and the first three are Deira International School, Dubai British School and Dubai British School Jumeirah Park:

```bash
curl -s "https://api.jael.ee/JLEE/uae_international_schools?emirate=*&inspection_rating=Outstanding&latitude=25.0..25.3&longitude=55.1..55.5" | jq -r '.[0:3][].school_name'
```

A wildcard on the Arabic name. This call returns 23 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uae_international_schools" --data-urlencode "emirate=*" --data-urlencode "school_name_ar=*الأمريكية*" | jq -r '.[0:3][].school_name_ar'
```

```text
مدرسة المعارف الأمريكية ش.ذ.م.م
الاكاديمية الأمريكية للبنات
مدرسة دونكرست الأمريكية ش.ذ.م.م
```

## Data source

The rows come from three official school lists, checked on 2026-10-28. Dubai: the Knowledge and Human Development Authority (KHDA) school directory, with fees from its Private Schools Open Data file for 2024-25. Sharjah: facts from the Sharjah Private Education Authority (SPEA) school directory, with names, curriculum, grades, contacts, fees, ratings and map points only, and no SPEA text, logos or images. Abu Dhabi: the Abu Dhabi Department of Education and Knowledge (ADEK) Private Schools dataset on the Abu Dhabi Open Data Platform, snapshot Q4 2024, with emails left out. Of 489 rows, 470 use the source's own map point and 19 use an area level point from OpenStreetMap. The data is not guaranteed to be complete or accurate, and the sources do not promise it stays available.

Credit lines:

* Dubai: Source: Knowledge and Human Development Authority (KHDA), Dubai, school directory and Private Schools Open Data 2024-25. Data was tidied and combined; it is not an official KHDA publication. KHDA asks that its open data is not used for profit.
* Sharjah: Sharjah school facts compiled from the Sharjah Private Education Authority school directory (spea.shj.ae), October 2026. SPEA keeps all rights in its content.
* Abu Dhabi: Contains information from the Abu Dhabi Department of Education and Knowledge (ADEK), Private Schools dataset (snapshot Q4 2024), published on the Abu Dhabi Open Data Platform under the Abu Dhabi Government Open Data License.
* Some coordinates: OpenStreetMap contributors, ODbL.

[Back to the API list](../../README.md)
