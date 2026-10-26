# Singapore Korean Restaurants

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 42](https://img.shields.io/badge/records-42-lightgrey)

Korean restaurants in Singapore. Each row is one outlet with its Korean and English name, category, popular dishes, atmosphere notes, address, phone, website and social media page.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_korean_restaurants` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

XML output (`.xml`) is not available for this API because the keys `name` and `name[kr]` clash in XML. Use JSON.

## Query parameters

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | No | No | Exact restaurant name in English. |
| `name[kr]` | Optional | No | No | Exact restaurant name in Korean. |
| `address[postalcode]` | Optional | No | No | Postal code. |
| `phone` | Optional | No | No | Phone number as stored, for example `+60 3 3093 0089`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../README.md#how-filters-work) for the wildcard and range syntax.
* Any field that is not in this table is returned in the output but cannot be used as a filter. Sending one returns an error.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Restaurant name in English. |
| `name[kr]` | string | Restaurant name in Korean. This is a literal key name that contains brackets. |
| `category` | string | Type of restaurant, such as `Korean BBQ`, `Korean Fried Chicken` or `Korean Restaurant`. |
| `menu` | string | Comma separated list of popular dishes. |
| `atmosphere` | string | Short comma separated notes on the setting, such as `casual` or `good for groups`. |
| `address.street` | string | Street address. |
| `address.postalcode` | string | Postal code, as text. |
| `address.city` | string | Always `Singapore`. |
| `address.state` | string, null | Not used in Singapore. Always null. |
| `address.countrycode` | string | Always `SG`. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `phone` | string | Phone number with country code. Null when not given. |
| `websiteurl` | string, null | Restaurant or chain website. Null when not given. |
| `socialmedia` | string, null | Instagram or Facebook page. Null when not given. |
| `recordupdated` | string | Date the row was last updated, in ISO 8601 format. |

## Example

Restaurants at one postal code.

```bash
curl -s -g "https://api.jael.ee/JLEE/sg_korean_restaurants?address[postalcode]=079903"
```

Response (1 row):

```json
[
  {
    "name": "Ko Ryo Jeong Korean Restaurant",
    "name[kr]": "고려정 한국식당",
    "category": "Korean Restaurant",
    "menu": "Pork Bulgogi Set, Chicken Bulgogi Set, Ramyun, Gimbap, Stew Lunch Set, Mini Pancake, Ba...",
    "atmosphere": "casual, cbd lunch",
    "address": {
      "street": "10 Anson Road #02-59, International Plaza",
      "postalcode": "079903",
      "city": "Singapore",
      "state": null,
      "countrycode": "SG"
    },
    "location": {
      "latitude": 1.276189,
      "longitude": 103.846299
    },
    "phone": "+65 6324 3120",
    "websiteurl": null,
    "socialmedia": "https://www.instagram.com/koryojeong.sg/",
    "recordupdated": "2026-10-05T00:00:00+00:00"
  }
]
```

[Back to all APIs](../README.md)
