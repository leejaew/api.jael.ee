# UAE Museums

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 82](https://img.shields.io/badge/records-82-lightgrey)

Museums across the United Arab Emirates, 82 rows from OpenStreetMap and Wikidata, with emirate, city, area, theme, opening year, website, ticket price where tagged and coordinates. Coverage is uneven and mostly Dubai, Sharjah and Abu Dhabi. Filter by name, emirate, city or museum_type, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uae_museums` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 17 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the museum name, such as Louvre Museum Abu Dhabi or Dubai Museum. |
| `name_ar` | Optional | Yes | No | Optional filter for the Arabic name, such as متحف أحمد بن ماجد or متحف عجمان; blank for 51 of 82 rows. |
| `museum_type` | Optional | Yes | No | Optional filter for the theme or type from OSM or Wikidata; values are history (5), art museum (4), archaeological museum (2), national museum (2); blank for 61 of 82 rows. |
| `address[street]` | Optional | Yes | No | Optional filter for the street from OSM address tags, such as شارع حصة بنت محمد or Maraya Art Centre, Al Qasba; blank for 73 of 82 rows. |
| `address[area]` | Optional | Yes | No | Optional filter for the neighbourhood, derived by reverse geocoding the point, such as Al Mairid or Al Bustan; blank for 12 of 82 rows. |
| `address[city]` | Optional | Yes | No | Optional filter for the city; values are Dubai (29), Sharjah (20), Abu Dhabi (12), Al Ain (6); blank for 1 of 82 rows. |
| `address[emirate]` | Optional | No | No | Optional exact filter for the emirate; values are Dubai (30), Sharjah (23), Abu Dhabi (19), Ras Al Khaimah (4). |
| `opened_year` | Optional | No | Yes | Optional filter for the opening year from OSM start_date or Wikidata; values run from 1937 to 2025; blank for 65 of 82 rows. |
| `fee_required` | Optional | No | No | Optional exact filter for the OSM fee flag (yes or no); values are yes (10), no (5); blank for 67 of 82 rows. |
| `ticket_price` | Optional | Yes | No | Optional filter for the ticket price text where tagged, such as 10 AED or 70 AED; blank for 80 of 82 rows. |
| `opening_hours` | Optional | Yes | No | Optional filter for the OSM opening hours text, such as Tu-Th, Sa-Su 08:30-19:30; Fr 15:00-19:30 or Mo-Su closed "Temporary closed"; blank for 63 of 82 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box; values run from 24.088187 to 25.808247. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box; values run from 52.30455 to 56.361431. |
| `source` | Optional | Yes | No | Optional filter for which sources fed the row; values are OpenStreetMap (45), OpenStreetMap; Wikidata (28), Wikidata (9). |
| `wikidata_id` | Optional | No | No | Optional exact filter for the Wikidata item id, such as Q12238661 or Q25036006; blank for 45 of 82 rows. |
| `osm_id` | Optional | No | No | Optional exact filter for the OSM object id, such as way/338757954 or way/239711162; blank for 9 of 82 rows. |
| `website` | Disallowed | No | No | Museum website link; returned in results, not used as a filter, such as https://etihadmuseum.dubaiculture.ae/ or https://www.louvreabudhabi.ae/; blank for 71 of 82 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The museum name, such as Louvre Museum Abu Dhabi or Dubai Museum. |
| `name_ar` | string | The Arabic name, such as متحف أحمد بن ماجد or متحف عجمان; blank for 51 of 82 rows. |
| `museum_type` | string | The theme or type from OSM or Wikidata; values are history (5), art museum (4), archaeological museum (2), national museum (2); blank for 61 of 82 rows. |
| `address.street` | string | The street from OSM address tags, such as شارع حصة بنت محمد or Maraya Art Centre, Al Qasba; blank for 73 of 82 rows. |
| `address.area` | string | The neighbourhood, derived by reverse geocoding the point, such as Al Mairid or Al Bustan; blank for 12 of 82 rows. |
| `address.city` | string | The city; values are Dubai (29), Sharjah (20), Abu Dhabi (12), Al Ain (6); blank for 1 of 82 rows. |
| `address.emirate` | string | The emirate; values are Dubai (30), Sharjah (23), Abu Dhabi (19), Ras Al Khaimah (4). |
| `opened_year` | string | The opening year from OSM start_date or Wikidata; values run from 1937 to 2025; blank for 65 of 82 rows. |
| `website` | string | Museum website link; returned in results, not used as a filter, such as https://etihadmuseum.dubaiculture.ae/ or https://www.louvreabudhabi.ae/; blank for 71 of 82 rows. |
| `fee_required` | string | The OSM fee flag (yes or no); values are yes (10), no (5); blank for 67 of 82 rows. |
| `ticket_price` | string | The ticket price text where tagged, such as 10 AED or 70 AED; blank for 80 of 82 rows. |
| `opening_hours` | string | The OSM opening hours text, such as Tu-Th, Sa-Su 08:30-19:30; Fr 15:00-19:30 or Mo-Su closed "Temporary closed"; blank for 63 of 82 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `source` | string | Which sources fed the row; values are OpenStreetMap (45), OpenStreetMap; Wikidata (28), Wikidata (9). |
| `wikidata_id` | string | The Wikidata item id, such as Q12238661 or Q25036006; blank for 45 of 82 rows. |
| `osm_id` | string | The OSM object id, such as way/338757954 or way/239711162; blank for 9 of 82 rows. |

## Example

The 23 museums in Sharjah.

```bash
curl -s "https://api.jael.ee/JLEE/uae_museums?address%5Bemirate%5D=Sharjah"
```

Response (23 rows, first 2 shown):

```json
[
  {
    "__id": 6,
    "name": "Al Eslah School Museum",
    "name_ar": "متحف مدرسة الإصلاح",
    "museum_type": null,
    "address": {
      "street": null,
      "area": "Al Gharb",
      "city": "Sharjah",
      "emirate": "Sharjah"
    },
    "opened_year": null,
    "website": null,
    "fee_required": null,
    "ticket_price": null,
    "opening_hours": null,
    "location": {
      "latitude": 25.358367,
      "longitude": 55.383795
    },
    "source": "OpenStreetMap; Wikidata",
    "wikidata_id": "Q115959765",
    "osm_id": "relation/19556120"
  },
  {
    "__id": 7,
    "name": "Al Mahatta Museum",
    "name_ar": "اشاره الشقه",
    "museum_type": null,
    "address": {
      "street": null,
      "area": "Al Qasimia",
      "city": "Sharjah",
      "emirate": "Sharjah"
    },
    "opened_year": null,
    "website": null,
    "fee_required": null,
    "ticket_price": null,
    "opening_hours": "Mo-Su closed \"Temporary closed\"",
    "location": {
      "latitude": 25.345952,
      "longitude": 55.395927
    },
    "source": "OpenStreetMap",
    "wikidata_id": null,
    "osm_id": "node/1467815877"
  }
]
```

Another call:

Louvre Museum Abu Dhabi by name.

```bash
curl -sg "https://api.jael.ee/JLEE/uae_museums?name=*Louvre*"
```

Returns 1 row.

## Data source

Data © OpenStreetMap contributors, available under the Open Database License (https://www.openstreetmap.org/copyright), merged with Wikidata (CC0) for opening year and type. Areas come from Nominatim reverse lookups and are approximate. The source column on each row shows which sources it used.

[Back to all APIs](../../README.md)
