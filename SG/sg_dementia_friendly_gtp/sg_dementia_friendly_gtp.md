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

Required: 1 field. Optional: 6 fields. Disallowed: 4 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `tag` | Required | Yes | No | Required. Allowed values: other (522); transit (194); aac (93); cc (15); care_home (7). Use * to list all rows. |
| `name` | Optional | Yes | No | Name of the place. |
| `address[street]` | Optional | Yes | No | Street address, with the unit number where there is one. |
| `address[postalcode]` | Optional | No | No | Six digit postal code, as a number. Leading zeros are not kept. |
| `phone[main]` | Optional | No | No | Main phone number. Null when not given. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. |
| `phone[alternative]` | Disallowed | No | No | Second phone number. Null for most rows. |
| `hours` | Disallowed | No | No | Opening hours as text. Null when not given. |
| `source` | Disallowed | No | No | Name of the source data set and its licence. |
| `sourceupdated` | Disallowed | No | No | Date the data was last refreshed, in ISO 8601 format. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

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

All active ageing centres.

```bash
curl -s "https://api.jael.ee/JLEE/sg_dementia_friendly_gtp?tag=aac"
```

Response (93 rows, first 2 shown):

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
    "source": "MOH Dementia Friendly Go-To-Points (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_e2d10e591bc8560b8946a0da24790fa5/view. Singapore Open Data Licence.",
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
    "source": "MOH Dementia Friendly Go-To-Points (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_e2d10e591bc8560b8946a0da24790fa5/view. Singapore Open Data Licence.",
    "sourceupdated": "2026-10-04T00:00:00+00:00"
  }
]
```

Another call:

Places with Hospital in the name, of any type.

```bash
curl -s "https://api.jael.ee/JLEE/sg_dementia_friendly_gtp?tag=*&name=*Hospital*"
```

Returns 1 row.

## Data source

MOH Dementia Friendly Go To Points, data.gov.sg (https://data.gov.sg/datasets/d_e2d10e591bc8560b8946a0da24790fa5/view), Singapore Open Data Licence.

[Back to all APIs](../../README.md)
