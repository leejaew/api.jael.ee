# Singapore SportSG Facilities

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 87](https://img.shields.io/badge/records-87-lightgrey)

Sport facilities listed by Sport Singapore (SportSG). Each row is one facility with a type tag, address and coordinates. Types include sport centres, pools, gyms, stadiums, pickleball courts and padel courts.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_sportsg_facilities` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 5 fields. Disallowed: 2 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `tag` | Required | Yes | No | Required. Allowed values: pickleball (31); sport_centre (27); padel (11); pool (6); gym (6); stadium (2); gym_pool (1); sport_park (1); clubhouse (1); playfield (1). Use * to list all rows. |
| `name` | Optional | Yes | No | Facility name. |
| `address[street]` | Optional | Yes | No | Street address. Null for about a third of the rows. |
| `address[postalcode]` | Optional | No | No | Six digit postal code, as text. Leading zeros are kept. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees. |
| `source` | Disallowed | No | No | Name of the source data set, its coverage and licence. |
| `sourceupdated` | Disallowed | No | No | Date or period the data covers, such as `2025-07` or `2024`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | Facility name. |
| `tag` | string | Facility type: `pickleball`, `sport_centre`, `padel`, `pool`, `gym`, `stadium`, `gym_pool`, `sport_park`, `clubhouse` or `playfield`. |
| `address.street` | string, null | Street address. Null for about a third of the rows. |
| `address.postalcode` | string | Six digit postal code, as text. Leading zeros are kept. |
| `location.latitude` | number | Latitude in decimal degrees. |
| `location.longitude` | number | Longitude in decimal degrees. |
| `source` | string | Name of the source data set, its coverage and licence. |
| `sourceupdated` | string | Date or period the data covers, such as `2025-07` or `2024`. |

## Example

All padel facilities.

```bash
curl -s "https://api.jael.ee/JLEE/sg_sportsg_facilities?tag=padel"
```

Response (11 rows, first 2 shown):

```json
[
  {
    "name": "Hollandse Club",
    "tag": "padel",
    "address": {
      "street": "22 Camden Park",
      "postalcode": "299814"
    },
    "location": {
      "latitude": 1.33,
      "longitude": 103.817
    },
    "source": "SportSG Sport Facilities National Census, data.gov.sg https://data.gov.sg/datasets/d_2cfb0867cdeb2b7303068995699dc33b/view. 2024 Sports Facilities National Census. Singapore Open Data Licence. Facility: Padel Court, venue Hollandse Club. Street: Hollandse Club https://www.hollandseclub.org.sg/contact-us (22 Camden Park, Singapore 299814).",
    "sourceupdated": "2024"
  },
  {
    "name": "Prime Padel Grand Copthorne",
    "tag": "padel",
    "address": {
      "street": "392 Havelock Road",
      "postalcode": "169663"
    },
    "location": {
      "latitude": 1.29,
      "longitude": 103.835
    },
    "source": "SportSG Sport Facilities National Census, data.gov.sg https://data.gov.sg/datasets/d_2cfb0867cdeb2b7303068995699dc33b/view. 2024 Sports Facilities National Census. Singapore Open Data Licence. Facility: Padel Court, venue listed as \"Prime Pedal\" (Padel misspelled) postal 169663. Operator name and street: Prime Padel, Grand Copthorne Waterfront, https://www.primepadelsport.com/locations/singapore (392 Havelock Road, 6th Floor, Singapore 169663).",
    "sourceupdated": "2024"
  }
]
```

Another call:

Facilities of any type with Active in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_sportsg_facilities?tag=*&name=*Active*"
```

Returns 12 rows.

## Data source

SportSG Sport Facilities, data.gov.sg (https://data.gov.sg), Singapore Open Data Licence.

[Back to all APIs](../../README.md)
