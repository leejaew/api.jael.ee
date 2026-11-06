# Taiwan tourist service sites

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 591](https://img.shields.io/badge/records-591-lightgrey) ![updated 2026-11-06](https://img.shields.io/badge/updated-2026--11--06-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

All 591 tourist service centres, visitor centres and inquiry stations from the Tourism Administration English pack (joined to Chinese names where matched), with class, city, town, address, phone, service hours, facilities, status and coordinates. Filter by the required `city`, then by name, town, class, status or a latitude and longitude range.

Class counts: airport centre 4, transport hub centre 58, scenic area visitor centre 61, county visitor centre 36, inquiry station 432. The list covers 22 cities and counties. Largest include Nantou County (48), Kaohsiung City (43) and Penghu County (30). Taipei City has 25. `service_status` is `open` on all 591 rows.

English pack names are sometimes still in Chinese. Eleven rows have blank `name_zh` and `address_zh` (no Chinese pack match). Two hundred seventeen Chinese-only sites were not added. Phone is blank on 48 rows where the source listed only a mobile; mobiles were removed.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_tourist_service_sites` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row, with 17 fields and no `__id` |
| Record limit | None |

## Query parameters

Required: 1 field (`city`). Optional: 9 fields. Disallowed: 7 fields.

`city` must be in every call. Use `city=*` to list all 591 rows. A call without `city` returns HTTP 400. Disallowed fields are returned in the output but cannot be used as filters.

`lat` and `lng` are numbers and take a range with two dots, such as `lat=25.0..25.1`. A hyphen range returns HTTP 400. Every other field is text.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `city` | Required | Yes | No | County or city in English, such as `Nantou County` (48), `Kaohsiung City` (43), `Penghu County` (30) or `Taipei City` (25). Use `city=*` for all rows. |
| `name` | Optional | Yes | No | English site name. `name=*Visitor*` returns 143 rows with `city=*`. |
| `name_zh` | Optional | Yes | No | Chinese site name. Blank on 11 English-only rows. |
| `town` | Optional | Yes | No | Township or district in English. `town=Douliu*` returns 5 rows with `city=Yunlin County`. |
| `id` | Optional | No | No | Exact Tourism Administration site id, such as `TourismServiceSite_A15010000H_000001`. |
| `class` | Optional | No | No | Exact class label: `airport centre` (4), `transport hub centre` (58), `scenic area visitor centre` (61), `county visitor centre` (36) or `inquiry station` (432). |
| `class_code` | Optional | No | No | Exact class code as text: `1` to `5`. `class_code=1` returns the 4 airport centres. |
| `service_status` | Optional | No | No | Exact status. All 591 rows are `open`. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84). Use a range, for example `lat=25.0..25.1`. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees (WGS 84). Use a range, for example `lng=121.5..121.6`. |
| `address` | Disallowed | No | No | English street address. |
| `address_zh` | Disallowed | No | No | Chinese street address. Blank on 11 English-only rows. |
| `phone` | Disallowed | No | No | Landline phone. Blank on 48 mobile-only rows. |
| `service_hours` | Disallowed | No | No | Opening hours as listed. |
| `facilities` | Disallowed | No | No | Facilities text when the register gives it. |
| `source` | Disallowed | No | No | Source credit for the row. |
| `updated` | Disallowed | No | No | Source pack date as text: `2026-11-06`. |

Partial matching uses `*` wildcards. Filters are combined with AND, and text matches ignore case. Ranges use two dots (`min..max`). Only `lat` and `lng` accept ranges. See [How filters work](../../README.md#how-filters-work).

## Response fields

| Field | Type | Description |
|---|---|---|
| `id` | string | Tourism Administration tourist service site id. |
| `name` | string | English site name (sometimes still Chinese in the English pack). |
| `name_zh` | string | Chinese site name. Null on 11 English-only rows. |
| `class` | string | Class label (airport centre, transport hub centre, scenic area visitor centre, county visitor centre or inquiry station). |
| `class_code` | string | Class code `1` to `5` as text. |
| `city` | string | County or city in English. |
| `town` | string | Township or district in English. |
| `address` | string | English street address. |
| `address_zh` | string | Chinese street address. Null on 11 English-only rows. |
| `phone` | string | Landline phone. Null when the source listed only a mobile. |
| `lat` | number | Latitude (WGS 84). |
| `lng` | number | Longitude (WGS 84). |
| `service_hours` | string | Opening hours as listed. |
| `facilities` | string | Facilities text when present. Null when blank. |
| `service_status` | string | Status from the register. All rows are `open`. |
| `source` | string | Credit line for the row. |
| `updated` | string | Source pack date, `YYYY-MM-DD`. |

## Examples

Airport centre by id. This call returns 1 row:

```bash
curl -s "https://api.jael.ee/JLEE/tw_tourist_service_sites?city=*&id=TourismServiceSite_A15010000H_000001"
```

```json
[
  {
    "id": "TourismServiceSite_A15010000H_000001",
    "name": "Terminal 1, Tourist Service Center, Taiwan Taoyuan International Airport, Tourism Administration",
    "name_zh": "桃園國際機場旅客服務中心-第一航廈",
    "class": "airport centre",
    "class_code": "1",
    "city": "Taoyuan City",
    "town": "Dayuan District",
    "address": "No. 15, Hangzhan S. Rd.",
    "address_zh": "航站南路15號",
    "phone": "(03)3982194",
    "lat": 25.07704,
    "lng": 121.232,
    "service_hours": "07:00-23:30",
    "facilities": null,
    "service_status": "open",
    "source": "Tourism Administration, MOTC (data.gov.tw 44055)",
    "updated": "2026-11-06"
  }
]
```

Inquiry stations only. This call returns 432 rows:

```bash
curl -s "https://api.jael.ee/JLEE/tw_tourist_service_sites?city=*&class=inquiry%20station"
```

Name wildcard. This call returns 143 rows:

```bash
curl -s "https://api.jael.ee/JLEE/tw_tourist_service_sites?city=*&name=*Visitor*"
```

Yunlin County, Douliu area. This call returns 5 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_tourist_service_sites" --data-urlencode "city=Yunlin County" --data-urlencode "town=Douliu*"
```

Map box over central Taipei. This call returns 19 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_tourist_service_sites" --data-urlencode "city=*" --data-urlencode "lat=25.0..25.1" --data-urlencode "lng=121.5..121.6"
```

## Data source

Source: Tourism Administration, Ministry of Transportation and Communications (MOTC), through [data.gov.tw dataset 44055](https://data.gov.tw/en/datasets/44055). English pack: [TourismServiceSite-json.zip (En)](https://media.taiwan.net.tw/XMLReleaseALL_public/v2.0/En/TourismServiceSite-json.zip). Chinese pack: [TourismServiceSite-json.zip (Zh_tw)](https://media.taiwan.net.tw/XMLReleaseALL_public/v2.0/Zh_tw/TourismServiceSite-json.zip). Pack date 6 November 2026.

Licensed under the [Open Government Data License, version 1.0 (OGDL v1.0)](https://data.gov.tw/license), compatible with Creative Commons Attribution 4.0 International (CC BY 4.0). Commercial reuse is allowed with attribution.

Changes: English and Chinese rows joined on `TourismServiceSiteID`; class codes mapped to English labels (1 airport centre, 2 transport hub centre, 3 scenic area visitor centre, 4 county visitor centre, 5 inquiry station); Taiwan mobile numbers removed from phone; empty unused source fields dropped; `source` and `updated` added. No logos. Not endorsed by the Tourism Administration or MOTC.

[Back to the API list](../../README.md)
