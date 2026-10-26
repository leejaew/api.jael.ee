# Singapore National Monuments

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 77](https://img.shields.io/badge/records-77-lightgrey)

The 77 national monuments of Singapore listed by the National Heritage Board, one row per monument, with name, short history text, address parts, page and photo links and coordinates. Filter by name, description keyword, street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_national_monuments` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 8 fields. Disallowed: 2 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | No | Row ID. |
| `name` | Optional | Yes | No | Optional filter for the national monument name; wildcards work, for example `*church*`. |
| `description` | Optional | Yes | No | Optional keyword search in the short history text; use `*kw*` to find a word anywhere. |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 30-B; blank for many rows. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name; wildcards work; blank for a few rows. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay; blank for some rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2647 to 1.4474. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6831 to 103.9696. |
| `url` | Disallowed | No | No | Link to the National Heritage Board page; returned in results, not used as a filter. |
| `photo_url` | Disallowed | No | No | Link to a photo; returned in results, not used as a filter. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The national monument name. |
| `description` | string | Short history text. |
| `address.block` | string | The block or house number, such as 30-B; blank for many rows. |
| `address.street` | string | The street name. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay; blank for some rows. |
| `url` | string | Link to the National Heritage Board page; returned in results, not used as a filter. |
| `photo_url` | string | Link to a photo; returned in results, not used as a filter. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Monuments with church in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_national_monuments?name=*church*"
```

Response (9 rows, first 2 shown):

```json
[
  {
    "__id": 3,
    "name": "St. George's Church",
    "description": "Saint George's Church is situated in the quiet environs off Minden Road, a short distance from the bustling shopping district of Orchard Road.",
    "address": {
      "block": null,
      "street": "Minden Road",
      "postalcode": "248818"
    },
    "url": "https://www.roots.gov.sg/places/places-landing/Places/national-monuments/saint-georges-church",
    "photo_url": "https://roots.sg/~/media/Roots/Images/monuments/054-st-georges-church/st-georges-church-01.png",
    "location": {
      "latitude": 1.30479,
      "longitude": 103.815131
    }
  },
  {
    "__id": 4,
    "name": "St Joseph's Church",
    "description": "Saint Joseph's Church on Victoria Street was the headquarters of the Portuguese Mission in Singapore.",
    "address": {
      "block": "143",
      "street": "Victoria Street",
      "postalcode": "188020"
    },
    "url": "https://www.roots.gov.sg/places/places-landing/Places/national-monuments/st-josephs-church",
    "photo_url": "https://roots.sg/~/media/Roots/Images/monuments/019-st-josephs-church/st-josephs-church.png",
    "location": {
      "latitude": 1.298358,
      "longitude": 103.853152
    }
  }
]
```

Another call:

Monuments whose history mentions a temple, on a street.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_national_monuments?description=*temple*&address[street]=*Street*"
```

Returns 2 rows.

## Data source

National Monuments of Singapore, National Heritage Board, data.gov.sg (https://data.gov.sg), dataset d_b29c230ec6b609e29ed42f71ca9a8767, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated April 14, 2026.

[Back to all APIs](../../README.md)
