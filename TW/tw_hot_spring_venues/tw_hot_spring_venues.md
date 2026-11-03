# Taiwan Hot Spring Venues

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 825](https://img.shields.io/badge/records-825-lightgrey) ![updated 2026-09-15](https://img.shields.io/badge/updated-2026--09--15-orange) ![license OGDL 1.0](https://img.shields.io/badge/license-OGDL%201.0-green)

All 825 hot spring mark records in Taiwan, in English and Traditional Chinese. County and city governments issue the hot spring mark (溫泉標章) to hotels, bath houses, B&Bs and other places that offer hot spring water. Each row has the business name, county or city, district, postcode, address, spring type, mark status, issue and expiry dates, issuing government, phone and a map point. Expired, revoked and canceled marks are kept, so you can check the history of a place. Filter by the required `county_en`, then by district, spring type, mark status, name, a date wildcard or a coordinate range.

Rows by status: 459 in use (`active`), 298 expired, 58 revoked and 10 canceled. Rows by county or city: Yilan County 298, New Taipei City 109, Taipei City 78, Hualien County 59, Tainan City 58, Taitung County 54, Nantou County 43, Miaoli County 28, Taichung City 27, Pingtung County 23, Hsinchu County 15, Taoyuan City 14, Kaohsiung City 13, Chiayi County 4 and Changhua County 2. The busiest districts are Jiaoxi Township in Yilan (272), Beitou in Taipei (72), Wulai in New Taipei (52) and Baihe in Tainan (47).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_hot_spring_venues` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`county_en`). Optional: 21 fields. Disallowed: 3 fields.

`county_en` must be in every call. Use `county_en=*` to list all 825 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `county_en` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Dates are text in the form `YYYY-MM-DD`, so filter them with an exact date or a prefix wildcard, such as `2026*` or `2026-06*`. Only `latitude` and `longitude` take ranges. Chinese values must be URL encoded, which `curl --data-urlencode` does for you. In a plain URL, write `&` inside a value as `%26`.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `county_en` | Required | Yes | No | County or city in English, 15 values, such as `Yilan County` (298), `New Taipei City` (109) or `Taipei City` (78). Wildcards work, so `county_en=*` lists all 825, `county_en=*City` lists the 299 in cities and `county_en=*County` the 526 in counties. |
| `name_zh` | Optional | Yes | No | Business name in Traditional Chinese as the source gives it, often with the company name, such as `*民宿*` (121). |
| `name_en` | Optional | Yes | No | Business name in English, such as `*Hot Spring*` (372) or `*B&B*` (121). Translated by us from Chinese, so spellings may differ from signs. |
| `county_zh` | Optional | No | No | County or city in Chinese, such as `宜蘭縣` (298) or `臺北市` (78). |
| `district_zh` | Optional | Yes | No | District or township in Chinese, such as `礁溪鄉` (272) or `北投區` (72). |
| `district_en` | Optional | Yes | No | District or township in English, 69 values, such as `Jiaoxi Township` (272), `Beitou Dist.` (72) or `Wulai Dist.` (52). |
| `postcode` | Optional | No | No | 3 digit postcode of the district, as text, such as `262` (272, Jiaoxi) or `112` (72, Beitou). |
| `address_zh` | Optional | Yes | No | Address in Chinese, such as `*溫泉路*`. Clear typos are fixed and phone numbers typed into the address are removed. A few rows give a land parcel number instead of a street address. |
| `address_en` | Optional | Yes | No | Address in English, such as `*Wenquan Rd.*`. Translated by us with official road names where known and Hanyu Pinyin otherwise. For 27 rows that list several house numbers, the English shows the main one only. |
| `spring_type_zh` | Optional | Yes | No | Spring type in Chinese as the source gives it, such as `碳酸氫鹽泉` (559) or `硫磺泉` (47). |
| `spring_type_en` | Optional | Yes | No | Spring type in English, 19 values, such as `Bicarbonate spring` (608), `Sulfur spring` (51), `Acidic*` (28) or `*chloride*` (133). |
| `mark_status` | Optional | No | No | Mark status code: `active` (459, mark in use), `expired` (298), `revoked` (58) or `canceled` (10). The status comes from the source; 17 active rows have an expiry date that has passed. |
| `status_zh` | Optional | No | No | Status in Chinese as the source gives it, such as `使用中` (459). |
| `status_en` | Optional | No | No | Status in English: `In use` (459), `Expired` (298), `Revoked` (58) or `Canceled` (10). |
| `issue_date` | Optional | Yes | No | Date the mark was issued, `YYYY-MM-DD` text, from `2010-03-08` to `2026-11-04`. Exact match or wildcard, such as `issue_date=2026*` for marks issued in 2026 (119) or `issue_date=2026-06*` for one month (20). |
| `expiry_date` | Optional | Yes | No | Date the mark expires, `YYYY-MM-DD` text, from `2013-03-08` to `2029-11-03` (marks last 3 years). Exact match or wildcard, such as `expiry_date=2029*` for marks that expire in 2029 (113). |
| `issuing_authority_zh` | Optional | No | No | County or city government that issued the mark, in Chinese, such as `宜蘭縣政府` (298). |
| `issuing_authority_en` | Optional | No | No | Issuing government in English, such as `Yilan County Government` (298). |
| `phone` | Optional | No | No | Phone number with the area code, as text, such as `03-9887600`. Leading zeros lost in the source are restored. Null on 138 rows (133 blank and 5 masked in the source). To call from abroad, dial +886 and drop the leading 0. |
| `phone_ext` | Optional | No | No | Phone extension, as text. Filled on 3 rows only. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.0096 to 25.2296. Example: `latitude=25.12..25.16&longitude=121.49..121.53` for Beitou (69). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 120.295885 to 121.943601. Use it with `latitude` to search a box. |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from. See the notes. |
| `source` | Disallowed | No | No | Link to the source dataset, `https://data.gov.tw/dataset/38876`. |
| `updated` | Disallowed | No | No | Date the source dataset was last updated, `2026-09-15` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name_zh` | string | Business name in Traditional Chinese. |
| `name_en` | string | Business name in English (our translation). |
| `county_zh` | string | County or city in Chinese. |
| `county_en` | string | County or city in English. |
| `district_zh` | string | District or township in Chinese. |
| `district_en` | string | District or township in English. |
| `postcode` | string | 3 digit postcode, as text. |
| `address_zh` | string | Address in Chinese. |
| `address_en` | string | Address in English (our translation). |
| `spring_type_zh` | string | Spring type in Chinese. |
| `spring_type_en` | string | Spring type in English. |
| `mark_status` | string | `active`, `expired`, `revoked` or `canceled`. |
| `status_zh` | string | Status in Chinese. |
| `status_en` | string | Status in English. |
| `issue_date` | string | Date the mark was issued, `YYYY-MM-DD`. |
| `expiry_date` | string | Date the mark expires, `YYYY-MM-DD`. |
| `issuing_authority_zh` | string | Issuing government in Chinese. |
| `issuing_authority_en` | string | Issuing government in English. |
| `phone` | string | Phone number with the area code. Null for 138 rows. |
| `phone_ext` | string | Phone extension. Null for 822 rows. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the source dataset. |
| `updated` | string | Date the source dataset was last updated. |

Notes:

* Mark status: the source keeps old marks, so one place can have several rows, such as an expired mark and the new one that replaced it. 182 rows repeat a business and address, and 3 rows are exact copies in the source. To see only current marks, add `mark_status=active`.
* Status and dates: the status is the one the source gives. 17 rows marked in use have an expiry date that has passed, and 5 expired, 7 revoked and 2 canceled rows have an expiry date still to come. 7 marks have an issue date after the dataset update date of 2026-09-15. One row in the source is a test record (鴻維測試). All are kept as given.
* Names: the source has no English names. `name_en` is our translation, so it may differ from the name a business uses in English.
* Addresses: the source has no English addresses. `address_en` is our translation of `address_zh`. Clear typos in the source were fixed, such as 溫泉鄉 for 溫泉村 and broken characters shown as ?, and a phone number typed into one address was removed.
* Phones: 147 phone numbers lost their leading zero in the source and were restored. 5 phones are masked in the source and are null here.
* Spring types: `spring_type_en` is our English for the Chinese spring type, such as `Bicarbonate spring` for 碳酸氫鹽泉 and 碳酸氫根泉, `Simple thermal spring` for 單純泉 and `Cold spring, carbonate type` for cold springs.
* Map points: every row has a point. `geocode_quality` shows where it comes from, best first: `address_point` (334, exact match in the county address point data), `tourism_lodging_address` (329, Tourism Administration lodging database matched by address), `address_point_nearby` (9, nearest house number on the same road), `tourism_lodging_name` (26, lodging database matched by name in the same district), `osm_poi_name` (21, OpenStreetMap place with the same name), `osm_street` (43, middle of the street), `osm_village` (36, middle of the village) and `district_centroid` (27, middle of the district only). Points of the last three kinds are rough, so check the address before you go.

## Examples

Hot spring places in Beitou, Taipei with a mark in use, shown as a full row. This call returns 46 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hot_spring_venues" --data-urlencode "county_en=Taipei City" --data-urlencode "district_en=Beitou Dist." --data-urlencode "mark_status=active"
```

The first row of the response:

```json
[
  {
    "__id": 13,
    "name_zh": "彩色人生興業股份有限公司",
    "name_en": "Caise Rensheng Enterprise Co., Ltd.",
    "county_zh": "臺北市",
    "county_en": "Taipei City",
    "district_zh": "北投區",
    "district_en": "Beitou Dist.",
    "postcode": "112",
    "address_zh": "臺北市北投區中山路32號",
    "address_en": "No. 32, Zhongshan Rd., Beitou Dist., Taipei City 112, Taiwan",
    "spring_type_zh": "硫磺泉",
    "spring_type_en": "Sulfur spring",
    "mark_status": "active",
    "status_zh": "使用中",
    "status_en": "In use",
    "issue_date": "2024-10-15",
    "expiry_date": "2027-10-14",
    "issuing_authority_zh": "臺北市政府",
    "issuing_authority_en": "Taipei City Government",
    "phone": "02-66118888",
    "phone_ext": null,
    "latitude": 25.137805,
    "longitude": 121.510726,
    "geocode_quality": "address_point",
    "source": "https://data.gov.tw/dataset/38876",
    "updated": "2026-09-15"
  }
]
```

A wildcard on the required field and on the spring type. All acidic springs in Taiwan with a mark in use. This call returns 17 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hot_spring_venues" --data-urlencode "county_en=*" --data-urlencode "spring_type_en=Acidic*" --data-urlencode "mark_status=active" | jq -r '.[] | [.county_en, .district_en, .name_en, .spring_type_en] | @tsv'
```

```text
Taipei City	Beitou Dist.	Huangchi Hot Spring Hotel Co., Ltd.	Acidic sulfate spring
Taipei City	Beitou Dist.	Beitou Park Open Air Hot Spring Bath	Acidic chloride spring
Taipei City	Beitou Dist.	Beitou Qinghuang Hot Spring	Acidic iron sulfate chloride spring
Taipei City	Beitou Dist.	Huangchi Building 2	Acidic sulfate spring
New Taipei City	Jinshan Dist.	Qifengshi Hot Spring Mountain Lodge (Qifengshi Snack Shop)	Acidic sulfate spring
New Taipei City	Wanli Dist.	Jintang Hot Spring Resort	Acidic sulfate chloride spring
Taipei City	Beitou Dist.	Yinggang Hot Spring Resort Co., Ltd.	Acidic iron chloride sulfate spring
Taipei City	Beitou Dist.	Takinoyu Bathhouse	Acidic iron chloride sulfate spring
Taipei City	Beitou Dist.	Heyuan Hot Spring Restaurant (Tanglai Hot Spring Restaurant)	Acidic sulfate spring
Taipei City	Beitou Dist.	Huangchi Building 3	Acidic sulfate spring
Taipei City	Beitou Dist.	Shanzhilin Hot Spring Restaurant	Acidic sulfate spring
Taipei City	Beitou Dist.	Villa 32 (Caise Rensheng Enterprise Co., Ltd.)	Acidic iron chloride sulfate spring
New Taipei City	Jinshan Dist.	TS Motel (Dingshuai Hot Spring Motel)	Acidic chloride sulfate spring
Taipei City	Shilin Dist.	Xuyanggu Construction and Development Co., Ltd. (Riyue Farm)	Acidic iron sulfur sulfate chloride spring
New Taipei City	Jinshan Dist.	InterContinental New Taipei North Coast Hot Spring Hotel	Acidic chloride sulfate spring
New Taipei City	Wanli Dist.	Jinyongquan Hot Spring Resort	Acidic chloride spring
New Taipei City	Wanli Dist.	Mushe Hot Spring Resort Hotel	Acidic chloride sulfate spring
```

A date wildcard. Marks in Yilan County issued in 2026. This call returns 47 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hot_spring_venues" --data-urlencode "county_en=Yilan County" --data-urlencode "issue_date=2026*" | jq -r '.[] | [.name_en, .issue_date, .expiry_date] | @tsv'
```

A Chinese filter. Places in Jiaoxi Township (礁溪鄉). This call returns 272 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hot_spring_venues" --data-urlencode "county_en=*" --data-urlencode "district_zh=礁溪鄉" | jq -r '.[].name_zh'
```

## Data source

The list of marks, Chinese names, counties, districts, addresses, spring types, statuses, dates, issuing governments and phones comes from the Tourism Administration open data set [Hot Spring Mark Holders](https://data.gov.tw/dataset/38876) (溫泉標章), data.gov.tw dataset 38876, updated 2026-09-15 and downloaded 2026-11-03. Map points come from county address point data, the Tourism Administration [lodging database](https://data.gov.tw/dataset/7780) (data.gov.tw dataset 7780) and OpenStreetMap. English names, addresses and spring types were translated by us. The data can go out of date, so check with the business before you go.

Credit lines:

* Source: Tourism Administration, Ministry of Transportation and Communications, Hot Spring Mark Holders (data.gov.tw dataset 38876), 2026-09-15. Open Government Data License, version 1.0 (https://data.gov.tw/license). This license lets anyone use, copy, change and share the data for free, for any purpose, including commercial use, as long as the source is credited.
* Map points from county address point data and the Tourism Administration lodging database (data.gov.tw dataset 7780), under the same license.
* Some map points © OpenStreetMap contributors, under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not affiliated with or endorsed by the Tourism Administration or any agency.

[Back to the API list](../../README.md)
