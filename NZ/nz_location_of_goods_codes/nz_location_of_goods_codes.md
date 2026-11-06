# New Zealand Location of Goods codes

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 328](https://img.shields.io/badge/records-328-lightgrey) ![updated 2026-11-05](https://img.shields.io/badge/updated-2026--11--05-orange)

All 328 premises from the New Zealand Customs Service Location of Goods list for Trade Single Window (TSW) filings, as at 5 November 2026. Each row has a premises name, street address, city, TF or port authority code, CCA or port code, and TSW delivery notification type. Filter by the required `city`, then by premises name, TF code, CCA code or notification type.

The list covers 31 cities. The largest are Auckland (149), Christchurch (59) and Tauranga (16). There are no coordinates. TF and CCA codes are text and are not unique: TF code `31773` appears on 5 Champion Freight rows, and CCA code `NZBLU` appears on 2 Invercargill rows.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_location_of_goods_codes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row, with 8 fields and no `__id` |
| Record limit | None |

## Query parameters

Required: 1 field (`city`). Optional: 4 fields. Disallowed: 3 fields.

`city` must be in every call. Use `city=*` to list all 328 rows. A call without `city` returns HTTP 400. Disallowed fields are returned in the output but cannot be used as filters.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `city` | Required | Yes | No | City from the Customs list. `city=Auckland` returns 149 rows; `city=Christchurch` returns 59; `city=Tauranga` returns 16. Use `city=*` for all rows. |
| `premises_name` | Optional | Yes | No | Premises name as listed. `premises_name=*NORTHPORT*` returns 2 rows with `city=*`. |
| `tf_code` | Optional | No | No | Exact TF or port authority code as text, such as `tf_code=32523` (1 row) or `tf_code=31773` (5 rows) with `city=*`. Not a unique key. |
| `cca_code` | Optional | No | No | Exact CCA or port code as text, such as `cca_code=14695B` (1 row) or `cca_code=NZBLU` (2 rows) with `city=*`. Not a unique key. |
| `notification_type` | Optional | Yes | No | TSW delivery notification type: `Email` (263), `Messaging` (30), or `Not set up to receive TSW delivery notifications` (35). |
| `address` | Disallowed | No | No | Full street address from the Customs list. |
| `source` | Disallowed | No | No | Source credit for the row. |
| `updated` | Disallowed | No | No | Source list date as text: `2026-11-05`. |

Partial matching uses `*` wildcards. Filters are combined with AND, and text matches ignore case. Ranges are not used on this API. See [How filters work](../../README.md#how-filters-work).

## Response fields

| Field | Type | Description |
|---|---|---|
| `premises_name` | string | Premises name from the Customs Location of Goods list. |
| `address` | string | Physical street address from the list. |
| `city` | string | City from the list (Title Case). |
| `tf_code` | string | TF or port authority code. Text. Not a unique key. |
| `cca_code` | string | CCA or port code. Text. Not a unique key. |
| `notification_type` | string | How the premises receives TSW delivery notifications. |
| `source` | string | Source credit for the row. |
| `updated` | string | Source list date, `2026-11-05` (`YYYY-MM-DD`). |

## Examples

Auckland. This call returns 149 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_location_of_goods_codes?city=Auckland"
```

Response excerpt, showing the first row:

```json
[
  {
    "premises_name": "A HARTRODT (NEW ZEALAND) LTD",
    "address": "10 Brigade Road Airport Oaks Auckland New Zealand 2022",
    "city": "Auckland",
    "tf_code": "18856",
    "cca_code": "8615A",
    "notification_type": "Email",
    "source": "NZ Customs Location of Goods list, as at 2026-11-05",
    "updated": "2026-11-05"
  }
]
```

TF code lookup. This call returns 1 row (Fairfield Freight Hub in Ashburton):

```bash
curl -s "https://api.jael.ee/JLEE/nz_location_of_goods_codes?city=*&tf_code=32523"
```

```json
[
  {
    "premises_name": "FAIRFIELD FREIGHT HUB",
    "address": "109 Works Road Fairton Ashburton 7772 New Zealand",
    "city": "Ashburton",
    "tf_code": "32523",
    "cca_code": "14695B",
    "notification_type": "Email",
    "source": "NZ Customs Location of Goods list, as at 2026-11-05",
    "updated": "2026-11-05"
  }
]
```

Premises name wildcard. This call returns 2 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_location_of_goods_codes?city=*&premises_name=*NORTHPORT*"
```

TF code with five matching premises. This call returns 5 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_location_of_goods_codes?city=*&tf_code=31773"
```

## Data source

Source: New Zealand Customs Service, [Location of Goods Code List](https://www.customs.govt.nz/business/trade-single-window-tsw/location-of-goods-codes), as at 5 November 2026 ([spreadsheet](https://www.customs.govt.nz/media/1qjcqoiu/location-of-goods.xlsx)).

The [Customs copyright page](https://www.customs.govt.nz/about-us/using-our-website/copyright) licenses the material under Creative Commons Attribution 4.0 New Zealand (CC BY 4.0 NZ). The suggested attribution also names Creative Commons Attribution 4.0 International (CC BY 4.0). Commercial reuse is allowed with attribution to the Crown and New Zealand Customs Service. Logos, emblems and the Coat of Arms are excluded.

Suggested credit: "This work is based on/includes New Zealand Customs data which is licensed by New Zealand Customs Service for re-use under the Creative Commons Attribution 4.0 International licence."

Changes: extracted all 328 data rows, mapped headers to snake_case, trimmed whitespace, normalized nine all-caps city spellings to Title Case, kept TF and CCA codes as text, and added `source` and `updated`. No coordinates were added.

[Back to the API list](../../README.md)
