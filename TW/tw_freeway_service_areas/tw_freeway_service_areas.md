# Taiwan Freeway Service Areas

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 22](https://img.shields.io/badge/records-22-lightgrey) ![updated 2026-09-02](https://img.shields.io/badge/updated-2026--09--02-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

All 22 Freeway Bureau service area sites on Taiwan National Freeways 1, 3 and 5 (15 named areas; northbound and southbound counted apart when they sit on opposite sides), with English and Chinese names, operator, contract dates, theme, car and large vehicle parking counts and a map point. Filter by the required `freeway`, then by area, direction, operator or a latitude and longitude range.

Rows by freeway: National Freeway 1 (11), National Freeway 3 (9) and National Freeway 5 (2). Rows by direction: northbound 7, southbound 7 and both directions same side 8.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_freeway_service_areas` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`freeway`). Optional: 18 fields. Disallowed: 7 fields.

`freeway` must be in every call. Use `freeway=*` to list all 22 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `freeway` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

`car_spaces`, `large_vehicle_spaces`, `accessible_spaces`, `rehab_bus_spaces`, `family_spaces`, `latitude` and `longitude` take ranges. Contract dates are text, so use a wildcard, such as `contract_end=2030*`.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `freeway` | Required | Yes | No | Freeway name in English: `National Freeway 1` (11), `National Freeway 3` (9) or `National Freeway 5` (2). Wildcards work, so `freeway=*` lists all 22 rows. Partial `*Freeway 1*` also matches. |
| `freeway_zh` | Optional | No | No | Exact Chinese freeway code: `國1` (11), `國3` (9) or `國5` (2). |
| `area_en` | Optional | Yes | No | Service area English name, such as `Hukou` (2), `Zhongli` (1) or `*ao*` (Su'ao). Wildcards work. 15 distinct areas. |
| `area_zh` | Optional | Yes | No | Chinese area name, such as `湖口` (2) or `中壢`. Wildcards work. |
| `direction` | Optional | Yes | No | Site direction in English: `northbound` (7), `southbound` (7) or `both directions same side` (8). Wildcards work. |
| `direction_zh` | Optional | No | No | Exact Chinese direction: `北上` (7), `南下` (7) or `南北向同一側` (8). |
| `address_en` | Optional | Yes | No | English address (careful translation from the Chinese source), such as `*Hukou*` or `*Taoyuan*`. Wildcards work. |
| `address_zh` | Optional | Yes | No | Chinese address as the Freeway Bureau lists it. Wildcards work. |
| `operator_en` | Optional | Yes | No | English operator name: `*FamilyMart*` (4), `*President Chain Store*` or `*7-Eleven*` (8), `*Naroon Lake*` (4) or `*Hsin Tung Yang*` (6). Wildcards work. |
| `operator_zh` | Optional | Yes | No | Chinese operator company name, such as `*統一超商*` (8) or `*全家*` (4). Wildcards work. |
| `contract_start` | Optional | Yes | No | Contract start date `YYYY-MM-DD`, such as `2019-06-01` or `2019*`. Text. Wildcards work. |
| `contract_end` | Optional | Yes | No | Contract end date `YYYY-MM-DD`, such as `2030*` (5) or `2026*`. Text. Wildcards work. |
| `car_spaces` | Optional | No | Yes | Number of small car parking spaces, from 110 to 709. Use a range, for example `car_spaces=300..800` (5). |
| `large_vehicle_spaces` | Optional | No | Yes | Total large vehicle spaces (coaches, trucks and trailers counted together from the source detail). Use a range if needed. |
| `accessible_spaces` | Optional | No | Yes | Number of accessible parking spaces. Use a range if needed. |
| `rehab_bus_spaces` | Optional | No | Yes | Fixed rehab bus parking spaces. `0` (9) means the site only has a drop off spot, not a fixed bay (see `rehab_bus_note_zh`). |
| `family_spaces` | Optional | No | Yes | Number of family or parent and child parking spaces. |
| `latitude` | Optional | No | Yes | Latitude of the site in decimal degrees (WGS 84), from 22.905878 to 25.008808. Use a range, for example `latitude=24..25` (9). |
| `longitude` | Optional | No | Yes | Longitude of the site in decimal degrees (WGS 84), from 120.265106 to 121.815535. Use a range, for example `longitude=121..122`. |
| `location_km` | Disallowed | No | No | Kilometer marker on the freeway as the source lists it, such as `國道1號55k`. |
| `theme_en` | Disallowed | No | No | Short English summary of the area theme, translated from the Freeway Bureau Chinese theme text. |
| `theme_zh` | Disallowed | No | No | Full Chinese theme text as the source gives it. |
| `large_vehicle_detail_zh` | Disallowed | No | No | Chinese detail string for large vehicle bays (coaches, trucks, trailers). |
| `rehab_bus_note_zh` | Disallowed | No | No | Chinese note when rehab buses only have a drop off area and no fixed bay. Blank when a fixed count is set. |
| `source` | Disallowed | No | No | Data source and credit for the row. |
| `updated` | Disallowed | No | No | Date the overview source dataset was last updated, `YYYY-MM-DD` (`2026-09-02`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `freeway` returns HTTP 400 with `Your request is missing the following required parameters: freeway. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.
* Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `freeway` | string | Freeway name in English. |
| `freeway_zh` | string | Freeway code in Chinese. |
| `area_en` | string | Service area name in English. |
| `area_zh` | string | Service area name in Chinese. |
| `direction` | string | Direction in English. |
| `direction_zh` | string | Direction in Chinese. |
| `location_km` | string | Kilometer marker. |
| `address_en` | string | Address in English. |
| `address_zh` | string | Address in Chinese. |
| `operator_en` | string | Operator in English. |
| `operator_zh` | string | Operator in Chinese. |
| `contract_start` | string | Contract start date, `YYYY-MM-DD`. |
| `contract_end` | string | Contract end date, `YYYY-MM-DD`. |
| `theme_en` | string | Short English theme summary. |
| `theme_zh` | string | Full Chinese theme text. |
| `car_spaces` | integer | Small car parking spaces. |
| `large_vehicle_spaces` | integer | Total large vehicle spaces. |
| `large_vehicle_detail_zh` | string | Chinese detail for large vehicle bays. |
| `accessible_spaces` | integer | Accessible parking spaces. |
| `rehab_bus_spaces` | integer | Fixed rehab bus spaces. |
| `rehab_bus_note_zh` | string | Note when there is only a drop off spot. Null when a fixed count is set. |
| `family_spaces` | integer | Family parking spaces. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `source` | string | Credit line for the row. |
| `updated` | string | Overview dataset date, `YYYY-MM-DD`. |

## Examples

One area on Freeway 1. Hukou northbound and southbound. This call returns 2 rows; the first:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_freeway_service_areas" --data-urlencode "freeway=*" --data-urlencode "area_en=Hukou"
```

```json
[
  {
    "__id": 2,
    "freeway": "National Freeway 1",
    "freeway_zh": "國1",
    "area_en": "Hukou",
    "area_zh": "湖口",
    "direction": "northbound",
    "direction_zh": "北上",
    "location_km": "國道1號86k",
    "address_en": "No. 158, Rongguang Rd., Fengshan Village, Hukou Township, Hsinchu County",
    "address_zh": "新竹縣湖口鄉鳳山村12鄰榮光路158號",
    "operator_en": "Naroon Lake Recreation Co., Ltd.",
    "operator_zh": "南仁湖育樂股份有限公司",
    "contract_start": "2019-06-01",
    "contract_end": "2030-05-31",
    "theme_en": "Nostalgic old street and LINE stickers creative theme, with sticker displays along the tree-lined avenue and an ecological pond.",
    "theme_zh": "以「懷舊老街與LINE文創」為區站主題，Line貼圖主題布置，與原有林蔭大道結合互動，在地風景林蔭大道與ㄌㄞˋ在湖口的空間營造，打造多樣性的生態池。",
    "car_spaces": 144,
    "large_vehicle_spaces": 48,
    "large_vehicle_detail_zh": "大客貨車38格 聯結車10格",
    "accessible_spaces": 5,
    "rehab_bus_spaces": 1,
    "rehab_bus_note_zh": null,
    "family_spaces": 4,
    "latitude": 24.860986,
    "longitude": 121.012073,
    "source": "Freeway Bureau, MOTC, National Freeway Service Area Overview (data.gov.tw 8161) and Parking Facilities (data.gov.tw 46801), Open Government Data License v1.0",
    "updated": "2026-09-02"
  },
  ...
]
```

Chinese area name. Qingshui. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_freeway_service_areas" --data-urlencode "freeway=*" --data-urlencode "area_zh=清水" | jq -r '.[] | [.area_en, .freeway, .direction, .operator_en] | @tsv'
```

```text
Qingshui	National Freeway 3	both directions same side	President Chain Store Corporation (7-Eleven)
```

Operator wildcard. FamilyMart sites. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_freeway_service_areas" --data-urlencode "freeway=*" --data-urlencode "operator_en=*FamilyMart*" | jq length
```

Car parking range. Sites with 300 to 800 car spaces. This call returns 5 rows:

```bash
curl -s "https://api.jael.ee/JLEE/tw_freeway_service_areas?freeway=*&car_spaces=300..800" | jq length
```

## Data source

The service areas come from the Freeway Bureau, MOTC, [National Freeway Service Area Overview](https://data.gov.tw/dataset/8161) and [National Freeway Service Area Parking Facilities](https://data.gov.tw/dataset/46801) on data.gov.tw, overview updated 2 September 2026, fetched on 5 November 2026. Licensed under the [Open Government Data License, version 1.0 (OGDL v1.0)](https://data.gov.tw/license), which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Changes: English area names, directions, operators, addresses and theme summaries were added; parking counts were joined from dataset 46801. The data is not guaranteed to be complete or accurate.

Credit lines:

* Source: Freeway Bureau, MOTC, National Freeway Service Area Overview (https://data.gov.tw/dataset/8161) and National Freeway Service Area Parking Facilities (https://data.gov.tw/dataset/46801), overview updated 2 September 2026, fetched 5 November 2026. Licensed under the Open Government Data License, version 1.0 (https://data.gov.tw/license).
* No logos. Not endorsed by the Freeway Bureau.

[Back to the API list](../../README.md)
