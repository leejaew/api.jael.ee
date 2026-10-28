# Taiwan International Schools

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 24](https://img.shields.io/badge/records-24-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![license OGDL 1.0](https://img.shields.io/badge/license-OGDL%201.0-green)

International schools for foreign residents in Taiwan, 24 campuses from the Ministry of Education list of March 2026, with English and Traditional Chinese names, city, district and address, postal code, national type, grades offered, phone, website and map coordinates. Taipei European School has two campuses, so it appears twice. Coordinates come from OpenStreetMap and are accurate to about street level.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_international_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 16 fields. Disallowed: 4 fields.

No parameter is required, so a call with no filter returns all 24 rows. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name_en` | Optional | Yes | No | School name in English, such as `*American*` or `Taipei*`; wildcards work. Taipei European School has two rows, one per campus. |
| `name_zh` | Optional | Yes | No | School name in Traditional Chinese, such as `*美國*` or `台北*`; wildcards work. |
| `city_en` | Optional | Yes | No | City or county in English, such as `Taipei City` (8), `Kaohsiung City` (6), `Taichung City` (3) or `*Hsinchu*`; wildcards work. |
| `city_zh` | Optional | Yes | No | City or county in Traditional Chinese, such as `臺北市` or `*高雄*`; wildcards work. |
| `district_en` | Optional | Yes | No | District in English, such as `Shilin District` or `*Zhongli*`; wildcards work. 15 distinct values. |
| `district_zh` | Optional | Yes | No | District in Traditional Chinese, such as `士林區`; wildcards work. |
| `address_en` | Optional | Yes | No | Street address in English, such as `*Zhongshan North Road*`; wildcards work. |
| `address_zh` | Optional | Yes | No | Street address in Traditional Chinese, with the city prefix, such as `*中山北路*`; wildcards work. |
| `postal_code` | Optional | Yes | No | Taiwan postal code as text, such as `111039` or `111*`; wildcards work. |
| `nationality_type` | Optional | No | No | School's national type: `American` (13), `Other` (3), `Japanese` (3), `European` (2), `Korean` (2) or `Finnish` (1). |
| `grades_en` | Optional | No | No | Grades offered in English, such as `Kindergarten to Grade 12` (9) or `Grades 1 to 12` (5). |
| `grades_zh` | Optional | No | No | Grades offered in Traditional Chinese, such as `幼兒園至12年級`. |
| `lowest_grade` | Optional | No | Yes | Lowest grade as a whole number, 0 for kindergarten and 1 to 12 for grades. Use a range such as `lowest_grade=0..1`. |
| `highest_grade` | Optional | No | Yes | Highest grade as a whole number from 6 to 12. Use a range such as `highest_grade=12..12` for schools through Grade 12. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS84), 6 decimals, from 22.626372 to 25.1369. No blanks. Use a range, for example `latitude=24.0..25.5`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS84), 6 decimals, from 120.278211 to 121.587945. No blanks. Use a range, for example `longitude=121.0..122.0`. |
| `phone` | Disallowed | No | No | School phone number as text with +886 country code. Not filterable. |
| `website` | Disallowed | No | No | Link to the school's own website (https). Not filterable. |
| `source` | Disallowed | No | No | Source and licence credit for the row. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the Ministry of Education list, 2026-03-09 on every row. Not filterable. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Chinese text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `name_en` | string | School name in English. |
| `name_zh` | string | School name in Traditional Chinese. The two Taipei European School campuses carry the level in brackets. |
| `city_en` | string | City or county in English. |
| `city_zh` | string | City or county in Traditional Chinese. |
| `district_en` | string | District in English. |
| `district_zh` | string | District in Traditional Chinese. |
| `address_en` | string | Street address in English. |
| `address_zh` | string | Street address in Traditional Chinese. |
| `postal_code` | string | Taiwan postal code, as text. |
| `nationality_type` | string | National type of the school: `American`, `Japanese`, `European`, `Korean`, `Finnish` or `Other`. |
| `grades_en` | string | Grades offered, in English. |
| `grades_zh` | string | Grades offered, in Traditional Chinese. |
| `lowest_grade` | integer | Lowest grade as a number. 0 means kindergarten. |
| `highest_grade` | integer | Highest grade as a number. |
| `phone` | string | School phone number with +886 country code, as text. |
| `website` | string | School website (https). |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `source` | string | Source and licence credit for the row. |
| `source_as_of` | string | Date of the Ministry of Education list. |

Notes: Chinese names, addresses, postal codes and phone numbers come from the Ministry of Education open data file. English names and addresses follow the Ministry of Education English list, and some English text was edited for natural order and spelling. Grades are numbers: kindergarten is 0 and grades 1 to 12 are 1 to 12. Five school websites in the source were dead or out of date, so they were replaced with the schools' current https pages. The Kaohsiung Finland International School opened on 1 September 2026.

## Examples

Schools in Taichung City. This call returns 3 rows: American School in Taichung, Morrison Academy Taichung and Taichung Japanese School.

```bash
curl -sG "https://api.jael.ee/JLEE/tw_international_schools" --data-urlencode "city_en=Taichung City"
```

Response excerpt (first of 3 rows):

```json
[
  {
    "__id": 40,
    "name_en": "American School in Taichung",
    "name_zh": "臺中美國學校",
    "city_en": "Taichung City",
    "city_zh": "臺中市",
    "district_en": "Beitun District",
    "district_zh": "北屯區",
    "address_en": "No. 21-1, Zhuyuan Lane, Beitun District, Taichung City",
    "address_zh": "臺中市北屯區苧園巷21-1號",
    "postal_code": "406051",
    "nationality_type": "American",
    "grades_en": "Grades 1 to 12",
    "grades_zh": "1年級至12年級",
    "lowest_grade": 1,
    "highest_grade": 12,
    "phone": "+886-4-2239-7532",
    "website": "https://www.astaichung.com",
    "latitude": 24.169396,
    "longitude": 120.744517,
    "source": "Ministry of Education, Taiwan: Schools for International Residents in ROC (Open Government Data License v1.0). https://data.gov.tw/dataset/32957",
    "source_as_of": "2026-03-09"
  }
]
```

A wildcard on the city. Schools in Hsinchu City or Hsinchu County. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_international_schools" --data-urlencode "city_en=*Hsinchu*" | jq -r '.[].name_en'
```

```text
Hsinchu American School
Hsinchu International School
Pacific American School, Hsinchu County
Hsinchu County American School
```

A wildcard on the Chinese name. Schools with 美國 in the name. This call returns 13 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_international_schools" --data-urlencode "name_zh=*美國*" | jq -r '.[].name_en'
```

A filter and a range together. American schools in the northern half of the island, between latitude 24 and 25.5. This call returns 11 rows, and the first three are Taipei American School, Grace Christian Academy and Taipei Adventist American School:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_international_schools" --data-urlencode "nationality_type=American" --data-urlencode "latitude=24.0..25.5" | jq -r '.[].name_en'
```

A range on grades. Schools that start at kindergarten and end at Grade 6. This call returns 4 rows: Taipei European School Primary Campus, Taipei Korean School, Dominican International School Kaohsiung and Kaohsiung Korean International School:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_international_schools" --data-urlencode "lowest_grade=0..0" --data-urlencode "highest_grade=6..6" | jq -r '.[].name_en'
```

## Data source

The list comes from the Ministry of Education, Taiwan: the [Schools for International Residents in ROC](https://data.gov.tw/dataset/32957) open data file on data.gov.tw (list dated 2026-03-09) and the [Ministry of Education English page](https://english.moe.gov.tw/cp-108-18978-da64a-1.html). It is released under the [Open Government Data License, version 1.0](https://data.gov.tw/license), which allows free reuse, including commercial use, with credit. The Ministry of Education does not endorse this API. Coordinates were looked up from the addresses with OpenStreetMap Nominatim, data © OpenStreetMap contributors (ODbL).

Credit line: Source: Ministry of Education, Taiwan, Schools for International Residents in ROC [2026], released under the Open Government Data License, version 1.0, https://data.gov.tw/license

[Back to the API list](../../README.md)
