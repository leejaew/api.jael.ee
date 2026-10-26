# Singapore Veterinary Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 40](https://img.shields.io/badge/records-40-lightgrey)

Veterinary clinics and animal hospitals in Singapore that are mapped in OpenStreetMap: 40 places, a partial list that is not the full licensed list. Fields are name, address parts, postal code, phone, opening hours text and map coordinates. Filter by name, street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_vet_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 11 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `osm_id` | Optional | No | No | Exact OpenStreetMap id as text, such as node/3306882408 or way/327193351; one per row. |
| `name` | Optional | Yes | No | Optional filter for the clinic name, such as `*Veterinary*` or `*Animal*`; wildcards work. Every row has a name. |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 109 or 564A; blank for 14 rows. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Balestier Road or Clementi Street 11; blank for 11 rows. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01; blank for 30 rows. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 225 or 17/19; blank for 29 rows. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay; blank for 14 rows. Nine were looked up from OneMap. |
| `phone` | Optional | No | No | Exact phone number as text in the form +65 6776 3450; blank for 27 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.273259 to 1.426603. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.734544 to 103.922682. |
| `opening_hours` | Disallowed | Yes | No | Optional filter for opening hours in OpenStreetMap notation, such as 24/7 or `*Mo`-`Fr*`; blank for 27 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `osm_id` | string | OpenStreetMap id as text, such as node/3306882408 or way/327193351; one per row. |
| `name` | string | The clinic name, such as `*Veterinary*` or `*Animal*`. |
| `address.block` | string | The block or house number, such as 109 or 564A; blank for 14 rows. |
| `address.street` | string | The street name, such as Balestier Road or Clementi Street 11; blank for 11 rows. |
| `address.level` | string | The floor, such as #01; blank for 30 rows. |
| `address.unit` | string | The unit number, such as 225 or 17/19; blank for 29 rows. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay; blank for 14 rows. Nine were looked up from OneMap. |
| `phone` | string | Phone number as text in the form +65 6776 3450; blank for 27 rows. |
| `opening_hours` | string | Opening hours in OpenStreetMap notation, such as 24/7 or `*Mo`-`Fr*`; blank for 27 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Vet clinics on Balestier Road.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_vet_centres?address[street]=Balestier%20Road"
```

Response (4 rows, first 2 shown):

```json
[
  {
    "__id": 14,
    "osm_id": "node/6499063741",
    "name": "Nam Sang Veterinary Clinic",
    "address": {
      "block": "2",
      "street": "Balestier Road",
      "level": "#01",
      "unit": "697",
      "postalcode": "320002"
    },
    "phone": "+65 6258 8138",
    "opening_hours": "Mo 10:00-13:00; Tu 10:00-14:00,15:00-18:00; We 10:00-14:00,15:00-17:00; Th 10:00-14:00,15:00-18:00; Fr 10:00-14:00,15:00-17:00; Sa 10:00-14:00,15:00-18:00; Su,PH off",
    "location": {
      "latitude": 1.326092,
      "longitude": 103.8436
    }
  },
  {
    "__id": 16,
    "osm_id": "node/7139419646",
    "name": "Advanced Vetcare",
    "address": {
      "block": "564A",
      "street": "Balestier Road",
      "level": null,
      "unit": null,
      "postalcode": "329880"
    },
    "phone": "+65 6565 1788",
    "opening_hours": "Mo-Su 10:00-19:00",
    "location": {
      "latitude": 1.326896,
      "longitude": 103.844311
    }
  }
]
```

Another call:

Places with Veterinary in the name.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_vet_centres?name=*Veterinary*"
```

Returns 15 rows.

## Data source

Veterinary clinics and hospitals in Singapore from OpenStreetMap (amenity=veterinary). Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright).

[Back to all APIs](../../README.md)
