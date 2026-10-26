# Singapore Hawker Centres

![Hawker centre icon](sg_hawker_centres.png)

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 107](https://img.shields.io/badge/records-107-lightgrey)

Hawker centres and markets run by the Singapore government. Each row is one centre with its address, postal code, centre type, owner, the number of stalls (cooked food and market produce) and a list of dishes sold there.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_hawker_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 9 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `type[type_of_centre]` | Required | Yes | No | Required. Allowed values: MHC (77); HC (27); MK (3). Use * to list all rows. |
| `name_of_centre` | Optional | Yes | No | Name of the centre, for example `*Food Centre*`. |
| `address` | Optional | Yes | No | Address text, for example `*Bedok*`. |
| `postal_code` | Optional | Yes | No | Six digit postal code, for example `069111`. |
| `menu` | Optional | Yes | No | Dish search, for example `*laksa*`. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `type[owner]` | Optional | No | No | Owner of the centre, for example `Government`. |
| `stall_count[no_of_stalls]` | Optional | No | Yes | Total number of stalls. |
| `stall_count[no_of_cooked_food_stalls]` | Optional | No | Yes | Number of cooked food stalls. |
| `stall_count[no_of_mkt_produce_stalls]` | Optional | No | Yes | Number of market produce stalls. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name_of_centre` | string | Name of the hawker centre or market. |
| `address` | string | Full address. The postal code is repeated at the end in the form `S(123456)`. |
| `postal_code` | string | Six digit postal code, as text. Keeps the leading zero. |
| `type.type_of_centre` | string | Centre type: `HC` is a hawker centre, `MHC` is a market and hawker centre, `MK` is a market. |
| `type.owner` | string | Owner of the centre, for example `Government`. |
| `stall_count.no_of_stalls` | integer | Total number of stalls. |
| `stall_count.no_of_cooked_food_stalls` | integer | Number of cooked food stalls. |
| `stall_count.no_of_mkt_produce_stalls` | integer | Number of market produce stalls. |
| `menu` | string | Comma separated list of dishes and drinks sold at the centre. |

## Example

Find a centre by part of its name.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_hawker_centres?type[type_of_centre]=*&name_of_centre=*Amoy*"
```

Response (1 row):

```json
[
  {
    "__id": 109,
    "name_of_centre": "Amoy Street Food Centre",
    "address": "National Development Building, Annex B, Telok Ayer Street, S(069111)",
    "postal_code": "069111",
    "type": {
      "type_of_centre": "HC",
      "owner": "Government"
    },
    "stall_count": {
      "no_of_stalls": 135,
      "no_of_cooked_food_stalls": 134,
      "no_of_mkt_produce_stalls": 1
    },
    "menu": "Fish Soup, Balinese Gyudon Egg Bowl, Kway Chap, Prawn Noodles, Nasi Lemak Ayam Goreng, Roast Beef Don, Sliced Fish Porridge, Fish and Prawn Soup, Braised Belly Rice, Roti Prata, Chendol With, Prawn Vadai, Pork Ribs Lamian, South Indian Meals, Yuan Chun Famous Lor Mee, Hainanese chicken rice, Iced coffee, Nasi lemak, Roti canai, Char kway teow, Wonton noodles"
  }
]
```

Another call:

Hawker centres with 100 to 200 stalls.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_hawker_centres?type[type_of_centre]=HC&stall_count[no_of_stalls]=100..200"
```

Returns 3 rows.

## Data source

List of Government Markets Hawker Centres, data.gov.sg (https://data.gov.sg), Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Contains information accessed on October 15, 2017.

[Back to all APIs](../../README.md)
