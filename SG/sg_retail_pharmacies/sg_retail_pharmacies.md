# Singapore Retail Pharmacies

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 249](https://img.shields.io/badge/records-249-lightgrey)

Retail pharmacy locations in Singapore from the Health Sciences Authority. Each row is one outlet with its brand (Guardian, Unity, Watsons or independent), full address and coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_retail_pharmacies` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 4 fields. Disallowed: 8 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | No | No | Exact outlet name. |
| `brand` | Optional | No | No | `guardian`, `unity`, `watsons` or `independent`. |
| `tag` | Optional | No | No | Same values as `brand`. |
| `address[postalcode]` | Optional | No | No | Six digit postal code. |
| `address[block]` | Disallowed | No | No | Block or building number. |
| `address[street]` | Disallowed | No | No | Street name. |
| `address[level]` | Disallowed | No | No | Floor, for example `#02`. Null when not given. |
| `address[unit]` | Disallowed | No | No | Unit number or numbers. Null when not given. |
| `location[latitude]` | Disallowed | No | No | Latitude in decimal degrees. |
| `location[longitude]` | Disallowed | No | No | Longitude in decimal degrees. |
| `source` | Disallowed | No | No | Name of the source data set and its licence. |
| `sourceupdated` | Disallowed | No | No | Date the data was last refreshed, in ISO 8601 format. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Outlet name, usually the company name followed by the mall or area in brackets. |
| `brand` | string | Pharmacy chain: `guardian`, `unity`, `watsons` or `independent`. |
| `tag` | string | Same value as `brand`. |
| `address.block` | string | Block or building number. |
| `address.street` | string | Street name. |
| `address.level` | string, null | Floor, for example `#02`. Null when not given. |
| `address.unit` | string, null | Unit number or numbers. Null when not given. |
| `address.postalcode` | string | Six digit postal code, as text. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `source` | string | Name of the source data set and its licence. |
| `sourceupdated` | string | Date the data was last refreshed, in ISO 8601 format. |

## Example

All Watsons outlets.

```bash
curl -s "https://api.jael.ee/JLEE/sg_retail_pharmacies?brand=watsons"
```

Response, showing 2 of 33 rows:

```json
[
  {
    "name": "Watson's Personal Care Stores Pte Ltd (Bugis Junction)",
    "brand": "watsons",
    "tag": "watsons",
    "address": {
      "block": "200",
      "street": "Victoria Street",
      "level": "#02",
      "unit": "29 TO 33/33A",
      "postalcode": "188021"
    },
    "location": {
      "latitude": 1.299113,
      "longitude": 103.855411
    },
    "source": "HSA Retail pharmacy locations (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_bb9...",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  },
  {
    "name": "Watson's Personal Care Stores Pte Ltd (Causeway Point)",
    "brand": "watsons",
    "tag": "watsons",
    "address": {
      "block": "1",
      "street": "Woodlands Square",
      "level": "#B1",
      "unit": "14/15",
      "postalcode": "738099"
    },
    "location": {
      "latitude": 1.436008,
      "longitude": 103.785689
    },
    "source": "HSA Retail pharmacy locations (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_bb9...",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  },
  ...
]
```

## Data source

HSA Retail pharmacy locations, data.gov.sg (https://data.gov.sg/datasets/d_bb92615f43de22933e4479558b1f6c36/view), Singapore Open Data Licence.

[Back to all APIs](../../README.md)
