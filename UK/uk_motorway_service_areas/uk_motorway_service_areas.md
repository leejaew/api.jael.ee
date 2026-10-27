# UK Motorway Service Areas

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 107](https://img.shields.io/badge/records-107-lightgrey)

Motorway service areas, rest areas and truck stops in the United Kingdom. Each row is one site with its motorway, junction, direction, operator, type, address, postcode, county, region, flags for HGV parking, fuel, EV charging and a hotel, and map coordinates. Northern Ireland has no motorway service areas, so it has no rows. Small independent truck stops that are not on the national lists may be missing.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_motorway_service_areas` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 15 fields. Disallowed: 3 fields.

The `region` parameter is required on every call. Use `region=*` to list all sites. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | UK nation: `England` (91 sites), `Scotland` (10) or `Wales` (6). `region=*` lists all 107 sites. |
| `name` | Optional | Yes | No | Site name, for example `Tebay`, `*Gateway*` or `Leigh Delamere`. |
| `motorway` | Optional | Yes | No | Main road the site is on, such as `M6`, `M1`, `A1(M)` or `M6 Toll`. Use `M6*` to include M6 Toll. 30 roads. |
| `junction` | Optional | Yes | No | Junction or junction span as text, such as `J16` or `J11A to J12`. Use `*J16*` to match. |
| `direction` | Optional | No | No | `Both`, `Northbound` or `Southbound`. `Both` means one site serves both directions. |
| `operator` | Optional | Yes | No | Operator, such as `Moto`, `Welcome Break`, `Roadchef`, `Extra` or `Westmorland`. |
| `type` | Optional | No | No | `Motorway services` (97), `Truck stop` (7) or `Rest area` (3). |
| `address` | Optional | Yes | No | Short address text, for example `*Leeds*`. |
| `postcode` | Optional | Yes | No | UK postcode as text, such as `NW7 3HU` or `CW*`. |
| `county` | Optional | Yes | No | County or council area, such as `Cumbria`, `Somerset` or `Greater London`. 44 values. |
| `hgv_parking` | Optional | No | No | `Yes` or `No` for lorry (HGV) parking on site. `Yes` for 106 sites. |
| `fuel` | Optional | No | No | `Yes` or `No` for a fuel station on site. Blank for 1 site. |
| `ev_charging` | Optional | No | No | `Yes` or `No` for EV chargers listed on the sources. `No` means none are listed, not that none exist. Blank for 3 sites. |
| `hotel` | Optional | No | No | `Yes` or `No` for a hotel on or next to the site. Blank for 1 site. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees, from 50.714119 to 56.208744. Use a range to search a box, for example `latitude=52..53`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees, from -4.065935 to 1.044112. Use a range to search a box, for example `longitude=-3..-1`. |
| `website` | Disallowed | No | No | Operator page for the site. |
| `source` | Disallowed | No | No | Pages the row was built from. |
| `updated` | Disallowed | No | No | Date the row was last checked, such as `2026-10-27`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Site name. |
| `motorway` | string | Main road the site is on. A site on two roads shows one. |
| `junction` | string | Junction or junction span, such as `J2 to J4`. |
| `direction` | string | `Both`, `Northbound` or `Southbound`. |
| `operator` | string | Company that runs the site. |
| `type` | string | `Motorway services`, `Truck stop` or `Rest area`. |
| `address` | string | Short address text. |
| `postcode` | string | UK postcode. |
| `county` | string | County or council area. |
| `region` | string | `England`, `Scotland` or `Wales`. |
| `hgv_parking` | string | `Yes` or `No`. |
| `fuel` | string | `Yes` or `No`. Empty when not known. |
| `ev_charging` | string | `Yes` or `No`. Empty when not known. |
| `hotel` | string | `Yes` or `No`. Empty when not known. |
| `latitude` | number | Latitude in decimal degrees, taken from the site postcode. |
| `longitude` | number | Longitude in decimal degrees, taken from the site postcode. |
| `website` | string | Operator page for the site. |
| `source` | string | Pages the row was built from. |
| `updated` | string | Date the row was last checked. |

## Examples

Services on the M74 in Scotland.

```bash
curl -s "https://api.jael.ee/JLEE/uk_motorway_service_areas?region=Scotland&motorway=M74"
```

Response (5 rows, first row shown, `source` shortened):

```json
[
  {
    "__id": 87,
    "name": "Bothwell",
    "motorway": "M74",
    "junction": "J4 to J5",
    "direction": "Southbound",
    "operator": "Roadchef",
    "type": "Motorway services",
    "address": "Fallside Road, Bothwell",
    "postcode": "G71 8BG",
    "county": "South Lanarkshire",
    "region": "Scotland",
    "hgv_parking": "Yes",
    "fuel": "Yes",
    "ev_charging": "Yes",
    "hotel": "No",
    "latitude": 55.815528,
    "longitude": -4.063513,
    "website": "https://www.roadchef.com/motorway-services/bothwell",
    "source": "https://motorwayservices.uk/Bothwell; https://api.postcodes.io/postcodes/G71%208BG; ...",
    "updated": "2026-10-27"
  }
]
```

The 7 truck stops across the UK, using the wildcard on `region`. The names:

```bash
curl -s "https://api.jael.ee/JLEE/uk_motorway_service_areas?region=*&type=Truck%20stop" | jq -r '.[].name'
```

```text
Junction 29 Truckstop
Lymm Truckstop
Carnforth Truckstop
Junction 38 Truckstop
Ashford International Truckstop
Route 74 Truckstop
Barton Park Truckstop
```

Sites with a hotel in the English Midlands, using ranges on latitude and longitude. This call returns 14 sites, and the first five names are:

```bash
curl -s "https://api.jael.ee/JLEE/uk_motorway_service_areas?region=England&latitude=52..53&longitude=-3..-1&hotel=Yes" | jq -r '.[].name'
```

```text
Watford Gap
Leicester Forest East
Donington Park
Trowell
Frankley
```

## Data source

Site facts (operator, junction, direction and amenities) come from the operator pages of Moto, Welcome Break, Roadchef, Extra and Westmorland, and from the UK motorway services directory at [motorwayservices.uk](https://motorwayservices.uk). The site postcode is resolved to coordinates with [postcodes.io](https://postcodes.io). The data.gov.uk list [Motorway Service Areas and Truck Stops](https://www.data.gov.uk/dataset/21d65bec-8880-406c-bac6-2ee630dc210d/motorway-service-areas-and-truck-stops) and Wikipedia were used to cross check coverage. Each row names its pages in `source`. Amenities change, so check with the operator before you travel.

[Back to the API list](../../README.md)
