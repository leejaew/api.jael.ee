# Singapore Hotels

![Hotel icon](sg_hotels.png)

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 468](https://img.shields.io/badge/records-468-lightgrey)

Hotels and hostels licensed by the Hotels Licensing Board in Singapore. Each row is one property with its type, room count, address, phone, coordinates and, where known, Instagram and Facebook pages.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_hotels` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Hotel name, for example `*Copthorne*`. |
| `tag` | Optional | No | No | `hotel` or `hostel`. |
| `address[postalcode]` | Optional | No | No | Six digit postal code. |
| `phone` | Optional | No | No | Phone number as stored, for example `+65 6373 6988`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../README.md#how-filters-work) for the wildcard and range syntax.
* Any field that is not in this table is returned in the output but cannot be used as a filter. Sending one returns an error.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Property name. |
| `tag` | string | `hotel` or `hostel`. |
| `address.street` | string, null | Street address. Null for about half of the rows. |
| `address.city` | string | Always `Singapore`. |
| `address.state` | string | Always `Singapore`. |
| `address.postalcode` | string | Six digit postal code, as text. |
| `country` | string | Always `Singapore`. |
| `phone` | string, null | Phone number with country code. Null when not given. |
| `totalrooms` | string | Number of rooms, as text. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `social.instagram` | string, null | Instagram page. Null when not known. |
| `social.facebook` | string, null | Facebook page. Null when not known. |
| `source` | string | Name of the source data set, its coverage and licence. |
| `sourceupdated` | string | Date the data was last refreshed, in ISO 8601 format. |

## Example

Hotels with Copthorne in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_hotels?name=*Copthorne*"
```

Response (2 rows):

```json
[
  {
    "name": "Copthorne King Hotel Singapore",
    "tag": "hotel",
    "address": {
      "street": null,
      "city": "Singapore",
      "state": "Singapore",
      "postalcode": "169632"
    },
    "country": "Singapore",
    "phone": null,
    "totalrooms": "313",
    "location": {
      "latitude": 1.28825959921985,
      "longitude": 103.83664249676404
    },
    "social": {
      "instagram": null,
      "facebook": null
    },
    "source": "HLB Licensed Hotels (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_654e22f14e5bb...",
    "sourceupdated": "2025-07-11T00:00:00+00:00"
  },
  {
    "name": "Grand Copthorne Waterfront Hotel Singapore",
    "tag": "hotel",
    "address": {
      "street": "392 Havelock Road",
      "city": "Singapore",
      "state": "Singapore",
      "postalcode": "169663"
    },
    "country": "Singapore",
    "phone": "+65 6733 0880",
    "totalrooms": "573",
    "location": {
      "latitude": 1.2902819270049377,
      "longitude": 103.83514795784397
    },
    "social": {
      "instagram": "https://www.instagram.com/grandcopthornewaterfront/",
      "facebook": "https://www.facebook.com/GrandCopthorneWaterfrontHotel/"
    },
    "source": "HLB Licensed Hotels (GEOJSON), data.gov.sg https://data.gov.sg/datasets/d_654e22f14e5bb...",
    "sourceupdated": "2025-07-11T00:00:00+00:00"
  }
]
```

## Data source

Hotels licensed by the Hotels Licensing Board (HLB), data.gov.sg (https://data.gov.sg/datasets/d_654e22f14e5bb817423f0e0c9ac4f632/view), Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Coverage from July 2025.

## Icon

The file `sg_hotels.png` in this folder is an icon made by Vectors Market from https://www.flaticon.com, licensed CC 3.0 BY.

[Back to all APIs](../README.md)
