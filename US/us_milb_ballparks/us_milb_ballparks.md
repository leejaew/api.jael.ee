# US Minor League Baseball Ballparks

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 120](https://img.shields.io/badge/records-120-lightgrey)

Ballparks used by Minor League Baseball clubs at the four full season levels: Triple A, Double A, High A and Single A. Each row is one club with its ballpark, league, MLB parent club, seating capacity, the year the park opened, address, coordinates and the club website. One club plays in Canada.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_milb_ballparks` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `stadium` | Optional | Yes | No | Ballpark name. |
| `club` | Optional | Yes | No | Minor league club name. |
| `league` | Optional | Yes | No | League name, for example `*International*`. |
| `parentclub` | Optional | Yes | No | MLB club the team is affiliated with. |
| `address[street]` | Optional | Yes | No | Street address. |
| `address[city]` | Optional | Yes | No | City. |
| `level` | Optional | No | No | `Triple-A`, `Double-A`, `High-A` or `Single-A`. |
| `division` | Optional | No | No | Division inside the league: `East`, `West`, `North`, `South`, `Northeast`, `Southwest` or `None`. |
| `address[state]` | Optional | No | No | State or province, spelled out. |
| `address[postalcode]` | Optional | No | No | ZIP or postal code. |
| `address[countrycode]` | Optional | No | No | `US` or `CA`. |
| `capacity` | Optional | No | Yes | Seating capacity. Range example: `5000..8000`. |
| `opened` | Optional | No | Yes | Year the ballpark opened. Range example: `2000..2010`. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../README.md#how-filters-work) for the wildcard and range syntax.
* Any field that is not in this table is returned in the output but cannot be used as a filter. Sending one returns an error.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `stadium` | string | Ballpark name. |
| `club` | string | Minor league club name. |
| `level` | string | Level of play: `Triple-A`, `Double-A`, `High-A` or `Single-A`. |
| `league` | string | League the club plays in. |
| `division` | string | Division inside the league. The text `None` is used where the league has no divisions. |
| `parentclub` | string | MLB club the team is affiliated with. |
| `capacity` | integer | Seating capacity. |
| `opened` | integer | Year the ballpark opened. |
| `address.street` | string | Street address. |
| `address.city` | string | City. |
| `address.state` | string | State or province, spelled out. |
| `address.postalcode` | string | ZIP or postal code, as text. |
| `address.countrycode` | string | `US` or `CA`. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `websiteurl` | string | Club page on milb.com. |
| `recordupdated` | string | Date the row was last updated, in ISO 8601 format. |

## Example

Triple-A ballparks with a capacity between 10,000 and 16,600.

```bash
curl -s "https://api.jael.ee/JLEE/us_milb_ballparks?level=Triple-A&capacity=10000..16600"
```

Response, showing 2 of 21 rows:

```json
[
  {
    "stadium": "Sahlen Field",
    "club": "Buffalo Bisons",
    "level": "Triple-A",
    "league": "International League",
    "division": "East",
    "parentclub": "Toronto Blue Jays",
    "capacity": 16600,
    "opened": 1988,
    "address": {
      "street": "1 James D. Griffin Plaza",
      "city": "Buffalo",
      "state": "New York",
      "postalcode": "14203",
      "countrycode": "US"
    },
    "location": {
      "latitude": 42.881306,
      "longitude": -78.874278
    },
    "websiteurl": "https://www.milb.com/buffalo",
    "recordupdated": "2026-10-06T00:00:00+00:00"
  },
  {
    "stadium": "Truist Field",
    "club": "Charlotte Knights",
    "level": "Triple-A",
    "league": "International League",
    "division": "East",
    "parentclub": "Chicago White Sox",
    "capacity": 10200,
    "opened": 2014,
    "address": {
      "street": "324 South Mint Street",
      "city": "Charlotte",
      "state": "North Carolina",
      "postalcode": "28202",
      "countrycode": "US"
    },
    "location": {
      "latitude": 35.227988,
      "longitude": -80.849011
    },
    "websiteurl": "https://www.milb.com/charlotte",
    "recordupdated": "2026-10-06T00:00:00+00:00"
  },
  ...
]
```

[Back to all APIs](../README.md)
