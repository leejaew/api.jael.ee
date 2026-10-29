# US Foreign Trade Zones

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 260](https://img.shields.io/badge/records-260-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

Every US general purpose Foreign Trade Zone on the FTZ Board's OFIS list, read on 2026-10-29: 260 zones in 49 states and Puerto Rico. Each row has the zone number, location, state, Census region, the grantee (the public body or company that holds the zone grant) and its office address, approval date, status, service area, CBP port of entry and field office, acres, counts of active sites and subzones, and map coordinates. OFIS does not publish a Census region, port code, field office or coordinates, so we added them. Filter by the required `region`, then narrow by state, grantee, port, status, date or a number range.

Zone 248 (Eureka, California) is left out because the zone has lapsed. Subzones (single company plants under a zone, 1,186 records in OFIS) are not rows here. Each zone gives its count of active subzones and active sites instead.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_foreign_trade_zones` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 23 fields. Disallowed: 4 fields.

`region` must be in every call. Use `region=*` to list all 260 zones. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | US Census region of the zone: `South` (111), `West` (57), `Midwest` (49), `Northeast` (40) or `Territories` (3, Puerto Rico). Wildcards work, so `region=*` lists all 260 zones. |
| `zone_number` | Optional | No | No | FTZ number as 3 digit text with leading zeros, such as `001` or `049`. One zone per number. |
| `location` | Optional | Yes | No | Zone location label from the FTZ Board, such as `New York` or `*Houston*`. |
| `state` | Optional | Yes | No | State in full, such as `Texas` (35), `Florida` (19), `California` (16) or `Puerto Rico` (3). 51 values. |
| `state_code` | Optional | No | No | 2 letter state code, such as `TX` or `PR`. |
| `grantee` | Optional | Yes | No | Grantee that holds the zone grant, such as `*Port Authority*` or `Port of *`. |
| `grantee_address` | Optional | Yes | No | Street address of the grantee's office, such as `*Plaza*`. |
| `grantee_city` | Optional | Yes | No | City of the grantee's office, such as `Houston` or `San*`. |
| `grantee_state_code` | Optional | No | No | 2 letter state code of the grantee's office. Not the same as `state_code` for 4 zones. |
| `grantee_zip` | Optional | Yes | No | ZIP code of the grantee's office as text, such as `10006` or `770*`. |
| `approved_date` | Optional | Yes | No | Date the FTZ Board approved the zone, `YYYY-MM-DD` text from `1936-01-30` to `2025-01-10`. Use a wildcard for a year or month, such as `approved_date=2023*` or `approved_date=1980-*`. |
| `ofis_status` | Optional | No | No | Zone status in OFIS: `Active` (189), `Approved` (58, approved but not yet activated as a whole) or `Not Active` (13). |
| `service_area` | Optional | Yes | No | Counties or parishes the zone serves, as published, such as `*Harris*`. Blank for 64 zones that still use the older site based setup. |
| `port_of_entry` | Optional | Yes | No | CBP port of entry the zone is tied to, as published (state, port), such as `*Savannah*`. |
| `port_code` | Optional | No | No | 4 digit CBP port code as text, such as `1703`. Blank for 23 zones with no clean match. |
| `cbp_field_office` | Optional | Yes | No | CBP field office of that port, such as `Chicago` (42), `New Orleans` (28) or `Houston` (26). 21 values. Blank for 23 zones. |
| `activation_limit_acres` | Optional | No | Yes | Most acres the zone may activate at one time, from 0 to 3000. Example: `activation_limit_acres=1000..3000`. |
| `activated_acres` | Optional | No | Yes | Acres activated now, from 0 to 1998. Example: `activated_acres=500..2000`. |
| `active_sites` | Optional | No | Yes | Count of active zone sites, from 0 to 69. Example: `active_sites=10..100`. |
| `active_subzones` | Optional | No | Yes | Count of active subzones, from 0 to 20. Example: `active_subzones=1..20`. |
| `on_not_permitted_list` | Optional | No | No | `yes` for the 3 zones on the FTZ Board's list of zones not permitted to admit goods (no zone schedule on file), `no` for the other 257. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84) of the grantee office or zone location, from 18.01163 to 64.845077. Example: `latitude=29..31`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84) of the grantee office or zone location, from -157.863242 to -66.116666. Example: `longitude=-96..-94`. |
| `geocode_quality` | Optional | Yes | No | How the point was placed: `grantee_address_exact` (117), `grantee_address_approx` (56), `grantee_city_centroid` (50), `grantee_address_osm` (19), `zone_location_centroid` (16) or `zone_location_place` (2). Wildcards work, such as `zone_location*`. |
| `grantee_website` | Disallowed | No | No | Grantee website (https). Blank for 68 zones. |
| `zone_url` | Disallowed | No | No | Zone page on OFIS. |
| `source` | Disallowed | No | No | Source of the row, the OFIS zone list. |
| `source_as_of` | Disallowed | No | No | Date the data was read from OFIS, `2026-10-29` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `zone_number` | string | FTZ number, 3 digit text with leading zeros. |
| `location` | string | Zone location label from the FTZ Board. |
| `state` | string | State in full. |
| `state_code` | string | 2 letter state code. |
| `region` | string | US Census region, or `Territories` for Puerto Rico. Added by us. |
| `grantee` | string | Public body or company that holds the zone grant. |
| `grantee_address` | string | Street address of the grantee's office. |
| `grantee_city` | string | City of the grantee's office. |
| `grantee_state_code` | string | State code of the grantee's office. |
| `grantee_zip` | string | ZIP code of the grantee's office, as text. |
| `grantee_website` | string | Grantee website (https). Null for 68 zones. |
| `approved_date` | string | Date the FTZ Board approved the zone, `YYYY-MM-DD`. |
| `ofis_status` | string | `Active`, `Approved` or `Not Active`. |
| `service_area` | string | Counties or parishes the zone serves. Null for 64 zones. |
| `port_of_entry` | string | CBP port of entry as OFIS lists it (state, port). |
| `port_code` | string | 4 digit CBP port code. Null for 23 zones. Added by us. |
| `cbp_field_office` | string | CBP field office of the port. Null for 23 zones. Added by us. |
| `activation_limit_acres` | number | Most acres the zone may activate at one time. |
| `activated_acres` | number | Acres activated now. |
| `active_sites` | integer | Count of active zone sites. |
| `active_subzones` | integer | Count of active subzones. |
| `on_not_permitted_list` | string | `yes` or `no`. |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. Added by us. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. Added by us. |
| `geocode_quality` | string | How the point was placed. Added by us. |
| `zone_url` | string | Zone page on OFIS (https). |
| `source` | string | OFIS zone list (https). |
| `source_as_of` | string | Date the data was read from OFIS. |

Notes: OFIS publishes no coordinates. Each point marks the grantee office when that office is in the zone's state and near the zone, and the zone location otherwise. It does not mark every zone site, and many zones cover one or more whole counties. Points come from the US Census geocoder first, then OpenStreetMap Nominatim, then city centers. All 260 points fall inside their state. Port codes and field offices were matched by state and port name to our [`us_cbp_ports_of_entry`](../us_cbp_ports_of_entry/us_cbp_ports_of_entry.md) data. Two blanks in OFIS were filled from other public records: the approval date of zone 086 (Tacoma, Board Order 216) and the ZIP code of zone 215 (Sebring). Contact names, emails and phone numbers are left out.

## Examples

Two filters together. Zones in Ohio, in the Midwest region. This call returns 9 rows, from 008 Toledo to 270 Lawrence County:

```bash
curl -sG "https://api.jael.ee/JLEE/us_foreign_trade_zones" --data-urlencode "region=Midwest" --data-urlencode "state=Ohio"
```

Response excerpt (the first row):

```json
[
  {
    "__id": 6,
    "zone_number": "008",
    "location": "Toledo",
    "state": "Ohio",
    "state_code": "OH",
    "region": "Midwest",
    "grantee": "Toledo-Lucas County Port Authority",
    "grantee_address": "One Maritime Plaza, 7th Floor",
    "grantee_city": "Toledo",
    "grantee_state_code": "OH",
    "grantee_zip": "43604-1866",
    "grantee_website": "https://www.toledoport.org",
    "approved_date": "1960-10-11",
    "ofis_status": "Active",
    "service_area": "Defiance, Erie, Fulton, Henry, Lucas, Ottawa, Paulding, Sandusky, Williams, and Wood Counties",
    "port_of_entry": "OH, Toledo-Sandusky",
    "port_code": "4105",
    "cbp_field_office": "Chicago",
    "activation_limit_acres": 2000.0,
    "activated_acres": 241.18,
    "active_sites": 2,
    "active_subzones": 1,
    "on_not_permitted_list": "no",
    "latitude": 41.652914,
    "longitude": -83.537817,
    "geocode_quality": "grantee_city_centroid",
    "zone_url": "https://ofis.trade.gov/Zones/Details/226",
    "source": "https://ofis.trade.gov/Zones",
    "source_as_of": "2026-10-29"
  }
]
```

A wildcard on the grantee. Zones in Washington state with "Port" in the grantee name. This call returns 9 rows, and the first three are 005 Seattle (Port of Seattle), 085 Everett (Port of Everett) and 086 Tacoma (Port of Tacoma). Without the state filter, `grantee=*Port*` returns 82 zones across the country:

```bash
curl -sG "https://api.jael.ee/JLEE/us_foreign_trade_zones" --data-urlencode "region=*" --data-urlencode "state=Washington" --data-urlencode "grantee=*Port*" | jq -r '.[] | [.zone_number, .location, .grantee] | @tsv'
```

A range on a number. Zones with 500 to 2,000 acres activated now. This call returns 18 rows, from 026 Atlanta (1469.97 acres) to 291 Cameron Parish (1049 acres). The largest is 062 Brownsville with 1998 acres:

```bash
curl -sG "https://api.jael.ee/JLEE/us_foreign_trade_zones" --data-urlencode "region=*" --data-urlencode "activated_acres=500..2000" | jq -r '.[] | [.zone_number, .location, .activated_acres] | @tsv'
```

## Data source

The list comes from the U.S. Foreign-Trade Zones Board's [Online FTZ Information System (OFIS)](https://ofis.trade.gov/Zones), run by the International Trade Administration, read on 2026-10-29. This is a US government work and is in the public domain. The same data is listed on data.commerce.gov under the US public domain label. Port codes and CBP field offices come from the U.S. Customs and Border Protection [Locate a Port of Entry](https://www.cbp.gov/about/contact/ports) pages, which are also public domain. Coordinates come from the US Census geocoder (public domain) and, for 87 zones, from OpenStreetMap. Region, port code, field office and geocode fields are added by us, and are not published by the FTZ Board.

Credit line: Source: U.S. Foreign-Trade Zones Board, International Trade Administration, U.S. Department of Commerce (OFIS, https://ofis.trade.gov/Zones). The Foreign-Trade Zones Board, the International Trade Administration and the U.S. Department of Commerce do not endorse or sponsor this API.

Port codes and field offices: Source: U.S. Customs and Border Protection, Locate a Port of Entry (https://www.cbp.gov/about/contact/ports).

Map points placed with OpenStreetMap (87 zones with `geocode_quality` of `grantee_address_osm`, `grantee_city_centroid`, `zone_location_centroid` or `zone_location_place`): © OpenStreetMap contributors, ODbL (https://www.openstreetmap.org/copyright).

[Back to the API list](../../README.md)
