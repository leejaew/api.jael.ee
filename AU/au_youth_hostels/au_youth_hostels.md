# Australia Youth Hostels

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 18](https://img.shields.io/badge/records-18-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![use free, non commercial](https://img.shields.io/badge/use-free%2C%20non%20commercial-yellow)

All 18 open YHA Australia youth hostels as of October 2026, in New South Wales, Queensland, Victoria, South Australia, Tasmania and Western Australia. Each row has the hostel name, state, travel region, town, address, postcode, map coordinates and a link to the hostel page. YHA Australia cut its network from 37 to 19 properties in 2025, and MODE by YHA in Sydney, which opens in 2027, is left out until it opens. This is the Australian sibling of [`uk_youth_hostels`](../../UK/uk_youth_hostels/uk_youth_hostels.md).

> **Terms of use.** This API is free to use for non commercial purposes. It holds plain facts (names, addresses, locations and links) checked on [yha.com.au](https://www.yha.com.au/hostels/), and it carries no open licence. YHA Australia's [terms](https://www.yha.com.au/disclaimer/) do not allow commercial reuse of its website content without YHA's written permission, so do not resell this data or build a paid product on it. YHA Australia does not endorse this API. Check details with the hostel before you travel.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_youth_hostels` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 12 fields. Disallowed: 2 fields.

No parameter is required, so a call with no parameters returns all 18 hostels. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `hostel_name` | Optional | Yes | No | Hostel name, such as `YHA Sydney Harbour` or `*Byron*`. |
| `operator` | Optional | No | No | `YHA Australia` on every row. |
| `operating_model` | Optional | No | No | The label YHA's site data gives each property: `operated` (12), `managed` (3) or `leased` (3). YHA does not explain these labels. |
| `state` | Optional | Yes | No | State in full: `New South Wales` (8), `Queensland` (3), `Victoria` (3), `South Australia` (2), `Tasmania` (1) or `Western Australia` (1). |
| `state_code` | Optional | No | No | State code: `NSW`, `QLD`, `VIC`, `SA`, `TAS` or `WA`. |
| `region` | Optional | Yes | No | Travel region, such as `Sydney`, `Great Ocean Road` or `*Queensland*`. |
| `town` | Optional | Yes | No | Town or suburb, such as `Byron Bay` or `Thredbo`. |
| `address` | Optional | Yes | No | Street address, such as `*Street*`. |
| `postcode` | Optional | Yes | No | 4 digit postcode as text, such as `2000` or `2*`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84) of YHA's own map pin, from -42.88185 to -16.924943. Example: `latitude=-34..-33`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84) of YHA's own map pin, from 115.752568 to 153.614861. Example: `longitude=150..152`. |
| `geocode_quality` | Optional | No | No | `address_match` (17, pin checked against the street address) or `near_landmark` (1, Pittwater, reached by ferry). |
| `hostel_url` | Disallowed | No | No | Hostel page on yha.com.au. |
| `source_as_of` | Disallowed | No | No | Date the facts were checked on yha.com.au, `2026-10-29`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `hostel_name` | string | Hostel name as YHA lists it. |
| `operator` | string | `YHA Australia`. |
| `operating_model` | string | `operated`, `managed` or `leased`, as YHA's site data labels the property. |
| `state` | string | State in full. |
| `state_code` | string | State code. |
| `region` | string | Travel region. |
| `town` | string | Town or suburb. |
| `address` | string | Street address, without state and postcode. |
| `postcode` | string | 4 digit postcode. |
| `latitude` | number | Latitude of YHA's map pin (WGS 84), 6 decimals. |
| `longitude` | number | Longitude of YHA's map pin (WGS 84), 6 decimals. |
| `geocode_quality` | string | `address_match` or `near_landmark`. |
| `hostel_url` | string | Hostel page on yha.com.au. |
| `source_as_of` | string | Date the facts were checked. |

Notes: coordinates are YHA's own map pins. Each pin was checked against the street address in OpenStreetMap, and all 17 address matches are within 0.2 km. YHA Pittwater Eco has no street address (you reach it by ferry or water taxi), so its pin is checked against Halls Wharf instead.

## Examples

Hostels on the Great Ocean Road. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/au_youth_hostels" --data-urlencode "region=Great Ocean Road"
```

Response:

```json
[
  {
    "__id": 15,
    "hostel_name": "YHA Apollo Bay Eco",
    "operator": "YHA Australia",
    "operating_model": "managed",
    "state": "Victoria",
    "state_code": "VIC",
    "region": "Great Ocean Road",
    "town": "Apollo Bay",
    "address": "5 Pascoe Street, Apollo Bay",
    "postcode": "3233",
    "latitude": -38.758559,
    "longitude": 143.668786,
    "geocode_quality": "address_match",
    "hostel_url": "https://www.yha.com.au/hostels/vic/great-ocean-road/apollo-bay/",
    "source_as_of": "2026-10-29"
  }
]
```

A wildcard on the name. This call returns 2 rows, YHA Byron Bay and YHA Cape Byron (Byron Bay):

```bash
curl -sG "https://api.jael.ee/JLEE/au_youth_hostels" --data-urlencode "hostel_name=*Byron*" | jq -r '.[].hostel_name'
```

A box on latitude and longitude around Sydney and the Blue Mountains. This call returns 4 rows: YHA Blue Mountains (Katoomba), YHA Pittwater Eco (Sydney), YHA Sydney Central and YHA Sydney Harbour.

```bash
curl -sG "https://api.jael.ee/JLEE/au_youth_hostels" --data-urlencode "latitude=-34..-33" --data-urlencode "longitude=150..152" | jq -r '.[].hostel_name'
```

Hostels in Queensland. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/au_youth_hostels" --data-urlencode "state_code=QLD" | jq -r '.[].hostel_name'
```

## Data source

Hostel names, addresses, locations and links are public facts from the [YHA Australia hostels page](https://www.yha.com.au/hostels/) and each hostel's own page, checked on 29 October 2026. Hostels that left the network are listed on YHA's [network departure page](https://www.yha.com.au/important-information/network-departure/). The pin checks used OpenStreetMap data. YHA, YHA Australia and the hostel names belong to YHA Ltd and are used here only to name the hostels.

Credit lines:

* Hostel facts from yha.com.au (YHA Australia), checked 29 October 2026. YHA Australia does not endorse this API.
* Location checks use data © OpenStreetMap contributors, available under the Open Database License (ODbL).

[Back to the API list](../../README.md)
