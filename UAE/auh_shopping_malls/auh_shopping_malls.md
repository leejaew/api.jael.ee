# Abu Dhabi Shopping Malls

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 43](https://img.shields.io/badge/records-43-lightgrey)

Shopping malls and shopping centres in the emirate of Abu Dhabi, 43 rows from OpenStreetMap and the Abu Dhabi SDI attractions layer, with region (Abu Dhabi, Al Ain or Al Dhafra), Arabic name, address text, store count, phone, website and coordinates. Filter by name, region, area or city, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/auh_shopping_malls` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 16 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the mall name, such as Yas Mall or Marina Mall. |
| `name_ar` | Optional | Yes | No | Optional filter for the Arabic name, such as أبو ظبي مول or الفوعة مول; blank for 18 of 43 rows. |
| `region` | Optional | No | No | Optional exact filter for the region of the emirate; values are Abu Dhabi (31), Al Ain (10), Al Dhafra (2). |
| `address[street]` | Optional | Yes | No | Optional filter for the street from OSM address tags, such as شارع حمد البادي الظاهري or شارع الأَزْيَاء; blank for 38 of 43 rows. |
| `address[area]` | Optional | Yes | No | Optional filter for the district; from the SDI layer where it has one, otherwise from reverse geocoding, such as Al Zahiyah or Al Bateen; blank for 8 of 43 rows. |
| `address[city]` | Optional | Yes | No | Optional filter for the city; Abu Dhabi or Al Ain for most rows, and the town name for Al Dhafra; values are Abu Dhabi (31), Al Ain (10), Zayed City (1), Delma (1). |
| `address[emirate]` | Optional | No | No | Optional exact filter for the emirate; Abu Dhabi for every row; values are Abu Dhabi (43). |
| `opened_year` | Optional | No | Yes | Optional filter for the opening year taken from OSM or Wikipedia; values run from 2014 to 2014; blank for 42 of 43 rows. |
| `number_of_stores` | Optional | No | Yes | Optional filter for the number of stores as stated by Wikipedia or OSM; values run from 20 to 400; blank for 35 of 43 rows. |
| `gross_leasable_area_sqm` | Optional | No | Yes | Optional filter for gross leasable area in square meters; no source gave it, so it is empty; blank for every row. |
| `phone` | Optional | No | No | Optional exact filter for the phone number as published, kept as text, such as +971 2 6454858 or +971 600 565464; blank for 37 of 43 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box; values run from 23.659608 to 24.523702. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box; values run from 52.307753 to 55.807612. |
| `source` | Optional | Yes | No | Optional filter for which sources fed the row; values are OpenStreetMap (24), OpenStreetMap; Abu Dhabi SDI (10), OpenStreetMap; Abu Dhabi SDI; Wikipedia (7), Abu Dhabi SDI (1). |
| `osm_id` | Optional | No | No | Optional exact filter for the OSM object id, such as way/150781737 or way/102807888; blank for 1 of 43 rows. |
| `website` | Disallowed | No | No | Mall website link; returned in results, not used as a filter, such as https://www.abudhabi-mall.com/ or https://www.bawabatalsharqmall.ae/; blank for 38 of 43 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The mall name, such as Yas Mall or Marina Mall. |
| `name_ar` | string | The Arabic name, such as أبو ظبي مول or الفوعة مول; blank for 18 of 43 rows. |
| `region` | string | The region of the emirate; values are Abu Dhabi (31), Al Ain (10), Al Dhafra (2). |
| `address.street` | string | The street from OSM address tags, such as شارع حمد البادي الظاهري or شارع الأَزْيَاء; blank for 38 of 43 rows. |
| `address.area` | string | The district; from the SDI layer where it has one, otherwise from reverse geocoding, such as Al Zahiyah or Al Bateen; blank for 8 of 43 rows. |
| `address.city` | string | The city; Abu Dhabi or Al Ain for most rows, and the town name for Al Dhafra; values are Abu Dhabi (31), Al Ain (10), Zayed City (1), Delma (1). |
| `address.emirate` | string | The emirate; Abu Dhabi for every row; values are Abu Dhabi (43). |
| `opened_year` | string | The opening year taken from OSM or Wikipedia; values run from 2014 to 2014; blank for 42 of 43 rows. |
| `number_of_stores` | integer | The number of stores as stated by Wikipedia or OSM; values run from 20 to 400; blank for 35 of 43 rows. |
| `gross_leasable_area_sqm` | string | Gross leasable area in square meters; no source gave it, so it is empty; blank for every row. |
| `phone` | string | The phone number as published, kept as text, such as +971 2 6454858 or +971 600 565464; blank for 37 of 43 rows. |
| `website` | string | Mall website link; returned in results, not used as a filter, such as https://www.abudhabi-mall.com/ or https://www.bawabatalsharqmall.ae/; blank for 38 of 43 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `source` | string | Which sources fed the row; values are OpenStreetMap (24), OpenStreetMap; Abu Dhabi SDI (10), OpenStreetMap; Abu Dhabi SDI; Wikipedia (7), Abu Dhabi SDI (1). |
| `osm_id` | string | The OSM object id, such as way/150781737 or way/102807888; blank for 1 of 43 rows. |

## Example

The 10 malls in the Al Ain region.

```bash
curl -s "https://api.jael.ee/JLEE/auh_shopping_malls?region=Al%20Ain"
```

Response (10 rows, first 2 shown):

```json
[
  {
    "__id": 2,
    "name": "Al Ain City Centre",
    "name_ar": null,
    "region": "Al Ain",
    "address": {
      "street": "شارع حمد البادي الظاهري",
      "area": null,
      "city": "Al Ain",
      "emirate": "Abu Dhabi"
    },
    "opened_year": null,
    "number_of_stores": null,
    "gross_leasable_area_sqm": null,
    "phone": null,
    "website": null,
    "location": {
      "latitude": 24.204694,
      "longitude": 55.793109
    },
    "source": "OpenStreetMap",
    "osm_id": "way/102807888"
  },
  {
    "__id": 4,
    "name": "Al Foah Mall",
    "name_ar": "الفوعة مول",
    "region": "Al Ain",
    "address": {
      "street": null,
      "area": "Al Foah",
      "city": "Al Ain",
      "emirate": "Abu Dhabi"
    },
    "opened_year": null,
    "number_of_stores": null,
    "gross_leasable_area_sqm": null,
    "phone": null,
    "website": null,
    "location": {
      "latitude": 24.344,
      "longitude": 55.792631
    },
    "source": "Abu Dhabi SDI",
    "osm_id": null
  }
]
```

Another call:

Yas Mall by name.

```bash
curl -sg "https://api.jael.ee/JLEE/auh_shopping_malls?name=*Yas*"
```

Returns 1 row.

## Data source

Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright). Neighborhood names come from OpenStreetMap Nominatim reverse lookups, so they are approximate. Extracted 2026-10-27. Ten malls also match the Abu Dhabi SDI attractions layer (© AD-SDI, DCT, DED), and the source column shows which sources fed each row. Region for some rows is estimated from the nearest park polygon.

[Back to all APIs](../../README.md)
