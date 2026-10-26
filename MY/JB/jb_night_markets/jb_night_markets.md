# Johor Bahru Night Markets

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 29](https://img.shields.io/badge/records-29-lightgrey)

The 29 pasar malam night markets listed by the Johor Bahru City Council (MBJB), one row per market, with weekday, number of stalls, area, street where named, the legal operating window and approximate coordinates. Filter by day_of_week, name, area or stalls, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/jb_night_markets` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 14 fields. Disallowed: 2 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the market location as listed by MBJB, such as Larkin, Taman Daya or Jalan Permas 1/2, Permas Jaya. Some names repeat an area on different days. |
| `day_of_week` | Optional | No | No | Optional exact filter for the weekday in English: Monday (5 rows), Tuesday (5), Wednesday (6), Friday (5), Saturday (4) or Sunday (4). There is no Thursday market on the MBJB list. |
| `day_of_week_ms` | Optional | No | No | Optional exact filter for the weekday as MBJB writes it in Malay: Isnin, Selasa, Rabu, Jumaat, Sabtu or Ahad. |
| `stalls` | Optional | No | Yes | Optional filter for the number of stalls (jumlah petak) listed for the market, from 60 to 320, such as 265 for Larkin. All 29 rows have a value. |
| `area` | Optional | Yes | No | Optional filter for the housing area or village, such as Permas Jaya, Taman Rinting, Kampung Setanggi or Larkin. For Jalan Dataran Kempas the area is Kempas. |
| `address_text` | Optional | Yes | No | Optional filter for the location text as printed on the MBJB page, such as Jalan Jelatang, Taman Megah Ria. The page gives no house numbers or postal codes. |
| `address[street]` | Optional | Yes | No | Optional filter for the road named in the location text, such as Jalan Perang or Jalan Nibong 17. Blank for 21 rows where only an area is listed. |
| `address[city]` | Optional | No | No | Optional exact filter for the city. It is Johor Bahru for all 29 rows, since MBJB lists markets inside its own area. |
| `address[state]` | Optional | No | No | Optional exact filter for the state. It is Johor for all 29 rows. |
| `address[postalcode]` | Optional | No | No | Optional exact filter for the postal code taken from the OpenStreetMap match, such as 81200 or 80150. Blank for 11 rows where there was no safe match. |
| `coordinate_precision` | Optional | No | No | Optional exact filter for how good the coordinates are: road (6 rows, matched to the named road), area (16 rows, a point in the named neighborhood or village), area approximate (4 rows, one of several possible roads or places) or blank (3 rows with no coordinates). |
| `location[latitude]` | Optional | No | Yes | Optional latitude range filter in decimal degrees (WGS84), from 1.4807 to 1.5588. Blank for 3 rows. These are approximate points, not the exact market ground. |
| `location[longitude]` | Optional | No | Yes | Optional longitude range filter in decimal degrees (WGS84), from 103.6860 to 103.8691. Blank for 3 rows. These are approximate points, not the exact market ground. |
| `permitted_start_time` | Disallowed | No | No | The earliest legal opening time, 24 hour text. It is 16:00 for all 29 rows because the 2003 MBJB night market by-law defines a night market as 4.00 pm to midnight. Real opening times may differ. |
| `permitted_end_time` | Disallowed | No | No | The latest legal closing time, 24 hour text. It is 00:00 (midnight) for all 29 rows, taken from the same by-law. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The market location as listed by MBJB, such as Larkin, Taman Daya or Jalan Permas 1/2, Permas Jaya. Some names repeat an area on different days. |
| `day_of_week` | string | The weekday in English: Monday (5 rows), Tuesday (5), Wednesday (6), Friday (5), Saturday (4) or Sunday (4). There is no Thursday market on the MBJB list. |
| `day_of_week_ms` | string | The weekday as MBJB writes it in Malay: Isnin, Selasa, Rabu, Jumaat, Sabtu or Ahad. |
| `permitted_start_time` | string | The earliest legal opening time, 24 hour text. It is 16:00 for all 29 rows because the 2003 MBJB night market by-law defines a night market as 4.00 pm to midnight. Real opening times may differ. |
| `permitted_end_time` | string | The latest legal closing time, 24 hour text. It is 00:00 (midnight) for all 29 rows, taken from the same by-law. |
| `stalls` | integer | The number of stalls (jumlah petak) listed for the market, from 60 to 320, such as 265 for Larkin. All 29 rows have a value. |
| `area` | string | The housing area or village, such as Permas Jaya, Taman Rinting, Kampung Setanggi or Larkin. For Jalan Dataran Kempas the area is Kempas. |
| `address_text` | string | The location text as printed on the MBJB page, such as Jalan Jelatang, Taman Megah Ria. The page gives no house numbers or postal codes. |
| `address.street` | string | The road named in the location text, such as Jalan Perang or Jalan Nibong 17. Blank for 21 rows where only an area is listed. |
| `address.city` | string | The city. It is Johor Bahru for all 29 rows, since MBJB lists markets inside its own area. |
| `address.state` | string | The state. It is Johor for all 29 rows. |
| `address.postalcode` | string | The postal code taken from the OpenStreetMap match, such as 81200 or 80150. Blank for 11 rows where there was no safe match. |
| `coordinate_precision` | string | How good the coordinates are: road (6 rows, matched to the named road), area (16 rows, a point in the named neighborhood or village), area approximate (4 rows, one of several possible roads or places) or blank (3 rows with no coordinates). |
| `location.latitude` | string | Optional latitude range filter in decimal degrees (WGS84), from 1.4807 to 1.5588. Blank for 3 rows. These are approximate points, not the exact market ground. |
| `location.longitude` | string | Optional longitude range filter in decimal degrees (WGS84), from 103.6860 to 103.8691. Blank for 3 rows. These are approximate points, not the exact market ground. |

## Example

All night markets held on Fridays.

```bash
curl -s "https://api.jael.ee/JLEE/jb_night_markets?day_of_week=Friday"
```

Response (5 rows, first 2 shown):

```json
[
  {
    "__id": 17,
    "name": "Bandar Baru Uda",
    "day_of_week": "Friday",
    "day_of_week_ms": "Jumaat",
    "permitted_start_time": "16:00",
    "permitted_end_time": "00:00",
    "stalls": 142,
    "area": "Bandar Baru Uda",
    "address_text": "Bandar Baru Uda",
    "address": {
      "street": null,
      "city": "Johor Bahru",
      "state": "Johor",
      "postalcode": "81200"
    },
    "coordinate_precision": "area",
    "location": {
      "latitude": 1.497288,
      "longitude": 103.721231
    }
  },
  {
    "__id": 18,
    "name": "Lurah Kempas Baru",
    "day_of_week": "Friday",
    "day_of_week_ms": "Jumaat",
    "permitted_start_time": "16:00",
    "permitted_end_time": "00:00",
    "stalls": 80,
    "area": "Lurah Kempas Baru",
    "address_text": "Lurah Kempas Baru",
    "address": {
      "street": null,
      "city": "Johor Bahru",
      "state": "Johor",
      "postalcode": null
    },
    "coordinate_precision": "area approximate",
    "location": {
      "latitude": 1.529341,
      "longitude": 103.710894
    }
  }
]
```

Another call:

Markets whose name mentions Taman Daya.

```bash
curl -sg "https://api.jael.ee/JLEE/jb_night_markets?name=*Taman%20Daya*"
```

Returns 2 rows.

## Data source

Weekly pasar malam locations and permitted hours from the public notice of Majlis Bandaraya Johor Bahru (MBJB). Coordinates and postal codes: Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright). Nominatim reverse lookups make some area names approximate.

[Back to all APIs](../../../README.md)
