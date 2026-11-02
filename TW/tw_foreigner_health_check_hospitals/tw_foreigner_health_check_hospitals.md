# Taiwan Foreigner Health Check Hospitals

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 76](https://img.shields.io/badge/records-76-lightgrey) ![updated 2026-10-27](https://img.shields.io/badge/updated-2026--10--27-orange) ![license OGDL v1.0 and ODbL](https://img.shields.io/badge/license-OGDL%20v1.0%20%2B%20ODbL-green)

The 76 hospitals that Taiwan Centers for Disease Control (Taiwan CDC) approves for the health checks of foreign workers and other foreign residents (外國人健檢指定醫院), in 22 cities and counties, in English and Traditional Chinese. These are the hospitals that can do the health check you need for a work permit or a residence permit in Taiwan. Each row has the CDC list code, names, city or county, district, postcode, address, phone, the date the CDC approval ends, the MOHW medical institution code, notes and a map point. Filter by the required `city_county_en`, then by name, district, approval date or year, or a coordinate range.

Rows by city or county: Taichung City and Kaohsiung City 9 each, Taipei City and Taoyuan City 8 each, New Taipei City and Tainan City 6 each, Changhua County and Chiayi City 4 each, Yilan County 3, Keelung City, Miaoli County, Chiayi County, Pingtung County, Hualien County and Penghu County 2 each, and 1 each in the other 7. Rows by the year the approval ends: 2026 (5), 2027 (38), 2028 (13) and 2029 (20).

> **Before you go.** Taiwan Adventist Hospital (臺安醫院) is on the list but does not do health checks for migrant workers. Each approval has an end date, and Taiwan CDC updates the list from time to time, so check `valid_until` and call the hospital before you book. Not affiliated with or endorsed by Taiwan CDC, MOHW or any agency.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_foreigner_health_check_hospitals` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`city_county_en`). Optional: 16 fields. Disallowed: 3 fields.

`city_county_en` must be in every call. Use `city_county_en=*` to list all 76 hospitals. Disallowed means the field is returned in the output but cannot be used as a filter. Without `city_county_en` the API returns HTTP 400.

Dates are text in the form `YYYY-MM-DD`, so filter them with a prefix wildcard, such as `2028*`. `valid_until_year`, `latitude` and `longitude` take ranges. Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city_county_en` | Required | Yes | No | City or county in English, 22 values, such as `Taichung City` (9), `Kaohsiung City` (9), `Taipei City` (8) or `Taoyuan City` (8). Wildcards work, so `city_county_en=*` lists all 76, `*Taipei*` (14) matches Taipei City and New Taipei City, `*County` lists the 23 in counties and `*City` the 53 in cities. |
| `city_county_zh` | Optional | Yes | No | City or county in Chinese, such as `臺北市` (8) or `高雄市` (9). Uses the official 臺 form, not 台. |
| `cdc_code` | Optional | No | No | Code of the row on the Taiwan CDC list, such as `A14` (1). Unique per row. The letter is the CDC area group. |
| `name_en` | Optional | Yes | No | Hospital name in English as Taiwan CDC lists it, such as `*Veterans*` (6), `*Chang-Gung*` (4), `*Ministry of Health*` (14) or `*Branch*` (20). |
| `name_zh` | Optional | Yes | No | Full hospital name in Chinese as Taiwan CDC lists it, such as `衛生福利部*` (14), `*長庚*` (4) or `*馬偕*` (3). |
| `district_en` | Optional | Yes | No | District, township or county city in English, such as `Taoyuan Dist.` (4) or `East Dist.` (4, in Chiayi City and Tainan City). Spelling follows Chunghwa Post. |
| `district_zh` | Optional | Yes | No | District, township or county city in Chinese, such as `中正區` (1) or `羅東鎮` (2). |
| `postcode` | Optional | Yes | No | 3 digit postcode from Chunghwa Post, kept as text, such as `600` (4) or `330` (4). |
| `address_en` | Optional | Yes | No | Street address in English, such as `*Zhongzheng Rd.*` (6). Built by us from the Chinese address with Chunghwa Post road names. Floor details are left out. |
| `address_zh` | Optional | Yes | No | Street address in Chinese, such as `*中正路*` (6). |
| `phone` | Optional | Yes | No | Main phone number in international form, such as `+886 2 *` (16, the Taipei area code). One row has an extension written as `ext. 1201`. |
| `valid_until` | Optional | Yes | No | Last day of the CDC approval, `YYYY-MM-DD`, such as `2027-06-30` (29) or `2026-12-31` (5). Use a prefix wildcard for a year, such as `2028*` (13). |
| `valid_until_year` | Optional | No | Yes | Year the CDC approval ends: 2026 (5), 2027 (38), 2028 (13) or 2029 (20). Example: `valid_until_year=2026..2027` (43). |
| `mohw_code` | Optional | No | No | 10 digit MOHW medical institution code, kept as text so leading zeros stay, such as `0131060010` (1). Null on 6 rows. See the notes. |
| `notes` | Optional | Yes | No | Notes on the row, such as `*migrant worker*` (1). Filled on 11 rows, null on the rest. `notes=*` lists the 11. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.4738623 to 26.1611298. Example: `latitude=24.9..25.2` with `longitude=121.3..121.8` (20, greater Taipei). Check `geocode_quality` before you use the point. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 118.4180617 to 121.7728755. Example: `longitude=118..120` (4, the outlying islands of Penghu, Kinmen and Matsu). |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from: `osm_hospital` (71) or `street` (5). See the notes. |
| `source` | Disallowed | No | No | Data source and license credit for the row. |
| `updated` | Disallowed | No | No | Date the row was built, `2026-11-03` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `cdc_code` | string | Code of the row on the Taiwan CDC list. |
| `name_en` | string | Hospital name in English. |
| `name_zh` | string | Full hospital name in Traditional Chinese. |
| `city_county_en` | string | City or county in English. |
| `city_county_zh` | string | City or county in Chinese. |
| `district_en` | string | District in English. |
| `district_zh` | string | District in Chinese. |
| `postcode` | string | 3 digit postcode, as text. |
| `address_en` | string | Street address in English. |
| `address_zh` | string | Street address in Chinese. |
| `phone` | string | Main phone number in international form. |
| `valid_until` | string | Last day of the CDC approval, `YYYY-MM-DD`. |
| `valid_until_year` | integer | Year the CDC approval ends. |
| `mohw_code` | string | 10 digit MOHW medical institution code, as text. Null for 6 rows. |
| `notes` | string | Notes on the row. Null for 65 rows. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | `osm_hospital` or `street`. |
| `source` | string | Data source and license credit. |
| `updated` | string | Date the row was built. |

Notes:

* Migrant worker checks: Taiwan CDC marks Taiwan Adventist Hospital (`A14`) as not giving health checks to migrant workers (該院未提供移工健檢服務). The row stays in the API because the hospital is on the list, and the CDC note is in `notes`.
* Rows with no MOHW code: 6 rows are branches or campuses that have no code of their own in the MOHW medical institution data, so `mohw_code` is null. They are 5 Taipei City Hospital branches (Yang-Ming, Zhongxiao, Jen-Ai, Hoping and Zhongxing) and the San-Min campus of Min-Sheng Healthcare. Their addresses and phones come from the hospital websites.
* Map points: 71 points are the hospital itself in OpenStreetMap, checked against its district (`osm_hospital`). 5 points are only on the street of the address, not the building (`street`): Lo-Sheng Hospital, TaoYuan General Hospital, the NTUH Hsin-Chu Branch, Chiayi Hospital and Penghu Hospital.
* Approval dates: each hospital is approved for a set time. 5 approvals end on 2026-12-31. Check `valid_until` and the current Taiwan CDC list before you go.
* Dates: the Chinese list gives dates in the Republic of China (ROC, 民國) calendar, such as 至116年12月31日. We added 1911 to the year, so 116 becomes 2027, and checked every date against the English list.
* Names: `name_en` and `name_zh` are as Taiwan CDC lists them, so English spellings vary, such as KeeLung and TaoYuan. Lo-Sheng Hospital was renamed in 2026 from 樂生療養院 to 衛生福利部樂生醫院; the row shows its registered address, and its notes give the address of the new medical building.

## Examples

A wildcard on the required field. All hospitals in Taoyuan City. This call returns 8 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_foreigner_health_check_hospitals" --data-urlencode "city_county_en=Taoyuan*" | jq -r '.[] | [.cdc_code, .name_en, .district_en, .valid_until] | @tsv'
```

```text
I01	Chang-Gung Memorial Hospital LinKou Branch	Guishan Dist.	2029-12-31
I02	TaoYuan General Hospital, Ministry of Health and Welfare	Taoyuan Dist.	2029-12-31
I03	TaoYuan Armed Forces General Hospital	Longtan Dist.	2027-06-30
I04	Min-Sheng Healthcare, San-Min Campus	Taoyuan Dist.	2029-12-31
I05	Taipei Veterans General Hospital, Taoyuan Branch	Taoyuan Dist.	2028-06-30
I06	LandSeed International Hospital	Pingzhen Dist.	2029-12-31
I07	Saint Paul's Hospital	Taoyuan Dist.	2028-06-30
I08	Ten-Chen Medical Group	Zhongli Dist.	2029-12-31
```

A range on the approval year. Hospitals whose approval ends in 2026. This call returns 5 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_foreigner_health_check_hospitals" --data-urlencode "city_county_en=*" --data-urlencode "valid_until_year=2026..2026" | jq -r '.[] | [.city_county_en, .name_en, .valid_until] | @tsv'
```

```text
Taipei City	Tri-Service General Hospital Songshan Branch	2026-12-31
Hsinchu City	National Taiwan University Hospital Hsin-Chu Branch	2026-12-31
Kaohsiung City	Chang-Gung Memorial Hospital KaoHsiung Branch	2026-12-31
Kaohsiung City	E-Da Hospital	2026-12-31
Hualien County	Mennonite Christian Hospital	2026-12-31
```

A filter on the notes, shown as a full row. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_foreigner_health_check_hospitals" --data-urlencode "city_county_en=Taipei City" --data-urlencode "notes=*migrant worker*"
```

```json
[
  {
    "__id": 8,
    "cdc_code": "A14",
    "name_en": "Taiwan Adventist Hospital",
    "name_zh": "基督復臨安息日會醫療財團法人臺安醫院",
    "city_county_en": "Taipei City",
    "city_county_zh": "臺北市",
    "district_en": "Songshan Dist.",
    "district_zh": "松山區",
    "postcode": "105",
    "address_en": "No. 424, Sec. 2, Bade Rd., Songshan Dist., Taipei City 105, Taiwan",
    "address_zh": "臺北市松山區八德路二段424號",
    "phone": "+886 2 27718151",
    "valid_until": "2027-12-31",
    "valid_until_year": 2027,
    "mohw_code": "1101010021",
    "notes": "CDC note: this hospital does not provide migrant worker health checks (該院未提供移工健檢服務).",
    "latitude": 25.0474734,
    "longitude": 121.5472645,
    "geocode_quality": "osm_hospital",
    "source": "Taiwan CDC hospitals for foreign worker health checks (list updated 2026-10-27); address and phone: MOHW medical institution data 2025-12-31 (OGDL v1); postcode and English address: Chunghwa Post (OGDL v1); map point: OpenStreetMap Nominatim (ODbL)",
    "updated": "2026-11-03"
  }
]
```

A Chinese filter. Chang Gung hospitals (長庚). This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_foreigner_health_check_hospitals" --data-urlencode "city_county_en=*" --data-urlencode "name_zh=*長庚*" | jq -r '.[] | [.city_county_zh, .name_zh, .phone] | @tsv'
```

```text
基隆市	長庚醫療財團法人基隆長庚紀念醫院	+886 2 24313131
桃園市	長庚醫療財團法人林口長庚紀念醫院	+886 3 3281200
嘉義縣	長庚醫療財團法人嘉義長庚紀念醫院	+886 5 3621000
高雄市	長庚醫療財團法人高雄長庚紀念醫院	+886 7 7317123
```

A box of latitude and longitude. Hospitals in greater Taipei. This call returns 20 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_foreigner_health_check_hospitals" --data-urlencode "city_county_en=*" --data-urlencode "latitude=24.9..25.2" --data-urlencode "longitude=121.3..121.8" | jq -r '.[] | [.city_county_en, .name_en] | @tsv'
```

## Data source

The list of hospitals, CDC codes, English and Chinese names, cities and counties, approval end dates and notes come from the Taiwan CDC [list of hospitals designated for foreigner health checks](https://www.cdc.gov.tw/Category/Page/nU7y97g0GqJbB3kn5B-nPg) (外國人健檢指定醫院名單(國內)) and its English version, updated 2026-10-27. Addresses, phones and MOHW codes come from the Ministry of Health and Welfare (MOHW) medical institution data of 2025-12-31, and from the hospital websites for 6 branch rows. Postcodes, districts and English road names come from the Chunghwa Post postcode and English address files. Map points come from OpenStreetMap through Nominatim. We joined the English and Chinese lists, added addresses, phones and postcodes, put phones in +886 form, converted the ROC dates and added the map points. The data can go out of date, so check with the hospital before you go.

Credit lines:

* Source: Taiwan Centers for Disease Control, list of hospitals designated for foreigner health checks (外國人健檢指定醫院名單), 2026-10-27. Used under the Taiwan CDC open data declaration (政府網站資料開放宣告) (https://www.cdc.gov.tw/Category/Page/TxkBIR9agw_IBRRmvn9TcQ), which is in line with the Open Government Data License, version 1.0. It lets anyone use, copy, change and share the site data for free, for any purpose, as long as the source is credited. It does not cover logos or trademarks, and it does not mean Taiwan CDC endorses this API.
* Addresses, phones and MOHW codes: Ministry of Health and Welfare, medical institution data, 2025-12-31. Postcodes and English road names: Chunghwa Post. Both under the Open Government Data License, version 1.0 (https://data.gov.tw/license). This license lets anyone use, copy, change and share the data for free, for any purpose, including commercial use, as long as the source is credited.
* Map points © OpenStreetMap contributors, under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright). You may use, share and adapt the data if you credit OpenStreetMap and share any changed database under the same license.
* No logos of Taiwan CDC, MOHW or Chunghwa Post are used.
* Not affiliated with or endorsed by Taiwan CDC, MOHW or any agency.

[Back to the API list](../../README.md)
