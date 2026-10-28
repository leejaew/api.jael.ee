# Hong Kong Elderly Care Homes

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 857](https://img.shields.io/badge/records-857-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange)

Licensed residential care homes for the elderly in Hong Kong, 857 rows from the Social Welfare Department list. Each row has the English and Traditional Chinese name and address, licence number, care type, district, phone, accreditation, faith, meal type, a conviction record flag and map coordinates. It also lists 29 cross border homes in Guangdong, marked with district `Guangdong Province`. The source file changes daily, so this is a snapshot from 2026-10-28.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_elderly_care_homes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`district`). Optional: 15 fields. Disallowed: 5 fields.

`district` must be in every call. Use `district=*` to list all 857 homes. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `district` | Required | Yes | No | Hong Kong district of the home, such as `Kwai Tsing` (77), `Sham Shui Po` (76), `Eastern` (71), `Kowloon City` (70) or `Yau Tsim Mong` (64), or `Guangdong Province` (29) for cross border homes. Wildcards work, so `district=*` lists all 857 homes. |
| `name` | Optional | Yes | No | Home name in English, such as `*Kin Pak*` or `(Ap Lei Chau)*`. |
| `name_zh` | Optional | Yes | No | Home name in Traditional Chinese, such as `*護老*` or `*安老院*`. |
| `licence_number` | Optional | Yes | No | Social Welfare Department licence number, such as `L0008` or `L00*`. Blank for the 29 Guangdong homes. |
| `care_type` | Optional | Yes | No | Care places the home is licensed for. Values include `Care and Attention` (632), `Care and Attention (Continuum of Care)` (120), `Home for the Aged, Care and Attention` (42) and `Nursing Home` (9). Use a wildcard such as `*Nursing Home*` to match combined values. |
| `region` | Optional | No | No | `New Territories` (369), `Kowloon` (291), `Hong Kong Island` (168) or `Guangdong` (29). |
| `address` | Optional | Yes | No | Street address in English, such as `*Main Street*`. |
| `address_zh` | Optional | Yes | No | Street address in Traditional Chinese, such as `*鴨脷洲*`. |
| `accreditation` | Optional | No | No | `Yes` (107) or `No` (750), for whether the home holds Social Welfare Department accreditation. |
| `religious_background` | Optional | Yes | No | Faith of the home, such as `None`, `Christian`, `Buddhist`, `Catholic` or `Taoist`. A cell can list more than one, so `*Buddhist*` matches them all. Blank for 128 homes. |
| `meal_provision` | Optional | Yes | No | Meal type offered: `Non-vegetarian` (617), `Non-vegetarian, Vegetarian` (156) or `Vegetarian` (13). Blank for 71 homes. Use `*Vegetarian*` to match any home that offers vegetarian meals. |
| `convictions_flag` | Optional | No | No | `Yes` (9) or `No` (848), for whether the Social Welfare Department record shows a conviction record for the home in the last 24 months. |
| `community_care_voucher_provider` | Optional | No | No | `Yes` (49) or `No` (808), for homes that are also community care service voucher providers. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.20648 to 22.54548. Blank for 60 homes with no safe address match. Use a range to search a box, for example `latitude=22.3..22.4`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 113.9385 to 114.29351. Blank for 60 homes with no safe address match. Use a range to search a box, for example `longitude=114.1..114.2`. |
| `geocode_quality` | Optional | No | No | How well the address matched the map lookup: `exact` (781), `approximate` (16) or `none` (60). |
| `phone` | Disallowed | No | No | Main phone number as text, 8 digits without a country code for Hong Kong homes. |
| `phone_2` | Disallowed | No | No | Second phone number as text. Blank for 583 homes. |
| `fax` | Disallowed | No | No | Fax number as text. |
| `source` | Disallowed | No | No | Data source and licence credit for the row. |
| `updated` | Disallowed | No | No | Date the data was pulled, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Chinese text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Home name in English. |
| `name_zh` | string | Home name in Traditional Chinese. |
| `licence_number` | string | Social Welfare Department licence number. Null for Guangdong homes. |
| `care_type` | string | Care places the home is licensed for. |
| `district` | string | Hong Kong district, or `Guangdong Province`. |
| `region` | string | `Hong Kong Island`, `Kowloon`, `New Territories` or `Guangdong`. |
| `address` | string | Street address in English. |
| `address_zh` | string | Street address in Traditional Chinese. |
| `phone` | string | Main phone number, as text. |
| `phone_2` | string | Second phone number, as text. Often null. |
| `fax` | string | Fax number, as text. |
| `accreditation` | string | `Yes` or `No`. |
| `religious_background` | string | Faith of the home, or `None`. Can list more than one. |
| `meal_provision` | string | Meal type offered. Null if not stated. |
| `convictions_flag` | string | `Yes` if the record shows a conviction record in the last 24 months, otherwise `No`. |
| `community_care_voucher_provider` | string | `Yes` if the home is also a community care service voucher provider. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). Null if no safe match. |
| `longitude` | number | Longitude in decimal degrees (WGS 84). Null if no safe match. |
| `geocode_quality` | string | `exact`, `approximate` or `none`. |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the data was pulled. |

Notes: the source list holds 1,105 service units. This API keeps the 828 licensed homes with a licence number and the 29 Guangdong cross border homes, and leaves out 247 community care and day care providers that are not licensed residential homes. Names and addresses came in capital letters and were put into title case, so a few odd words may remain. Source spellings such as Shamshuipo are kept. The 52 homes that also act as community care service voucher providers had a second name part, which was dropped, and `community_care_voucher_provider` shows it for 49 homes. Coordinates come from the Hong Kong Government Address Lookup Service. Rows marked `approximate` or `none` should not be used for exact map pins. The `convictions_flag` field repeats a public Social Welfare Department field and is not a finding of this API.

## Examples

Nursing homes in Kwai Tsing, listing the names. This call returns 6 rows:

```bash
curl -s "https://api.jael.ee/JLEE/hk_elderly_care_homes?district=Kwai%20Tsing&care_type=*Nursing*" | jq -r '.[].name'
```

```text
Po Leung Kuk Comfort Court for the Senior Cum Evergreen Day Care Centre for the Elderly
Evangelical Lutheran Church of Hong Kong, Grace Court
Evergreen Nursing Home Cum Day Care Centre
Evergreen (Kwai Chung Estate) Nursing Home
Aura Nursing Home Cum Day Care Centre
The Methodist Church, Hong Kong Yang Memorial Methodist Social Service Tsing Yi Nursing Home
```

Accredited homes in Wan Chai:

```bash
curl -s "https://api.jael.ee/JLEE/hk_elderly_care_homes?district=Wan%20Chai&accreditation=Yes"
```

Response (1 row):

```json
[
  {
    "__id": 764,
    "name": "Pine Residence",
    "name_zh": "松心薈",
    "licence_number": "L1471",
    "care_type": "Care and Attention",
    "district": "Wan Chai",
    "region": "Hong Kong Island",
    "address": "Lobby on G/F and the Front (Or Southern) Portions of 1/F, 2/F and 3/F of 1 Leighton Road, Causeway Bay, Hong Kong",
    "address_zh": "香港銅鑼灣禮頓道1號地面大堂及1樓、2樓和3樓各層前座（或南面）部分",
    "phone": "35687498",
    "phone_2": "30188006",
    "fax": "35687499",
    "accreditation": "Yes",
    "religious_background": null,
    "meal_provision": "Non-vegetarian, Vegetarian",
    "convictions_flag": "No",
    "community_care_voucher_provider": "No",
    "latitude": 22.27672,
    "longitude": 114.18068,
    "geocode_quality": "exact",
    "source": "Social Welfare Department via DATA.GOV.HK; coordinates from Address Lookup Service",
    "updated": "2026-10-28"
  }
]
```

A wildcard on the Chinese name. This call returns 402 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/hk_elderly_care_homes" --data-urlencode "district=*" --data-urlencode "name_zh=*護老*" | jq -r '.[0:3][].name_zh'
```

```text
(鴨脷洲)健柏護老之家
新陶園護老院
康泰護老之家有限公司
```

Homes in a box around the Wan Chai and Causeway Bay waterfront, with ranges on latitude and longitude. This call returns 21 rows, and the first three are Bedford Nursing Centre, Fook Yen Home for the Aged Limited and Lok Seen Aged Hostel:

```bash
curl -s "https://api.jael.ee/JLEE/hk_elderly_care_homes?district=*&latitude=22.28..22.30&longitude=114.15..114.20" | jq -r '.[0:3][].name'
```

A cross border home in Guangdong, which has no licence number or coordinates:

```bash
curl -s "https://api.jael.ee/JLEE/hk_elderly_care_homes?district=Guangdong*&name=*Yee*"
```

Response (1 row):

```json
[
  {
    "__id": 829,
    "name": "Hong Kong Jockey Club Shenzhen Society for Rehabilitation Yee Hong Heights",
    "name_zh": "香港賽馬會深圳復康會頤康院",
    "licence_number": null,
    "care_type": "Home for the Aged, Care and Attention, Nursing Home",
    "district": "Guangdong Province",
    "region": "Guangdong",
    "address": "1 Qingyun Road, Yantian District, Shenzhen",
    "address_zh": "深圳市鹽田區青雲路1號",
    "phone": "85225343428",
    "phone_2": "8675525282528",
    "fax": "8675525285111",
    "accreditation": "Yes",
    "religious_background": "None",
    "meal_provision": "Non-vegetarian, Vegetarian",
    "convictions_flag": "No",
    "community_care_voucher_provider": "No",
    "latitude": null,
    "longitude": null,
    "geocode_quality": "none",
    "source": "Social Welfare Department via DATA.GOV.HK; coordinates from Address Lookup Service",
    "updated": "2026-10-28"
  }
]
```

## Data source

The home list comes from the Social Welfare Department open data file on residential care homes for the elderly, published on DATA.GOV.HK and downloaded on 2026-10-28. English and Traditional Chinese fields come from the same file. Map coordinates come from the Address Lookup Service of the Digital Policy Office, also on DATA.GOV.HK. Of 857 rows, 781 matched exactly, 16 matched approximately and 60 have no coordinates, which includes the 29 Guangdong homes. The data is not guaranteed to be complete or accurate.

Credit line: Source: Social Welfare Department, Government of the Hong Kong SAR, via DATA.GOV.HK (Basic Information and Service Quality Information of Residential Care Homes for the Elderly). Coordinates from the Address Lookup Service of the Digital Policy Office, via DATA.GOV.HK. Data is provided AS IS by the Government and the Relevant Organisations. Use of the data follows the [DATA.GOV.HK terms and conditions](https://data.gov.hk/en/terms-and-conditions).

[Back to the API list](../../README.md)
