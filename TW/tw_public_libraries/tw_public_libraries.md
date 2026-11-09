# Taiwan public libraries

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 615](https://img.shields.io/badge/records-615-lightgrey) ![updated 2026-11-09](https://img.shields.io/badge/updated-2026--11--09-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

615 public libraries across Taiwan's 22 counties and cities, from the National Library of Public Information list, with Chinese and English names, districts, addresses, phones, websites and map points. `county_city_en` is required. You can also filter by district, name or a latitude and longitude box.

English names: 62 rows use official English names (mainly Taipei Public Library branches, plus National Library of Public Information and National Taiwan Library). The other 553 rows use consistent translated names. The source has no opening hours.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_public_libraries` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per library, with all 17 fields and no `__id`. |
| Record limit | None. Results are not capped. |

## Query parameters

Required: 1 field. Optional: 13 fields. Disallowed: 3 fields.

`county_city_en` is required. Partial matching uses `*` wildcards, text matches ignore case, and filters are combined with AND. `lat` and `lng` accept ranges with two dots (`min..max`). Other fields are strings and accept exact matches unless Partial is Yes.

Disallowed fields are returned but cannot be used as filters. Filtering on `intro_zh`, `source` or `updated` returns HTTP 400. A call without `county_city_en` also returns HTTP 400. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `county_city_en` | Required | No | No | Exact county or city in English, from Chunghwa Post spellings with Dist. expanded to District. Values: New Taipei City (104); Kaohsiung City (64); Taipei City (57); Taichung City (50); Taoyuan City (46); Tainan City (45); Pingtung County (37); Changhua County (28); Yunlin County (25); Miaoli County (20); Chiayi County (20); Taitung County (16); Hualien County (16); Yilan County (16); Nantou County (15); Hsinchu County (14); Hsinchu City (11); Keelung City (10); Penghu County (7); Lienchiang County (6); Kinmen County (5); Chiayi City (3). |
| `name_en` | Optional | Yes | No | English library name. 62 official; 553 translated. Use wildcards, such as `*Memorial*` or `*Township*`. |
| `name_zh` | Optional | Yes | No | Chinese library name. All 615 names are distinct. Use wildcards, such as `*分館*`. |
| `name_en_source` | Optional | No | No | Exact English name source: `official` (62) or `translated` (553). |
| `county_city_zh` | Optional | No | No | Exact county or city in Chinese, with 臺 used consistently. Values: 新北市 (104); 高雄市 (64); 臺北市 (57); 臺中市 (50); 桃園市 (46); 臺南市 (45); 屏東縣 (37); 彰化縣 (28); 雲林縣 (25); 苗栗縣 (20); 嘉義縣 (20); 臺東縣 (16); 花蓮縣 (16); 宜蘭縣 (16); 南投縣 (15); 新竹縣 (14); 新竹市 (11); 基隆市 (10); 澎湖縣 (7); 連江縣 (6); 金門縣 (5); 嘉義市 (3). |
| `district_en` | Optional | Yes | No | District, township or city in English from Chunghwa Post. 352 distinct values. Use wildcards, such as `*Banqiao*`. |
| `district_zh` | Optional | Yes | No | District, township or city in Chinese from the source Area field. Partial matching is allowed. |
| `postcode` | Optional | No | No | Published postcode as text (three to five digits). Exact match; no wildcards. |
| `address_zh` | Optional | Yes | No | Published Chinese address, with county 臺 spelling normalized. Partial matching is allowed. |
| `phone` | Optional | No | No | Published library landline as text. Exact match. |
| `fax` | Optional | No | No | Published library fax as text. Blank in 128 rows. Exact match. |
| `website` | Optional | Yes | No | Published website. 614 https links; 1 blank. Partial matching is allowed. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, rounded to six decimal places. Use two dots for a range, such as `24..25`. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, rounded to six decimal places. Use two dots for a range, such as `120..121`. |
| `intro_zh` | Disallowed | No | No | Plain text intro from the source HTML. Returned but not filterable. |
| `source` | Disallowed | No | No | Source credit string, the same in every row. Returned but not filterable. |
| `updated` | Disallowed | No | No | Build date as text: `2026-11-09`. Returned but not filterable. |

## Response fields

`lat` and `lng` are numbers. All other fields are strings. Blank source cells are returned as `null`.

| Field | Type | Description |
|---|---|---|
| `name_en` | string | English library name. |
| `name_zh` | string | Chinese library name. |
| `name_en_source` | string | `official` or `translated`. |
| `county_city_en` | string | County or city in English. |
| `county_city_zh` | string | County or city in Chinese, with 臺 used consistently. |
| `district_en` | string | District, township or city in English. |
| `district_zh` | string | District, township or city in Chinese. |
| `postcode` | string | Published postcode as text. |
| `address_zh` | string | Chinese street address. |
| `phone` | string | Library landline as published. |
| `fax` | string | Library fax as published, or null when blank. |
| `website` | string | Library website, or null when blank. |
| `lat` | number | Latitude in decimal degrees. |
| `lng` | number | Longitude in decimal degrees. |
| `intro_zh` | string | Plain text intro from the source. |
| `source` | string | `National Library of Public Information, data.gov.tw dataset 99567 (OGDL v1.0)`. |
| `updated` | string | Build date `2026-11-09` as text. |

## Example

Kaohsiung libraries whose Chinese name contains 新興分館. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_libraries' --data-urlencode 'county_city_en=Kaohsiung City' --data-urlencode 'name_zh=*新興分館*'
```

Response excerpt:

```json
[
  {
    "name_en": "Kaohsiung Public Library Xinxing Branch",
    "name_zh": "高雄市立圖書館新興分館",
    "name_en_source": "translated",
    "county_city_en": "Kaohsiung City",
    "county_city_zh": "高雄市",
    "district_en": "Xinxing District",
    "district_zh": "新興區",
    "postcode": "800",
    "address_zh": "高雄市新興區中正三路34號3樓",
    "phone": "07-2372563",
    "fax": "07-2381594",
    "website": "https://www.ksml.edu.tw/lib/content/index.aspx?Parser=1,36,358,350",
    "lat": 22.6311,
    "lng": 120.31,
    "intro_zh": "面積400坪，在新興區行政中心三樓，交通便利。",
    "source": "National Library of Public Information, data.gov.tw dataset 99567 (OGDL v1.0)",
    "updated": "2026-11-09"
  }
]
```

## Data notes

* One source test row, 測試圖書館 in Taichung, was dropped (swapped coordinates and empty website). 615 rows remain.
* English district and county labels come from Chunghwa Post spellings. Dist. was expanded to District. One source typo, Taoyuan City City for Yangmei District, was normalized to Taoyuan City.
* Four township English names that collided across counties (Xihu and Dongshi) were disambiguated with the county prefix.
* All 615 map points come from the NLPI source and fall inside Taiwan, including Penghu, Kinmen and Lienchiang. Latitude bounds 22.003700..26.367800. Longitude bounds 118.242898..121.926800.
* No personal mobile numbers were in the source. Fax is blank in 128 rows. One website is blank (Alishan Township Public Library). Eighteen intros still contain a broken ? character from the source file.
* Changes from the source: shorter field names, English names with an official or translated flag, English county and district labels, whitespace cleanup, HTML stripped from intros, and coordinates rounded to six decimal places.

## Data source and license

Data: National Library of Public Information, Taiwan, Public Library Directory, [dataset 99567](https://data.gov.tw/dataset/99567). [Source JSON](https://plisnet.nlpi.edu.tw/api/API/LibraryInfoData). Portal metadata updated 16 July 2026. The `updated` field and badge show the build date `2026-11-09`.

English districts and counties: Chunghwa Post Co., Ltd., English spellings of administrative districts from the three digit postal code table (OGDL v1.0).

English library names: official names for Taipei Public Library branches from the [Taipei Public Library English site](https://english.tpml.gov.taipei/), plus National Library of Public Information and National Taiwan Library. Other English names are consistent translations.

The Open Data is made available to the public under the [Open Government Data License, version 1.0 (OGDL v1.0)](https://data.gov.tw/license). Credit the National Library of Public Information (and Chunghwa Post for English district names) when you copy, share, change or use the data for business. Patent and trademark rights are not covered. No warranty or endorsement. No OpenStreetMap or ODbL data was used.

[Back to the API list](../../README.md)
