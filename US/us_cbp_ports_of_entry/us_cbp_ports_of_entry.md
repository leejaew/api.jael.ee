# US CBP Ports of Entry

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 346](https://img.shields.io/badge/records-346-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

Every US Customs and Border Protection port of entry listed on cbp.gov, 346 airports, seaports and land border crossings in the 50 states, Puerto Rico, the US Virgin Islands, Guam and the Northern Mariana Islands. Each row has the port name, 4 digit port code, address, state, field operations office, port type and map coordinates. CBP does not publish a port type or coordinates, so we added both.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_cbp_ports_of_entry` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`field_office`). Optional: 12 fields. Disallowed: 5 fields.

`field_office` must be in every call. Use `field_office=*` to list all 346 ports. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `field_office` | Required | Yes | No | CBP field operations office, 20 values: `Seattle` (58), `Boston` (43), `Chicago` (43), `Portland` (27), `Houston` (20) and more. Wildcards work, so `field_office=*` lists all 346 ports. |
| `port_name` | Optional | Yes | No | Port name as CBP lists it, such as `*Miami*` or `Port Everglades*`. |
| `port_code` | Optional | Yes | No | 4 digit CBP port code as text, such as `5203` or `52*`. Two codes (3307 and 3708) are each shared by two CBP listings. |
| `state` | Optional | Yes | No | 2 letter state or territory code, such as `TX` (32), `FL` (24) or `ND` (22). 54 values, including `PR`, `VI`, `GU` and `MP`. |
| `state_name` | Optional | Yes | No | Full state or territory name, such as `Texas` or `*Dakota`. |
| `city` | Optional | Yes | No | City or place of the port, such as `El Paso` or `*Falls`. 310 distinct values. |
| `address` | Optional | Yes | No | Street address of the port, such as `*Airport*` or `*Bridge*`. |
| `zip` | Optional | Yes | No | ZIP code as text with leading zeros kept, such as `33316` or `333*`. |
| `port_type` | Optional | No | No | Type of port: `airport` (135), `land border` (99), `seaport` (94) or `other` (18). Derived by us from the name and address, not published by CBP. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 13.4443 to 64.819012. No blanks. Example: `latitude=25..30`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -166.543897 to 145.730354. No blanks. Example: `longitude=-100..-90`. |
| `geocode_quality` | Optional | No | No | How well the coordinates fit the port: `exact` (238) or `approximate` (108). Do not use approximate rows for exact map pins. |
| `geocode_method` | Optional | No | No | How the coordinates were found: `census_exact`, `census_non_exact`, `nominatim_address`, `nominatim_venue_name` or `zip_centroid`. |
| `phone` | Disallowed | No | No | Port phone number with +1 country code, as text. Blank for 60 ports. |
| `source` | Disallowed | No | No | CBP page the row was taken from. |
| `source_as_of` | Disallowed | No | No | Date the CBP pages were read, `2026-10-29` on every row. |
| `last_modified` | Disallowed | No | No | Date CBP last modified the port page, as text. |
| `notes` | Disallowed | No | No | Our flags for the row, such as an inferred field office or a shared code. Blank for most ports. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `port_name` | string | Port name as CBP lists it. |
| `port_code` | string | 4 digit CBP port code, as text. Not unique for 3307 and 3708. |
| `state` | string | 2 letter state or territory code. |
| `state_name` | string | Full state or territory name. |
| `city` | string | City or place of the port. |
| `address` | string | Street address, without the ZIP. |
| `zip` | string | ZIP code, as text. |
| `field_office` | string | CBP field operations office. Six rows were inferred from the same state and are flagged in `notes`. |
| `port_type` | string | `airport`, `land border`, `seaport` or `other`. Derived by us. |
| `phone` | string | Phone with +1 country code. Null for 60 ports. |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `geocode_quality` | string | `exact` or `approximate`. |
| `geocode_method` | string | How the coordinates were found. |
| `source` | string | CBP page for the port (https). |
| `source_as_of` | string | Date the CBP pages were read. |
| `last_modified` | string | Date CBP last modified the port page. |
| `notes` | string | Our flags for the row. Null for most ports. |

Notes: port codes come from the CBP port pages and match the ACE Schedule D list (February 2026) for 332 of the 346 rows. The other 14 keep the code from the CBP page and are flagged in `notes`. Two codes are shared by two listings: 3307 (Denver and Northern Colorado) and 3708 (Kenosha and Racine). Coordinates come from the US Census geocoder first, then OpenStreetMap Nominatim, then ZIP code centers for 39 rows, which are marked `approximate`. Two ports with no CBP staff on site and one port with no code were left out.

## Examples

Ports under the Miami field office. This call returns 5 rows: Key West, Miami International Airport, Miami Seaport, Port Everglades/Fort Lauderdale and West Palm Beach.

```bash
curl -sG "https://api.jael.ee/JLEE/us_cbp_ports_of_entry" --data-urlencode "field_office=Miami"
```

Response excerpt (the fourth row):

```json
[
  {
    "__id": 78,
    "port_name": "Port Everglades/Fort Lauderdale",
    "port_code": "5203",
    "state": "FL",
    "state_name": "Florida",
    "city": "Fort Lauderdale",
    "address": "1800 Eller Drive, Suite 104",
    "zip": "33316",
    "field_office": "Miami",
    "port_type": "seaport",
    "phone": "+1 954-761-2000",
    "latitude": 26.081577,
    "longitude": -80.123758,
    "geocode_quality": "exact",
    "geocode_method": "nominatim_address",
    "source": "https://www.cbp.gov/about/contact/ports/port-evergladesfort-lauderdale-florida-5203",
    "source_as_of": "2026-10-29",
    "last_modified": "2025-11-13",
    "notes": null
  }
]
```

Two filters together. Land border ports in Texas. This call returns 12 rows, and the first three are Brownsville, Del Rio and Eagle Pass:

```bash
curl -sG "https://api.jael.ee/JLEE/us_cbp_ports_of_entry" --data-urlencode "field_office=*" --data-urlencode "state=TX" --data-urlencode "port_type=land border" | jq -r '.[].port_name'
```

A wildcard on the port code. Ports with a code starting 52, in Florida. This call returns 5 rows: Key West, Miami International Airport, Miami Seaport, Port Everglades/Fort Lauderdale and West Palm Beach:

```bash
curl -sG "https://api.jael.ee/JLEE/us_cbp_ports_of_entry" --data-urlencode "field_office=*" --data-urlencode "port_code=52*" | jq -r '.[].port_name'
```

A wildcard on the name. This call returns 1 row, Port Everglades/Fort Lauderdale with code 5203:

```bash
curl -sG "https://api.jael.ee/JLEE/us_cbp_ports_of_entry" --data-urlencode "field_office=*" --data-urlencode "port_name=*Everglades*" | jq -r '.[] | [.port_name, .port_code] | @tsv'
```

A box on latitude and longitude. Ports near the Rio Grande Valley between latitude 25 and 26. This call returns 1 row, Brownsville:

```bash
curl -sG "https://api.jael.ee/JLEE/us_cbp_ports_of_entry" --data-urlencode "field_office=*" --data-urlencode "latitude=25..26" --data-urlencode "longitude=-100..-97" | jq -r '.[].port_name'
```

## Data source

The list comes from the U.S. Customs and Border Protection [Locate a Port of Entry](https://www.cbp.gov/about/contact/ports) pages (one page per port) and the ACE Schedule D port code list (February 2026). The [CBP copyright notice](https://www.cbp.gov/site-policy-notices/copyright-notice) says content is in the public domain unless a copyright is indicated, and may be used without permission with CBP cited as the source. No copyright is marked on the pages used. Port type, geocode fields and notes are added by us, and are not published by CBP.

Credit line: Source: U.S. Customs and Border Protection, Locate a Port of Entry (https://www.cbp.gov/about/contact/ports). U.S. Customs and Border Protection does not endorse or sponsor this API.

[Back to the API list](../../README.md)
