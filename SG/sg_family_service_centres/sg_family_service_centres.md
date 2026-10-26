# Singapore Family Service Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 55](https://img.shields.io/badge/records-55-lightgrey)

Family Service Centres in Singapore, which give counselling and practical help to families. Each row is one centre with its address, phone, email and coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_family_service_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | No | No | Exact centre name. |
| `address[postalcode]` | Optional | No | No | Six digit postal code, for example `440053`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../README.md#how-filters-work) for the wildcard and range syntax.
* Any field that is not in this table is returned in the output but cannot be used as a filter. Sending one returns an error.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Centre name, with the operator and area. |
| `address.street` | string | Street address with unit number. |
| `address.postalcode` | integer | Six digit postal code, as a number. Leading zeros are not kept. |
| `phone` | string | Phone number with country code. |
| `email` | string | Contact email address. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `source` | string | Name of the source data set and its licence. |
| `sourceupdated` | string | Date the data was last refreshed, in ISO 8601 format. |

## Example

Find the centre at one postal code.

```bash
curl -s -g "https://api.jael.ee/JLEE/sg_family_service_centres?address[postalcode]=440053"
```

Response (1 row):

```json
[
  {
    "name": "Montfort Care Family Service (Marine Parade)",
    "address": {
      "street": "53 Marine Terrace, #01-227",
      "postalcode": 440053
    },
    "phone": "+65 6445 0100",
    "email": "familyservice@montfortcare.org.sg",
    "location": {
      "latitude": 1.30514,
      "longitude": 103.916337
    },
    "source": "MSF Family Service Centres (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_add23c...",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  }
]
```

## Data source

MSF Family Service Centres, data.gov.sg (https://data.gov.sg/datasets/d_add23c06f7267e799185c79ccaa2099b/view), Singapore Open Data Licence.

[Back to all APIs](../README.md)
