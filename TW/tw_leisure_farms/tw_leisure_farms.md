# Taiwan Leisure Farms

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 422](https://img.shields.io/badge/records-422-lightgrey) ![updated 2026-11-15](https://img.shields.io/badge/updated-2026--11--15-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

Ministry of Agriculture, 2026, Directory of Leisure Farms with Permits (data.gov.tw 24873), Open Government Data License v1.0, https://data.gov.tw/license

422 permitted leisure farms in Taiwan from the Ministry of Agriculture directory covering 1 to 30 November 2026, in 19 counties. The largest are Miaoli County (64), Yilan County (54) and Pingtung County (52). Each record has the Chinese name, the official English name when the ministry published one (31 farms), county, address, map point, website and a short features text. 21 official names are marked closed. No field is required, so you can filter by county, operating status, or a latitude and longitude box.

This is not an official Ministry of Agriculture service. Encode Chinese values in the query string, for example with `curl -G --data-urlencode`. County names use 臺 rather than 台, so search for `臺中市`, not `台中市`. The source has no official unique key. The API returns a generated `__id` that is unique on every row and is the stable record key. `name_zh` is unique in this snapshot, but names can pick up a closed note on a later refresh.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_leisure_farms` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per farm. All matches are returned. |

## Query parameters

Required: none. Optional: 20 fields. Disallowed: 2 fields.

A call with no filters returns all 422 records. Partial filters accept `*`, such as `name_zh=*停業中*`, `county=*Chiayi*` or `name_en=*Leisure Farm*`. `operating_status`, `has_website` and `certified_item` are exact matches. `latitude` and `longitude` accept a range with two dots, such as `latitude=24..25&longitude=121..122`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `name_zh` | Optional | Yes | No | Farm name in Chinese as the Ministry of Agriculture lists it, such as 三富休閒農場(紫森林) or 飛牛牧場休閒農場. Never blank. Unique on all 422 rows. 21 names include the official closed note 停業中, such as 月眉休閒農場（停業中）. Partial wildcards allowed, such as `*停業中*` (21) or `*牧場*` (2). |
| `name_en` | Optional | Yes | No | Farm name in English as the ministry published it, such as Sanfu Leisure Farm(三富休閒農場). Present on 31 rows, blank (`null`) on the rest. English names were not invented. Some official English values still include the Chinese name in parentheses. Partial wildcards allowed, such as `*Leisure Farm*` (25). |
| `county` | Optional | Yes | No | County or city in English, from official Chunghwa Post spellings, in 19 values. The largest are Miaoli County (64), Yilan County (54), Pingtung County (52), Taoyuan City (36) and Taichung City (32). `county=*Chiayi*` matches both Chiayi County and Chiayi City (14). Never blank. Partial wildcards allowed. |
| `county_zh` | Optional | Yes | No | County or city in Chinese, using the official 臺 form, such as 苗栗縣 (64) or 臺中市 (32). Never blank. Partial wildcards allowed. |
| `county_code` | Optional | Yes | No | Official county or city code as text, as the source published it, such as 10005 (Miaoli County) or 68000 (Taoyuan City). Kinmen County is 09020, with a leading zero, so this column is text. Never blank. 19 values, one per county. Partial wildcards allowed, such as `100*` or `09020` (1). |
| `district` | Optional | Yes | No | District, township or county-administered city in English, from official Chunghwa Post spellings, such as Dahu Township (15, 苗栗縣大湖鄉) or Hengchun Township (8, 屏東縣恆春鎮). A few names use a typographic apostrophe, such as Tai’an Township, Nan’ao Township, Ji’an Township and Da’an Dist. Never blank. 164 distinct values. Partial wildcards allowed, such as `*Township*` (270) or `Tai*`. |
| `district_zh` | Optional | Yes | No | District, township or county-administered city in Chinese as published, such as 大湖鄉 (15), 冬山鄉 (9) or 頭份市 (7). 164 distinct values. Never blank. Partial wildcards allowed, such as `*鄉` or `大湖鄉` (15). |
| `postal_code` | Optional | Yes | No | Three-digit postal code as published, such as 269 (冬山鄉) or 364 (大湖鄉). Never blank. 164 distinct values, one per district in this list. Text, even though none of these codes have a leading zero. One official oddity is kept as published: 大丘園休閒農場 in 彰化縣二水鄉 is listed as 529, while the Chunghwa Post district list uses 530 for 二水鄉. Partial wildcards allowed, such as `26*` or `529`. |
| `address_zh` | Optional | Yes | No | Street address in Chinese as published, such as 中山村新寮二路161巷88號. Some rows give only a village or lane, without the county in the address cell; use `county_zh` and `district_zh` with it. Blank (`null`) on 2 rows (三灣星光休閒農場 and 大崗山生態休閒農場). Partial wildcards allowed, such as `*中山村*`. |
| `address_en` | Optional | Yes | No | Street address in English as the ministry published it, such as No. 82, Ln. 161, Xinliao 2nd Rd., Zhongshan Vil. Present on the same 31 rows as `name_en`, blank (`null`) on the rest. English addresses were not invented. Partial wildcards allowed. |
| `phone` | Optional | Yes | No | Business landline, with the area code, such as 03 9588690, 037 951111, 049 2895938, 082 332074 or 0800 007111. Taiwan mobile numbers were omitted. 244 rows have a phone; the rest are blank (`null`), including 122 telephone cells that held only the placeholder (09) and cells with no usable landline. Partial wildcards allowed, such as `037*` (36) or `0800*` (2). |
| `website` | Optional | Yes | No | Website as published, such as https://www.sanfufarm.com.tw/. Present on 141 rows, blank (`null`) on 281. Links may be out of date. Partial wildcards allowed, such as `*https://*` (125). |
| `has_website` | Optional | No | No | `yes` if `website` is present (141) or `no` if it is blank (281). Never blank. Exact match. |
| `features_zh` | Optional | Yes | No | Official features text in Chinese, as published, after HTML entities were decoded and four embedded Taiwan mobile numbers were removed from the text. Present on 297 rows, blank (`null`) on 125. The text can run to a few short paragraphs. Partial wildcards allowed. |
| `features_en` | Optional | Yes | No | Official features text in English, as published, after HTML entities were decoded. Present on the same 31 rows as `name_en`, blank (`null`) on the rest. Partial wildcards allowed. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees, taken from the official Latitude column. Present on 405 rows, blank (`null`) on 17. The kept values run from 21.929886 to 25.291073. Range uses two dots, such as `latitude=24..25` (212). A hyphen such as `latitude=24-25` returns HTTP 400. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees, taken from the official Longitude column. Present on the same 405 rows as `latitude`, blank (`null`) on 17. The kept values run from 118.328564 (Kinmen) to 121.868334. Range uses two dots, such as `longitude=121..122`. Combined with latitude, `latitude=24..25&longitude=121..122` returns 104. A hyphen returns HTTP 400. |
| `certified_item` | Optional | No | No | English label from the official 通過認證項目 field: `experience` (20, 體驗) or `dining` (13, 餐飲). Blank (`null`) on 389 rows. Exact match. |
| `certified_item_zh` | Optional | Yes | No | Official certified item in Chinese as published: 體驗 (20) or 餐飲 (13). Blank (`null`) on 389 rows. Partial wildcards allowed, such as `體驗`. |
| `operating_status` | Optional | No | No | `closed` if the official Chinese name includes 停業中 (21), otherwise `operating` (401). This is taken from the name, not from a separate official status field. Never blank. Exact match. |
| `source` | Disallowed | No | No | Ministry of Agriculture, Directory of Leisure Farms with Permits (data.gov.tw dataset 24873), in all 422 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date the official JSON was fetched, YYYY-MM-DD (`2026-11-15`). The coverage window on every official row is 1 to 30 November 2026, the snapshot month, not a per-farm permit expiry. Text. Not filterable. |

## Response fields

`latitude` and `longitude` are numbers. `__id` is an integer. All other values are strings. Blank cells are returned as `null`. Every row has a unique `__id`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `name_zh` | string | Farm name in Chinese as the ministry lists it. Unique in this snapshot. |
| `name_en` | string | Farm name in English as the ministry published it, or `null` when none was published. |
| `county` | string | County or city in English, from official Chunghwa Post spellings. |
| `county_zh` | string | County or city in Chinese, using the official 臺 form. |
| `county_code` | string | Official county or city code as text. |
| `district` | string | District, township or county-administered city in English, from official Chunghwa Post spellings. |
| `district_zh` | string | District, township or county-administered city in Chinese as published. |
| `postal_code` | string | Three-digit postal code as published. |
| `address_zh` | string | Street address in Chinese, or `null` on 2 rows. |
| `address_en` | string | Street address in English as the ministry published it, or `null` when none was published. |
| `phone` | string | Business landline, or `null` when none was kept. |
| `website` | string | Website as published, or `null` when the source had none. |
| `has_website` | string | `yes` or `no`. |
| `features_zh` | string | Official features text in Chinese, or `null` when the source had none. |
| `features_en` | string | Official features text in English, or `null` when none was published. |
| `latitude` | number | Latitude in decimal degrees, or `null` when the official point was 0,0. |
| `longitude` | number | Longitude in decimal degrees, or `null` when the official point was 0,0. |
| `certified_item` | string | `experience` or `dining`, or `null` when the source had none. |
| `certified_item_zh` | string | Official certified item in Chinese, or `null` when the source had none. |
| `operating_status` | string | `operating` or `closed`. |
| `source` | string | Ministry of Agriculture, Directory of Leisure Farms with Permits (data.gov.tw dataset 24873). |
| `source_as_of` | string | Date the official JSON was fetched, `2026-11-15`. |

## Examples

三富休閒農場(紫森林) / Sanfu Leisure Farm in Yilan County, looked up by Chinese name. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_leisure_farms' --data-urlencode 'name_zh=三富休閒農場(紫森林)'
```

```json
{
  "__id": 423,
  "name_zh": "三富休閒農場(紫森林)",
  "name_en": "Sanfu Leisure Farm(三富休閒農場)",
  "county": "Yilan County",
  "county_zh": "宜蘭縣",
  "county_code": "10002",
  "district": "Dongshan Township",
  "district_zh": "冬山鄉",
  "postal_code": "269",
  "address_zh": "中山村新寮二路161巷88號",
  "address_en": "No. 82, Ln. 161, Xinliao 2nd Rd., Zhongshan Vil.",
  "phone": "03 9588690",
  "website": "https://www.sanfufarm.com.tw/",
  "has_website": "yes",
  "features_zh": "【目前僅開放住客入園，暫停一日遊及入園參觀】\n三富休閒農場設立於1989年，佔地面積約14公頃，緊鄰仁山植物園及新寮瀑布，景緻天然怡人，生態景觀豐富而特殊，四季各具不同風情，營業項目有：特色餐飲、休閒住宿、會議研習、高空繩索、自然旅遊、套裝行程、環境教育、創意DIY。\n三富休閒農場自成立以來，秉持對土地與生態環境保護的熱忱，結合四季時令，讓各種植物與昆蟲，保持其自然的食物鏈關係，利用自然生態工法維持生態平衡。這塊土地自轉型休閒農場以來，常年不使用農藥及化學肥料，以尊重環境倫理的方式，讓園內作...",
  "features_en": "Founded in 1989, Sanfu Leisure Farm is situated near Renshan Botanical Garden and Sinliao Waterfall with an area of approximately 10 hectares. The beautiful scenery there includes lime flowers, pomelo flowers, purple peonies, Chinese mulberry, and cinnamon flowers, as well as mountain spring water, butterflies, and a variety of cute animals.\nBind pieces of paper into fabric book covers to make a one-of-a-kind handmade book. Or you can paint gourds, wooden clogs, frog piggy banks, or taste some ...",
  "latitude": 24.617375,
  "longitude": 121.754694,
  "certified_item": "experience",
  "certified_item_zh": "體驗",
  "operating_status": "operating",
  "source": "Ministry of Agriculture, Directory of Leisure Farms with Permits (data.gov.tw dataset 24873)",
  "source_as_of": "2026-11-15"
}
```

Farms whose official Chinese name includes 停業中 (closed). This call returns 21 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_leisure_farms' --data-urlencode 'name_zh=*停業中*'
```

Farms in a map box in northern Taiwan. This call returns 104 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_leisure_farms' --data-urlencode 'latitude=24..25' --data-urlencode 'longitude=121..122'
```

Farms in Miaoli County. This call returns 64 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_leisure_farms' --data-urlencode 'county=Miaoli County'
```

Farms in Chiayi County or Chiayi City. This call returns 14 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_leisure_farms' --data-urlencode 'county=*Chiayi*'
```

Farms whose official English name includes Leisure Farm. This call returns 25 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_leisure_farms' --data-urlencode 'name_en=*Leisure Farm*'
```

## Data source

Ministry of Agriculture, 2026, Directory of Leisure Farms with Permits (data.gov.tw 24873), Open Government Data License v1.0, https://data.gov.tw/license

The records come from the official JSON for [取得許可登記證之休閒農場名錄 / Directory of Leisure Farms with Permits](https://data.gov.tw/dataset/24873), data.gov.tw dataset 24873 ([English dataset page](https://data.gov.tw/en/datasets/24873)), also listed as MOA open data id 376. The providing unit on the [MOA page](https://data.moa.gov.tw/open_detail.aspx?id=376) is the Agency of Rural Development and Soil and Water Conservation. The official JSON was fetched at 3:42:11 pm UTC on 15 November 2026 (11:42 pm on 15 November 2026, Asia/Singapore time). There was no HTTP Last-Modified header. The data.gov.tw portal modified date is 18 June 2026, the JSON resource quality check time is 27 October 2026, and the MOA detail page data-update time is 9 June 2026 10:05:07 (those last stamps look like Taiwan time, which is the same offset as Asia/Singapore). Official update frequency is irregular. The coverage window in this snapshot is 1 to 30 November 2026. `source_as_of` is `2026-11-15`. For the latest list, visit the [dataset page on data.gov.tw](https://data.gov.tw/dataset/24873).

The data is used under the [Open Government Data License, version 1.0](https://data.gov.tw/license) (政府資料開放授權條款-第1版). The license lets you copy, adapt, share and use the data for any purpose, including commercial use, as long as you credit the Ministry of Agriculture, name Directory of Leisure Farms with Permits (data.gov.tw 24873), and link to the license. If attribution is missing, the rights granted under the license are void from the start. The license is compatible with Creative Commons Attribution 4.0 International. It does not grant patent or trademark rights, and agency logos are not licensed. No Ministry of Agriculture logo is used. The data comes with no warranty and is not a recommendation, permission or endorsement. This is not an official Ministry of Agriculture service. Use of this data does not mean the Ministry of Agriculture endorses this API or its maker.

English county and district names are official Chunghwa Post spellings, used under the same license. Credit Chunghwa Post for those labels. English farm names, addresses and features text appear only where the ministry published them (31 rows).

Taiwan mobile numbers were omitted, including placeholder (09) telephone cells and four numbers that had been written into a features text. Those omitted numbers are not listed here. Official landlines were kept. The official Photo and Fax fields were not included.

Map points come only from the official file. 405 rows use the published latitude and longitude. 17 rows were published at 0,0 and were left blank. No missing point was filled in from another gazetteer.

[Back to the API list](../../README.md)
