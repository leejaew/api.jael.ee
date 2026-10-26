# Dubai Shopping Malls

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 106](https://img.shields.io/badge/records-106-lightgrey)

Shopping malls, shopping centres and souks in the emirate of Dubai, 106 rows from OpenStreetMap with a few Wikipedia figures, with Arabic name, address text, opening year, store count, gross leasable area, phone, website and coordinates. Filter by name or area, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/dxb_shopping_malls` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 11 fields. Disallowed: 5 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the mall name, such as The Dubai Mall or Ibn Battuta Mall. |
| `name_ar` | Optional | Yes | No | Optional filter for the Arabic name, such as البرشاء مول or مركز الدانة; blank for 65 of 106 rows. |
| `address[street]` | Optional | Yes | No | Optional filter for the street from OSM address tags, such as 8 Street or Damascus Street; blank for 77 of 106 rows. |
| `address[area]` | Optional | Yes | No | Optional filter for the neighbourhood, derived by reverse geocoding the point, such as Al Mankhool or Al Karama. |
| `address[city]` | Optional | Yes | No | Optional filter for the city; Dubai for every row; values are Dubai (106). |
| `address[emirate]` | Optional | No | No | Optional exact filter for the emirate; Dubai for every row; values are Dubai (106). |
| `phone` | Optional | No | No | Optional exact filter for the phone number as published, kept as text, such as 044557151 or +971 4 329 8000; blank for 90 of 106 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box; values run from 24.912673 to 25.291991. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box; values run from 55.010897 to 55.473662. |
| `osm_id` | Optional | No | No | Optional exact filter for the OSM object id, such as way/110242011 or way/242009759. |
| `opened_year` | Disallowed | No | No | The opening year taken from OSM start_date or Wikipedia; values run from 1991 to 2022; blank for 97 of 106 rows. |
| `number_of_stores` | Disallowed | No | No | The number of stores as stated by Wikipedia or OSM; values run from 20 to 1200; blank for 96 of 106 rows. |
| `gross_leasable_area_sqm` | Disallowed | No | No | Gross leasable area in square meters as stated by Wikipedia; values run from 3716 to 350000; blank for 99 of 106 rows. |
| `source` | Disallowed | No | No | Which sources fed the row; values are OpenStreetMap (93), OpenStreetMap; Wikipedia (13). |
| `website` | Disallowed | No | No | Mall website link; returned in results, not used as a filter, such as https://www.albarshamall.ae/ or https://www.arabiancenter.com/; blank for 74 of 106 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The mall name, such as The Dubai Mall or Ibn Battuta Mall. |
| `name_ar` | string | The Arabic name, such as البرشاء مول or مركز الدانة; blank for 65 of 106 rows. |
| `address.street` | string | The street from OSM address tags, such as 8 Street or Damascus Street; blank for 77 of 106 rows. |
| `address.area` | string | The neighbourhood, derived by reverse geocoding the point, such as Al Mankhool or Al Karama. |
| `address.city` | string | The city; Dubai for every row; values are Dubai (106). |
| `address.emirate` | string | The emirate; Dubai for every row; values are Dubai (106). |
| `opened_year` | string | The opening year taken from OSM start_date or Wikipedia; values run from 1991 to 2022; blank for 97 of 106 rows. |
| `number_of_stores` | string | The number of stores as stated by Wikipedia or OSM; values run from 20 to 1200; blank for 96 of 106 rows. |
| `gross_leasable_area_sqm` | string | Gross leasable area in square meters as stated by Wikipedia; values run from 3716 to 350000; blank for 99 of 106 rows. |
| `phone` | string | The phone number as published, kept as text, such as 044557151 or +971 4 329 8000; blank for 90 of 106 rows. |
| `website` | string | Mall website link; returned in results, not used as a filter, such as https://www.albarshamall.ae/ or https://www.arabiancenter.com/; blank for 74 of 106 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `source` | string | Which sources fed the row; values are OpenStreetMap (93), OpenStreetMap; Wikipedia (13). |
| `osm_id` | string | The OSM object id, such as way/110242011 or way/242009759. |

## Example

Mall names containing City Centre.

```bash
curl -s "https://api.jael.ee/JLEE/dxb_shopping_malls?name=*City%20Centre*"
```

Response (4 rows, first 2 shown):

```json
[
  {
    "__id": 37,
    "name": "City Centre Al Barsha",
    "name_ar": null,
    "address": {
      "street": null,
      "area": "Al Barsha South",
      "city": "Dubai",
      "emirate": "Dubai"
    },
    "opened_year": null,
    "number_of_stores": null,
    "gross_leasable_area_sqm": null,
    "phone": null,
    "website": null,
    "location": {
      "latitude": 25.069356,
      "longitude": 55.241961
    },
    "source": "OpenStreetMap",
    "osm_id": "way/767770594"
  },
  {
    "__id": 38,
    "name": "City Centre Al Shindagha",
    "name_ar": null,
    "address": {
      "street": null,
      "area": "Al Souk Al Kabeer",
      "city": "Dubai",
      "emirate": "Dubai"
    },
    "opened_year": null,
    "number_of_stores": null,
    "gross_leasable_area_sqm": null,
    "phone": "+971 800 226255",
    "website": "https://www.citycentreshindagha.com/",
    "location": {
      "latitude": 25.264003,
      "longitude": 55.287063
    },
    "source": "OpenStreetMap",
    "osm_id": "way/246355080"
  }
]
```

Another call:

Malls whose derived area is in Mirdif.

```bash
curl -sg "https://api.jael.ee/JLEE/dxb_shopping_malls?address[area]=*Mirdif*"
```

Returns 7 rows.

## Data source

Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright). Neighborhood names come from OpenStreetMap Nominatim reverse lookups, so they are approximate. Extracted 2026-10-27. Opening year, store count and leasable area for 13 malls come from English Wikipedia (CC BY-SA 4.0).

[Back to all APIs](../../README.md)
