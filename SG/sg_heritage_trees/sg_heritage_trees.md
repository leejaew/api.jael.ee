# Singapore Heritage Trees

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 255](https://img.shields.io/badge/records-255-lightgrey)

The 255 trees on the NParks Heritage Trees register in Singapore, one row per tree, with tree code, scientific and common name, where it grows, page link and coordinates. Filter by common_name, scientific_name, found_in or tree_id, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_heritage_trees` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 6 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `tree_id` | Optional | No | No | Optional exact filter for the heritage tree code, such as HT 2007-163. |
| `scientific_name` | Optional | Yes | No | Optional filter for the Latin species name, such as Dalbergia oliveri; some carry the author, so use wildcards. |
| `common_name` | Optional | Yes | No | Optional filter for the common name, such as Teak or Mangrove Apple; blank for 19 rows. |
| `found_in` | Optional | Yes | No | Optional filter for the place text, such as Fort Canning Park or Pulau Ubin; blank for 83 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.218 to 1.4633. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6837 to 103.9885. |
| `url` | Disallowed | No | No | Link to the NParks tree page; returned in results, not used as a filter. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `tree_id` | string | The heritage tree code, such as HT 2007-163. |
| `scientific_name` | string | The Latin species name, such as Dalbergia oliveri; some carry the author, so use wildcards. |
| `common_name` | string | The common name, such as Teak or Mangrove Apple; blank for 19 rows. |
| `found_in` | string | The place text, such as Fort Canning Park or Pulau Ubin; blank for 83 rows. |
| `url` | string | Link to the NParks tree page; returned in results, not used as a filter. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Rain trees on the register.

```bash
curl -s "https://api.jael.ee/JLEE/sg_heritage_trees?common_name=*Rain%20Tree*"
```

Response (12 rows, first 2 shown):

```json
[
  {
    "tree_id": "HT 2003-107",
    "scientific_name": "Samanea saman (Jacq.) Merr.",
    "common_name": "Rain Tree",
    "found_in": "Top of the slope at junction of Spottiswoode Park Road and Everton Road",
    "url": "https://heritagetrees.nparks.gov.sg/ht-2003-107/",
    "location": {
      "latitude": 1.273637,
      "longitude": 103.838461
    }
  },
  {
    "tree_id": "HT 2003-106",
    "scientific_name": "Samanea saman (Jacq.) Merr.",
    "common_name": "Rain Tree",
    "found_in": null,
    "url": "https://heritagetrees.nparks.gov.sg/ht-2003-106/",
    "location": {
      "latitude": 1.324083,
      "longitude": 103.815744
    }
  }
]
```

Another call:

Trees in the Singapore Botanic Gardens.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_heritage_trees?found_in=*Botanic*"
```

Returns 56 rows.

## Data source

Heritage Trees, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_644ff187b6d14d6316f47284a4a6c81f, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated August 18, 2025.

[Back to all APIs](../../README.md)
