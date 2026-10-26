# Kuala Lumpur Park and Ride

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 77](https://img.shields.io/badge/records-77-lightgrey)

77 park and ride car parks at rail stations in Kuala Lumpur and the Klang Valley (Rapid KL LRT and MRT, KTM Komuter), one row per facility, with line, parking operator, car, motorcycle, OKU and women bays, published fees and station coordinates. Filter by name, line, network, parking_operator, status or city, or use a bay count or latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/kl_park_and_ride` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 20 fields. Disallowed: 5 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `network` | Required | Yes | No | Required. Allowed values: LRT (39); MRT (28); KTM Komuter (10). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the station or car park name, such as Sungai Buloh, Gombak or Kepong Sentral A. KTM stations with several car parks have one row per zone (for example Kuala Lumpur Station A to D). Putra Heights and the two Sungai Besi stations are named to tell them apart. |
| `station_code` | Optional | No | No | Optional exact filter for the rail station code, such as KJ37; SP31, AG18, SA24, KG35 or PY39. Shared stations list two codes. Blank for the 10 KTM Komuter rows. |
| `line` | Optional | Yes | No | Optional filter for the rail line, such as Kelana Jaya Line, Ampang Line, Sri Petaling Line, Shah Alam Line, Kajang Line, Putrajaya Line or KTM Komuter. Interchange stations list two lines separated by a semicolon, so use wildcards. |
| `rail_operator` | Optional | Yes | No | Optional filter for the train operator, such as Prasarana Malaysia (Rapid Rail), MRT Corp / Prasarana Malaysia (Rapid Rail) or Keretapi Tanah Melayu (KTMB). |
| `parking_operator` | Optional | Yes | No | Optional filter for the company that runs the car park, such as ParkIt Solutions Sdn Bhd (7 rows), Taha Meera Enterprise (9), Metro Parking (M) Sdn Bhd (6) or Prasarana Malaysia (Rapid KL) (32). Blank for 9 rows. |
| `operator_phone` | Optional | No | No | Optional exact filter for the parking operator phone number as published, such as 03-20812777 or +6015-4600 0710. Blank for 42 rows. |
| `status` | Optional | No | No | Optional exact filter for the car park status: open (67 rows), parking run by another authority, bays not listed (7), temporarily unavailable (1), partially opened (1) or closed (1). |
| `car_bays` | Optional | No | Yes | Optional filter for the number of car bays, from 13 to 1,355. Blank for 7 rows where the bay count is not published. |
| `motorcycle_bays` | Optional | No | Yes | Optional filter for the number of motorcycle bays, from 0 to 294. Blank for 50 rows where none is published. |
| `fee_train_user_rm` | Optional | No | Yes | Optional filter for the daily rate in ringgit for train users, such as 4.3 (26 LRT rows) or 4.5 (21 MRT rows). Blank for 30 rows where the owner sets the price or no rate is published. |
| `fee_non_user_first_hour_rm` | Optional | No | Yes | Optional filter for the first hour rate in ringgit for people who do not take the train, such as 3 or 3.3. Blank for 31 rows. |
| `fee_non_user_next_hour_rm` | Optional | No | Yes | Optional filter for the rate in ringgit for each extra hour for non train users, such as 1 or 1.1. Blank for 31 rows. |
| `fee_max_per_day_rm` | Optional | No | Yes | Optional filter for the daily maximum in ringgit for non train users, such as 11, 15 or 16.1. Blank for 31 rows. |
| `address[street]` | Optional | Yes | No | Optional filter for the nearest road to the station, looked up from the coordinates with OpenStreetMap, such as Jalan Sultan Hishamuddin. It is not an official car park address. Blank for 4 rows. |
| `address[city]` | Optional | Yes | No | Optional filter for the city or town, such as Kuala Lumpur (37 rows), Petaling Jaya (10), Puchong (6), Kajang (6) or Klang (5). |
| `address[state]` | Optional | No | No | Optional exact filter for the state or federal territory: Selangor (40 rows) or Kuala Lumpur (37). |
| `address[postalcode]` | Optional | No | No | Optional exact filter for the five digit postal code looked up from the coordinates, such as 50000 or 47650. Always check it, since it comes from the nearest mapped road. |
| `location[latitude]` | Optional | No | Yes | Optional latitude range filter for the station coordinates in decimal degrees (WGS84), from 2.95 to 3.23. |
| `location[longitude]` | Optional | No | Yes | Optional longitude range filter for the station coordinates in decimal degrees (WGS84), from 101.44 to 101.79. |
| `parking_authority` | Disallowed | No | No | The local authority that runs the parking when the rail operator lists no bays: Dewan Bandaraya Kuala Lumpur (4 rows), Majlis Bandaraya Petaling Jaya (2) or Majlis Sukan Negara (1). Blank for 70 rows. |
| `oku_bays` | Disallowed | No | No | The bays reserved for disabled drivers (OKU), from 2 to 24. Published for only 11 rows, so blank for 66. |
| `women_bays` | Disallowed | No | No | The bays reserved for women, from 40 to 200. Published for only 5 rows, so blank for 72. |
| `fee_note` | Disallowed | No | No | The fee notes, such as the KTM rates and hours or the note that the owner sets the charges. Blank for 7 rows. |
| `data_source` | Disallowed | No | No | Where the row came from: MyRapid Park N' Ride page (40 rows), MRT Corp Park N' Ride page (27), KTM Parking website, page dated 2016 (9) or a Railway Assets Corporation statement for Klang (1). |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The station or car park name, such as Sungai Buloh, Gombak or Kepong Sentral A. KTM stations with several car parks have one row per zone (for example Kuala Lumpur Station A to D). Putra Heights and the two Sungai Besi stations are named to tell them apart. |
| `station_code` | string | The rail station code, such as KJ37; SP31, AG18, SA24, KG35 or PY39. Shared stations list two codes. Blank for the 10 KTM Komuter rows. |
| `line` | string | The rail line, such as Kelana Jaya Line, Ampang Line, Sri Petaling Line, Shah Alam Line, Kajang Line, Putrajaya Line or KTM Komuter. Interchange stations list two lines separated by a semicolon, so use wildcards. |
| `network` | string | Required. Allowed values: LRT (39); MRT (28); KTM Komuter (10). Use * to list all rows. |
| `rail_operator` | string | The train operator, such as Prasarana Malaysia (Rapid Rail), MRT Corp / Prasarana Malaysia (Rapid Rail) or Keretapi Tanah Melayu (KTMB). |
| `parking_operator` | string | The company that runs the car park, such as ParkIt Solutions Sdn Bhd (7 rows), Taha Meera Enterprise (9), Metro Parking (M) Sdn Bhd (6) or Prasarana Malaysia (Rapid KL) (32). Blank for 9 rows. |
| `parking_authority` | string | The local authority that runs the parking when the rail operator lists no bays: Dewan Bandaraya Kuala Lumpur (4 rows), Majlis Bandaraya Petaling Jaya (2) or Majlis Sukan Negara (1). Blank for 70 rows. |
| `operator_phone` | string | The parking operator phone number as published, such as 03-20812777 or +6015-4600 0710. Blank for 42 rows. |
| `status` | string | The car park status: open (67 rows), parking run by another authority, bays not listed (7), temporarily unavailable (1), partially opened (1) or closed (1). |
| `car_bays` | integer | The number of car bays, from 13 to 1,355. Blank for 7 rows where the bay count is not published. |
| `motorcycle_bays` | string | The number of motorcycle bays, from 0 to 294. Blank for 50 rows where none is published. |
| `oku_bays` | string | Bays reserved for disabled drivers (OKU), from 2 to 24. Published for only 11 rows, so blank for 66. |
| `women_bays` | string | Bays reserved for women, from 40 to 200. Published for only 5 rows, so blank for 72. |
| `fee_train_user_rm` | number | The daily rate in ringgit for train users, such as 4.3 (26 LRT rows) or 4.5 (21 MRT rows). Blank for 30 rows where the owner sets the price or no rate is published. |
| `fee_non_user_first_hour_rm` | number | The first hour rate in ringgit for people who do not take the train, such as 3 or 3.3. Blank for 31 rows. |
| `fee_non_user_next_hour_rm` | number | The rate in ringgit for each extra hour for non train users, such as 1 or 1.1. Blank for 31 rows. |
| `fee_max_per_day_rm` | number | The daily maximum in ringgit for non train users, such as 11, 15 or 16.1. Blank for 31 rows. |
| `fee_note` | string | The fee notes, such as the KTM rates and hours or the note that the owner sets the charges. Blank for 7 rows. |
| `data_source` | string | Where the row came from: MyRapid Park N' Ride page (40 rows), MRT Corp Park N' Ride page (27), KTM Parking website, page dated 2016 (9) or a Railway Assets Corporation statement for Klang (1). |
| `address.street` | string | The nearest road to the station, looked up from the coordinates with OpenStreetMap, such as Jalan Sultan Hishamuddin. It is not an official car park address. Blank for 4 rows. |
| `address.city` | string | The city or town, such as Kuala Lumpur (37 rows), Petaling Jaya (10), Puchong (6), Kajang (6) or Klang (5). |
| `address.state` | string | The state or federal territory: Selangor (40 rows) or Kuala Lumpur (37). |
| `address.postalcode` | string | The five digit postal code looked up from the coordinates, such as 50000 or 47650. Always check it, since it comes from the nearest mapped road. |
| `location.latitude` | number | Optional latitude range filter for the station coordinates in decimal degrees (WGS84), from 2.95 to 3.23. |
| `location.longitude` | number | Optional longitude range filter for the station coordinates in decimal degrees (WGS84), from 101.44 to 101.79. |

## Example

MRT stations with a ParkIt car park.

```bash
curl -s "https://api.jael.ee/JLEE/kl_park_and_ride?network=MRT&parking_operator=*ParkIt*"
```

Response (7 rows, first 2 shown):

```json
[
  {
    "__id": 43,
    "name": "Phileo Damansara",
    "station_code": "KG12",
    "line": "Kajang Line",
    "network": "MRT",
    "rail_operator": "MRT Corp / Prasarana Malaysia (Rapid Rail)",
    "parking_operator": "ParkIt Solutions Sdn Bhd",
    "parking_authority": null,
    "operator_phone": "+6015-4600 0710",
    "status": "open",
    "car_bays": 516,
    "motorcycle_bays": 178,
    "oku_bays": null,
    "women_bays": null,
    "fee_train_user_rm": 4.5,
    "fee_non_user_first_hour_rm": 3.3,
    "fee_non_user_next_hour_rm": 1.1,
    "fee_max_per_day_rm": 16.1,
    "fee_note": "MRT user rate is per day per entry. Motorcycle RM1.10 per entry per day at all park and ride. Rates include 6% SST.",
    "data_source": "MRT Corp Park N' Ride page",
    "address": {
      "street": "Sprint Expressway (Damansara Link)",
      "city": "Petaling Jaya",
      "state": "Selangor",
      "postalcode": "47350"
    },
    "location": {
      "latitude": 3.129864,
      "longitude": 101.642471
    }
  },
  {
    "__id": 46,
    "name": "Taman Midah",
    "station_code": "KG24",
    "line": "Kajang Line",
    "network": "MRT",
    "rail_operator": "MRT Corp / Prasarana Malaysia (Rapid Rail)",
    "parking_operator": "ParkIt Solutions Sdn Bhd",
    "parking_authority": null,
    "operator_phone": "+6015-4600 0710",
    "status": "open",
    "car_bays": 1355,
    "motorcycle_bays": 259,
    "oku_bays": null,
    "women_bays": null,
    "fee_train_user_rm": 4.5,
    "fee_non_user_first_hour_rm": 2.2,
    "fee_non_user_next_hour_rm": 1.1,
    "fee_max_per_day_rm": 11.0,
    "fee_note": "MRT user rate is per day per entry. Motorcycle RM1.10 per entry per day at all park and ride. Rates include 6% SST.",
    "data_source": "MRT Corp Park N' Ride page",
    "address": {
      "street": "Jalan Midah 1",
      "city": "Kuala Lumpur",
      "state": "Kuala Lumpur",
      "postalcode": "56000"
    },
    "location": {
      "latitude": 3.104505,
      "longitude": 101.732186
    }
  }
]
```

Another call:

Stations on the Kelana Jaya Line.

```bash
curl -sg "https://api.jael.ee/JLEE/kl_park_and_ride?network=*&line=*Kelana%20Jaya*"
```

Returns 15 rows.

## Data source

Station coordinates from the Prasarana Malaysia and KTMB GTFS feeds on data.gov.my ([CC BY 4.0](https://developer.data.gov.my/faq)). Bay counts, operators and fees are facts from public operator pages. Addresses: Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright).

[Back to all APIs](../../README.md)
