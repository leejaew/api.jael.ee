# Johor Bahru Korean Restaurants

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 46](https://img.shields.io/badge/records-46-lightgrey)

Korean restaurants in Johor Bahru and the nearby city of Iskandar Puteri. Each row is one outlet with its Korean and English name, category, popular dishes, atmosphere notes, address, phone, website and social media page.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/jb_korean_restaurants` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

XML output (`.xml`) is not available for this API because the keys `name` and `name[kr]` clash in XML. Use JSON.

## Query parameters

Required: none. Optional: 10 fields. Disallowed: 6 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Restaurant name in English. |
| `address[city]` | Optional | No | No | Area or city name, for example `Iskandar Puteri`. |
| `phone` | Optional | No | No | Phone number as stored, for example `+60 7 207 3348`. |
| `name[kr]` | Optional | No | No | Restaurant name in Korean. This is a literal key name that contains brackets. |
| `category` | Optional | No | No | Type of restaurant, such as `Korean BBQ`, `Korean Fried Chicken` or `Korean Restaurant`. |
| `menu` | Optional | Yes | No | Comma separated list of popular dishes. |
| `address[street]` | Optional | Yes | No | Street address. |
| `address[postalcode]` | Optional | No | No | Postal code, as text. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. |
| `atmosphere` | Disallowed | No | No | Short comma separated notes on the setting, such as `casual` or `good for groups`. |
| `address[state]` | Disallowed | No | No | Always `Johor`. |
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
| `address.city` | string | `Johor Bahru` or `Iskandar Puteri`. |
| `address.state` | string | Always `Johor`. |
| `address.countrycode` | string | Always `MY`. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `phone` | string | Phone number with country code. Null when not given. |
| `websiteurl` | string, null | Restaurant or chain website. Null when not given. |
| `socialmedia` | string, null | Instagram or Facebook page. Null when not given. |
| `recordupdated` | string | Date the row was last updated, in ISO 8601 format. |

## Example

Restaurants in Iskandar Puteri.

```bash
curl -sg "https://api.jael.ee/JLEE/jb_korean_restaurants?address[city]=Iskandar%20Puteri"
```

Response (3 rows, first 2 shown):

```json
[
  {
    "name": "MEETMEAT BBQ",
    "name[kr]": "미트미트숙성고깃집",
    "category": "Korean BBQ",
    "menu": "Short Rib Steak, Seasoned Short Rib Steak, Boneless Short Rib, Bone Pork Belly, Pork Belly, Jowl, Spicy Pork Backbone Soup, Kimchi Stew, Soybean Stew, Cast Iron Fried Rice",
    "atmosphere": "staff grill for you, indoor dining, inconsistent service",
    "address": {
      "street": "Lot 71-01, Sunway Emerald Boulevard, 88, Persiaran Medini 3, Sunway Iskandar",
      "postalcode": "79250",
      "city": "Iskandar Puteri",
      "state": "Johor",
      "countrycode": "MY"
    },
    "location": {
      "latitude": 1.395527,
      "longitude": 103.633965
    },
    "phone": "+60 10 669 6934",
    "websiteurl": null,
    "socialmedia": "https://www.instagram.com/meetmeatbbq/",
    "recordupdated": "2026-10-05T00:00:00+00:00"
  },
  {
    "name": "Hong Dae Ip Gu Eco Botanic",
    "name[kr]": "홍대입구",
    "category": "Korean Restaurant",
    "menu": "Grilled Beef with Mushrooms, Korean Ramen, Seafood Pancake, Crab Gimbap, Roe Bibimbap, Steamed Egg, Marinated Pork, Kimchi Stew",
    "atmosphere": "casual, cosy, groups, lively",
    "address": {
      "street": "69, Jalan Eko Botani 3/5, Eco Botani",
      "postalcode": "79100",
      "city": "Iskandar Puteri",
      "state": "Johor",
      "countrycode": "MY"
    },
    "location": {
      "latitude": 1.439257,
      "longitude": 103.619594
    },
    "phone": "+60 13 361 6341",
    "websiteurl": null,
    "socialmedia": null,
    "recordupdated": "2026-10-05T00:00:00+00:00"
  }
]
```

Another call:

Restaurants with Seoul in the name.

```bash
curl -s "https://api.jael.ee/JLEE/jb_korean_restaurants?name=*Seoul*"
```

Returns 5 rows.

[Back to all APIs](../../../README.md)
