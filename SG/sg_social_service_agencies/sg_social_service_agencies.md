# Singapore Social Service Agencies

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 908](https://img.shields.io/badge/records-908-lightgrey)

Social service agencies that are members of the National Council of Social Service (NCSS) in Singapore. Each row is one agency with a service type tag, address, website and coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_social_service_agencies` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 5 fields. Disallowed: 3 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `tag` | Required | Yes | No | Required. Allowed values: other (767); family (53); eldercare (49); youth (29); dialysis (10). Use * to list all rows. |
| `name` | Optional | Yes | No | Agency name. |
| `address[postalcode]` | Optional | No | No | Six digit postal code. |
| `address[street]` | Optional | Yes | No | Street address with unit number. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. |
| `website` | Disallowed | No | No | Agency website. Null when not given. |
| `source` | Disallowed | No | No | Name of the source data set and its licence. |
| `sourceupdated` | Disallowed | No | No | Date the data was last refreshed, in ISO 8601 format. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Agency name. |
| `tag` | string | Service type: `family`, `eldercare`, `youth`, `dialysis` or `other`. |
| `address.street` | string | Street address with unit number. |
| `address.postalcode` | string | Six digit postal code, as text. |
| `website` | string, null | Agency website. Null when not given. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `source` | string | Name of the source data set and its licence. |
| `sourceupdated` | string | Date the data was last refreshed, in ISO 8601 format. |

## Example

All dialysis centres.

```bash
curl -s "https://api.jael.ee/JLEE/sg_social_service_agencies?tag=dialysis"
```

Response (10 rows, first 2 shown):

```json
[
  {
    "name": "NKF Clementi Dialysis Centre",
    "tag": "dialysis",
    "address": {
      "street": "326 Clementi Ave 5, #01-175",
      "postalcode": "120326"
    },
    "website": null,
    "location": {
      "latitude": 1.314431,
      "longitude": 103.766358
    },
    "source": "NCSS member social service agencies (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_736e30b5a0111242dfde98d7b32cfda2/view. Singapore Open Data Licence.",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  },
  {
    "name": "NKF Hong Kah Dialysis Centre",
    "tag": "dialysis",
    "address": {
      "street": "536 Jurong West St 52, #01-497",
      "postalcode": "640536"
    },
    "website": null,
    "location": {
      "latitude": 1.349921,
      "longitude": 103.716718
    },
    "source": "NCSS member social service agencies (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_736e30b5a0111242dfde98d7b32cfda2/view. Singapore Open Data Licence.",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  }
]
```

Another call:

Agencies of any type with Caregivers in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_social_service_agencies?tag=*&name=*Caregivers*"
```

Returns 4 rows.

## Data source

NCSS member social service agencies, data.gov.sg (https://data.gov.sg/datasets/d_736e30b5a0111242dfde98d7b32cfda2/view), Singapore Open Data Licence.

[Back to all APIs](../../README.md)
