# Taiwan Night Markets

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 107](https://img.shields.io/badge/records-107-lightgrey) ![languages EN | ZH](https://img.shields.io/badge/languages-EN%20%7C%20ZH-orange)

Night markets in 15 cities and counties across Taiwan, in English and Traditional Chinese. Each row is one market with its name, city, district, address, open days, open hours, stall count where known, a short note and map coordinates. Hsinchu and Changhua are not included, because no usable open data set was found for them.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_night_markets` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 15 fields. Disallowed: 3 fields.

The `city_en` parameter is required on every call. Use `city_en=*` to list all markets. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city_en` | Required | Yes | No | City or county in English, such as `Tainan City`, `Taipei City`, `Kaohsiung City` or `Nantou County`. `city_en=*` lists all 107 markets and `city_en=*Taipei*` matches Taipei City and New Taipei City. |
| `name_en` | Optional | Yes | No | Market name in English, for example `*Shilin*`. |
| `name_zh` | Optional | Yes | No | Market name in Traditional Chinese, for example `*士林*`. |
| `city_zh` | Optional | Yes | No | City or county in Traditional Chinese, for example `臺南市`. |
| `district_en` | Optional | Yes | No | District or township in English, for example `Shilin District` or `Fenglin Township`. |
| `district_zh` | Optional | Yes | No | District or township in Traditional Chinese, for example `士林區`. |
| `address_en` | Optional | Yes | No | Address in English, for example `*Zhongzheng Rd.*`. |
| `address_zh` | Optional | Yes | No | Address in Traditional Chinese, for example `*中正路*`. |
| `open_days_en` | Optional | Yes | No | Open days in English, such as `Daily`, `Sun` or `Wed, Sat`. Use `*Sat*` to find markets open on Saturday. Blank for 22 markets. |
| `open_days_zh` | Optional | Yes | No | Open days in Traditional Chinese, such as `每日`. Blank for 22 markets. |
| `opening_type_en` | Optional | No | No | `Daily`, `Specific weekdays` or `Weekends only`. Blank for 22 markets. |
| `stall_count` | Optional | No | Yes | Number of stalls, from 15 to 549. Range supported, for example `stall_count=100..300`. Filled for 27 markets. |
| `features_en` | Optional | Yes | No | Keyword search in the short English note, for example `*oyster*`. |
| `features_zh` | Optional | Yes | No | Keyword search in the short Chinese note, for example `*蚵仔煎*`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees, from 22.592225 to 25.14266. Use a range to search a box. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees, from 120.157291 to 121.789748. Use a range to search a box. |
| `open_hours` | Disallowed | No | No | Opening hours as text, such as `17:00-24:00`. Filled for about 55 percent of markets. |
| `source` | Disallowed | No | No | Agencies and data set links each row was built from. |
| `updated` | Disallowed | No | No | Date the row was last checked, such as `2026-10-27`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name_en` | string | Market name in English. |
| `name_zh` | string | Market name in Traditional Chinese. |
| `city_en` | string | City or county in English. |
| `city_zh` | string | City or county in Traditional Chinese. |
| `district_en` | string | District or township in English. |
| `district_zh` | string | District or township in Traditional Chinese. |
| `address_en` | string | Address in English, with postal code. |
| `address_zh` | string | Address in Traditional Chinese. |
| `open_days_en` | string | Open days in English. Empty when the source gives none. |
| `open_days_zh` | string | Open days in Traditional Chinese. Empty when the source gives none. |
| `open_hours` | string | Opening hours in 24 hour form. Empty when not known. |
| `stall_count` | integer, null | Number of stalls. Null when not known. |
| `features_en` | string | Short note on the market in English. Empty when none. |
| `features_zh` | string | Short note on the market in Traditional Chinese. Empty when none. |
| `latitude` | number | Latitude in decimal degrees. About 40 values come from the source data and the rest are approximate road or site points. |
| `longitude` | number | Longitude in decimal degrees. |
| `opening_type_en` | string | `Daily`, `Specific weekdays` or `Weekends only`. Empty when not known. |
| `source` | string | Agencies and data set links the row was built from. |
| `updated` | string | Date the row was last checked. |

## Examples

Large markets in Taichung, with 150 to 600 stalls.

```bash
curl -s "https://api.jael.ee/JLEE/tw_night_markets?city_en=Taichung%20City&stall_count=150..600"
```

Response (4 rows, first row shown, `source` shortened):

```json
[
  {
    "__id": 258,
    "name_en": "Fengjia Night Market",
    "name_zh": "逢甲觀光夜市攤販集中區",
    "city_en": "Taichung City",
    "city_zh": "臺中市",
    "district_en": "Xitun District",
    "district_zh": "西屯區",
    "address_en": "Wenhua Rd., south section (from the Feng Chia University side gate to Fuxing Rd.), Xitun District, Taichung City 407, Taiwan",
    "address_zh": "臺中市西屯區文華路南段（逢甲大學側門至福星路）",
    "open_days_en": "Daily",
    "open_days_zh": "每周 7天 (星期一至日)",
    "open_hours": "16:00-01:00",
    "stall_count": 184,
    "features_en": "Draws young shoppers with creative food and low priced fashion, and is listed among Taichung's top ten attractions.",
    "features_zh": "以創意美食與平價流行服飾吸引年輕人，是臺中十大觀光景點之一。",
    "latitude": 24.17951,
    "longitude": 120.64632,
    "opening_type_en": "Daily",
    "source": "Taichung City Government Economic Development Bureau: https://data.gov.tw/dataset/85028 | ...",
    "updated": "2026-10-27"
  }
]
```

Tainan markets open on Saturday, with a wildcard on the open days. This call returns 9 markets. The names:

```bash
curl -s "https://api.jael.ee/JLEE/tw_night_markets?city_en=*Tainan*&open_days_en=*Sat*" | jq -r '.[].name_en'
```

```text
Anzhong Night Market
Dayi Night Market
Garden Night Market
Houbicuo Night Market
Longtian Night Market
Wusheng Night Market
Xuejia Traditional Night Market
Yujing Night Market
Zhongzhouliao Night Market
```

## Data source

Taiwan government open data on [data.gov.tw](https://data.gov.tw): the national night market list from the Department of Commerce Development, Ministry of Economic Affairs ([data set 95760](https://data.gov.tw/dataset/95760)), the Taichung ([85028](https://data.gov.tw/dataset/85028)), Taoyuan ([25965](https://data.gov.tw/dataset/25965)) and Tainan ([53634](https://data.gov.tw/dataset/53634)) city lists, and the Tourism Administration attractions data ([7777](https://data.gov.tw/dataset/7777)). Licence: Open Government Data License v1, credit to each agency. English text is edited by hand, and each row names its sources in `source`. Hours and open days can change, so check before you travel.

[Back to the API list](../../README.md)
