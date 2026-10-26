# US Stadiums

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 154](https://img.shields.io/badge/records-154-lightgrey)

Home stadiums for every club in the five major North American pro leagues: NFL, MLB, NBA, NHL and MLS. Each row is one club and its stadium, with the league, conference and division, the street address and the latitude and longitude. A few clubs in Canada are included. You must pass a `tag` (league code) on every request.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_stadiums` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

XML output (`.xml`) is not available for this API because the keys `league`, `league[conference]` and `league[division]` clash in XML. Use JSON.

## Query parameters

Required: `tag`. Optional: 11 fields. Disallowed: 3 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `tag` | Required | No | No | League code. One of `NFL`, `MLB`, `NBA`, `NHL`, `MLS`. Required on every request. |
| `stadium` | Optional | Yes | No | Stadium name, for example `*Field*`. |
| `club` | Optional | Yes | No | Home team name, for example `*Steelers*`. |
| `country` | Optional | Yes | No | `United States` or `Canada`. |
| `category` | Optional | No | No | Sport: `American Football`, `Baseball`, `Basketball`, `Football (Soccer)` or `Ice Hockey`. |
| `league` | Optional | No | No | Full league name, for example `National Football League`. |
| `league[conference]` | Optional | No | No | Conference: `AFC`, `NFC`, `American League`, `National League`, `Eastern` or `Western`. |
| `league[division]` | Optional | No | No | Division, for example `North`, `Central` or `Pacific`. Not set for MLS clubs. |
| `address[postalcode]` | Optional | Yes | No | ZIP or postal code of the stadium. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. Range example: `40..42`. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. Range example: `-90..-80`. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. Numbers are not sequential across leagues. |
| `address[street]` | Disallowed | No | No | Street address of the stadium. |
| `address[city]` | Disallowed | No | No | City. |
| `address[state]` | Disallowed | No | No | State or province, spelled out. |

* A request without the required parameter returns an error that names the missing parameter.
* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. Numbers are not sequential across leagues. |
| `stadium` | string | Stadium name. |
| `category` | string | Sport played at the stadium. |
| `league` | string | Full league name. |
| `league[conference]` | string | Conference of the club. This is a literal key name that contains brackets, not a nested object. |
| `league[division]` | string, null | Division of the club within its conference. Null for MLS clubs. Also a literal key name. |
| `tag` | string | League code: `NFL`, `MLB`, `NBA`, `NHL` or `MLS`. |
| `club` | string | Name of the home team. |
| `address.street` | string | Street address of the stadium. |
| `address.city` | string | City. |
| `address.state` | string | State or province, spelled out. |
| `address.postalcode` | string, null | ZIP or postal code, as text. |
| `country` | string | `United States` or `Canada`. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees (negative in the Western Hemisphere). |

## Example

NFL stadiums with Field in the name. The `tag` parameter is required.

```bash
curl -s -g "https://api.jael.ee/JLEE/us_stadiums?tag=NFL&stadium=*Field*"
```

Response, showing 2 of 7 rows:

```json
[
  {
    "__id": 75035,
    "stadium": "Empower Field at Mile High",
    "category": "American Football",
    "league": "National Football League",
    "league[conference]": "AFC",
    "league[division]": "West",
    "tag": "NFL",
    "club": "Denver Broncos",
    "address": {
      "street": "1701 Bryant Street",
      "city": "Denver",
      "state": "Colorado",
      "postalcode": "80204"
    },
    "country": "United States",
    "location": {
      "latitude": 39.743457,
      "longitude": -105.013198
    }
  },
  {
    "__id": 75037,
    "stadium": "Ford Field",
    "category": "American Football",
    "league": "National Football League",
    "league[conference]": "NFC",
    "league[division]": "North",
    "tag": "NFL",
    "club": "Detroit Lions",
    "address": {
      "street": "2000 Brush Street",
      "city": "Detroit",
      "state": "Michigan",
      "postalcode": "48226"
    },
    "country": "United States",
    "location": {
      "latitude": 42.34,
      "longitude": -83.045556
    }
  },
  ...
]
```

MLB stadiums between 40 and 42 degrees north. This one uses a range on `location[latitude]`.

```bash
curl -s -g "https://api.jael.ee/JLEE/us_stadiums?tag=MLB&location[latitude]=40..42"
```

Response, showing 2 of 6 rows:

```json
[
  {
    "__id": 75095,
    "stadium": "Citi Field",
    "category": "Baseball",
    "league": "Major League Baseball",
    "league[conference]": "National League",
    "league[division]": "East",
    "tag": "MLB",
    "club": "New York Mets",
    "address": {
      "street": "41 Seaver Way",
      "city": "Queens",
      "state": "New York",
      "postalcode": "11368"
    },
    "country": "United States",
    "location": {
      "latitude": 40.757031,
      "longitude": -73.845763
    }
  },
  {
    "__id": 75110,
    "stadium": "PNC Park",
    "category": "Baseball",
    "league": "Major League Baseball",
    "league[conference]": "National League",
    "league[division]": "Central",
    "tag": "MLB",
    "club": "Pittsburgh Pirates",
    "address": {
      "street": "115 Federal Street",
      "city": "Pittsburgh",
      "state": "Pennsylvania",
      "postalcode": "15212"
    },
    "country": "United States",
    "location": {
      "latitude": 40.446926,
      "longitude": -80.005606
    }
  },
  ...
]
```

[Back to all APIs](../../README.md)
