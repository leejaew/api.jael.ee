# UK Ports

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 160](https://img.shields.io/badge/records-160-lightgrey) ![updated 2026-07-29](https://img.shields.io/badge/updated-2026--07--29-orange) ![license OGL v3 + ODbL](https://img.shields.io/badge/license-OGL%20v3%20%2B%20ODbL-green)

All 160 UK statistical ports in the Department for Transport freight statistics, with UN/LOCODE, country, major or minor status, port group, the smaller wharves reported under each, 2024 and 2025 freight tonnage (and the 2025 cargo mix for the 33 biggest) and an approximate map point. Filter by the required `country`, then by port, type, group, tonnage or a latitude and longitude range.

Rows by country: England (86), Scotland (43), Wales (19) and Northern Ireland (12). Major ports: 53. Minor ports: 107. The 11 ports that handled more than 10 million tonnes in 2025 include London (56,584 kt), Grimsby and Immingham (44,875 kt) and Milford Haven (38,279 kt).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_ports` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`country`). Optional: 17 fields. Disallowed: 3 fields.

`country` must be in every call. Use `country=*` to list all 160 ports. Disallowed means the field is returned in the output but cannot be used as a filter. Without `country` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Tonnage and coordinate fields take ranges. Blank tonnage rows are not returned by a tonnage range filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `country` | Required | Yes | No | Country of the port: `England` (86), `Scotland` (43), `Wales` (19) or `Northern Ireland` (12). Wildcards work, so `country=*` lists all 160 ports and `country=*land` gives England, Scotland and Northern Ireland (141). |
| `port_locode` | Optional | No | No | Exact UN/LOCODE DfT uses for the statistical port, such as `GBDVR` (Dover) or `GBLON` (London). Unique on every row. |
| `other_locodes` | Optional | Yes | No | Other DfT locodes merged into this row because they report under the same statistical port name, such as `GBKYL` for Kyle of Lochalsh. Wildcards work. Blank on 156 rows. |
| `port_name` | Optional | Yes | No | Statistical port name as DfT gives it, such as `Dover`, `Grimsby & Immingham` or `Port Talbot`. Wildcards work, so `port_name=Port*` gives 7. Unique on every row. |
| `port_type` | Optional | No | No | Exact: `Major` (53, the larger ports DfT reports in full every quarter) or `Minor` (107). |
| `port_group` | Optional | Yes | No | DfT regional port group, such as `Scotland East Coast` (22), `West Country` (21), `Thames and Kent` (12), `Humber` (7) or `Haven` (4); 13 groups. Wildcards work. Blank for Bangor (Wales). |
| `reporting_ports` | Optional | Yes | No | Smaller wharves and terminals whose traffic DfT counts under this port, as Name (LOCODE) joined with semicolons; use wildcards, such as `reporting_ports=*Tilbury*` (London). Blank on 118 rows. |
| `reporting_port_count` | Optional | No | Yes | Number of reporting ports counted under this port, 0 to 23 (London 23, Orkney 20, Clyde 18). Use a range, such as `reporting_port_count=10..30`. |
| `tonnage_2025_kt` | Optional | No | Yes | Total freight handled in 2025 in thousand tonnes (DfT PORT0101), 0 to 56584.317 (London). Use a range, such as `tonnage_2025_kt=10000..100000` for the 11 ports over 10 million tonnes. Blank on 46 ports where DfT shows z (not applicable). |
| `inwards_2025_kt` | Optional | No | Yes | Inward (unloaded) freight in 2025 in thousand tonnes. Blank on 46 ports. |
| `outwards_2025_kt` | Optional | No | Yes | Outward (loaded) freight in 2025 in thousand tonnes. Blank on 46 ports. |
| `tonnage_2024_kt` | Optional | No | Yes | Total freight handled in 2024 in thousand tonnes. Blank on 46 ports. |
| `liquid_bulk_kt` | Optional | No | Yes | 2025 liquid bulk (oil, gas, chemicals) in thousand tonnes. Only filled for the 33 ports that handled more than 2 million tonnes in 2025 (DfT table PORT0304); blank on the other 127. |
| `dry_bulk_other_kt` | Optional | No | Yes | 2025 dry bulk plus other general cargo in thousand tonnes. Only filled for those 33 ports. |
| `roro_kt` | Optional | No | Yes | 2025 roll on roll off traffic (lorries, trailers, ferries) in thousand tonnes, 0 to 18521.467 (Dover). Only filled for those 33 ports. |
| `containers_kt` | Optional | No | Yes | 2025 container traffic (lift on lift off) in thousand tonnes. Use a range, such as `containers_kt=1000..20000` (9 ports). Only filled for those 33 ports. |
| `latitude` | Optional | No | Yes | Latitude of the port in decimal degrees (WGS 84), from 49.916667 to 60.461042. The point is an approximate port location; see `geocode_quality`. |
| `longitude` | Optional | No | Yes | Longitude of the port in decimal degrees (WGS 84), from -7.466667 to 1.754724. Use a range, for example `latitude=50..51&longitude=-6..-4` for Cornwall and south Devon (13 ports). |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from: `dft_port_list` (27), `unlocode` (72, UN/LOCODE, rounded to the nearest minute, about 1 to 2 km), `nominatim_port` (30, a harbour, dock or terminal found in OpenStreetMap) or `nominatim_place` (31, the town or place the port is named after in OpenStreetMap). |
| `source` | Disallowed | No | No | Data source page for the DfT freight tables. |
| `updated` | Disallowed | No | No | Date DfT published the 2025 annual port tables, `YYYY-MM-DD` (`2026-07-29`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `country` returns HTTP 400 with `Your request is missing the following required parameters: country. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.

## Response fields

| Field | Type | Description |
|---|---|---|
| `port_locode` | string | UN/LOCODE for the statistical port. Unique key. |
| `other_locodes` | string | Other DfT locodes merged into this row. Null on 156 rows. |
| `port_name` | string | Statistical port name. |
| `country` | string | England, Scotland, Wales or Northern Ireland. |
| `port_type` | string | `Major` or `Minor`. |
| `port_group` | string | DfT regional port group. Null for 1 row. |
| `reporting_ports` | string | Smaller wharves counted under this port. Null on 118 rows. |
| `reporting_port_count` | integer | Number of reporting ports. |
| `tonnage_2025_kt` | number | 2025 total freight, thousand tonnes. Null on 46 rows. |
| `inwards_2025_kt` | number | 2025 inward freight, thousand tonnes. Null on 46 rows. |
| `outwards_2025_kt` | number | 2025 outward freight, thousand tonnes. Null on 46 rows. |
| `tonnage_2024_kt` | number | 2024 total freight, thousand tonnes. Null on 46 rows. |
| `liquid_bulk_kt` | number | 2025 liquid bulk, thousand tonnes. Null on 127 rows. |
| `dry_bulk_other_kt` | number | 2025 dry bulk and other cargo, thousand tonnes. Null on 127 rows. |
| `roro_kt` | number | 2025 roll on roll off, thousand tonnes. Null on 127 rows. |
| `containers_kt` | number | 2025 containers, thousand tonnes. Null on 127 rows. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the DfT source page. |
| `updated` | string | Date of the 2025 annual tables, `YYYY-MM-DD`. |

Notes:

* Map points are approximate. Prefer the address or official port maps for navigation.
* Bangor (Wales) has a blank `port_group` because the DfT freight table puts a Bangor in the Northern Ireland group while the port list gives Bangor as GBBNG in Wales.

## Examples

Exact locode. Dover. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_ports" --data-urlencode "country=*" --data-urlencode "port_locode=GBDVR"
```

```json
[
  {
    "port_locode": "GBDVR",
    "other_locodes": null,
    "port_name": "Dover",
    "country": "England",
    "port_type": "Major",
    "port_group": "Thames and Kent",
    "reporting_ports": null,
    "reporting_port_count": 0,
    "tonnage_2025_kt": 18918.193,
    "inwards_2025_kt": 12606.921,
    "outwards_2025_kt": 6311.272,
    "tonnage_2024_kt": 19402.301,
    "liquid_bulk_kt": 0.0,
    "dry_bulk_other_kt": 338.266,
    "roro_kt": 18521.467,
    "containers_kt": 58.46,
    "latitude": 51.126251,
    "longitude": 1.330588,
    "geocode_quality": "nominatim_port",
    "source": "https://www.gov.uk/government/statistical-data-sets/port-and-domestic-waterborne-freight-statistics-port",
    "updated": "2026-07-29"
  }
]
```

Ports over 10 million tonnes in 2025. This call returns 11 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_ports" --data-urlencode "country=*" --data-urlencode "tonnage_2025_kt=10000..100000" | jq -r '.[] | [.port_name, .tonnage_2025_kt] | @tsv'
```

```text
Belfast	18243.156
Dover	18918.193
Felixstowe	22690.133
Forth	16988.394
Grimsby & Immingham	44874.622
Liverpool	31349.245
London	56584.317
Milford Haven	38278.596
Rivers Hull & Humber	10340.811
Southampton	34376.153
Tees & Hartlepool	23411.127
```

Major ports in Wales. This call returns 7 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ports?country=Wales&port_type=Major" | jq length
```

Reporting ports wildcard. London (Tilbury). This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_ports" --data-urlencode "country=*" --data-urlencode "reporting_ports=*Tilbury*" | jq -r '.[].port_name'
```

## Data source

The ports and tonnage come from the Department for Transport, [Port and domestic waterborne freight statistics](https://www.gov.uk/government/statistical-data-sets/port-and-domestic-waterborne-freight-statistics-port), tables PORT0101 and PORT0304 (2025 annual tables, published 29 July 2026) and the Major and minor port list for freight statistics, fetched on 5 November 2026. Published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited.

Changes: one row per statistical port with its reporting ports joined, z values left blank, note markers removed from group names, map points added. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: Department for Transport, Port and domestic waterborne freight statistics (https://www.gov.uk/government/statistical-data-sets/port-and-domestic-waterborne-freight-statistics-port), tables PORT0101 and PORT0304 (2025 annual tables, published 29 July 2026) and the Major and minor port list for freight statistics, fetched 5 November 2026.
* Map points from the DfT port list, UN/LOCODE (UNECE) and OpenStreetMap via Nominatim; map data © OpenStreetMap contributors, available under the Open Database Licence (ODbL) (https://www.openstreetmap.org/copyright).
* Not endorsed by DfT. No DfT logos are used.

[Back to the API list](../../README.md)
