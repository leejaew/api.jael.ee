# Singapore Golf Courses

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 16](https://img.shields.io/badge/records-16-lightgrey)

The 16 golf courses on the Singapore Government list of golf courses (July 2025): name, club type, number of holes, lease end date, address, postal code, phone, Wikidata id and map coordinates. Par is filled for 3 courses only. Filter by name, club type, holes, street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_golf_courses` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 13 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the course name as on the official list, such as `*Sentosa*` or `*Country Club*`; wildcards work. Every row has a name. |
| `club_type` | Optional | No | No | Exact club type from the official list: Private club, Public course or Hybrid public and private club. |
| `holes` | Optional | No | Yes | Number of holes on the official list, such as 9, 18, 27, 36 or 45. A range works, for example 18 and above. |
| `par` | Optional | No | Yes | Course par from the course own web page; filled for 3 courses (Serapong 72, Tanah Merah Garden 71, Tampines 72) and blank for the other 13. |
| `lease_expiry` | Optional | No | No | Land lease or tenancy end date as text in the form 2040-12-31, from the official list; blank for Sembawang Country Club (military licence). |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 27 or 240; blank for Mandai Executive Golf Course. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Sime Road or Changi Coast Road. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code as text, so leading zeros stay, such as 099892. |
| `phone` | Optional | No | No | Exact phone number as text in the form +65 6275 0022; blank for 3 rows. |
| `wikidata_id` | Optional | No | No | Wikidata id of the club, such as Q2290074 for Sentosa Golf Club; filled for 4 rows (the club level id is repeated for both courses of a club). |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.243355 to 1.427279. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.732983 to 103.98433. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The course name as on the official list, such as `*Sentosa*` or `*Country Club*`. |
| `club_type` | string | Club type from the official list: Private club, Public course or Hybrid public and private club. |
| `holes` | integer | Number of holes on the official list, such as 9, 18, 27, 36 or 45. A range works, for example 18 and above. |
| `par` | string | Course par from the course own web page; filled for 3 courses (Serapong 72, Tanah Merah Garden 71, Tampines 72) and blank for the other 13. |
| `lease_expiry` | string | Land lease or tenancy end date as text in the form 2040-12-31, from the official list; blank for Sembawang Country Club (military licence). |
| `address.block` | string | The block or house number, such as 27 or 240; blank for Mandai Executive Golf Course. |
| `address.street` | string | The street name, such as Sime Road or Changi Coast Road. |
| `address.postalcode` | string | Six digit postal code as text, so leading zeros stay, such as 099892. |
| `phone` | string | Phone number as text in the form +65 6275 0022; blank for 3 rows. |
| `wikidata_id` | string | Wikidata id of the club, such as Q2290074 for Sentosa Golf Club; filled for 4 rows (the club level id is repeated for both courses of a club). |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

All 18 hole courses.

```bash
curl -s "https://api.jael.ee/JLEE/sg_golf_courses?holes=18"
```

Response (11 rows, first 2 shown):

```json
[
  {
    "__id": 2,
    "name": "Keppel Club (Sime)",
    "club_type": "Hybrid public and private club",
    "holes": 18,
    "par": null,
    "lease_expiry": "2030-12-31",
    "address": {
      "block": "239",
      "street": "Sime Road",
      "postalcode": "289685"
    },
    "phone": null,
    "wikidata_id": null,
    "location": {
      "latitude": 1.341883,
      "longitude": 103.811358
    }
  },
  {
    "__id": 5,
    "name": "National Service Resort & Country Club (Changi)",
    "club_type": "Private club",
    "holes": 18,
    "par": null,
    "lease_expiry": "2040-12-31",
    "address": {
      "block": "10",
      "street": "Changi Coast Walk",
      "postalcode": "499739"
    },
    "phone": "+65 6542 8288",
    "wikidata_id": null,
    "location": {
      "latitude": 1.319008,
      "longitude": 103.971495
    }
  }
]
```

Another call:

The two courses at Sentosa Golf Club.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_golf_courses?name=*Sentosa*"
```

Returns 2 rows.

## Data source

Course list from the Ministry of Law Singapore, List of Golf Courses in Singapore (7 July 2025), used as a short factual list with attribution. Addresses and coordinates: [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), Open Database License, OneMap (Singapore Land Authority) and Wikidata (CC0). Par values are read from club pages.

[Back to all APIs](../../README.md)
