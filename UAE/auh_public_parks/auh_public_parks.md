# Abu Dhabi Public Parks

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 558](https://img.shields.io/badge/records-558-lightgrey)

Public parks of the emirate of Abu Dhabi, 558 polygons from the Abu Dhabi Municipality CX_Parks layer reduced to one centre point each, with classification, region (Abu Dhabi, Al Ain or Al Dhafra), district, area in hectares, English name where OpenStreetMap supplies one, and coordinates. Filter by region, classification, district or name, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/auh_public_parks` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 9 fields. Disallowed: 4 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | Required. Allowed values: Al Ain (325); Abu Dhabi (164); Al Dhafra (69). Use * to list all rows. |
| `name` | Optional | Yes | No | Optional filter for the park name exactly as the municipality publishes it, mostly in Arabic; blank for 234 of 558 rows. |
| `name_en` | Optional | Yes | No | Optional filter for an English name taken from a matching OSM park within 80 meters, such as Recreation Park or Al Bahia Park; blank for 523 of 558 rows. |
| `classification` | Optional | No | No | Optional exact filter for the municipality area class; values are suburban (366), rural (81), urban (59), highly urban (52). |
| `address[area]` | Optional | Yes | No | Optional filter for the district from the municipality data, such as Bida Al Mutawa or Dalma Island; blank for 174 of 558 rows. |
| `address[emirate]` | Optional | No | No | Optional exact filter for the emirate; Abu Dhabi for every row; values are Abu Dhabi (558). |
| `area_hectares` | Optional | No | Yes | Optional filter for the park polygon size in hectares; values run from 0.0141 to 32.2708. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84) of the polygon centre; use a range to search a box; values run from 22.962097 to 24.845974. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84) of the polygon centre; use a range to search a box; values run from 51.749481 to 55.938524. |
| `osm_id` | Optional | No | No | Optional exact filter for the matched OSM park id, such as way/204777307 or relation/17688794; blank for 521 of 558 rows. |
| `fee_required` | Disallowed | No | No | The entry fee flag where OSM supplies it (yes or no); values are no (1); blank for 557 of 558 rows. |
| `entry_fee` | Disallowed | No | No | The fee text; empty for every row. |
| `opening_hours` | Disallowed | No | No | Opening hours text where OSM supplies it, such as Mo-Su 06:00-00:00; blank for 557 of 558 rows. |
| `source_id` | Disallowed | No | No | The municipality feature id, such as DMA_11_3_2025_7877#1996#1 or DMA_11_3_2025_7877#1996#2. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | The park name exactly as the municipality publishes it, mostly in Arabic; blank for 234 of 558 rows. |
| `name_en` | string | An English name taken from a matching OSM park within 80 meters, such as Recreation Park or Al Bahia Park; blank for 523 of 558 rows. |
| `classification` | string | The municipality area class; values are suburban (366), rural (81), urban (59), highly urban (52). |
| `region` | string | The municipality region; values are Al Ain (325), Abu Dhabi (164), Al Dhafra (69). |
| `address.area` | string | The district from the municipality data, such as Bida Al Mutawa or Dalma Island; blank for 174 of 558 rows. |
| `address.emirate` | string | The emirate; Abu Dhabi for every row; values are Abu Dhabi (558). |
| `area_hectares` | number | The park polygon size in hectares; values run from 0.0141 to 32.2708. |
| `fee_required` | string | The entry fee flag where OSM supplies it (yes or no); values are no (1); blank for 557 of 558 rows. |
| `entry_fee` | string | The fee text; empty for every row. |
| `opening_hours` | string | Opening hours text where OSM supplies it, such as Mo-Su 06:00-00:00; blank for 557 of 558 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84) of the polygon centre. |
| `location.longitude` | number | Longitude in decimal degrees (WGS84) of the polygon centre. |
| `source_id` | string | The municipality feature id, such as DMA_11_3_2025_7877#1996#1 or DMA_11_3_2025_7877#1996#2. |
| `osm_id` | string | The matched OSM park id, such as way/204777307 or relation/17688794; blank for 521 of 558 rows. |

## Example

Rural parks in the Al Dhafra region.

```bash
curl -s "https://api.jael.ee/JLEE/auh_public_parks?region=Al%20Dhafra&classification=rural"
```

Response (19 rows, first 2 shown):

```json
[
  {
    "name": "حديقة / بدع المطاوعة 1",
    "name_en": null,
    "classification": "rural",
    "region": "Al Dhafra",
    "address": {
      "area": "Bida Al Mutawa",
      "emirate": "Abu Dhabi"
    },
    "area_hectares": 0.2533,
    "fee_required": null,
    "entry_fee": null,
    "opening_hours": null,
    "location": {
      "latitude": 23.7772,
      "longitude": 52.573341
    },
    "source_id": "DMA_11_3_2025_7877#1996#1",
    "osm_id": null
  },
  {
    "name": "حديقة حمرور",
    "name_en": null,
    "classification": "rural",
    "region": "Al Dhafra",
    "address": {
      "area": "Liwa",
      "emirate": "Abu Dhabi"
    },
    "area_hectares": 0.036,
    "fee_required": null,
    "entry_fee": null,
    "opening_hours": null,
    "location": {
      "latitude": 23.116448,
      "longitude": 53.609044
    },
    "source_id": "DMA_11_3_2025_7877#1996#3",
    "osm_id": null
  }
]
```

Another call:

Mushrif Park by its English name.

```bash
curl -s "https://api.jael.ee/JLEE/auh_public_parks?region=*&name_en=*Mushrif*"
```

Returns 1 row.

## Data source

Abu Dhabi SDI OpenData CX_Parks layer (Department of Municipalities and Transport), with the licence not stated on the layer, so reuse terms are unclear. English names come from OpenStreetMap, © OpenStreetMap contributors, Open Database License (https://www.openstreetmap.org/copyright). Names are mostly Arabic, 234 are blank in the source, and park polygons are reduced to centre points.

[Back to all APIs](../../README.md)
