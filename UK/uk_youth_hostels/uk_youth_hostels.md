# UK Youth Hostels

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 141](https://img.shields.io/badge/records-141-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange)

Every YHA hostel in England and Wales and every Hostelling Scotland hostel, 141 hostels as listed on the two operators' websites on 28 October 2026, including partner and affiliate hostels run by third parties. Each row has the hostel name, operator, partner flag, nation, region, town, council area, address, postcode, map coordinates and a link to the hostel's own page. Seasonal or temporarily closed hostels are not marked, so check with the hostel before you travel.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_youth_hostels` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`nation`). Optional: 10 fields. Disallowed: 2 fields.

`nation` must be in every call. Use `nation=*` to list all 141 hostels. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `nation` | Required | Yes | No | `England` (79), `Scotland` (50) or `Wales` (12). Wildcards work, so `nation=*` lists all 141 hostels. |
| `hostel_name` | Optional | Yes | No | Hostel name, such as `*Ambleside*` or `YHA Ed*`. Names are unique across the 141 rows. |
| `operator` | Optional | No | No | Who runs the hostel network: `YHA` (91) or `Hostelling Scotland` (50). |
| `partner` | Optional | No | No | `yes` (44) is an independently run partner hostel (a YHA Partner or a Hostelling Scotland Affiliate). `no` (97) is a hostel run by the network itself. |
| `region` | Optional | Yes | No | Region group, such as `Scottish Highlands` (27), `North West of England` (19), `Scottish Islands` (15), `South West of England` (14) or `Wales` (12). Wildcards work. |
| `town` | Optional | Yes | No | Post town or village, such as `Keswick` or `*Glen*`. 124 distinct values. |
| `local_authority` | Optional | Yes | No | Council area, such as `Highland` or `Cumberland`. 65 distinct values. |
| `address` | Optional | Yes | No | Street and locality text as listed, without the postcode, such as `*Lake District*` or `Waterhead*`. |
| `postcode` | Optional | Yes | No | UK postcode as text, such as `LA22 0EU` or `LA22*`. A few nearby hostels share a postcode. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 50.117926 to 60.152934. No blanks. Use a range to search a box, for example `latitude=54.0..55.0`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -7.386761 to 1.212643. No blanks. Use a range to search a box, for example `longitude=-3.5..-2.5`. |
| `hostel_url` | Disallowed | No | No | Link to the hostel's own page on the operator's website. |
| `source_as_of` | Disallowed | No | No | Date the hostel list was read, `2026-10-28` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `hostel_name` | string | Hostel name as the operator lists it. |
| `operator` | string | `YHA` or `Hostelling Scotland`. |
| `partner` | string | `yes` for a partner or affiliate hostel, otherwise `no`. |
| `nation` | string | `England`, `Scotland` or `Wales`. |
| `region` | string | Region group. |
| `town` | string | Post town or village. |
| `local_authority` | string | Council area. |
| `address` | string | Street and locality, without the postcode. |
| `postcode` | string | UK postcode. |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `hostel_url` | string | Link to the hostel's own page. |
| `source_as_of` | string | Date the hostel list was read. |

Notes: the map point is the one shown on each hostel's own page, not the postcode centre, which can be several kilometers off for remote hostels. Perth Youth Hostel is seasonal, open from mid June to the end of August. Two YHA partner hostels that say they are temporarily closed (Land's End and Mankinholes) are not included. The live YHA list shows 93 hostels.

## Examples

Hostels in Wales that are partner hostels. This call returns 3 rows, `YHA Borth`, `YHA Kings` and `YHA Port Eynon`:

```bash
curl -s "https://api.jael.ee/JLEE/uk_youth_hostels?nation=Wales&partner=yes" | jq -r '.[].hostel_name'
```

```text
YHA Borth
YHA Kings
YHA Port Eynon
```

A hostel by name, with a wildcard. This call returns 1 row:

```bash
curl -s "https://api.jael.ee/JLEE/uk_youth_hostels?nation=England&hostel_name=*Ambleside*"
```

Response (1 row):

```json
[
  {
    "__id": 2,
    "hostel_name": "YHA Ambleside",
    "operator": "YHA",
    "partner": "no",
    "nation": "England",
    "region": "North West of England",
    "town": "Ambleside",
    "local_authority": "Westmorland and Furness",
    "address": "Waterhead, Ambleside, Cumbria",
    "postcode": "LA22 0EU",
    "latitude": 54.41908,
    "longitude": -2.961772,
    "hostel_url": "https://www.yha.org.uk/hostel/yha-ambleside",
    "source_as_of": "2026-10-28"
  }
]
```

Hostels in the Lake District, using ranges on latitude and longitude. This call returns 12 rows, and the first three are YHA Ambleside, YHA Black Sail and YHA Borrowdale:

```bash
curl -s "https://api.jael.ee/JLEE/uk_youth_hostels?nation=England&latitude=54.0..54.6&longitude=-3.3..-2.8" | jq -r '.[0:3][].hostel_name'
```

A wildcard on the postcode. This call returns 4 rows, and the first three are YHA Ambleside, YHA Grasmere Butharlyp Howe and YHA Hawkshead:

```bash
curl -s "https://api.jael.ee/JLEE/uk_youth_hostels?nation=*&postcode=LA22*" | jq -r '.[0:3][].hostel_name'
```

Hostelling Scotland hostels only. This call returns 50 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_youth_hostels?nation=*&operator=Hostelling%20Scotland" | jq length
```

## Data source

The rows are plain facts (name, address, postcode, operator, partner flag and map point) from the public hostel lists of YHA (England and Wales) and Hostelling Scotland, read on 2026-10-28. Both sites reserve all rights, and no text, descriptions, photos, prices or contact details are copied. Council areas were matched with postcodes.io (open data from the Office for National Statistics and Ordnance Survey under the Open Government Licence), which was also used to check the coordinates.

Credit line: Hostel names, addresses and locations are public facts from yha.org.uk and hostellingscotland.org.uk, checked 2026-10-28. YHA and Hostelling Scotland do not endorse this service. Check details with the hostel before you travel.

[Back to the API list](../../README.md)
