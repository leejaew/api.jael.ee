# Malaysia National and State Parks

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 65](https://img.shields.io/badge/records-65-lightgrey)

A list of 65 national, state and marine parks in Malaysia: all 47 Sarawak national parks plus 9 Sabah Parks parks, 4 Johor national parks, the three Taman Negara state parts, Penang National Park and Royal Belum State Park. Each row has the operator, first gazette year or date, published area in hectares and, for 38 parks, coordinates. Filter by state, operator, park type, year or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/my_national_parks` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 9 fields. Disallowed: 5 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | Required. Allowed values: Sarawak (47); Sabah (9); Johor (4); Pahang (1); Kelantan (1); Terengganu (1); Penang (1); Perak (1). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the park name, such as Gunung Mulu National Park, Kinabalu Park or Taman Negara Pahang. |
| `operator` | Optional | Yes | No | Optional filter for the managing body: Sarawak Forestry Corporation (47 rows), Sabah Parks (9), Johor National Parks Corporation (4), PERHILITAN (4) or Perak State Parks Corporation (1). |
| `park_type` | Optional | No | No | Optional exact filter for the park type: National Park (55 rows), Park (6), Marine Park (3) or State Park (1). |
| `year_gazetted` | Optional | No | Yes | Optional filter for the year first gazetted, a number from 1938 to 2021; Sarawak years are the first gazette even where the park was later extended. |
| `gazette_date` | Optional | No | No | Optional exact filter for the first gazette date as text in YYYY-MM-DD form, such as 1974-08-01; blank for 16 rows (Sabah, Johor and Peninsular parks where only the year is known). |
| `area_ha` | Optional | No | Yes | Optional filter for the published area in hectares, a number from 325 to 1011772; blank for 2 rows (Sipadan Island Park and Tun Sakaran Marine Park); includes sea area where noted. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.0458 to 7.25; blank for 27 rows. Points are centres of the mapped area, not entrances. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 100.1977 to 118.7334; blank for 27 rows. |
| `marine_area_ha` | Disallowed | No | No | The part of the area that is sea, in hectares, from 627 to 1011772; filled for only 8 Sarawak rows, blank elsewhere. |
| `area_note` | Disallowed | No | No | The a short note about the area or year, such as extended 2 times since first gazette; blank for 43 rows. |
| `coordinate_source` | Disallowed | No | No | Where the coordinates came from: OpenStreetMap (27 rows), Wikidata (11) or blank (27 rows with no coordinates). |
| `coordinate_ref` | Disallowed | No | No | The map record behind the coordinates, such as r14725880 (OpenStreetMap relation, way or node) or Q834893 (Wikidata item); blank for 27 rows. |
| `data_source` | Disallowed | No | No | The source of the area and year, such as Sabah Parks park page or Sarawak Forestry Corporation totally protected areas list. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The park name, such as Gunung Mulu National Park, Kinabalu Park or Taman Negara Pahang. |
| `state` | string | Required. Allowed values: Sarawak (47); Sabah (9); Johor (4); Pahang (1); Kelantan (1); Terengganu (1); Penang (1); Perak (1). Use * to list all rows. |
| `operator` | string | The managing body: Sarawak Forestry Corporation (47 rows), Sabah Parks (9), Johor National Parks Corporation (4), PERHILITAN (4) or Perak State Parks Corporation (1). |
| `park_type` | string | The park type: National Park (55 rows), Park (6), Marine Park (3) or State Park (1). |
| `year_gazetted` | integer | The year first gazetted, a number from 1938 to 2021; Sarawak years are the first gazette even where the park was later extended. |
| `gazette_date` | string | The first gazette date as text in YYYY-MM-DD form, such as 1974-08-01; blank for 16 rows (Sabah, Johor and Peninsular parks where only the year is known). |
| `area_ha` | number | The published area in hectares, a number from 325 to 1011772; blank for 2 rows (Sipadan Island Park and Tun Sakaran Marine Park); includes sea area where noted. |
| `marine_area_ha` | string | The part of the area that is sea, in hectares, from 627 to 1011772; filled for only 8 Sarawak rows, blank elsewhere. |
| `area_note` | string | A short note about the area or year, such as extended 2 times since first gazette; blank for 43 rows. |
| `coordinate_source` | string | Where the coordinates came from: OpenStreetMap (27 rows), Wikidata (11) or blank (27 rows with no coordinates). |
| `coordinate_ref` | string | The map record behind the coordinates, such as r14725880 (OpenStreetMap relation, way or node) or Q834893 (Wikidata item); blank for 27 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `data_source` | string | The source of the area and year, such as Sabah Parks park page or Sarawak Forestry Corporation totally protected areas list. |

## Example

Sabah Parks parks listed as Park.

```bash
curl -s "https://api.jael.ee/JLEE/my_national_parks?state=Sabah&park_type=Park"
```

Response (6 rows, first 2 shown):

```json
[
  {
    "__id": 48,
    "name": "Kinabalu Park",
    "state": "Sabah",
    "operator": "Sabah Parks",
    "park_type": "Park",
    "year_gazetted": 1964,
    "gazette_date": null,
    "area_ha": 75400.0,
    "marine_area_ha": null,
    "area_note": null,
    "coordinate_source": "Wikidata",
    "coordinate_ref": "Q834893",
    "location": {
      "latitude": 6.15,
      "longitude": 116.65
    },
    "data_source": "Sabah Parks park page"
  },
  {
    "__id": 49,
    "name": "Tunku Abdul Rahman Park",
    "state": "Sabah",
    "operator": "Sabah Parks",
    "park_type": "Park",
    "year_gazetted": 1974,
    "gazette_date": null,
    "area_ha": 5000.0,
    "marine_area_ha": null,
    "area_note": null,
    "coordinate_source": "Wikidata",
    "coordinate_ref": "Q1097024",
    "location": {
      "latitude": 5.96667,
      "longitude": 116.0
    },
    "data_source": "Sabah Parks park page"
  }
]
```

Another call:

Sarawak national parks that were extended after first gazette.

```bash
curl -sg "https://api.jael.ee/JLEE/my_national_parks?state=*&operator=*Forestry*"
```

Returns 47 rows.

## Data source

Park names, gazette years and areas are facts from the Sarawak Forestry Corporation, Sabah Parks, Johor National Parks Corporation and other official park pages (no text or images copied). Coordinates: [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), Open Database License (27 rows) and Wikidata, CC0 (11 rows).

[Back to all APIs](../../README.md)
