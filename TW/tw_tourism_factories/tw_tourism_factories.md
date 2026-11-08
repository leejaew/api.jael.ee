# Taiwan tourism factories

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 158](https://img.shields.io/badge/records-158-lightgrey) ![updated 2024-07-30](https://img.shields.io/badge/updated-2024--07--30-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

158 tourism factories in Taiwan's Industrial Development Administration source list, across 18 counties and cities, with Chinese names, booking phones, addresses, websites and map points. English names and categories cover 155 factories. Filter by county, name, category, district or a latitude and longitude box.

Ten map points are township centers, not factory sites. The source marks Republic of Chocolate, 宏亞食品-巧克力共和國(休館改裝), closed for renovation. The source list is old; check the venue before a visit.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_tourism_factories` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per factory, with all 15 fields and no `__id`. |
| Record limit | None. A call without filters returns all 158 rows. |

## Query parameters

Required: none. Optional: 13 fields. Disallowed: 2 fields.

All filters are optional. Partial matching uses `*` wildcards, text matches ignore case, and filters are combined with AND. `lat` and `lng` accept ranges with two dots (`min..max`). Other fields are strings and accept exact matches unless Partial is Yes.

Disallowed fields are returned but cannot be used as filters. Filtering on `source` or `updated` returns HTTP 400. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `name_en` | Optional | Yes | No | English factory name from the official English directory. Present in 155 rows; null in 3. Use wildcards, such as `*Cake*`. |
| `name_zh` | Optional | Yes | No | Chinese factory name. All 158 names are distinct. Source notes, including the renovation closure, stay in the name. |
| `category` | Optional | Yes | No | English category from the English directory. 27 categories; null in 3 rows. Examples: Cuisine (24), Cake and Pastry (20), Winery (16) and Biotechnology (13). |
| `region` | Optional | No | No | Exact region: North (39), Central (55), South (44) or East (20). |
| `county_city` | Optional | No | No | Exact county or city in Chinese. County names use 臺 consistently. Values: 南投縣 (13); 嘉義市 (2); 嘉義縣 (9); 基隆市 (1); 宜蘭縣 (16); 屏東縣 (4); 彰化縣 (15); 新北市 (13); 新竹市 (1); 新竹縣 (2); 桃園市 (22); 臺中市 (10); 臺南市 (22); 臺東縣 (1); 花蓮縣 (3); 苗栗縣 (7); 雲林縣 (10); 高雄市 (7). |
| `district` | Optional | Yes | No | District, township or city taken from the Chinese address. Partial matching is allowed. |
| `postcode` | Optional | No | No | Published five or six digit postcode as text. Exact match; no wildcards. |
| `address_zh` | Optional | Yes | No | Published Chinese address, with whitespace cleaned. Partial matching is allowed. |
| `phone` | Optional | No | No | Main Taiwan landline as text, in `+886 area local` format, with `ext` when given. Exact match. |
| `website` | Optional | Yes | No | Factory website. The published scheme is kept unless its https version answered. Some links may be out of date. Partial matching is allowed. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, rounded to six decimal places. Use two dots for a range, such as `24..25`. Ten points are only township centers. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, rounded to six decimal places. Use two dots for a range, such as `120..121`. |
| `coord_source` | Optional | No | No | Exact map point source: Tourism Administration (148) or Chunghwa Post township center (10). Township centers are rough area locations, not factory sites. |
| `source` | Disallowed | No | No | IDA source page URL, the same in every row. Returned but not filterable. |
| `updated` | Disallowed | No | No | Portal metadata update date as text: `2024-07-30`. Returned but not filterable. This is not a per venue check date. |

## Response fields

`lat` and `lng` are numbers. All other fields are strings. Blank source cells are returned as `null`.

| Field | Type | Description |
|---|---|---|
| `name_en` | string | English factory name. Null for three factories without an English directory match. |
| `name_zh` | string | Chinese factory name, including any source closure note. |
| `category` | string | English factory category. Null for three factories without an English directory match. |
| `region` | string | North, Central, South or East. |
| `county_city` | string | County or city in Chinese, with 臺 used consistently. |
| `district` | string | District, township or city from the address. |
| `postcode` | string | Published postcode as text. |
| `address_zh` | string | Chinese street address. |
| `phone` | string | Main Taiwan landline in international format, with the first extension when given. |
| `website` | string | Factory website as published, changed to https only where it answered. |
| `lat` | number | Latitude in decimal degrees. Ten rows use an area center instead of a factory point. |
| `lng` | number | Longitude in decimal degrees. Ten rows use an area center instead of a factory point. |
| `coord_source` | string | Tourism Administration for 148 venue points; Chunghwa Post township center for 10 rough area points. |
| `source` | string | Source page: `https://data.gov.tw/dataset/6848`. |
| `updated` | string | IDA portal metadata update date, `2024-07-30`, as text. |

## Example

English factory names containing Cake. This call returns 5 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_tourism_factories' --data-urlencode 'name_en=*Cake*'
```

Response excerpt, one of the five returned records:

```json
[
  {
    "name_en": "Siang Shuai Cake",
    "name_zh": "香帥蛋糕芋製所",
    "category": "Cake and Pastry",
    "region": "North",
    "county_city": "新北市",
    "district": "汐止區",
    "postcode": "221031",
    "address_zh": "新北市汐止區汐萬路一段211巷26號",
    "phone": "+886 2 26486558",
    "website": "https://www.scake.com.tw/information/9",
    "lat": 25.07196,
    "lng": 121.6511,
    "coord_source": "Tourism Administration",
    "source": "https://data.gov.tw/dataset/6848",
    "updated": "2024-07-30"
  }
]
```

## Data notes

* Three source rows have no English directory match, so `name_en` and `category` are null: 豐得好食觀光工廠, 光遠燈籠觀光工廠 and 華美光學eye玩視界. The English directory has 156 entries. Its TNTBIO Foreverland entry was not added because it is absent from the IDA file.
* The 148 venue points come from the government Attractions database. The other 10 rows use official Chunghwa Post township centers matched by county, township and three digit postcode. Those centers give a rough area and may be far from the factory. Their source XML was generated on 12 August 2016; its portal metadata was updated on 27 June 2023.
* Some venue point addresses differ from the IDA address, including the Puli and Chiayi wineries. IDA address text was kept. Coordinates are rounded to six decimal places.
* Three mobile numbers were removed. The first landline and first extension were kept where more than one was given.
* Changes from the source: shorter field names, whitespace cleanup, English names and categories, English region labels, consistent county spelling, districts from addresses, phone format, mobile numbers removed, https where it answered, three broken Chinese name characters repaired, and map points with their source added. No English address or Chinese category was added because the English directory does not supply them.

## Data source and license

Data: Industrial Development Administration, Ministry of Economic Affairs, Taiwan, Tourism Factory Directory (觀光工廠名錄), [dataset 6848](https://data.gov.tw/dataset/6848). [Source CSV](https://www.ida.gov.tw/opendata/02/SDD6848.csv). IDA portal metadata updated 30 July 2024. The `updated` field and badge show that date, not the date each factory was checked.

English names and categories: factual names and labels from the [official tourism factory English directory](https://www.taiwanplace21.org.tw/English.php). Only names and categories were taken, not descriptions, photos or page design.

Map point credits: Tourism Administration, Ministry of Transportation and Communications, Taiwan, [Attractions database, dataset 7777](https://data.gov.tw/dataset/7777), 8 November 2026 release (148 venue points); Chunghwa Post Co., Ltd., [Three digit postal codes and administrative district center coordinates, dataset 25489](https://data.gov.tw/dataset/25489) (10 township centers).

The Open Data is made available to the public under the [Open Government Data License, version 1.0 (OGDL v1.0)](https://data.gov.tw/license). The IDA data and both government coordinate sources use this license. You may copy, share, change and use the data for business if you credit the source. Patent and trademark rights are not covered. No warranty or endorsement.

[Back to the API list](../../README.md)
