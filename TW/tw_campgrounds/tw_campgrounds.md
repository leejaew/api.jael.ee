# Taiwan Campgrounds

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,807](https://img.shields.io/badge/records-1%2C807-lightgrey) ![updated 2026-04-28](https://img.shields.io/badge/updated-2026--04--28-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

1,807 campgrounds in Taiwan from the Tourism Administration, Ministry of Transportation and Communications, Campground Information for the Whole of Taiwan (全台露營場資料), as listed in the official CSV last modified on 28 April 2026. 203 sites are listed as compliant and 1,604 as in violation. 1,048 are marked as being in an indigenous area. Each record has the Chinese name, county and district in Chinese and in official Chunghwa Post English, a county code, operating status, official map point, address, phone, website, compliance status, violation note, indigenous-area flag, and a setup date or setup note when the source gives one. No field is required, so you can filter by name, county, compliance or a map box.

Campground names stay in Chinese, because the source has no English names. Encode Chinese values in the query string, for example with `curl -G --data-urlencode`. County names use 臺 rather than 台, so search for `臺中市`, not `台中市`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_campgrounds` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per campground. All matches are returned. |

## Query parameters

Required: none. Optional: 18 fields. Disallowed: 2 fields.

A call with no filters returns all 1,807 records. Partial filters accept `*`, such as `name_zh=*農場*`, `county=*Hsinchu*` or `website=*facebook.com*`. `operating_status`, `compliance` and `indigenous_area` are exact match. `lat` and `lng` accept a range in the form `min..max`, for example `lat=24..25`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `name_zh` | Optional | Yes | No | Campground name in Chinese as the Tourism Administration lists it, such as 稻庄休閒農場 or 河畔露營區. Never blank. Unique on 1,806 of 1,807 rows; 河畔露營區 appears twice, once in Pingtung County (牡丹鄉) and once in Miaoli County (南庄鄉). Partial wildcards allowed, such as `*農場*` (190) or `河畔露營區` (2). |
| `county` | Optional | Yes | No | County or city in English, from official Chunghwa Post spellings, in 20 values. The largest are Miaoli County (401), Nantou County (384), Hsinchu County (273), Taoyuan City (137), Taichung City (111) and Kaohsiung City (109). `county=*Hsinchu*` matches both Hsinchu County and Hsinchu City (275). Never blank. Partial wildcards allowed. |
| `county_zh` | Optional | Yes | No | County or city in Chinese as published, using the official 臺 form, such as 苗栗縣 (401) or 臺中市 (111). Never blank. Partial wildcards allowed. |
| `county_code` | Optional | Yes | No | Official county or city code as text, as the source published it, such as `10005` (Miaoli County) or `68000` (Taoyuan City). Kinmen County is `9020`, four digits, with no leading zero in the source. Never blank. 20 values, one per county. Partial wildcards allowed, such as `100*` or `9020`. |
| `district` | Optional | Yes | No | District, township or county-administered city in English, from official Chunghwa Post spellings, such as Tai’an Township (146, 苗栗縣泰安鄉), Fuxing Dist. (114, 桃園市復興區) or Jianshi Township (113). A few names use a typographic apostrophe, such as Tai’an Township, Su’ao Township, Nan’ao Township, Ren’ai Township and Da’an Dist. Never blank. Partial wildcards allowed, such as `*Township*` or `Tai*`. |
| `district_zh` | Optional | Yes | No | District, township or county-administered city in Chinese as published, such as 泰安鄉 (146), 復興區 (114) or 尖石鄉 (113). 171 distinct values. Never blank. Partial wildcards allowed, such as `*鄉` or `復興區`. |
| `operating_status` | Optional | No | No | `operating` on all 1,807 rows, from the official 營業中 label. This snapshot lists only sites marked as operating. Exact match. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, from the official 緯度 column. Present on 1,737 rows, `null` on 70. The kept values run from 21.91121807203 to 25.2892131939382. Range only, both bounds required, for example `24..25` (1,121). A hyphen range returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, from the official 經度 column. Present on the same 1,737 rows as `lat`, `null` on 70. The kept values run from 118.355405 (Kinmen) to 121.936654158508. Range only, both bounds required, for example `121..122` (712). Combined with `lat=24..25`, `lng=121..122` returns 594 rows. A hyphen range returns HTTP 400. |
| `address` | Optional | Yes | No | Street address in Chinese as published, such as 宜蘭縣三星鄉福和路71巷16號. Some rows give only a lane or village, without the county in the address cell; use `county_zh` and `district_zh` with it. Blank on 279 rows. Partial wildcards allowed, such as `*福和路*`. |
| `phone` | Optional | Yes | No | Business landline, with the area code, such as `03 9892809`, `049 2977258`, `037 873887` or `082 318823`. Taiwan mobile numbers were omitted, and one telephone cell that mixed mobiles with a personal name was left blank. 229 rows have a phone; the rest are `null`. Partial wildcards allowed, such as `037*` (41) or `049*` (42). |
| `website` | Optional | Yes | No | Website as published. Blank on 874 rows. 516 of the 933 nonblank values are Facebook links. Links may be out of date. Partial wildcards allowed, such as `*facebook.com*` (516). |
| `compliance` | Optional | No | No | English label from the official two-way field: `compliant` (203, 符合相關法規露營場) or `in_violation` (1,604, 違反相關法規露營場). Exact match. |
| `compliance_zh` | Optional | Yes | No | Official status in Chinese as published: 符合相關法規露營場 (203) or 違反相關法規露營場 (1,604). Never blank. Partial wildcards allowed, such as `*符合*` (203). |
| `violation` | Optional | Yes | No | Official note of the rule cited when a site is listed as in violation, as published, such as 區域計畫法 (680) or 不符合土地管制法規 (410). There are 61 distinct nonblank values, and wording is not standardized. Blank on 230 rows, including 58 rows marked `in_violation` with no rule named, and most compliant rows. 31 compliant rows still have text in this field (7 of those say 無). Partial wildcards allowed, such as `*區域計畫法*` or `*農業發展條例*`. |
| `indigenous_area` | Optional | No | No | `yes` if the source marks the site as in an indigenous area (是, 1,048 rows) or `no` if it marks 否 (759). Never blank. Exact match, `yes` or `no`. |
| `setup_date` | Optional | Yes | No | Date the campground was set up, as text YYYY-MM-DD, converted from official ROC calendar numbers. Present on 118 rows, from 2004-10-01 to 2026-04-15. Years: 2025 (40), 2024 (21), 2026 (16), 2023 (11), and fewer in earlier years. Blank on the rest. Partial wildcards allowed, such as `2025*` (40) or `2026*` (16). Range is off because the column is text. |
| `setup_note` | Optional | Yes | No | Official note left in the setup-time column when the source did not give a date: 休閒農場請洽主管機關（農業部） (56) or 公有露營場請洽主管機關 (33). Blank on the other 1,718 rows. Partial wildcards allowed, such as `*農業部*` (56). |
| `source` | Disallowed | No | No | Tourism Administration, MOTC, Campground Information for the Whole of Taiwan (data.gov.tw dataset 132066), in all 1,807 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the official CSV file, YYYY-MM-DD (`2026-04-28`). Text. Not filterable. |

## Response fields

`__id` is an integer. `lat` and `lng` are numbers, or `null` when there is no map point. All other values are strings, or `null` when the source has no value, or when a mobile number or a personal name in a telephone cell was omitted. There is no official unique key in the source, and `name_zh` is not unique, so `__id` is the stable row key.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name_zh` | string | Campground name in Chinese. |
| `county` | string | County or city in official Chunghwa Post English. |
| `county_zh` | string | County or city in Chinese, using 臺 rather than 台. |
| `county_code` | string | Official county or city code, kept as text. |
| `district` | string | District, township or county-administered city in official Chunghwa Post English. |
| `district_zh` | string | District, township or county-administered city in Chinese. |
| `operating_status` | string | `operating` on every row. |
| `lat` | number | Latitude in decimal degrees, or `null` on 70 rows. |
| `lng` | number | Longitude in decimal degrees, or `null` on 70 rows. |
| `address` | string | Street address in Chinese, or `null` on 279 rows. |
| `phone` | string | Business landline, or `null` on 1,578 rows. |
| `website` | string | Website as published, or `null` on 874 rows. |
| `compliance` | string | `compliant` or `in_violation`. |
| `compliance_zh` | string | Official status in Chinese. |
| `violation` | string | Official violation note, or `null` on 230 rows. |
| `indigenous_area` | string | `yes` or `no`. |
| `setup_date` | string | Setup date, YYYY-MM-DD, or `null` when the source gave no date. |
| `setup_note` | string | Official setup note, or `null` on 1,718 rows. |
| `source` | string | Tourism Administration, MOTC, Campground Information for the Whole of Taiwan (data.gov.tw dataset 132066). |
| `source_as_of` | string | Date of the official CSV file, `2026-04-28`. |

## Examples

稻庄休閒農場, by official Chinese name. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_campgrounds' --data-urlencode 'name_zh=稻庄休閒農場'
```

```json
[
  {
    "__id": 1,
    "name_zh": "稻庄休閒農場",
    "county": "Yilan County",
    "county_zh": "宜蘭縣",
    "county_code": "10002",
    "district": "Sanxing Township",
    "district_zh": "三星鄉",
    "operating_status": "operating",
    "lat": 24.6693609205923,
    "lng": 121.628050820173,
    "address": "宜蘭縣三星鄉福和路71巷16號",
    "phone": "03 9892809",
    "website": "https://www.facebook.com/daozhuangfarm/?locale=zh_TW",
    "compliance": "compliant",
    "compliance_zh": "符合相關法規露營場",
    "violation": null,
    "indigenous_area": "no",
    "setup_date": null,
    "setup_note": "休閒農場請洽主管機關（農業部）",
    "source": "Tourism Administration, MOTC, Campground Information for the Whole of Taiwan (data.gov.tw dataset 132066)",
    "source_as_of": "2026-04-28"
  }
]
```

Campgrounds with latitude from 24 to 25. This call returns 1,121 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_campgrounds' --data-urlencode 'lat=24..25'
```

Campgrounds with longitude from 121 to 122. This call returns 712 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_campgrounds' --data-urlencode 'lng=121..122'
```

The same two ranges together, a map box. This call returns 594 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_campgrounds' --data-urlencode 'lat=24..25' --data-urlencode 'lng=121..122'
```

Sites listed as compliant. This call returns 203 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_campgrounds' --data-urlencode 'compliance=compliant'
```

## Data source

Source: Tourism Administration, Ministry of Transportation and Communications, [Campground Information for the Whole of Taiwan (全台露營場資料)](https://data.gov.tw/dataset/132066), data.gov.tw dataset 132066. The official file used for this snapshot is the [CSV on media.taiwan.net.tw](https://media.taiwan.net.tw/od/04_TD/全台露營場資料.csv). The file's HTTP Last-Modified time is 28 April 2026 03:55:12 GMT (11:55 am on 28 April 2026, Asia/Singapore time). The data.gov.tw dataset page was later touched on 15 September 2026, but the CSV itself did not change. The portal is updated about every three months.

The data is used under the [Open Government Data License, version 1.0](https://data.gov.tw/license) (政府資料開放授權條款-第1版). The license lets you copy, adapt, share and use the data for any purpose, including commercial use, as long as you credit the Tourism Administration, MOTC, name Campground Information for the Whole of Taiwan (全台露營場資料), and link to the license. If attribution is missing, the rights granted under the license are void from the start. The license is compatible with Creative Commons Attribution 4.0 International. It does not grant patent or trademark rights, and agency logos are not licensed. The data comes with no warranty and is not a recommendation, permission or endorsement. Use of this data does not mean the Tourism Administration or MOTC endorses this API or its maker.

English county and district names are official Chunghwa Post spellings, used under the same license. Credit Chunghwa Post for those labels. Campground names stay in Chinese, because the source has no English names. This API omits Taiwan mobile numbers and one personal name that appeared in a telephone cell, repairs a few broken encodings in names, websites and a violation note, converts official ROC setup dates to YYYY-MM-DD, and leaves two out-of-range official map points blank (草香居露營場 in 高雄市美濃區, and 三芝老農夫農場 in 新北市三芝區). Placeholder addresses 無 and 尚未設置地址 were left blank. Other values were left as published.

Map points come only from the official file. 1,737 rows have a point. 70 rows have none: 68 had no point in the source, and the two out-of-range points above were omitted. Five official points are shared by two rows each. 湖西苗圃童軍露營地 in Penghu County is published at 24.61867502, 121.7294211, which is in northeast Taiwan rather than Penghu; that point was kept as published. No missing point was filled in from another gazetteer.

This is a snapshot from 28 April 2026. 203 sites are listed as compliant and 1,604 as in violation. Some official rows disagree between the status and the violation note. For the latest list, visit the [dataset page on data.gov.tw](https://data.gov.tw/dataset/132066).

[Back to the API list](../../README.md)
