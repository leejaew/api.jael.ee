# Taiwan Kindergartens

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 6,747](https://img.shields.io/badge/records-6%2C747-lightgrey) ![updated 2026-01-07](https://img.shields.io/badge/updated-2026--01--07-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

6,747 licensed kindergartens in Taiwan for school year 114 (August 2025 to July 2026), from the Ministry of Education Department of Statistics kindergarten directory (幼兒園名錄) as listed on 7 January 2026. 2,429 are public and 4,318 are private, including 420 nonprofit kindergartens and 1,904 attached to elementary schools. Each record has the official school year, school code, Traditional Chinese name, public or private status, county, district, postcode, address and phone. No field is required, so you can filter by county, district, public or private status, name, postcode or school code.

Names, counties, districts and addresses are in Traditional Chinese only. The source has no English name. County names use 臺 rather than 台, so search for `臺北市` or `臺中市`, not `台北市` or `台中市`. Encode Chinese values in the query string, for example with `curl -G --data-urlencode`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_kindergartens` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per kindergarten. All matches are returned. |

## Query parameters

Required: none. Optional: 9 fields. Disallowed: 2 fields.

A call with no filters returns all 6,747 records. Partial filters accept `*`, such as `name=*非營利*`, `county=*北市*` or `school_code=0*`. `ownership` is exact match, using the official wording `公立` or `私立`. Text filters ignore case. No field accepts a range. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `school_year` | Optional | Yes | No | Ministry school year as text, `114` in every row (August 2025 to July 2026). The official file also holds years 104 to 113; this table keeps only the latest year. Never blank. Partial wildcards allowed, such as `114`. |
| `school_code` | Optional | Yes | No | Official Ministry school code, unique and never blank, so it is the stable record key. 6,468 codes are 6 characters, such as `011K02`. 279 branch campuses (分班) use a 9 character code with a hyphen, such as `011Z39-01`. Every code contains letters. 3,481 start with 0, so this field is text. Partial wildcards allowed, such as `0*` (3481), `011Z39` (1) or `011Z39-01` (1). |
| `name` | Optional | Yes | No | Official kindergarten name in Traditional Chinese, as the Ministry lists it, such as 新北市私立溫特爾幼兒園 or 新北市新店區大豐國民小學附設幼兒園. Never blank. All 6,747 names are unique. 420 names include 非營利 (all listed as private). 1,904 names include 國民小學附設, 46 include 國民中學附設 and 20 include 國民中小學附設. 279 names include 分班, matching the hyphenated branch codes. Some official names include an operator in parentheses, such as 委託財團法人彭婉如文教基金會辦理; those are organization names, not a person in charge. The official UTF-8 file has an ASCII question mark in 37 names where a less common character was lost in the source, such as 桃園市大園區?林國民小學附設幼兒園; those marks were left as published. Partial wildcards allowed, such as `*非營利*` (420), `*國民小學附設*` (1904) or `*分班*` (279). |
| `ownership` | Optional | No | No | Official public or private label as published: `公立` (2429) or `私立` (4318). Nonprofit kindergartens are listed as `私立` in the source. Exact match. |
| `county` | Optional | Yes | No | County or city after the official bracket code was removed, such as 新北市 or 臺中市. Never blank. 22 values, using 臺 rather than 台. Counts: 新北市 (1108), 臺中市 (767), 高雄市 (669), 臺北市 (658), 桃園市 (631), 臺南市 (549), 彰化縣 (335), 屏東縣 (298), 新竹縣 (261), 苗栗縣 (196), 南投縣 (180), 新竹市 (164), 雲林縣 (161), 嘉義縣 (157), 花蓮縣 (142), 宜蘭縣 (125), 臺東縣 (121), 基隆市 (103), 嘉義市 (67), 澎湖縣 (25), 金門縣 (25), 連江縣 (5). Partial wildcards allowed, such as `*北市*` (1766, 新北市 and 臺北市) or `臺中市`. |
| `district` | Optional | Yes | No | Township, city or district as the Ministry lists it, such as 板橋區, 中壢區 or 竹北市. Never blank. 356 values. Some names appear in more than one county: 東區 is 201 rows across 新竹市 (87), 臺南市 (52), 嘉義市 (38) and 臺中市 (24). 板橋區 is only in 新北市 (152). Partial wildcards allowed, such as `板橋區` (152) or `*東區*` or `東區` (201). |
| `postcode` | Optional | Yes | No | 3 digit postal code taken from the official address bracket, such as `220` or `237`, then removed from the address. Never blank. 364 values. Kept as text. The largest are `300` (164, Hsinchu City), `220` (152, Banqiao) and `330` (115, Taoyuan District). Partial wildcards allowed, such as `220` or `300`. |
| `address` | Optional | Yes | No | Street address in Traditional Chinese, as the Ministry lists it after the postal bracket was removed, such as 新北市三峽區龍埔里5鄰三樹路336號1、2、3樓. Never blank. Every address starts with the same county and district as those columns. The official file has an ASCII question mark in 62 addresses where a less common character was lost in the source; those marks were left as published. Partial wildcards allowed, such as `*中正路*` (136) or `*三峽區*`. |
| `phone` | Optional | Yes | No | Official institution phone as the Ministry lists it, usually an area code in brackets, such as `(02)26718181`. 1,719 numbers include an extension after `#`. Blank on 7 rows: 1 already blank in the source, and 6 where a mobile number listed as the institution phone was omitted. One row is `尚未申請`. One row has no area code. Partial wildcards allowed, such as `(02)*` (1868) or `*#*` (1719). |
| `source` | Disallowed | No | No | Ministry of Education, Department of Statistics, Kindergarten directory (幼兒園名錄), Taiwan, in all 6,747 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the official CSV and JSON file, YYYY-MM-DD (`2026-01-07`). Text. Not filterable. |

## Response fields

There is no `__id` field. All values are strings, or `null` when the source has no phone, or when a mobile number listed as the institution phone was omitted. `school_code` is the official public key.

| Field | Type | Description |
|---|---|---|
| `school_year` | string | Ministry school year, `114` on every row. |
| `school_code` | string | Official Ministry school code, unique on every row, kept as text. |
| `name` | string | Official kindergarten name in Traditional Chinese. |
| `ownership` | string | `公立` or `私立`. |
| `county` | string | County or city, using 臺 rather than 台. |
| `district` | string | Township, city or district. |
| `postcode` | string | 3 digit postal code, kept as text. |
| `address` | string | Street address in Traditional Chinese, without the postal bracket. |
| `phone` | string | Institution phone, or `null` on 7 rows. |
| `source` | string | Ministry of Education, Department of Statistics, Kindergarten directory (幼兒園名錄), Taiwan. |
| `source_as_of` | string | Date of the official file, `2026-01-07`. |

## Examples

One kindergarten by official school code. This call returns 1 record, 新北市私立新惠幼幼兒園:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_kindergartens' --data-urlencode 'school_code=011Z39'
```

```json
[
  {
    "school_year": "114",
    "school_code": "011Z39",
    "name": "新北市私立新惠幼幼兒園",
    "ownership": "私立",
    "county": "新北市",
    "district": "永和區",
    "postcode": "234",
    "address": "新北市永和區下溪里13鄰保安路252巷2號1.2樓",
    "phone": "(02)29209603",
    "source": "Ministry of Education, Department of Statistics, Kindergarten directory (幼兒園名錄), Taiwan",
    "source_as_of": "2026-01-07"
  }
]
```

Public kindergartens. This call returns 2,429 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_kindergartens' --data-urlencode 'ownership=公立'
```

Kindergartens in New Taipei City. This call returns 1,108 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_kindergartens' --data-urlencode 'county=新北市'
```

Nonprofit kindergartens, by name. This call returns 420 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_kindergartens' --data-urlencode 'name=*非營利*'
```

The branch campus of the same school, by its hyphenated code. This call returns 1 record, 新北市私立新惠幼幼兒園保安分班:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_kindergartens' --data-urlencode 'school_code=011Z39-01'
```

## Data source

Source: Ministry of Education, Department of Statistics, [Kindergarten directory (幼兒園名錄)](https://data.gov.tw/dataset/6086), data.gov.tw dataset 6086. The official file used for this snapshot is [k1_new.csv](https://stats.moe.gov.tw/files/opendata/k1_new.csv). The same list is also published as [k1_new.json](https://stats.moe.gov.tw/files/opendata/k1_new.json). The file's HTTP Last-Modified time is 7 January 2026 00:36:04 GMT (8:36 am on 7 January 2026, Asia/Singapore time). The data.gov.tw metadata timestamp 2026-08-14 11:54 is a quality check time, not a new data file.

The data is used under the [Open Government Data License, version 1.0](https://data.gov.tw/license) (政府資料開放授權條款-第1版). The license lets you copy, adapt, share and use the data for any purpose, including commercial use, as long as you credit the Ministry of Education, Department of Statistics, name the Kindergarten directory (幼兒園名錄), and link to the license. If attribution is missing, the rights granted under the license are void from the start. The license is compatible with Creative Commons Attribution 4.0 International. It does not grant patent or trademark rights, and ministry logos are not licensed. The data comes with no warranty. Use of this data does not mean the Ministry of Education endorses this API or its maker.

This API keeps only school year 114, removes the official bracket codes from county names, moves the postal bracket from the address into `postcode`, and omits mobile numbers that the directory listed as institution phones. Other names, addresses and phones were left as published. The official UTF-8 CSV and JSON both have an ASCII question mark in 95 of the 6,747 year-114 rows (37 names and 62 addresses) where a less common Chinese character was lost in the source. Those 95 rows were left unchanged. There are no map points, because the official directory has no coordinates.

This is a snapshot from 7 January 2026. It is a list of licensed kindergartens, not an enrollment or quality rating file, and it is not advice about any school. For the latest list, visit the [dataset page on data.gov.tw](https://data.gov.tw/dataset/6086).

[Back to the API list](../../README.md)
