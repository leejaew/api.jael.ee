# Taiwan Postpartum Care Homes

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 241](https://img.shields.io/badge/records-241-lightgrey) ![updated 2026-06-03](https://img.shields.io/badge/updated-2026--06--03-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

241 postpartum care homes (產後護理之家) from the Ministry of Health and Welfare, Department of Nursing and Health Care, December 2025 national register of licensed nursing institutions (全國開業護理機構清冊). The portal published this edition on 3 June 2026 and updates the register about once a year. The national file has 1,494 licensed nursing institutions; this API is the 241 postpartum care home subset. Each record has the official 10-digit institution code, Traditional Chinese name, county, district, address, phone, open postpartum and infant bed counts, evaluation result and official map point. No field is required, so you can filter by county, name, evaluation, a bed count range or a lat and lng box.

Institution names stay in Chinese, because the source has no English names. English county and district labels are official Chunghwa Post spellings. A few official English names use a typographic apostrophe, such as Da’an Dist. Chinese county names use the official 臺 form, so search for `臺北市`, not `台北市`. Encode Chinese values in the query string, for example with `curl -G --data-urlencode`.

220 homes are listed as passing their evaluation, 16 are not yet evaluated and 5 failed. Three homes are published with no open beds. Map points come from the official file and are present on every row. Taiwan mobile numbers were omitted; 239 of 241 rows have a business landline.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_postpartum_care_homes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per home. All matches are returned. |

## Query parameters

Required: none. Optional: 16 fields. Disallowed: 2 fields.

A call with no filters returns all 241 records. Partial filters accept `*`, such as `name_zh=*藍田*`, `county=*Hsinchu*` or `phone=02*`. `evaluation` is an exact match. `postpartum_beds`, `infant_beds`, `lat` and `lng` accept a range in the form `min..max`, for example `postpartum_beds=40..97` or `lat=24..25`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `institution_code` | Optional | Yes | No | Official 10-digit institution code, kept as text, such as 7401181072. Unique on every row. Never blank. This is the official public key. Partial wildcards allowed, such as `7401*` (40) or `7401181072` (1). |
| `name_zh` | Optional | Yes | No | Institution name in Chinese as the register lists it, such as 晶英產後護理之家 or 藍田產後護理之家. Never blank. Unique on 234 of 241 rows; 藍田產後護理之家 appears three times, and 安寶產後護理之家 and 沁月產後護理之家 appear twice each, in different counties. Partial wildcards allowed, such as `*藍田*` (6) or `藍田產後護理之家` (3). |
| `county` | Optional | Yes | No | County or city in English, from official Chunghwa Post spellings, in 17 values. The largest are Taipei City (44), New Taipei City (38), Taichung City (32), Taoyuan City (31), Tainan City (22) and Kaohsiung City (22). `county=*Hsinchu*` matches Hsinchu City and Hsinchu County (18). This snapshot has no rows in Nantou County, Taitung County, Penghu County, Kinmen County or Lienchiang County. Never blank. Partial wildcards allowed. |
| `county_zh` | Optional | Yes | No | County or city in Chinese as published, using the official 臺 form, such as 臺北市 (44) or 新北市 (38). Never blank. Partial wildcards allowed. |
| `district` | Optional | Yes | No | District, township or county-administered city in English, from official Chunghwa Post spellings, such as East Dist. (18), Taoyuan Dist. (14) or Zhubei City (8). A few names use a typographic apostrophe, such as Da’an Dist. Never blank. Partial wildcards allowed, such as `*Dist.` or `Zhubei*`. |
| `district_zh` | Optional | Yes | No | District, township or county-administered city in Chinese, taken from the official address, such as 東區, 桃園區 or 竹北市. Never blank. Partial wildcards allowed. |
| `address` | Optional | Yes | No | Official street address in Chinese as published, such as 臺北市中正區新生南路一段170巷13號3-5樓. Never blank. One official address is incomplete: 桃園市平鎮區129號 (秉坤婦幼醫院附設產後護理之家); the `locate_address` for that row has the street. Partial wildcards allowed, such as `*新生南路*` (2). |
| `locate_address` | Optional | Yes | No | Address the ministry used to place the map point, in Chinese, often with village (里) and neighborhood (鄰), such as 桃園市桃園區中成里6鄰慈文路957號. It differs from `address` on 223 of 241 rows. Never blank. Partial wildcards allowed, such as `*里*` (213). |
| `phone` | Optional | Yes | No | Business landline, with the area code, such as `02 27479797`, `03 3384889` or `037 552980`. Extensions are written as `ext`, such as `02 25012312 ext 611`. Taiwan mobile numbers were omitted. Two rows listed only a mobile and are blank (`null`); one row that mixed a landline and a mobile keeps only the landline, so 239 rows have a phone. A prefix of `03*` also matches Miaoli numbers that start 037. Partial wildcards allowed, such as `02*` (82) or `037*` (4). |
| `postpartum_beds` | Optional | No | Yes | Open postpartum care beds as an integer, from 0 to 97 (美麗人生產後護理之家 in New Taipei City). The 241 homes have 6,017 open postpartum beds in total. Three homes are published with 0 beds: 柏仁產後護理之家, 康馨產後護理之家 and 知欣產後護理之家, all in Kaohsiung City. 36 homes have 40 or more. Use a range, for example `postpartum_beds=40..97` (36) or `postpartum_beds=0..0` (3). A hyphen range such as `40-97` returns HTTP 400. |
| `infant_beds` | Optional | No | Yes | Open infant cots as an integer, from 0 to 109 (also 美麗人生產後護理之家). The 241 homes have 6,695 open infant cots in total. The same three Kaohsiung homes are published with 0 infant cots. Use a range, for example `infant_beds=80..109` (2). A hyphen range such as `80-109` returns HTTP 400. |
| `evaluation` | Optional | No | No | English label from the official evaluation result: `pass` (220, 合格), `fail` (5, 不合格) or `not_yet_evaluated` (16, 尚未評鑑). Exact match. |
| `evaluation_zh` | Optional | Yes | No | Official evaluation result in Chinese as published: 合格 (220), 尚未評鑑 (16) or 不合格 (5). Never blank. Partial wildcards allowed, such as `*合格*` (225, which also matches 不合格). |
| `expiry_date` | Optional | Yes | No | End date of the current evaluation pass, as text YYYY-MM-DD, converted from official ROC dates such as 116年12月31日. Present on 220 rows, all 31 December: 2026 (59), 2027 (70), 2028 (34) or 2029 (57). Blank (`null`) on all 16 `not_yet_evaluated` rows and all 5 `fail` rows. Partial wildcards allowed, such as `2026*` (59) or `2027*` (70). Range is off because the column is text. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, taken from the official WGS84緯度 column. Present on all 241 rows. Range uses two dots, such as `lat=24..25` (91). A hyphen range returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, taken from the official WGS84經度 column. Present on all 241 rows. Range uses two dots, such as `lng=121..122` (132). A hyphen range returns HTTP 400. Combined with a latitude range, `lat=24..25&lng=121..122` returns 42 rows. |
| `source` | Disallowed | No | No | Ministry of Health and Welfare, Department of Nursing and Health Care, National register of licensed nursing institutions (data.gov.tw dataset 115950), December 2025 list, in all 241 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date the portal published this resource, YYYY-MM-DD (`2026-06-03`), matching the data.gov.tw modifiedDate and resource quality check time 2026-06-03 14:08:50 (2:08 pm on 3 June 2026, Asia/Singapore, UTC+8). The official resource title is 114年12月全國開業護理機構清冊, a December 2025 list. Text. Not filterable. |

## Response fields

`__id`, `postpartum_beds` and `infant_beds` are integers. `lat` and `lng` are numbers. All other values are strings. Blank phone and expiry_date cells are returned as `null`. `__id` is a numeric row key. Callers should use `institution_code` to identify a home, because that official 10-digit code is unique and stable.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `institution_code` | string | Official 10-digit institution code, unique on every row. |
| `name_zh` | string | Institution name in Chinese as the register lists it. |
| `county` | string | County or city in English, from official Chunghwa Post spellings. |
| `county_zh` | string | County or city in Chinese, using the official 臺 form. |
| `district` | string | District, township or county-administered city in English. |
| `district_zh` | string | District, township or county-administered city in Chinese. |
| `address` | string | Official street address in Chinese. |
| `locate_address` | string | Address the ministry used to place the map point, in Chinese. |
| `phone` | string | Business landline with area code, or `null` when the source listed only a mobile number. |
| `postpartum_beds` | integer | Open postpartum care beds, 0 to 97. |
| `infant_beds` | integer | Open infant cots, 0 to 109. |
| `evaluation` | string | English label of the official result: `pass`, `fail` or `not_yet_evaluated`. |
| `evaluation_zh` | string | Official evaluation result in Chinese: 合格, 不合格 or 尚未評鑑. |
| `expiry_date` | string | End date of the current evaluation pass, YYYY-MM-DD, or `null` when the home has not passed. |
| `lat` | number | Latitude in decimal degrees, from the official file. |
| `lng` | number | Longitude in decimal degrees, from the official file. |
| `source` | string | Ministry of Health and Welfare, Department of Nursing and Health Care, National register of licensed nursing institutions (data.gov.tw dataset 115950), December 2025 list. |
| `source_as_of` | string | Date the portal published this resource, `2026-06-03`. |

## Examples

藍田產後護理之家 in Taipei City, looked up by its official institution code 7401181072. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_postpartum_care_homes' --data-urlencode 'institution_code=7401181072'
```

```json
[
  {
    "__id": 36,
    "institution_code": "7401181072",
    "name_zh": "藍田產後護理之家",
    "county": "Taipei City",
    "county_zh": "臺北市",
    "district": "Zhongzheng Dist.",
    "district_zh": "中正區",
    "address": "臺北市中正區新生南路一段170巷13號3-5樓",
    "locate_address": "臺北市中正區三愛里15鄰新生南路一段170巷13號",
    "phone": "02 33225000",
    "postpartum_beds": 9,
    "infant_beds": 11,
    "evaluation": "pass",
    "evaluation_zh": "合格",
    "expiry_date": "2026-12-31",
    "lat": 25.034305,
    "lng": 121.531321,
    "source": "Ministry of Health and Welfare, Department of Nursing and Health Care, National register of licensed nursing institutions (data.gov.tw dataset 115950), December 2025 list",
    "source_as_of": "2026-06-03"
  }
]
```

Homes whose official name includes 藍田. This call returns 6 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_postpartum_care_homes' --data-urlencode 'name_zh=*藍田*'
```

Homes in 臺北市. This call returns 44 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_postpartum_care_homes' --data-urlencode 'county_zh=臺北市'
```

Homes with 40 to 97 open postpartum beds. This call returns 36 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_postpartum_care_homes' --data-urlencode 'postpartum_beds=40..97'
```

Homes between latitude 24 and 25 and longitude 121 and 122. This call returns 42 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_postpartum_care_homes' --data-urlencode 'lat=24..25' --data-urlencode 'lng=121..122'
```

Homes listed as failing their evaluation. This call returns 5 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_postpartum_care_homes' --data-urlencode 'evaluation=fail'
```

## Data source

Source: Ministry of Health and Welfare, Department of Nursing and Health Care, [National register of licensed nursing institutions](https://data.gov.tw/dataset/115950) (全國開業護理機構清冊（含一般護理之家、居家護理所及產後護理之家）), data.gov.tw dataset 115950, also on the [English dataset page](https://data.gov.tw/en/datasets/115950). This API is the postpartum care home subset of the December 2025 national list (114年12月全國開業護理機構清冊). The portal published the resource on 3 June 2026 and updates it about once a year. English county and district names are official Chunghwa Post spellings, used under the same license; credit Chunghwa Post for those labels. Institution names stay in Chinese because the source has no English names. Map points are the official WGS84緯度 and WGS84經度 values and are present on every row.

The data is used under the [Open Government Data License, version 1.0](https://data.gov.tw/license) (政府資料開放授權條款-第1版). The license lets you copy, adapt, share and use the data for any purpose, including commercial use, as long as you credit the Ministry of Health and Welfare, Department of Nursing and Health Care, name the National register of licensed nursing institutions, and link to the license. If attribution is missing, the rights granted under the license are void from the start. The license is compatible with Creative Commons Attribution 4.0 International. It does not grant patent or trademark rights, and ministry logos are not licensed. The data comes with no warranty. `evaluation` and `evaluation_zh` are the official published result from the register. Use of this data does not mean the Ministry of Health and Welfare endorses this API or its maker.

The national file has 1,494 licensed nursing institutions: 752 home nursing facilities, 501 general nursing homes and 241 postpartum care homes. Only the 241 產後護理之家 rows are in this API, in source order. Taiwan mobile numbers were omitted, and landlines are written with a leading area code, with extensions as `ext`. Three Kaohsiung homes are published with 0 postpartum beds and 0 infant cots: 柏仁產後護理之家 and 康馨產後護理之家 are `not_yet_evaluated`; 知欣產後護理之家 is marked `pass`, with an expiry date of 2029-12-31. The official address 桃園市平鎮區129號 is missing a street; the `locate_address` has 延平路二段129號. 223 locate addresses differ from the published address, usually by adding village and neighborhood. This December 2025 snapshot has no postpartum care homes in Nantou, Taitung, Penghu, Kinmen or Lienchiang.

This is a snapshot of the December 2025 list as published on 3 June 2026. For the latest list, visit the [dataset page](https://data.gov.tw/dataset/115950).

[Back to the API list](../../README.md)
