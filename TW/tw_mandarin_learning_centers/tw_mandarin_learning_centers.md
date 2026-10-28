# Taiwan Mandarin Learning Centers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 71](https://img.shields.io/badge/records-71-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange) ![license OGDL 1.0](https://img.shields.io/badge/license-OGDL%201.0-green)

Mandarin teaching centers attached to 70 universities in Taiwan, 71 rows from the Ministry of Education list, with English and Traditional Chinese names and addresses, city, district, contacts, 2025 foreign student counts and map coordinates. National Taiwan University has two centers, so it appears twice. Coordinates come from OpenStreetMap, and 3 rows have none.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_mandarin_learning_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`city_en`). Optional: 16 fields. Disallowed: 5 fields.

`city_en` must be in every call. Use `city_en=*` to list all 71 centers. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city_en` | Required | Yes | No | City or county in English, 20 values, such as `Taipei City` (14), `Taichung City` (9), `Kaohsiung City` (9), `Taoyuan City` (8), `Hsinchu City` (4) or `Tainan City` (4). Wildcards work, so `city_en=*` lists all 71 centers. |
| `moe_list_no` | Optional | No | No | Row number on the Ministry of Education list, a whole number from 1 to 71. |
| `university_en` | Optional | Yes | No | University name in English, such as `*Normal*` or `National Taiwan*`. 70 distinct universities. |
| `university_zh` | Optional | Yes | No | University name in Traditional Chinese, such as `*師範*` or `國立*`. |
| `center_en` | Optional | Yes | No | Mandarin center name in English, such as `*Language Center*`. 28 distinct names. |
| `center_zh` | Optional | Yes | No | Mandarin center name in Traditional Chinese, such as `*華語*`. 29 distinct names. |
| `region_en` | Optional | No | No | Region: `North` (32), `South` (20), `Central` (14), `East` (4) or `Offshore` (1). |
| `region_zh` | Optional | No | No | Region in Chinese: `北部` (32), `南部` (20), `中部` (14), `東部` (4) or `離島` (1). |
| `city_zh` | Optional | Yes | No | City or county in Traditional Chinese, such as `臺北市` or `*高雄*`. |
| `district_en` | Optional | Yes | No | District, township or city of the center in English, such as `Zhongli District` or `*Daan*`. 47 distinct values. |
| `district_zh` | Optional | Yes | No | District in Traditional Chinese, such as `中壢區` or `*大安*`. |
| `address_en` | Optional | Yes | No | Street address in English, such as `*Roosevelt Rd*`. A few differ from the Chinese address. |
| `address_zh` | Optional | Yes | No | Street address in Traditional Chinese, with the city prefix, such as `*中大路*`. |
| `foreign_students_2025` | Optional | No | Yes | Foreign students at the center, ROC year 114, a whole number from 0 to 4567. Blank for the two National Taiwan University centers. Example: `foreign_students_2025=500..5000`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84) from OpenStreetMap, from 22.563654 to 25.14969. Blank for 3 rows. Example: `latitude=24.9..25.1`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84) from OpenStreetMap, from 118.322235 to 121.775731. Blank for 3 rows. Example: `longitude=121.4..121.7`. |
| `geocode_quality` | Optional | No | No | How well the address matched the map lookup: `exact` (61), `approximate` (7) or `none` (3). Do not use approximate or none rows for exact map pins. |
| `phone` | Disallowed | No | No | Center phone number with +886 and extension, as text. Can hold more than one number. |
| `email` | Disallowed | No | No | Center contact email. Blank for 2 centers. |
| `website` | Disallowed | No | No | Center web page as given by the source. Links were not tested. |
| `source` | Disallowed | No | No | Data source and licence credit for the row. |
| `updated` | Disallowed | No | No | Date the data was pulled, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Chinese text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `moe_list_no` | integer | Row number on the Ministry of Education list. |
| `university_en` | string | University name in English. |
| `university_zh` | string | University name in Traditional Chinese. |
| `center_en` | string | Mandarin center name in English. |
| `center_zh` | string | Mandarin center name in Traditional Chinese. |
| `region_en` | string | `North`, `Central`, `South`, `East` or `Offshore`. |
| `region_zh` | string | Region in Chinese. |
| `city_en` | string | City or county in English. |
| `city_zh` | string | City or county in Traditional Chinese. |
| `district_en` | string | District, township or city in English. |
| `district_zh` | string | District in Traditional Chinese. |
| `address_en` | string | Street address in English. |
| `address_zh` | string | Street address in Traditional Chinese. |
| `phone` | string | Phone with +886 and extension, as text. Can hold more than one number. |
| `email` | string | Center contact email. Null for 2 centers. |
| `website` | string | Center web page. |
| `foreign_students_2025` | integer | Foreign students at the center, ROC year 114. Null for 2 rows. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). Null for 3 rows. |
| `longitude` | number | Longitude in decimal degrees (WGS 84). Null for 3 rows. |
| `geocode_quality` | string | `exact`, `approximate` or `none`. |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the data was pulled. |

Notes: English names, addresses and districts are the Ministry of Education English list dated 2026-01-28. Chinese names, phone, email and website come from the open data file, and the region, city and Chinese district fields were added by us. Five rows (list numbers 5, 36, 47, 57 and 70) have English and Chinese addresses that disagree, for example different house numbers or campuses. Each is kept as the source gives it, and the coordinates follow the Chinese address. The English address for Tzu Chi University (list number 67) is our translation, because the source row was garbled. Seven rows are `approximate`, meaning a campus or road point, and three are `none` (Chinese Culture University, Tzu Chi University and Fo Guang University). Fifteen centers show 0 foreign students. Some phone cells hold more than one number. Websites and emails are as the source gives them and were not tested.

## Examples

Mandarin centers in Taipei City. This call returns 14 rows, and the first four are National Taipei University of Education, National Taiwan University (two centers) and National Taiwan Normal University:

```bash
curl -s "https://api.jael.ee/JLEE/tw_mandarin_learning_centers?city_en=Taipei%20City" | jq -r '.[].university_en'
```

One center by university name in Taoyuan City:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_mandarin_learning_centers" --data-urlencode "city_en=Taoyuan City" --data-urlencode "university_en=National Central*"
```

Response (1 row):

```json
[
  {
    "__id": 1,
    "moe_list_no": 1,
    "university_en": "National Central University",
    "university_zh": "國立中央大學",
    "center_en": "Language Center",
    "center_zh": "語言中心",
    "region_en": "North",
    "region_zh": "北部",
    "city_en": "Taoyuan City",
    "city_zh": "桃園市",
    "district_en": "Zhongli District",
    "district_zh": "中壢區",
    "address_en": "No. 300, Jhongda Rd., Jhongli District, Taoyuan City 32001, Taiwan",
    "address_zh": "桃園市中壢區中大路300號",
    "phone": "+886-3-4227151 ext. 33818",
    "email": "ncu3800@ncu.edu.tw",
    "website": "http://www.lc.ncu.edu.tw/main/clp/",
    "foreign_students_2025": 522,
    "latitude": 24.96844,
    "longitude": 121.19155,
    "geocode_quality": "exact",
    "source": "Ministry of Education (Taiwan), English list of university Mandarin centers, 2026.01.28: https://english.moe.gov.tw/dl-12555-541025f60069488c961a6a1947684b7c.html | Ministry of Education, Open Data: Mandarin teaching centers attached to colleges and universities (各大學校院附設華語文教學中心一覽表), data.gov.tw dataset 32995: https://data.gov.tw/dataset/32995 | Ministry of Education, Open Data: foreign students at college Mandarin centers by school (大專校院附設華語文中心外國學生人數-按校別), ROC year 114, data.gov.tw dataset 33001: https://data.gov.tw/dataset/33001 | Coordinates: OpenStreetMap contributors via Nominatim",
    "updated": "2026-10-28"
  }
]
```

A wildcard on the Chinese university name. Normal universities in any city. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_mandarin_learning_centers" --data-urlencode "city_en=*" --data-urlencode "university_zh=*師範*" | jq -r '.[].university_en'
```

```text
National Taiwan Normal University
National Changhua University of Education
National Kaohsiung Normal University
```

A range on foreign students. Centers with 1,000 to 5,000 foreign students. This call returns 9 rows, and the first three are National Taiwan Normal University, Tamkang University and Ming Chuan University:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_mandarin_learning_centers" --data-urlencode "city_en=*" --data-urlencode "foreign_students_2025=1000..5000" | jq -r '.[].university_en'
```

A box on latitude and longitude around Taipei. This call returns 12 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_mandarin_learning_centers" --data-urlencode "city_en=*" --data-urlencode "latitude=25..25.1" --data-urlencode "longitude=121.4..121.7" | jq -r '.[].university_en'
```

## Data source

The list comes from the Ministry of Education, Taiwan: the English list of university Mandarin centers dated 2026-01-28, and two open data files on data.gov.tw, the [list of Mandarin teaching centers attached to colleges and universities](https://data.gov.tw/dataset/32995) (modified 2026-08-18) and the [foreign students at college Mandarin centers by school](https://data.gov.tw/dataset/33001) (ROC year 114, modified 2026-09-08). The period of the student counts is not stated exactly by the source. All three were downloaded on 2026-10-28 and are used under the [Open Government Data License, version 1.0](https://data.gov.tw/license). Coordinates come from OpenStreetMap through Nominatim.

Credit line: Source: Ministry of Education, Taiwan, 2026, Mandarin teaching centers attached to colleges and universities (data.gov.tw dataset 32995) and Foreign students at college Mandarin centers by school (dataset 33001), via the Open Government Data License, version 1.0. Coordinates: © OpenStreetMap contributors (ODbL).

[Back to the API list](../../README.md)
