# US USCIS Offices

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 220](https://img.shields.io/badge/records-220-lightgrey)

USCIS field offices (93) and application support centers (127) in the United States and its territories: 220 places. Fields are name, office type, street address, city, state, ZIP code, district and service center codes, and map coordinates found by geocoding the address. Filter by state, city, office type or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_uscis_offices` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 12 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `office_type` | Required | Yes | No | Required. Allowed values: Application support center (127); Field office (93). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `office_code` | Optional | No | No | Exact USCIS office id as text, such as field_office:SFR or asc:XSE; one per row. |
| `name` | Optional | Yes | No | Optional filter for the office name, such as `*San Jose*` or `*Field Office*`; wildcards work. Every row has a name. |
| `address[street]` | Optional | Yes | No | Optional filter for the street line with number and any suite or room, such as `*Broadway*` or `*Suite 100*`. |
| `address[city]` | Optional | Yes | No | Optional filter for the city as USCIS lists it, such as Seattle or Los Angeles. |
| `address[state]` | Optional | No | No | Exact two letter state or territory code, such as CA, TX, PR or VI. |
| `address[postalcode]` | Optional | No | No | Exact five digit ZIP code as text, so leading zeros stay, such as 02203. |
| `district_code` | Optional | No | No | Exact USCIS district office code for a field office, such as SEA or NYC; blank for all 127 application support centers. |
| `service_center_code` | Optional | No | No | Exact USCIS service center code that serves the field office, such as NSC, TSC, VSC or CSC; blank for all 127 application support centers. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Blank for 9 rows. Values run from 13.487621 to 61.215066. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Blank for 9 rows. Values run from -157.862276 to 144.795695. |
| `geocode_precision` | Optional | No | No | How the coordinates were found: address (street number matched), street (street name only, number not matched) or building (building name only); blank where no match was found. |
| `address[building]` | Disallowed | No | No | The building name, such as `*Federal Building*`; blank for 199 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `office_code` | string | USCIS office id as text, such as field_office:SFR or asc:XSE; one per row. |
| `name` | string | The office name, such as `*San Jose*` or `*Field Office*`. |
| `office_type` | string | Required. Allowed values: Application support center (127); Field office (93). Use * to list all rows. |
| `address.building` | string | The building name, such as `*Federal Building*`; blank for 199 rows. |
| `address.street` | string | The street line with number and any suite or room, such as `*Broadway*` or `*Suite 100*`. |
| `address.city` | string | The city as USCIS lists it, such as Seattle or Los Angeles. |
| `address.state` | string | Two letter state or territory code, such as CA, TX, PR or VI. |
| `address.postalcode` | string | Five digit ZIP code as text, so leading zeros stay, such as 02203. |
| `district_code` | string | USCIS district office code for a field office, such as SEA or NYC; blank for all 127 application support centers. |
| `service_center_code` | string | USCIS service center code that serves the field office, such as NSC, TSC, VSC or CSC; blank for all 127 application support centers. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `geocode_precision` | string | How the coordinates were found: address (street number matched), street (street name only, number not matched) or building (building name only); blank where no match was found. |

## Example

USCIS field offices in California.

```bash
curl -sg "https://api.jael.ee/JLEE/us_uscis_offices?address[state]=CA&office_type=Field%20office"
```

Response (11 rows, first 2 shown):

```json
[
  {
    "__id": 25,
    "office_code": "field_office:FRE",
    "name": "Fresno Field Office",
    "office_type": "Field office",
    "address": {
      "building": null,
      "street": "744 P Street, Suite 120",
      "city": "Fresno",
      "state": "CA",
      "postalcode": "93721"
    },
    "district_code": "SFR",
    "service_center_code": "CSC",
    "location": {
      "latitude": 36.735771,
      "longitude": -119.781102
    },
    "geocode_precision": "address"
  },
  {
    "__id": 34,
    "office_code": "field_office:IMP",
    "name": "Imperial Field Office",
    "office_type": "Field office",
    "address": {
      "building": null,
      "street": "509 Industry Way",
      "city": "Imperial",
      "state": "CA",
      "postalcode": "92251"
    },
    "district_code": "SND",
    "service_center_code": "CSC",
    "location": {
      "latitude": 32.7842,
      "longitude": -115.527804
    },
    "geocode_precision": "address"
  }
]
```

Another call:

USCIS offices in Houston.

```bash
curl -sg "https://api.jael.ee/JLEE/us_uscis_offices?office_type=*&address[city]=*Houston*"
```

Returns 3 rows.

## Data source

USCIS field office by ZIP code table and Application Support Center locator data. Information on uscis.gov is in the public domain, with credit to [USCIS](https://www.uscis.gov/website-policies) requested. Coordinates: [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), Open Database License.

[Back to all APIs](../../README.md)
