# Kuala Lumpur Korean Restaurants

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 122](https://img.shields.io/badge/records-122-lightgrey)

Korean restaurants in Kuala Lumpur. Each row is one outlet with its Korean and English name, category, popular dishes, atmosphere notes, address, phone, website and social media page.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/kl_korean_restaurants` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

XML output (`.xml`) is not available for this API because the keys `name` and `name[kr]` clash in XML. Use JSON.

## Query parameters

Required: none. Optional: 3 fields. Disallowed: 13 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | No | No | Exact restaurant name in English. |
| `address[city]` | Optional | No | No | Area or city name, for example `Mont Kiara`. |
| `phone` | Optional | No | No | Phone number as stored, for example `+60 3 3093 0089`. |
| `name[kr]` | Disallowed | No | No | Restaurant name in Korean. This is a literal key name that contains brackets. |
| `category` | Disallowed | No | No | Type of restaurant, such as `Korean BBQ`, `Korean Fried Chicken` or `Korean Restaurant`. |
| `menu` | Disallowed | No | No | Comma separated list of popular dishes. |
| `atmosphere` | Disallowed | No | No | Short comma separated notes on the setting, such as `casual` or `good for groups`. |
| `address[street]` | Disallowed | No | No | Street address. |
| `address[postalcode]` | Disallowed | No | No | Postal code, as text. |
| `address[state]` | Disallowed | No | No | Always `Kuala Lumpur`. |
| `address[countrycode]` | Disallowed | No | No | Always `MY`. |
| `location[latitude]` | Disallowed | No | No | Latitude in decimal degrees. |
| `location[longitude]` | Disallowed | No | No | Longitude in decimal degrees. |
| `websiteurl` | Disallowed | No | No | Restaurant or chain website. Null when not given. |
| `socialmedia` | Disallowed | No | No | Instagram or Facebook page. Null when not given. |
| `recordupdated` | Disallowed | No | No | Date the row was last updated, in ISO 8601 format. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

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
| `address.city` | string | Area of Kuala Lumpur, for example `Mont Kiara` or `Bukit Bintang`. |
| `address.state` | string | Always `Kuala Lumpur`. |
| `address.countrycode` | string | Always `MY`. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `phone` | string, null | Phone number with country code. Null when not given. |
| `websiteurl` | string, null | Restaurant or chain website. Null when not given. |
| `socialmedia` | string, null | Instagram or Facebook page. Null when not given. |
| `recordupdated` | string | Date the row was last updated, in ISO 8601 format. |

## Example

Restaurants in Mont Kiara.

```bash
curl -s -g "https://api.jael.ee/JLEE/kl_korean_restaurants?address[city]=Mont%20Kiara"
```

Response, showing 2 of 13 rows:

```json
[
  {
    "name": "KyoChon VERVE Shops Mont Kiara",
    "name[kr]": "교촌치킨",
    "category": "Korean Fried Chicken",
    "menu": "Soy Garlic Series, Red Pepper Series, Honey Series, Honey Garlic Series, Real Fried Hal...",
    "atmosphere": "casual, good for groups",
    "address": {
      "street": "G-01, Verve Shops, 8, Jalan Kiara 5, Mont Kiara",
      "postalcode": "50480",
      "city": "Mont Kiara",
      "state": "Kuala Lumpur",
      "countrycode": "MY"
    },
    "location": {
      "latitude": 3.167219,
      "longitude": 101.646788
    },
    "phone": "+60 3 6211 0433",
    "websiteurl": "https://kyochon.com.my/",
    "socialmedia": "https://www.instagram.com/mykyochon/",
    "recordupdated": "2026-10-05T00:00:00+00:00"
  },
  {
    "name": "BHC Chicken 163 Retail Park",
    "name[kr]": "bhc치킨",
    "category": "Korean Fried Chicken",
    "menu": "Bburinkle Chicken, Hot Bburinkle, Macho King Chicken, Bburing Cheese Sticks, Bburing Cu...",
    "atmosphere": "casual, trendy, good for groups",
    "address": {
      "street": "1F-06, 8, Jalan Kiara, Mont Kiara",
      "postalcode": "50480",
      "city": "Mont Kiara",
      "state": "Kuala Lumpur",
      "countrycode": "MY"
    },
    "location": {
      "latitude": 3.16653,
      "longitude": 101.651991
    },
    "phone": "+60 16 305 1538",
    "websiteurl": null,
    "socialmedia": "https://www.instagram.com/mybhcchicken/",
    "recordupdated": "2026-10-05T00:00:00+00:00"
  },
  ...
]
```

[Back to all APIs](../../README.md)
