# Hong Kong Department of Health clinics

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access Public](https://img.shields.io/badge/access-Public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 202](https://img.shields.io/badge/records-202-lightgrey) ![updated 2026-08-13](https://img.shields.io/badge/updated-2026--08--13-orange)

202 service location records across 13 Department of Health service types in Hong Kong, with English and Traditional Chinese names and addresses, clinic phones, published hours and coordinates. Choose a required service type, then filter by name, hours text or a coordinate range.

These are dated source lists. Some services have eligibility limits, and some chest clinic hours are stale. Confirm current service and opening hours with DH before visiting. The badge shows the latest source file date; individual row dates run from 2020-04-23 to 2026-08-13.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_dh_clinics` |
| Method | `GET` |
| Authentication | None. Access is Public. |
| Response | JSON array with 13 fields per record and no `__id` |
| Record limit | None |

## Query parameters

Required: 1 field (`service_type`). Optional: 5 fields. Disallowed: 7 fields.

`service_type` must be in every call and must match a whole service label. It does not accept wildcards. A call without it returns HTTP 400. Disallowed fields appear in the response but cannot be used as filters.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `service_type` | Required | No | No | Exact service label, such as `Travel Health` (2). Required in every call; wildcards and ranges are off. |
| `name` | Optional | Yes | No | English name. Use `*Chai Wan*` to match that phrase within a name. |
| `name_zh` | Optional | Yes | No | Traditional Chinese name. With `service_type=Student Health`, `name_zh=*柴灣*` returns 2 rows. |
| `service_hours` | Optional | Yes | No | English hours text. With `service_type=Travel Health`, `service_hours=*Saturday*` returns 1 row. A text match does not confirm that a clinic is open. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees. Use two dots, such as `22.3..22.4`. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees. Use two dots, such as `114.1..114.25`. |
| `address` | Disallowed | No | No | English street address from the source list. |
| `address_zh` | Disallowed | No | No | Traditional Chinese street address. |
| `phone` | Disallowed | No | No | Clinic contact or inquiry landline(s), as text. Hong Kong country code: +852. |
| `appointment_phone` | Disallowed | No | No | Separate appointment landline(s) for the seven Families Clinics rows. Null on the other 195 rows. |
| `service_hours_zh` | Disallowed | No | No | Traditional Chinese primary hours text. For chest rows this covers registration only; for emergency dental rows it omits the separate AM cell. |
| `source` | Disallowed | No | No | Official Department of Health file URL for the row. |
| `updated` | Disallowed | No | No | English source file HTTP Last-Modified date as YYYY-MM-DD text. This is a file date, not confirmation of current clinic hours. |

Partial matching uses `*` wildcards. Filters are combined with AND, and text matches ignore case. Only `lat` and `lng` accept ranges, using two dots (`min..max`). Chinese text must be URL encoded; curl can do this with `-G --data-urlencode`. See [How filters work](../../README.md#how-filters-work).

### Allowed service types

Use the full label in `service_type`:

| Value | Records |
|---|---|
| `Maternal and Child Health` | 34 |
| `Chest Clinics and X-ray` | 21 |
| `Methadone` | 19 |
| `Elderly Health` | 18 |
| `Student Health` | 17 |
| `Emergency Dental (Public Sessions)` | 11 |
| `Social Hygiene` | 8 |
| `Dermatology` | 7 |
| `Child Assessment` | 7 |
| `Families Clinics` | 7 |
| `Travel Health` | 2 |
| `Government Dental (Eligible Persons)` | 42 |
| `School Dental` | 9 |

## Response fields

All fields are strings except `lat` and `lng`, which are numbers. Empty source cells are returned as null.

| Field | Type | Description |
|---|---|---|
| `service_type` | string | English service label. The 13 allowed values and their counts are listed below. |
| `name` | string | English clinic or center name. Eligibility and temporary closure notes remain in the name. |
| `name_zh` | string | Traditional Chinese clinic or center name. |
| `address` | string | English street address from the source list. |
| `address_zh` | string | Traditional Chinese street address. |
| `phone` | string | Clinic contact or inquiry landline(s), as text. Hong Kong country code: +852. |
| `appointment_phone` | string | Separate appointment landline(s) for the seven Families Clinics rows. Null on the other 195 rows. |
| `service_hours` | string | Published English hours and notes. Chest rows label registration and anti-TB treatment hours separately. Some chest schedules are stale. |
| `service_hours_zh` | string | Traditional Chinese primary hours text. For chest rows this covers registration only; for emergency dental rows it omits the separate AM cell. |
| `lat` | number | Latitude in decimal degrees. Seven Child Assessment points were converted from DMS after address checks. |
| `lng` | number | Longitude in decimal degrees. All 202 records have coordinates. |
| `source` | string | Official Department of Health file URL for the row. |
| `updated` | string | English source file HTTP Last-Modified date as YYYY-MM-DD text. This is a file date, not confirmation of current clinic hours. |

## Examples

Travel Health rows whose published hours mention Saturday. Verified against the live API on 2026-11-07 (MYT); this call returns 1 row:

```bash
curl -sS -A 'Mozilla/5.0' 'https://api.jael.ee/JLEE/hk_dh_clinics?service_type=Travel%20Health&service_hours=*Saturday*'
```

Response excerpt, showing selected fields from that row:

```json
[
  {
    "service_type": "Travel Health",
    "name": "Travel Health Centre ( Kowloon )",
    "name_zh": "旅遊健康中心 (九龍)",
    "phone": "2150 7235",
    "appointment_phone": null,
    "lat": 22.331811,
    "lng": 114.160309,
    "updated": "2021-03-30"
  }
]
```

Child Assessment records within a coordinate box. This call returns 5 rows:

```bash
curl -sSG -A 'Mozilla/5.0' 'https://api.jael.ee/JLEE/hk_dh_clinics' --data-urlencode 'service_type=Child Assessment' --data-urlencode 'lat=22.3..22.4' --data-urlencode 'lng=114.1..114.25'
```

## Data notes

- The 202 rows describe services at locations, not 202 distinct buildings. They include 42 government dental records for eligible persons, 11 emergency dental public session records and 9 school dental records. A clinic can appear under more than one service type.
- Maternal and Child Health includes three Woman Health Centers and source labels for temporary closures. Student Health includes four Special Assessment Centers. Families Clinics includes the Education and Training Center in Family Medicine. Source names retain their original spelling and eligibility notes.
- English and Traditional Chinese rows were joined by clinic telephone, with matching row order and coordinates used only to resolve ambiguous keys. All 202 English rows matched. No administrative district field is provided.
- Seven Child Assessment coordinate pairs used degrees, minutes and seconds written as decimal-looking values. They were converted to decimal degrees after address checks, placing each within 47 meters of a matching address reference. All 202 points fall within latitude 22.15..22.57 and longitude 113.83..114.45. These checks do not establish survey accuracy.
- The [current DH chest clinic page](https://www.info.gov.hk/tb_chest/en/contents/c614_1July2007.htm) differs from the downloadable list. Wanchai has provided treatment only since 2026-08-31. Shek Kip Mei and Sai Kung services are suspended. Sai Ying Pun registration is on Monday, Tuesday, Wednesday and Friday; Shaukeiwan registration is on Monday, Wednesday, Thursday and Friday. The API retains the downloaded hours, so check DH before visiting. Matching `*Saturday*` can also match a restriction or note.
- `updated` is the English source file HTTP Last-Modified date, using the UTC+8 calendar date. It does not mean that DH checked every clinic on that date. Other than the seven coordinate conversions, source facts and hours remain unchanged.

## Data source and license

Source: Department of Health, Government of the Hong Kong Special Administrative Region, through DATA.GOV.HK, [Lists of Clinics / Centres of the Department of Health](https://data.gov.hk/en-data/dataset/hk-dh-dh_hq-list-of-dh-clinics). Each row's `source` links to its official file.

The [DATA.GOV.HK Terms and Conditions](https://data.gov.hk/en/terms-and-conditions), version 1.2 (26 May 2025), allow commercial and noncommercial reuse free of charge with clear attribution to the Government, Department of Health and DATA.GOV.HK, acknowledging their intellectual property rights. The data is provided as is. No government logos are used, and no government endorsement is implied.

Changes: combined the clinic service lists, joined English and Traditional Chinese records, mapped column names to snake_case, cleaned whitespace, kept separate appointment phones, added source URLs and file dates, removed the empty district column, and converted seven verified Child Assessment coordinate pairs from DMS to decimal degrees. School assignment lists were excluded because they describe schools assigned to clinics already included. Eligibility and closure labels and the downloaded hours were retained.

[Back to the API list](../../README.md)
