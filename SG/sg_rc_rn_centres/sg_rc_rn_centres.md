# Singapore RC and RN Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 714](https://img.shields.io/badge/records-714-lightgrey)

Residents Committee (RC) and Residents Network (RN) centres in Singapore run by the People's Association, 714 rows with name, tag (rc or rn), address parts, postal code and coordinates. Filter by tag, name, street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_rc_rn_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 11 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the centre name, such as Eunos Zone 3 RN; wildcards work. |
| `tag` | Optional | No | No | Optional exact filter for the centre kind: rn (Residents Network, 655 rows) or rc (Residents Committee, 59 rows). |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 80 or 259C; text, so wildcards work. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Punggol Field; wildcards work. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01 or #B1; blank when the source has none. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 47; blank when the source has none. |
| `address[building]` | Optional | Yes | No | Optional filter for the building name; blank for every row in this dataset. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.272 to 1.456. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6897 to 103.9625. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The centre name, such as Eunos Zone 3 RN. |
| `tag` | string | The centre kind: rn (Residents Network, 655 rows) or rc (Residents Committee, 59 rows). |
| `address.block` | string | The block or house number, such as 80 or 259C; text, so wildcards work. |
| `address.street` | string | The street name, such as Punggol Field. |
| `address.level` | string | The floor, such as #01 or #B1; blank when the source has none. |
| `address.unit` | string | The unit number, such as 47; blank when the source has none. |
| `address.building` | string | The building name; blank for every row in this dataset. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Residents Committee centres on streets ending in Rd.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_rc_rn_centres?tag=rc&address[street]=*Rd"
```

Response (20 rows, first 2 shown):

```json
[
  {
    "__id": 24,
    "name": "Bukit Panjang Zone 1 RC",
    "tag": "rc",
    "address": {
      "block": "107",
      "street": "Gangsa Rd",
      "level": "#01",
      "unit": "187",
      "building": null,
      "postalcode": "670107"
    },
    "location": {
      "latitude": 1.377985,
      "longitude": 103.766715
    }
  },
  {
    "__id": 25,
    "name": "Bukit Panjang Zone 10 RC",
    "tag": "rc",
    "address": {
      "block": "441",
      "street": "Fajar Rd",
      "level": "#01",
      "unit": "458",
      "building": null,
      "postalcode": "670441"
    },
    "location": {
      "latitude": 1.383284,
      "longitude": 103.770742
    }
  }
]
```

Another call:

Residents Network centres in the Eunos zones.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_rc_rn_centres?name=Eunos*&tag=rn"
```

Returns 10 rows.

## Data source

Residents Committee and Residents Network Centres, People's Association, data.gov.sg (https://data.gov.sg), dataset d_9ae25d6b3fefdd15983c4e46ecc7fcbd, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated September 30, 2026. The tag is taken from the last word of the centre name, and the floor comes from the source floor number.

[Back to all APIs](../../README.md)
