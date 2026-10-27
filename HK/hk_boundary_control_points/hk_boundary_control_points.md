# Hong Kong Boundary Control Points

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 16](https://img.shields.io/badge/records-16-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange)

Every Hong Kong immigration control point: land, rail, sea and air crossings, plus Harbour Control and the River Trade Terminal. Each row has the English and Chinese name, mode, the place it connects to, district, address, status, opening hours, cargo and passenger flags, phone and map coordinates. Two crossings, Sha Tau Kok and Lok Ma Chau, are marked Suspended. Opening hours and status change often, so check the Immigration Department before you rely on them.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_boundary_control_points` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 13 fields. Disallowed: 5 fields.

No parameter is required because the list has only 16 rows. Call the API with no parameters to get all of them. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Control point name in English, such as `*Lo Wu*`, `*Bridge*` or `*Terminal*`. |
| `name_zh` | Optional | Yes | No | Control point name in Traditional Chinese, such as `*羅湖*`. |
| `mode` | Optional | No | No | How people cross: `Land` (7), `Sea` (5), `Rail` (3) or `Air` (1). Harbour Control and the River Trade Terminal count as Sea. |
| `crossing_with` | Optional | Yes | No | Where the crossing leads: `Shenzhen`, `Mainland China`, `Zhuhai and Macao`, `Macao` or `International`. |
| `district` | Optional | Yes | No | Hong Kong district, such as `North`, `Islands` or `Yuen Long`. The Huanggang and Shenzhen Bay Hong Kong Port Areas sit in Shenzhen, so use `*Shenzhen*` for them. |
| `address` | Optional | Yes | No | English address, such as `*Lantau*` or `*Sha Tau Kok*`. |
| `address_zh` | Optional | Yes | No | Traditional Chinese address. |
| `status` | Optional | No | No | `Open` (14) or `Suspended` (2), as of the date checked. |
| `open_24h` | Optional | No | No | `Yes` if all clearance services at the point run 24 hours a day (5 points), otherwise `No`. |
| `cargo_clearance` | Optional | No | No | `Yes` if the point clears goods vehicles, freight vessels or air cargo (6 points), otherwise `No`. |
| `passenger_clearance` | Optional | No | No | `Yes` if the point clears travelers (12 points), otherwise `No`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees, from 22.28856 to 22.5534. Use a range to search a box, for example `latitude=22.5..22.6`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees, from 113.93442 to 114.22329. Use a range to search a box, for example `longitude=114..114.3`. |
| `opening_hours` | Disallowed | No | No | Clearance hours as the Immigration Department states them, in plain text. Use `open_24h` to find 24 hour points. |
| `phone` | Disallowed | No | No | Immigration Department enquiry phone with the +852 prefix, as text. Blank for 1 row. |
| `website` | Disallowed | No | No | Immigration Department page for the control point. |
| `source` | Disallowed | No | No | Pages the row was built from, how the cargo flag was decided and the credit line. |
| `updated` | Disallowed | No | No | Date the row was last checked, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Control point name in English. |
| `name_zh` | string | Control point name in Traditional Chinese. |
| `mode` | string | `Land`, `Sea`, `Rail` or `Air`. |
| `crossing_with` | string | Where the crossing leads. |
| `district` | string | Hong Kong district, or the Hong Kong Port Area and Mainland city for two ports sited in Shenzhen. |
| `address` | string | English address. |
| `address_zh` | string | Traditional Chinese address. |
| `status` | string | `Open` or `Suspended`. |
| `open_24h` | string | `Yes` or `No`. |
| `opening_hours` | string | Clearance hours in plain text. Some points list different hours for passengers and goods vehicles. |
| `cargo_clearance` | string | `Yes` or `No`. |
| `passenger_clearance` | string | `Yes` or `No`. |
| `phone` | string | Enquiry phone with the +852 prefix. Two numbers are separated by a slash. |
| `website` | string | Immigration Department page. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). |
| `longitude` | number | Longitude in decimal degrees (WGS 84). |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the row was last checked. |

Notes on the flags: `cargo_clearance` is `Yes` for the Airport (air cargo) and the River Trade Terminal (vessels only, no passengers). Harbour Control is `No` because it clears vessels and crew, not cargo. `open_24h` is `No` for Shenzhen Bay because its passenger building closes at midnight, even though goods vehicles clear 24 hours. The Huanggang coordinates come from a map lookup and are accurate to a few hundred meters. Every other point uses the Immigration Department coordinates.

## Examples

Land crossings that are open and clear goods vehicles, with their hours. This call returns 4 rows:

```bash
curl -s "https://api.jael.ee/JLEE/hk_boundary_control_points?mode=Land&cargo_clearance=Yes&status=Open" | jq -r '.[] | "\(.name) | \(.opening_hours)"'
```

```text
Heung Yuen Wai Control Point | 07:00 to 22:00
Hong Kong-Zhuhai-Macao Bridge Control Point | 24 hours
Man Kam To Control Point | 07:00 to 22:00
Shenzhen Bay Control Point | Passenger Clearance Building and private cars' passenger clearance services: 06:30 to 24:00; Goods vehicle clearance services: 24 hours
```

The bridge to Zhuhai and Macao, with a wildcard on `name`:

```bash
curl -s "https://api.jael.ee/JLEE/hk_boundary_control_points?name=*Bridge*"
```

Response (1 row):

```json
[
  {
    "__id": 6,
    "name": "Hong Kong-Zhuhai-Macao Bridge Control Point",
    "name_zh": "港珠澳大橋出入境管制站",
    "mode": "Land",
    "crossing_with": "Zhuhai and Macao",
    "district": "Islands",
    "address": "Hong Kong-Zhuhai-Macao Bridge Control Point, Hong Kong-Zhuhai-Macao Bridge Hong Kong Port, Lantau",
    "address_zh": "大嶼山港珠澳大橋香港口岸港珠澳大橋管制站",
    "status": "Open",
    "open_24h": "Yes",
    "opening_hours": "24 hours",
    "cargo_clearance": "Yes",
    "passenger_clearance": "Yes",
    "phone": "+852 3195 2172",
    "website": "https://www.immd.gov.hk/eng/contactus/control_points.html",
    "latitude": 22.3181,
    "longitude": 113.95108,
    "source": "ImmD open data CSV (office_address_and_working_hours.csv) for name and coordinates; ImmD control point list and contact pages checked 2026-10-28 for hours, status, phone; cargo flag: Customs lists it 24 hours and publishes goods vehicle throughput; TD has an arrangement page for cross boundary goods vehicles. Source: Immigration Department, HKSAR Government, via DATA.GOV.HK",
    "updated": "2026-10-28"
  }
]
```

Ports whose district is in Shenzhen, with a wildcard on `district`:

```bash
curl -s "https://api.jael.ee/JLEE/hk_boundary_control_points?district=*Shenzhen*" | jq -r '.[].name'
```

```text
Huanggang Control Point
Shenzhen Bay Control Point
```

Crossings in the northern border box, with ranges on latitude and longitude. This call returns 7 rows:

```bash
curl -s "https://api.jael.ee/JLEE/hk_boundary_control_points?latitude=22.5..22.6&longitude=114..114.3" | jq -r '.[].name'
```

```text
Heung Yuen Wai Control Point
Huanggang Control Point
Lo Wu Control Point
Lok Ma Chau Control Point
Lok Ma Chau Spur Line Control Point
Man Kam To Control Point
Sha Tau Kok Control Point
```

## Data source

Names, English addresses, hours and coordinates for 15 control points come from the Hong Kong Immigration Department open data file on address and working hours. Status, phone numbers, Chinese addresses and the newest hours come from the Immigration Department control point pages, checked on 2026-10-28, and these pages win where the open data file is out of date. Huanggang reopened on 2026-10-12 and is not in the open data file, so its details come from the department pages. The cargo flags come from the Customs and Excise Department and Transport Department pages on goods vehicle crossings. Coordinate spot checks used OpenStreetMap, © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), under the Open Database Licence (ODbL).

Credit line: Source: Immigration Department, HKSAR Government, via DATA.GOV.HK. Use of the data follows the [DATA.GOV.HK terms and conditions](https://data.gov.hk/en/terms-and-conditions), and the data is provided as is.

[Back to the API list](../../README.md)
