# Hong Kong Licensed Hotels and Guesthouses

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1807](https://img.shields.io/badge/records-1807-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange)

Licensed hotels and guesthouses in Hong Kong, 1,807 premises from the Home Affairs Department list. Each row has the English and Traditional Chinese name and address, licence number, region, district, phone, fax, email and map coordinates. The source list changes monthly, so this is a snapshot from 2026-10-28 (source file dated 2026-10-08).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_licensed_hotels_guesthouses` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 9 fields. Disallowed: 5 fields.

`region` must be in every call. Use `region=*` to list all 1,807 premises. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | `Kowloon` (1,299), `Hong Kong Island` (270) or `New Territories` (238), read from the address. Wildcards work, so `region=*` lists all 1,807 premises. |
| `name` | Optional | Yes | No | Premises name in English, such as `*Mansion*` or `Dragon*`. 1,461 distinct names for 1,807 rows. |
| `name_zh` | Optional | Yes | No | Premises name in Traditional Chinese, such as `*賓館*`. 369 rows hold English text in the source. |
| `licence_number` | Optional | Yes | No | Home Affairs Department licence number, such as `H/0009` or `H/00*`. Unique for every row. |
| `district` | Optional | Yes | No | One of 18 Hong Kong districts, such as `Yau Tsim Mong` (1,178), `Wan Chai` (130) or `Islands` (109). Blank for 18 rows with no safe map match. |
| `address` | Optional | Yes | No | Street address in English, such as `*Nathan Road*` or `*Chungking*`. |
| `address_zh` | Optional | Yes | No | Street address in Traditional Chinese, such as `*彌敦道*`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.20338 to 22.52257. Blank for 18 rows. Use a range to search a box, for example `latitude=22.29..22.31`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 113.85827 to 114.33404. Blank for 18 rows. Use a range to search a box, for example `longitude=114.16..114.18`. |
| `geocode_quality` | Optional | No | No | How well the address matched the map lookup: `exact` (1,699), `approximate` (90) or `none` (18). |
| `phone` | Disallowed | No | No | Premises phone number as text, usually 8 digits with a space. Blank for 157 rows. |
| `fax` | Disallowed | No | No | Premises fax number as text. Blank for 1,083 rows. |
| `email` | Disallowed | No | No | Premises contact email. Blank for 666 rows. |
| `source` | Disallowed | No | No | Data source and licence credit for the row. |
| `updated` | Disallowed | No | No | Date the data was pulled, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Chinese text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Premises name in English. |
| `name_zh` | string | Premises name in Traditional Chinese. |
| `licence_number` | string | Home Affairs Department licence number. |
| `region` | string | `Hong Kong Island`, `Kowloon` or `New Territories`. |
| `district` | string | Hong Kong district. Null if no safe match. |
| `address` | string | Street address in English. |
| `address_zh` | string | Street address in Traditional Chinese. |
| `phone` | string | Phone number, as text. Often null or two numbers split by a slash. |
| `fax` | string | Fax number, as text. Often null. |
| `email` | string | Contact email. Often null. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). Null if no safe match. |
| `longitude` | number | Longitude in decimal degrees (WGS 84). Null if no safe match. |
| `geocode_quality` | string | `exact`, `approximate` or `none`. |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the data was pulled. |

Notes: the source list is a plain list of licences. It has no hotel or guesthouse type, no room counts, no status, no district and no coordinates, so none of those columns are invented. District and coordinates come from the Hong Kong Government Address Lookup Service, and 18 rows with no safe match have none. Rows marked `approximate` or `none` should not be used for exact map pins. Names and addresses came in capital letters and were put into title case, so a few odd words may remain. Source spellings are kept. Many guesthouses share a name or a building, so use `licence_number` to pick one premises. The source has English text in `name_zh` for 369 rows, which is kept as given.

## Examples

Hotels in Wan Chai on Hong Kong Island, listing the names. This call returns 27 rows:

```bash
curl -s "https://api.jael.ee/JLEE/hk_licensed_hotels_guesthouses?region=Hong%20Kong%20Island&district=Wan%20Chai&name=*Hotel*" | jq -r '.[0:3][].name'
```

```text
Renaissance Harbour View Hotel, HK
Empire Hotel Hong Kong
South Pacific Hotel
```

One premises by licence number:

```bash
curl -s "https://api.jael.ee/JLEE/hk_licensed_hotels_guesthouses?region=Kowloon&licence_number=H/0009"
```

Response (1 row):

```json
[
  {
    "__id": 1,
    "name": "Sheraton Hong Kong Hotel & Towers",
    "name_zh": "香港喜來登酒店",
    "licence_number": "H/0009",
    "region": "Kowloon",
    "district": "Yau Tsim Mong",
    "address": "20 Nathan Road, Tsim Sha Tsui, Kowloon",
    "address_zh": "九龍尖沙咀彌敦道20號",
    "phone": "2369 1111",
    "fax": "2739 8707",
    "email": "res.hongkong@sheraton.com",
    "latitude": 22.29526,
    "longitude": 114.17273,
    "geocode_quality": "exact",
    "source": "Home Affairs Department via DATA.GOV.HK; coordinates and districts from Address Lookup Service",
    "updated": "2026-10-28"
  }
]
```

A wildcard on the Chinese name. This call returns 539 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/hk_licensed_hotels_guesthouses" --data-urlencode "region=*" --data-urlencode "name_zh=*賓館*" | jq -r '.[0:3][].name_zh'
```

```text
明珠賓館
四季陽光國際賓館
海洋賓館
```

Premises in a box around Tsim Sha Tsui, with ranges on latitude and longitude. This call returns 797 rows, and the first three are Sheraton Hong Kong Hotel & Towers, Whole Sunshine International Guest House and Stanford Hillview Hotel:

```bash
curl -s "https://api.jael.ee/JLEE/hk_licensed_hotels_guesthouses?region=*&latitude=22.29..22.31&longitude=114.16..114.18" | jq -r '.[0:3][].name'
```

A premises with no coordinates. This call returns 12 rows, and the first is shown:

```bash
curl -s "https://api.jael.ee/JLEE/hk_licensed_hotels_guesthouses?region=New%20Territories&geocode_quality=none" | jq '.[0]'
```

```json
{
  "__id": 187,
  "name": "Cheung Chau Warwick Hotel",
  "name_zh": "長洲華威酒店",
  "licence_number": "H/1382",
  "region": "New Territories",
  "district": null,
  "address": "CC Lot 1147, East Bay, Cheung Chau",
  "address_zh": "長洲東灣第1147號地段",
  "phone": "2981 0976",
  "fax": "2981 9174",
  "email": "admin@warwickhotel.com.hk",
  "latitude": null,
  "longitude": null,
  "geocode_quality": "none",
  "source": "Home Affairs Department via DATA.GOV.HK; coordinates and districts from Address Lookup Service",
  "updated": "2026-10-28"
}
```

## Data source

The licence list comes from the Home Affairs Department (Office of the Licensing Authority) open data file of licensed hotels and guesthouses, published on DATA.GOV.HK. The file was last modified on 2026-10-08 and downloaded on 2026-10-28. English and Traditional Chinese fields come from the same file. Map coordinates and districts come from the Address Lookup Service of the Digital Policy Office, also on DATA.GOV.HK. Of 1,807 rows, 1,699 matched exactly, 90 matched approximately and 18 have no coordinates. The data is not guaranteed to be complete or accurate.

Credit line: Source: Home Affairs Department, Government of the Hong Kong SAR, via DATA.GOV.HK (List of licensed hotels and guesthouses). Coordinates and districts from the Address Lookup Service of the Digital Policy Office, via DATA.GOV.HK. Data is provided AS IS by the Government and the Relevant Organisations. Use of the data follows the [DATA.GOV.HK terms and conditions](https://data.gov.hk/en/terms-and-conditions).

[Back to the API list](../../README.md)
