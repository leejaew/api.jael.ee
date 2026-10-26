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

Required: 1 field. Optional: 9 fields. Disallowed: 6 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `category` | Required | Yes | No | Required. Allowed values: Korean Restaurant (61); Korean BBQ (35); Korean Fried Chicken (18); Korean Asian Buffet (3); Samgyetang Specialty Korean Restaurant (2); Korean Traditional Cuisine (2); Korean Pocha (1). Use * to list all rows. |
| `name` | Optional | Yes | No | Restaurant name in English. |
| `address[city]` | Optional | Yes | No | Area or city name, for example `Mont Kiara`. |
| `phone` | Optional | No | No | Phone number as stored, for example `+60 3 3093 0089`. |
| `name[kr]` | Optional | No | No | Restaurant name in Korean. This is a literal key name that contains brackets. |
| `menu` | Optional | Yes | No | Comma separated list of popular dishes. |
| `address[street]` | Optional | Yes | No | Street address. |
| `address[postalcode]` | Optional | No | No | Postal code, as text. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. |
| `atmosphere` | Disallowed | No | No | Short comma separated notes on the setting, such as `casual` or `good for groups`. |
| `address[state]` | Disallowed | No | No | Always `Kuala Lumpur`. |
| `address[countrycode]` | Disallowed | No | No | Always `MY`. |
| `websiteurl` | Disallowed | No | No | Restaurant or chain website. Null when not given. |
| `socialmedia` | Disallowed | No | No | Instagram or Facebook page. Null when not given. |
| `recordupdated` | Disallowed | No | No | Date the row was last updated, in ISO 8601 format. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../../README.md#how-filters-work) for the wildcard and range syntax.

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
curl -sg "https://api.jael.ee/JLEE/kl_korean_restaurants?category=*&address[city]=Mont%20Kiara"
```

Response (13 rows, first 2 shown):

```json
[
  {
    "name": "KyoChon VERVE Shops Mont Kiara",
    "name[kr]": "교촌치킨",
    "category": "Korean Fried Chicken",
    "menu": "Soy Garlic Series, Red Pepper Series, Honey Series, Honey Garlic Series, Real Fried Half Chicken, Yangnyeom Half Chicken, KyoChon Sampler, SalSal Platter",
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
    "menu": "Bburinkle Chicken, Hot Bburinkle, Macho King Chicken, Bburing Cheese Sticks, Bburing Curly Fries, Spicy Ramyeon Stew, Rose Rabokki, Rose Ramyeon, Kimchi Fried Rice, Corn Cheese",
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
  }
]
```

Another call:

Korean BBQ restaurants with Hanam in the name.

```bash
curl -s "https://api.jael.ee/JLEE/kl_korean_restaurants?category=Korean%20BBQ&name=*Hanam*"
```

Returns 3 rows.

[Back to all APIs](../../../README.md)
