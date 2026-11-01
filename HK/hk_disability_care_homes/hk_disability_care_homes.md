# Hong Kong Disability Care Homes

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 350](https://img.shields.io/badge/records-350-lightgrey) ![updated 2026-11-01](https://img.shields.io/badge/updated-2026--11--01-orange)

All 350 licensed residential care homes for persons with disabilities in Hong Kong, from the Social Welfare Department list. Each row has the name and address in English, Traditional Chinese and Simplified Chinese, licence number, home type, service type, district, region, phone, fax, a conviction record flag, subsidised and non-subsidised places, gender served and map coordinates. Together the homes offer 19,430 places across 18 districts. The source file changes daily, so this is a snapshot from 2026-11-01.

Rows by home type: Subvented 268, Private 65 and Self-financing 17. Rows by region: New Territories 195, Kowloon 97 and Hong Kong Island 58.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_disability_care_homes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`district`). Optional: 19 fields. Disallowed: 5 fields.

`district` must be in every call. Use `district=*` to list all 350 homes. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `district` | Required | Yes | No | Hong Kong district of the home, such as `Tuen Mun` (45), `Southern` (35), `Yuen Long` (33), `Kwun Tong` (31) or `Sha Tin` (31). Wildcards work, so `district=*` lists all 350 homes and `district=Sha*` matches Sha Tin and Sham Shui Po (60). |
| `name` | Optional | Yes | No | Home name in English, such as `*Hostel*` (105) or `SAHK*` (21). Some service units of one home share a licence number but have their own name. |
| `name_zh` | Optional | Yes | No | Home name in Traditional Chinese, such as `*宿舍*` (125) or `*扶康會*` (27). |
| `name_zh_hans` | Optional | Yes | No | Home name in Simplified Chinese, such as `*宿舍*`. Blank for 3 homes. |
| `licence_number` | Optional | Yes | No | Social Welfare Department licence or certificate number, such as `D0015` or `D01*`. Not unique: one licensed home can run several service units, so the 350 rows share 314 numbers (`D0015` has 4 rows). Use `__id` as the row key. |
| `home_type` | Optional | Yes | No | Kind of home: `Subvented` (268), `Private` (65) or `Self-financing` (17). |
| `service_type` | Optional | Yes | No | Service the unit provides, such as `Hostel for Severely Mentally Handicapped Persons` (74), `Hostel for Moderately Mentally Handicapped Persons` (50), `Private RCHD (Non-Bought Place Scheme)` (39), `Halfway House` (37), `Supported Hostel` (35), `Bought Place Scheme (BPS) for Private RCHDs` (26) or `Care and Attention Home for Severely Disabled Persons` (26). Use a wildcard such as `*Halfway*` (37). |
| `service_type_zh` | Optional | Yes | No | Service type in Traditional Chinese, such as `中途宿舍` (37) or `*宿舍*`. |
| `region` | Optional | No | No | `New Territories` (195), `Kowloon` (97) or `Hong Kong Island` (58). Exact match only. |
| `address` | Optional | Yes | No | Address in English, such as `*Estate*` or `*Tuen Mun*`. Village homes may show the lot number and an also known as address. |
| `address_zh` | Optional | Yes | No | Address in Traditional Chinese, such as `*屯門*`. |
| `address_zh_hans` | Optional | Yes | No | Address in Simplified Chinese, such as `*屯门*`. Blank for 2 homes. |
| `convictions_flag` | Optional | No | No | `Yes` or `No`, for whether the Social Welfare Department record shows a conviction record for the home. All 350 rows are `No` in this snapshot. |
| `subsidised_places` | Optional | No | Yes | Number of subsidised places, from 0 to 425. Blank for 4 homes where the source gives no number. Use a range, for example `subsidised_places=50..500`. |
| `non_subsidised_places` | Optional | No | Yes | Number of non-subsidised places, from 0 to 123. Use a range, for example `non_subsidised_places=1..200`. |
| `total_places` | Optional | No | Yes | Subsidised plus non-subsidised places, from 6 to 425. Blank for the 4 homes with no subsidised number. Use a range, for example `total_places=100..500` (34). |
| `gender_served` | Optional | Yes | No | Who the home serves: `Male, Female` (268), `Male` (50) or `Female` (28). Blank for 4 homes. `Female` matches only homes for women, while `*Female*` (296) matches every home that takes women. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.24066 to 22.52916. Use a range to search a box, for example `latitude=22.3..22.4`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 113.93335 to 114.2725. Use a range to search a box, for example `longitude=114.1..114.2`. |
| `geocode_quality` | Optional | No | No | How well the address matched the map lookup: `exact` (327) or `approximate` (23, matched to the estate, village or building group). Exact match only. |
| `phone` | Disallowed | No | No | Main phone number as text, 8 digits without a country code. |
| `phone_2` | Disallowed | No | No | Second phone number as text. Blank for 339 homes. |
| `fax` | Disallowed | No | No | Fax number as text. |
| `source` | Disallowed | No | No | Data source and licence credit for the row. |
| `updated` | Disallowed | No | No | Date the data was pulled, such as `2026-11-01`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Chinese text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID, from 1 to 350. Use it as the row key, since licence numbers repeat. |
| `name` | string | Home name in English. |
| `name_zh` | string | Home name in Traditional Chinese. |
| `name_zh_hans` | string | Home name in Simplified Chinese. Null for 3 homes. |
| `licence_number` | string | Social Welfare Department licence or certificate number. Not unique. |
| `home_type` | string | `Subvented`, `Private` or `Self-financing`. |
| `service_type` | string | Service the unit provides, in English. |
| `service_type_zh` | string | Service the unit provides, in Traditional Chinese. |
| `district` | string | Hong Kong district. |
| `region` | string | `Hong Kong Island`, `Kowloon` or `New Territories`. |
| `address` | string | Address in English. |
| `address_zh` | string | Address in Traditional Chinese. |
| `address_zh_hans` | string | Address in Simplified Chinese. Null for 2 homes. |
| `phone` | string | Main phone number, as text. |
| `phone_2` | string | Second phone number, as text. Often null. |
| `fax` | string | Fax number, as text. |
| `convictions_flag` | string | `Yes` if the record shows a conviction record, otherwise `No`. |
| `subsidised_places` | integer | Number of subsidised places. Null for 4 homes. |
| `non_subsidised_places` | integer | Number of non-subsidised places. |
| `total_places` | integer | Subsidised plus non-subsidised places. Null for 4 homes. |
| `gender_served` | string | `Male, Female`, `Male` or `Female`. Null for 4 homes. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). |
| `longitude` | number | Longitude in decimal degrees (WGS 84). |
| `geocode_quality` | string | `exact` or `approximate`. |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the data was pulled, as `YYYY-MM-DD`. |

Notes: the source has one row per service unit, so one licensed home can show up more than once with a different name and service type, and `licence_number` alone does not pick one row. The source gives the home type and service type in one field, which is split here into `home_type` and `service_type`. RCHD in the service type stands for residential care home for persons with disabilities. Names and addresses came in capital letters and were put into title case, so a few odd words may remain. Source spellings are kept. Blank values in the source, such as the Simplified Chinese name for 3 homes and the gender for 4 homes, stay blank and are not filled in. Every row has coordinates from the Hong Kong Government Address Lookup Service. Rows marked `approximate` should not be used for exact map pins. The `convictions_flag` field repeats a public Social Welfare Department field and is not a finding of this API.

## Examples

Halfway houses in Sha Tin, listing the names. This call returns 5 rows:

```bash
curl -s "https://api.jael.ee/JLEE/hk_disability_care_homes?district=Sha%20Tin&service_type=*Halfway*" | jq -r '.[].name'
```

```text
Stewards - Yiu On Halfway House
New Life Psychiatric Rehabilitation Association Pok Hong Halfway House
New Life Psychiatric Rehabilitation Association Sun Chui Halfway House
SRACP Chun Shek Halfway House
The Mental Health Association of Hong Kong Hin Keng House
```

Private homes in Wan Chai:

```bash
curl -s "https://api.jael.ee/JLEE/hk_disability_care_homes?district=Wan%20Chai&home_type=Private"
```

Response (1 row):

```json
[
  {
    "__id": 343,
    "name": "Harmony Care Home",
    "name_zh": "和樂軒",
    "name_zh_hans": "和乐轩",
    "licence_number": "D0356",
    "home_type": "Private",
    "service_type": "Bought Place Scheme (BPS) for Private RCHDs",
    "service_type_zh": "參與買位計劃",
    "district": "Wan Chai",
    "region": "Hong Kong Island",
    "address": "Shop No.8 on G/F and Portion of M/F, Wing Tak Mansion, 15 Canal Road West, Hong Kong",
    "address_zh": "香港堅拿道西15號永德大廈地下8號舖及部分閣樓",
    "address_zh_hans": "香港坚拿道西15号永德大厦地下8号铺及部分阁楼",
    "phone": "36123138",
    "phone_2": null,
    "fax": "35683468",
    "convictions_flag": "No",
    "subsidised_places": 61,
    "non_subsidised_places": 82,
    "total_places": 143,
    "gender_served": "Male, Female",
    "latitude": 22.27799,
    "longitude": 114.18082,
    "geocode_quality": "exact",
    "source": "Social Welfare Department via DATA.GOV.HK; coordinates from Address Lookup Service",
    "updated": "2026-11-01"
  }
]
```

Large homes anywhere in Hong Kong, with a range on `total_places`. This call returns 10 rows:

```bash
curl -s "https://api.jael.ee/JLEE/hk_disability_care_homes?district=*&total_places=200..500" | jq -r '.[] | "\(.name) | \(.district) | \(.total_places)"'
```

```text
Caritas Lai Hong Home | Kwai Tsing | 425
Po Leung Kuk Y.C.Cheng Centre | Sha Tin | 212
New Life Psychiatric Rehabilitation Association Tuen Mun Long Stay Care Home | Tuen Mun | 210
...
```

A wildcard on the Chinese name. This call returns 27 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/hk_disability_care_homes" --data-urlencode "district=*" --data-urlencode "name_zh=*扶康會*" | jq -r '.[0:3][].name_zh'
```

```text
扶康會潔康之家
扶康會興華成人訓練中心
扶康會思諾成人訓練中心
```

Homes in a box over central Kowloon, with ranges on latitude and longitude. This call returns 117 rows, and the first three are The Hong Kong Society for the Blind Kowloon Home for the Aged Blind, Kei Tak Rehabilitation Home Limited and Sing Hong Association Rehabilitation Centre:

```bash
curl -s "https://api.jael.ee/JLEE/hk_disability_care_homes?district=*&latitude=22.3..22.4&longitude=114.1..114.2" | jq -r '.[0:3][].name'
```

## Data source

The home list comes from the Social Welfare Department open data file [List of Residential Care Homes for Persons with Disabilities](https://data.gov.hk/en-data/dataset/hk-swd-lrb-list-of-residential-care-homes-for-persons-with-disabilities), published on DATA.GOV.HK and downloaded on 2026-11-01. English, Traditional Chinese and Simplified Chinese fields come from the same file. Map coordinates come from the [Address Lookup Service](https://www.als.gov.hk) of the Digital Policy Office. Of 350 rows, 327 matched exactly and 23 matched approximately. Changes: names and addresses put in title case, the type split into home type and service type, and place totals and region added. The data is not guaranteed to be complete or accurate.

Credit line: Source: Social Welfare Department, Government of the Hong Kong Special Administrative Region, via DATA.GOV.HK (List of Residential Care Homes for Persons with Disabilities). Coordinates from the Address Lookup Service of the Digital Policy Office. Data is provided AS IS by the Government and the Relevant Organisations. Use of the data follows the [DATA.GOV.HK terms and conditions](https://data.gov.hk/en/terms-and-conditions), which allow commercial and non-commercial use free of charge as long as the source is credited. This API is not endorsed by the Government of the HKSAR, the Social Welfare Department or DATA.GOV.HK, and it uses no government logos.

[Back to the API list](../../README.md)
