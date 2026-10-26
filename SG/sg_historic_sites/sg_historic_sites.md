# Singapore Historic Sites

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 100](https://img.shields.io/badge/records-100-lightgrey)

The 100 historic sites of Singapore marked by the National Heritage Board, one row per site, with name, short history text, address parts, page and photo links and coordinates. Filter by name, description keyword, street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_historic_sites` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 8 fields. Disallowed: 2 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the historic site name; wildcards work, for example `*church*`. |
| `description` | Optional | Yes | No | Optional keyword search in the short history text; use `*kw*` to find a word anywhere. |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 30-B; blank for many rows. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name; wildcards work; blank for a few rows. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay; blank for some rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2527 to 1.4489. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6993 to 103.9906. |
| `url` | Disallowed | No | No | Link to the National Heritage Board page; returned in results, not used as a filter. |
| `photo_url` | Disallowed | No | No | Link to a photo; returned in results, not used as a filter. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The historic site name. |
| `description` | string | Short history text. |
| `address.block` | string | The block or house number, such as 30-B; blank for many rows. |
| `address.street` | string | The street name. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay; blank for some rows. |
| `url` | string | Link to the National Heritage Board page; returned in results, not used as a filter. |
| `photo_url` | string | Link to a photo; returned in results, not used as a filter. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Historic markers.

```bash
curl -s "https://api.jael.ee/JLEE/sg_historic_sites?name=*Marker*"
```

Response (9 rows, first 2 shown):

```json
[
  {
    "__id": 2,
    "name": "Ho Chi Minh Historic Marker",
    "description": null,
    "address": {
      "block": null,
      "street": null,
      "postalcode": null
    },
    "url": "https://www.roots.gov.sg/places/places-landing/Places/historic-sites/ho-chi-minh",
    "photo_url": "https://roots.sg/~/media/Nhb/Images/Places/Historic%20Sites/Ho%20Chi%20Minh-Ho-Chi-Minh.jpg",
    "location": {
      "latitude": 1.286751,
      "longitude": 103.85193
    }
  },
  {
    "__id": 3,
    "name": "Jawaharlal Nehru Historic Marker",
    "description": null,
    "address": {
      "block": null,
      "street": null,
      "postalcode": null
    },
    "url": "https://www.roots.gov.sg/places/places-landing/Places/historic-sites/jawaharlal-nehru",
    "photo_url": "https://roots.sg/~/media/Roots/Images/historic-sites/034-jawaharlal-nehru/034jawaharlal-nehru.png",
    "location": {
      "latitude": 1.286829,
      "longitude": 103.851466
    }
  }
]
```

Another call:

Sites whose history text mentions 1942.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_historic_sites?description=*1942*"
```

Returns 13 rows.

## Data source

Historic Sites of Singapore, National Heritage Board, data.gov.sg (https://data.gov.sg), dataset d_31e16b12809e66673e90d8b04fdee1b2, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated April 14, 2026.

[Back to all APIs](../../README.md)
