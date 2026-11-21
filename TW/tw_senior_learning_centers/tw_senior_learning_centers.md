# TW Senior Learning Centers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 368](https://img.shields.io/badge/records-368-lightgrey) ![updated 2025-12-23](https://img.shields.io/badge/updated-2025--12--23-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

Ministry of Education, 2025, national senior learning centers contact list (全國樂齡學習中心聯繫資料), data.gov.tw dataset 163769, OGDL Taiwan 1.0, https://data.gov.tw/license. This Open Data is made available to the public under the [Open Government Data License, version 1.0](https://data.gov.tw/license). Source: [data.gov.tw dataset 163769](https://data.gov.tw/dataset/163769). District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.

The 368 senior learning centers (樂齡學習中心) on the Taiwan Ministry of Education list on data.gov.tw (dataset 163769, metadata dated 23 December 2025), in 21 counties and cities, with Chinese center and operator names, English county and district, operator type and a district center point. Every call needs `county_en`, and `county_en=*` returns all 368.

Not affiliated with or endorsed by the Ministry of Education. The list is a snapshot dated 23 December 2025, so check the Ministry of Education or the center before relying on a record. Map points are the center of the district from the Chunghwa Post district center table, not the exact site, so centers in the same district share a point. 10 telephone numbers that the list gave as 09 mobile numbers are left out (`phone` is `null`).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_senior_learning_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per center. All matches are returned. |

## Query parameters

Required: 1 field (`county_en`). Optional: 14 fields. Disallowed: 1 field.

`county_en` is required. A call without it returns HTTP 400. Use `county_en=*` to return all 368 records. Partial filters accept `*`, such as `county_en=*Hsinchu*`, `name=*樂齡*`, `address=*路*` or `phone=02-*`. `geoq`, `source_as_of` and `seq` are exact matches, and a star in a text field there is read as a plain character, so it returns `[]` (a star on the whole-number `seq` returns HTTP 400). All filters ignore case. `location[latitude]` and `location[longitude]` accept a range with two dots, lower number first, such as `location[latitude]=24..25`. A hyphen range such as `location[latitude]=24-25` returns HTTP 400.

Chinese filter values must be URL encoded. With curl, use `--data-urlencode`, as in the examples below. The `county` field uses 臺 (臺北市, 臺中市, 臺南市, 臺東縣), so `county=台北市` returns `[]`. Addresses usually start with a postcode, so `address=新北市*` matches nothing and `address=*新北市*` should be used instead.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `county_en` | Required | Yes | No | County or city in English, from the Chunghwa Post county and township table, 21 values on every row (none is blank, and there is no center in Chiayi City): Tainan City (38); Kaohsiung City (38); Pingtung County (34); New Taipei City (31); Taichung City (29); Changhua County (27); Yunlin County (20); Miaoli County (18); Chiayi County (18); Taitung County (16); Taoyuan City (13); Hsinchu County (13); Nantou County (13); Hualien County (13); Taipei City (12); Yilan County (12); Keelung City (8); Penghu County (6); Kinmen County (5); Hsinchu City (3); Lienchiang County (1). Every call must include `county_en`, and a call without it returns HTTP 400. Partial wildcards allowed, so `county_en=*` returns all 368 rows, and `*Hsinchu*` matches Hsinchu City and Hsinchu County (16). Case ignored. |
| `county` | Optional | Yes | No | County or city in Chinese as the list prints it, using 臺 (臺北市, 臺中市, 臺南市, 臺東縣). Never blank. Partial wildcards allowed, such as `臺*`. `台` does not match `臺`. Chinese filter values must be URL encoded. |
| `district` | Optional | Yes | No | District, township or county-level city in Chinese as the list prints it, such as 中山區. Never blank. Partial wildcards allowed, such as `*鄉`. |
| `district_en` | Optional | Yes | No | English name of the district, from the Chunghwa Post county and township table, with the Ministry of the Interior 2026 spellings Danshui District and Sanzhong District where the two lists differ. Never blank. Partial wildcards allowed, such as `*Township` or `*District`. |
| `name` | Optional | Yes | No | Name of the senior learning center in Chinese as the list prints it, 368 unique values, never blank. Partial wildcards allowed, such as `*樂齡*` or `*優質中心`. |
| `operator` | Optional | Yes | No | Organisation that runs the center, in Chinese, such as a school, a district office or a community development association. Never blank. Partial wildcards allowed, such as `*社區發展協會`. |
| `operator_type` | Optional | Yes | No | Kind of operator, worked out from the operator name here, not a column in the list, such as Elementary school (132), Association (69), Community development association (41) or Township office (36). 7 rows are Other. Never blank. Partial wildcards allowed, such as `*association*`. |
| `phone` | Optional | Yes | No | Center telephone number as the list prints it, text such as 02-24260155. 10 rows are `null` because the list gave a 09 mobile number, which is not published here. Partial wildcards allowed, such as `02-*`. |
| `fax` | Optional | Yes | No | Center fax number as the list prints it. Never blank in this extract; some rows print 無 when the source has no fax. Partial wildcards allowed, such as `02-*`. |
| `address` | Optional | Yes | No | Address in Chinese as the list prints it, often starting with a postcode, never blank. Partial wildcards allowed, such as `*新北市*` or `*路*`. |
| `seq` | Optional | No | No | Row number (序號) in the list, a whole number from 1 to 368. Unique on every row in this snapshot. Exact match, such as `seq=1`. A wildcard on this whole-number field returns HTTP 400. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees (WGS 84, 6 decimals) of the center point of the district, not of the address, from the Chunghwa Post district center table on data.gov.tw. Present on all 368 rows. Many rows share a point when two centers are in the same district. Range filter with two dots, such as `24..25`. A hyphen range returns 400. |
| `location[longitude]` | Optional | No | Yes | Longitude of the same district center point. Present on all 368 rows. Range filter with two dots, such as `121..122`. |
| `geoq` | Optional | No | No | Quality of the point: `district_center` on all 368 rows, because every county and district pair is in the Chunghwa Post district center table. Exact match. |
| `source_as_of` | Optional | No | No | Date of the data.gov.tw metadata update for this dataset, `2025-12-23`, the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: Ministry of Education senior learning centers list, OGDL Taiwan 1.0, with the credit for district points and English names. Returned in output only. |

## Response fields

`location.latitude` and `location.longitude` are numbers. `seq` is a whole number. All other values are strings. Blank cells are returned as `null` (`phone` on 10 rows). Latitude and longitude are nested under `location`. There is no `__id`: `seq` is unique on every row in this snapshot and is the record key.

| Field | Type | Description |
|---|---|---|
| `seq` | integer | Row number in the list, 1 to 368. Unique. This is the record key. |
| `county` | string | County or city in Chinese, using 臺. |
| `county_en` | string | County or city in English. |
| `district` | string | District, township or county-level city in Chinese. |
| `district_en` | string | District name in English. |
| `name` | string | Center name in Chinese. |
| `operator` | string | Organisation that runs the center, in Chinese. |
| `operator_type` | string | Kind of operator, derived from the operator name. |
| `phone` | string | Telephone number, or `null` when the list gave a mobile number. |
| `fax` | string | Fax number as the list prints it, including 無 when none is given. |
| `address` | string | Address in Chinese, often starting with a postcode. |
| `location` | object | `latitude` and `longitude` of the district center, not the site. |
| `geoq` | string | `district_center` on every row. |
| `source` | string | Credit line for the Ministry of Education list and the Chunghwa Post credit for points and English names. |
| `source_as_of` | string | Date of the data.gov.tw metadata update, `2025-12-23`. |

## Data notes

* The records are the 368 senior learning centers on the Ministry of Education list, data.gov.tw dataset 163769 (全國樂齡學習中心聯繫資料), metadata updated 23 December 2025, CSV from the Ministry of Education. There are 21 counties and cities and no center in Chiayi City. Names and `seq` are unique.
* Chinese center and operator names are kept as published. English county and district names come from the Chunghwa Post county and township table (data.gov.tw 5949) with the Ministry of the Interior 2026 spellings Danshui District and Sanzhong District, the same as tw_public_childcare_centers. Every county and district pair in the list is in the table.
* `operator_type` is derived from the operator name, not published. 7 operators did not fit a known pattern and are Other.
* 10 telephone numbers that the list gave as 09 mobile numbers are left out (`phone` is `null`). Fax numbers are kept as published.
* `lat` and `lng` are the center of the district from the Chunghwa Post district center table (data.gov.tw 25489), not the position of the site, so many sites share a point. `geoq` is `district_center` on all 368 rows. No address-level geocoder was used.
* This is a snapshot of the dataset metadata dated 23 December 2025, the `source_as_of` value. Check the Ministry of Education or the center before relying on a record.

## Examples

The first row in the list, the Zhongshan District senior learning center in Keelung, looked up by `seq`. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_senior_learning_centers' --data-urlencode 'county_en=*' --data-urlencode 'seq=1'
```

```json
[
  {
    "seq": 1,
    "county": "基隆市",
    "county_en": "Keelung City",
    "district": "中山區",
    "district_en": "Zhongshan District",
    "name": "基隆市中山區樂齡學習優質中心",
    "operator": "基隆市中山區通明社區發展協會",
    "operator_type": "Community development association",
    "phone": "02-24260155",
    "fax": "02-24260155",
    "address": "20342基隆市中山區中華路42號",
    "location": {
      "latitude": 25.149864,
      "longitude": 121.730891
    },
    "geoq": "district_center",
    "source": "Ministry of Education, 2025, national senior learning centers contact list (全國樂齡學習中心聯繫資料), data.gov.tw dataset 163769, OGDL Taiwan 1.0, https://data.gov.tw/license. District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.",
    "source_as_of": "2025-12-23"
  }
]
```

The one senior learning center in Lienchiang County (Matsu). This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_senior_learning_centers' --data-urlencode 'county_en=Lienchiang County'
```

```json
[
  {
    "seq": 368,
    "county": "連江縣",
    "county_en": "Lienchiang County",
    "district": "南竿鄉",
    "district_en": "Nangan Township",
    "name": "連江縣連江縣樂齡學習中心",
    "operator": "連江縣政府教育處",
    "operator_type": "Other",
    "phone": "0836-22067#6232",
    "fax": "0836-25582",
    "address": "20941連江縣南竿鄉清水村96-11號",
    "location": {
      "latitude": 26.154321,
      "longitude": 119.931128
    },
    "geoq": "district_center",
    "source": "Ministry of Education, 2025, national senior learning centers contact list (全國樂齡學習中心聯繫資料), data.gov.tw dataset 163769, OGDL Taiwan 1.0, https://data.gov.tw/license. District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.",
    "source_as_of": "2025-12-23"
  }
]
```

A center in Taitung County whose listed telephone was a mobile number, so `phone` is null. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_senior_learning_centers' --data-urlencode 'county_en=Taitung County' --data-urlencode 'seq=356'
```

```json
[
  {
    "seq": 356,
    "county": "臺東縣",
    "county_en": "Taitung County",
    "district": "太麻里鄉",
    "district_en": "Taimali Township",
    "name": "臺東縣太麻里鄉大王樂齡學習中心(優先區)",
    "operator": "社團法人臺東縣原住民族專業職能培訓推廣協會",
    "operator_type": "Association",
    "phone": null,
    "fax": "無",
    "address": "96342臺東縣太麻里鄉德輝路12號",
    "location": {
      "latitude": 22.590981,
      "longitude": 120.979764
    },
    "geoq": "district_center",
    "source": "Ministry of Education, 2025, national senior learning centers contact list (全國樂齡學習中心聯繫資料), data.gov.tw dataset 163769, OGDL Taiwan 1.0, https://data.gov.tw/license. District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.",
    "source_as_of": "2025-12-23"
  }
]
```

## Data source

Ministry of Education, 2025, national senior learning centers contact list (全國樂齡學習中心聯繫資料), [data.gov.tw dataset 163769](https://data.gov.tw/dataset/163769), metadata updated 23 December 2025. This Open Data is made available to the public under the [Open Government Data License, version 1.0](https://data.gov.tw/license). District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license. This data was changed from the source as described in the notes above. The Ministry of Education does not endorse this API.
