# Singapore SportSG Facilities

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 87](https://img.shields.io/badge/records-87-lightgrey)

Sport facilities listed by Sport Singapore (SportSG). Each row is one facility with a type tag, address and coordinates. Types include sport centres, pools, gyms, stadiums, pickleball courts and padel courts.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_sportsg_facilities` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

This API has no query parameters. A `GET` request returns every row.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Facility name. |
| `tag` | string | Facility type: `pickleball`, `sport_centre`, `padel`, `pool`, `gym`, `stadium`, `gym_pool`, `sport_park`, `clubhouse` or `playfield`. |
| `address.street` | string, null | Street address. Null for about a third of the rows. |
| `address.postalcode` | integer | Six digit postal code, as a number. Leading zeros are not kept. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `source` | string | Name of the source data set, its coverage and licence. |
| `sourceupdated` | string | Date or period the data covers, such as `2025-07` or `2024`. |

## Example

This API has no filters, so the call returns every row.

```bash
curl -s "https://api.jael.ee/JLEE/sg_sportsg_facilities"
```

Response, showing 2 of 87 rows:

```json
[
  {
    "name": "Bukit Canberra Sport Centre",
    "tag": "sport_centre",
    "address": {
      "street": "21 Canberra Link",
      "postalcode": 756973
    },
    "location": {
      "latitude": 1.4482633618766256,
      "longitude": 103.82276363764352
    },
    "source": "SportSG Sport Facilities (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_9b87bab5...",
    "sourceupdated": "2025-07"
  },
  {
    "name": "Bukit Gombak Sport Centre",
    "tag": "sport_centre",
    "address": {
      "street": "810 Bukit Batok West Avenue 5",
      "postalcode": 659088
    },
    "location": {
      "latitude": 1.359799534164251,
      "longitude": 103.7521702228675
    },
    "source": "SportSG Sport Facilities (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_9b87bab5...",
    "sourceupdated": "2025-07"
  },
  ...
]
```

## Data source

SportSG Sport Facilities, data.gov.sg (https://data.gov.sg), Singapore Open Data Licence.

[Back to all APIs](../README.md)
