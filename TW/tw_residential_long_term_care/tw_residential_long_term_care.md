# Taiwan Residential Long-term Care Institutions

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 162](https://img.shields.io/badge/records-162-lightgrey) ![updated 2026-08-31](https://img.shields.io/badge/updated-2026--08--31-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

162 residential long-term care institutions in Taiwan, as listed by the Ministry of Health and Welfare, Department of Long-term Care, in the official register for the end of August 2026 (住宿式長照機構清冊, 115年8月底). Each record has the official serial number, Traditional Chinese name, county, district, address, licensed beds and open beds. No field is required, so you can filter by county, district, name or a bed count range.

Names, counties, districts and addresses are in Traditional Chinese only. The source has no English name. County names use 臺 rather than 台, so search for `臺中市` or `臺北市`, not `台中市` or `台北市`. A filter of `台中市` returns 0 rows. Encode Chinese values in the query string, for example with `curl -G --data-urlencode`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_residential_long_term_care` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per institution. All matches are returned. |

## Query parameters

Required: none. Optional: 7 fields. Disallowed: 2 fields.

A call with no filters returns all 162 records. Partial filters accept `*`, such as `name=*佳醫*`, `county=*北市*` or `address=*三峽區*`. `list_no`, `licensed_beds` and `open_beds` accept a range in the form `min..max`, for example `licensed_beds=100..200`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `list_no` | Optional | No | Yes | Official serial number (序號) in this edition of the register, 1 to 162, never blank. Unique in this file, but it is a row number for this edition, not a stable institution code, and it may change when the Ministry issues a new list. Official serials 45 and 50 are the same institution (see the data notes). Range only, both bounds required, for example `1..26` (26, all 新北市 in this edition) or `45..45` (1). |
| `name` | Optional | Yes | No | Official institution name in Traditional Chinese, as the Ministry lists it, such as 太陽長照社團法人附設新北市私立林口住宿長照機構. Never blank. 161 of 162 names are unique; list numbers 45 and 50 share the name 佳恩長照社團法人私立楊梅佳醫住宿長照機構. Some official names use 台 rather than 臺, such as 台中市 and 台東縣 inside a title, while the county column always uses 臺. Some titles include an operator clause (委託…經營管理); those are organization names, not a person in charge. Partial wildcards allowed, such as `*佳醫*` (14), `*綜合*` (22) or `*委託*` (9). |
| `county` | Optional | Yes | No | County or city as the Ministry lists it, such as 新北市 or 臺中市. Never blank. 19 values, using 臺 rather than 台. This edition has no row for 新竹市, 金門縣 or 連江縣. Counts: 新北市 (26), 桃園市 (19), 屏東縣 (18), 高雄市 (17), 臺中市 (16), 臺北市 (12), 臺南市 (10), 宜蘭縣 (8), 南投縣 (8), 彰化縣 (7), 花蓮縣 (5), 苗栗縣 (3), 雲林縣 (3), 臺東縣 (3), 嘉義縣 (2), 嘉義市 (2), 新竹縣 (1), 澎湖縣 (1), 基隆市 (1). Partial wildcards allowed, such as `新北市` or `*北市*` (38, 新北市 and 臺北市). |
| `district` | Optional | Yes | No | Township, city or district taken from the official address after the county, such as 林口區, 平鎮區 or 羅東鎮. Never blank. 104 county and district pairs. A few district names appear in more than one county; 西區 in this list is only 嘉義市 (2). 三峽區 is only 新北市 (4). 平鎮區 is only 桃園市 (3). Partial wildcards allowed, such as `三峽區` (4) or `平鎮區` (3). |
| `address` | Optional | Yes | No | Street address in Traditional Chinese, as the Ministry lists it, such as 新北市林口區14鄰吉林街30號. Never blank. Every address starts with the same county as the county column. Official wording was left as published, including a repeated 新北市八里區 on list number 9, a space before an enumeration mark on list number 3, and a space in 港子尾 78 on list number 159. Partial wildcards allowed, such as `*三峽區*` (4) or `*中豐路*` (1). |
| `licensed_beds` | Optional | No | Yes | Licensed bed count (許可床) as an integer, 9 to 200, never blank. The 162 institutions have 17,358 licensed beds in total. Range only, both bounds required, for example `100..200` (83) or `9..50` (31). A hyphen range such as `100-200` returns HTTP 400. |
| `open_beds` | Optional | No | Yes | Open bed count (開放床) as an integer, 9 to 200, never blank. The 162 institutions have 14,806 open beds in total. Open beds are never higher than licensed beds: 116 rows have the same figure for both, and 46 have fewer open beds than licensed beds. Range only, both bounds required, for example `200..200` (6) or `9..50` (48). |
| `source` | Disallowed | No | No | Ministry of Health and Welfare, Department of Long-term Care, Residential long-term care institution register (住宿式長照機構清冊), Taiwan, in all 162 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the official list, YYYY-MM-DD (`2026-08-31`), meaning the register as at the end of August 2026 (115年8月底). Text. Not filterable. |

## Response fields

`__id` is an integer. `list_no`, `licensed_beds` and `open_beds` are integers. All other values are strings. There are no blank fields in this snapshot. `list_no` is unique in this edition, but it is a serial for this list, not a stable institution code, and official serials 45 and 50 name the same facility, so `__id` is kept as a row key.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `list_no` | integer | Official serial number in this edition of the register, 1 to 162. |
| `name` | string | Official institution name in Traditional Chinese. |
| `county` | string | County or city, using 臺 rather than 台. |
| `district` | string | Township, city or district. |
| `address` | string | Street address in Traditional Chinese. |
| `licensed_beds` | integer | Licensed bed count, 9 to 200. |
| `open_beds` | integer | Open bed count, 9 to 200. |
| `source` | string | Ministry of Health and Welfare, Department of Long-term Care, Residential long-term care institution register (住宿式長照機構清冊), Taiwan. |
| `source_as_of` | string | Date of the official list, `2026-08-31`. |

## Examples

太陽長照社團法人附設新北市私立林口住宿長照機構, the first row in this edition. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_residential_long_term_care' --data-urlencode 'list_no=1'
```

```json
[
  {
    "__id": 1,
    "list_no": 1,
    "name": "太陽長照社團法人附設新北市私立林口住宿長照機構",
    "county": "新北市",
    "district": "林口區",
    "address": "新北市林口區14鄰吉林街30號",
    "licensed_beds": 42,
    "open_beds": 42,
    "source": "Ministry of Health and Welfare, Department of Long-term Care, Residential long-term care institution register (住宿式長照機構清冊), Taiwan",
    "source_as_of": "2026-08-31"
  }
]
```

Institutions in New Taipei City. This call returns 26 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_residential_long_term_care' --data-urlencode 'county=新北市'
```

Institutions with 100 to 200 licensed beds. This call returns 83 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_residential_long_term_care' --data-urlencode 'licensed_beds=100..200'
```

Institutions whose official name includes 委託. This call returns 9 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_residential_long_term_care' --data-urlencode 'name=*委託*'
```

## Data source

Source: Ministry of Health and Welfare, Department of Long-term Care, [Residential long-term care institution register (住宿式長照機構清冊)](https://1966.gov.tw/LTC/cp-6443-71027-207.html), the official list as at the end of August 2026. The attachment is 住宿式長照機構清冊-簡版名單(115年8月底).pdf, also available from the [Ministry download page](https://www.mohw.gov.tw/dl-88905-86fa53f7-ec5d-47e8-acc1-0ffdb717b894.html). The 1966 page was last updated on 27 October 2026 (115-10-27). The official PDF was created on 7 October 2026 at 3:22 pm, Asia/Singapore time (the same offset as Taiwan). This table is that official 162-institution register. District was taken from the official address after the county.

The data is used under the [Open Government Data License, version 1.0](https://data.gov.tw/license) (政府資料開放授權條款-第1版), as declared in the [1966 open data declaration](https://1966.gov.tw/LTC/cp-6476-70761-207.html). The license lets you copy, adapt, share and use the data for any purpose, including commercial use, as long as you credit the Ministry of Health and Welfare, Department of Long-term Care, name the Residential long-term care institution register (住宿式長照機構清冊), and link to the license. If attribution is missing, the rights granted under the license are void from the start. The license is compatible with Creative Commons Attribution 4.0 International. It does not grant patent or trademark rights, and ministry logos are not licensed. The data comes with no warranty. This is a list of licensed residential institutions and their bed counts, not a quality rating, and it is not advice about any facility. Use of this data does not mean the Ministry of Health and Welfare endorses this API or its maker.

Official list numbers 45 and 50 are the same institution, 佳恩長照社團法人私立楊梅佳醫住宿長照機構, at the same 桃園市楊梅區 address, with 90 licensed beds and 90 open beds; both official rows were kept. List numbers 61 and 68 share the same 臺中市西屯區自立路106號 site; 68 is the official (A) campus of 佑全, as published. The official name for list number 152 repeats 普及 (普及普及協會). A few official addresses have small wording quirks that were left unchanged: list number 9 repeats 新北市八里區, list number 3 writes "2 、3", and list number 159 has a space in "港子尾 78". Some official names use 台 rather than 臺. There are no map points, because the official register has no coordinates. The register has no phone numbers and no director or person in charge field.

This is a snapshot of the register as at the end of August 2026. For the latest list, visit the [1966 register page](https://1966.gov.tw/LTC/cp-6443-71027-207.html).

[Back to the API list](../../README.md)
