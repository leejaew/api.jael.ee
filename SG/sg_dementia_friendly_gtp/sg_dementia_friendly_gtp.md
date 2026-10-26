# Singapore Dementia Friendly Go To Points

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 831](https://img.shields.io/badge/records-831-lightgrey)

Dementia Friendly Go To Points in Singapore. These are places where a person with dementia who is lost or needs help can go for support. Each row is one place with its type, address, phone numbers, opening hours and coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_dementia_friendly_gtp` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

This API has no query parameters. A `GET` request returns every row.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Name of the place. |
| `tag` | string | Type of place: `transit` (mostly MRT stations), `aac` (active ageing centres), `cc` (community clubs), `care_home` or `other`. |
| `address.street` | string | Street address, with the unit number where there is one. |
| `address.postalcode` | integer | Six digit postal code, as a number. Leading zeros are not kept. |
| `phone.main` | string, null | Main phone number. Null when not given. |
| `phone.alternative` | string, null | Second phone number. Null for most rows. |
| `hours` | string, null | Opening hours as text. Null when not given. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `source` | string | Name of the source data set and its licence. |
| `sourceupdated` | string | Date the data was last refreshed, in ISO 8601 format. |

## Example

This API has no filters, so the call returns every row.

```bash
curl -s "https://api.jael.ee/JLEE/sg_dementia_friendly_gtp"
```

Response, showing 2 of 831 rows:

```json
[
  {
    "name": "Allkin Singapore - Allkin Active Ageing Centre (AAC) @ Sin Ming 410",
    "tag": "aac",
    "address": {
      "street": "410 Sin Ming Avenue, #01-119",
      "postalcode": 570410
    },
    "phone": {
      "main": "+65 6251 5010",
      "alternative": null
    },
    "hours": "Monday to Friday: 9am - 6pm",
    "location": {
      "latitude": 1.362821,
      "longitude": 103.835492
    },
    "source": "MOH Dementia Friendly Go-To-Points (GEOJSON), data.gov.sg https://data.gov.sg/datasets/...",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  },
  {
    "name": "THK Active Ageing Centre @ Toa Payoh 15",
    "tag": "aac",
    "address": {
      "street": "Blk 15 Toa Payoh Lorong 7 #01-579",
      "postalcode": 310015
    },
    "phone": {
      "main": "+65 6251 6504",
      "alternative": null
    },
    "hours": "Monday to Friday: 8.30 am - 5.30 pm, Saturday: 8.30 am - 12.30 pm",
    "location": {
      "latitude": 1.335922,
      "longitude": 103.859058
    },
    "source": "MOH Dementia Friendly Go-To-Points (GEOJSON), data.gov.sg https://data.gov.sg/datasets/...",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  },
  ...
]
```

## Data source

MOH Dementia Friendly Go To Points, data.gov.sg (https://data.gov.sg/datasets/d_e2d10e591bc8560b8946a0da24790fa5/view), Singapore Open Data Licence.

[Back to all APIs](../README.md)
