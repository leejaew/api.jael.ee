# Singapore Active Ageing Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 93](https://img.shields.io/badge/records-93-lightgrey)

Active Ageing Centres in Singapore that appear in the MOH Dementia Friendly Go To Point list: 93 centres (not all 200 plus nationwide), with name, operator, address parts, postal code, opening hours, phone and map coordinates. Filter by name, operator, street, postal code, or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_active_ageing_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 11 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the centre name, such as `*Toa` `Payoh*`; wildcards work. All 93 rows have a name. |
| `operator` | Optional | Yes | No | Optional filter for the operator brand taken from the start of the name, such as THK (18 rows), NTUC Health (10), Lions Befrienders (10), Fei Yue (9); 1 row is blank. |
| `address[block]` | Optional | Yes | No | Optional filter for the block number, such as 117 or 509B; text, so wildcards work. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name as written in the source, such as Ang Mo Kio Ave 10 or Lor 1 Toa Payoh; wildcards work. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01 or #02; blank for 3 rows. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 01 or 1019; blank for 3 rows. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay, such as 090003. |
| `phone` | Optional | No | No | Exact phone number as text in the form +65 6681 4928; every row has one, and a few centres share a number. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.271472 to 1.454208. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.708986 to 103.960173. |
| `opening_hours` | Disallowed | Yes | No | Optional filter for the opening hours text, such as `*Saturday*`; 2 rows start with Temporary closed for renovation works. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The centre name, such as `*Toa` `Payoh*`. |
| `operator` | string | The operator brand taken from the start of the name, such as THK (18 rows), NTUC Health (10), Lions Befrienders (10), Fei Yue (9); 1 row is blank. |
| `address.block` | string | The block number, such as 117 or 509B; text, so wildcards work. |
| `address.street` | string | The street name as written in the source, such as Ang Mo Kio Ave 10 or Lor 1 Toa Payoh. |
| `address.level` | string | The floor, such as #01 or #02; blank for 3 rows. |
| `address.unit` | string | The unit number, such as 01 or 1019; blank for 3 rows. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay, such as 090003. |
| `opening_hours` | string | The opening hours text, such as `*Saturday*`; 2 rows start with Temporary closed for renovation works. |
| `phone` | string | Phone number as text in the form +65 6681 4928; every row has one, and a few centres share a number. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

All centres run under the THK name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_active_ageing_centres?operator=THK"
```

Response (18 rows, first 2 shown):

```json
[
  {
    "__id": 4,
    "name": "THK Active Ageing Centre @ Toa Payoh 15",
    "operator": "THK",
    "address": {
      "block": "15",
      "street": "Toa Payoh Lorong 7",
      "level": "#01",
      "unit": "579",
      "postalcode": "310015"
    },
    "opening_hours": "Monday to Friday: 8.30 am - 5.30 pm, Saturday: 8.30 am - 12.30 pm",
    "phone": "+65 6251 6504",
    "location": {
      "latitude": 1.335922,
      "longitude": 103.859058
    }
  },
  {
    "__id": 7,
    "name": "THK Active Ageing Centre @ MacPherson",
    "operator": "THK",
    "address": {
      "block": "90",
      "street": "Pipit Road",
      "level": "#01",
      "unit": "103",
      "postalcode": "370090"
    },
    "opening_hours": "Monday to Friday: 8.30 am - 5.30 pm, Saturday: 8.30 am - 12.30 pm",
    "phone": "+65 6745 6696",
    "location": {
      "latitude": 1.32338,
      "longitude": 103.886521
    }
  }
]
```

Another call:

Centres on any Toa Payoh street.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_active_ageing_centres?address[street]=*Toa%20Payoh*"
```

Returns 8 rows.

## Data source

Active Ageing Centres from the Ministry of Health Dementia Friendly Go To Points dataset on data.gov.sg, published under the [Singapore Open Data Licence v1.0](https://data.gov.sg/open-data-licence).

[Back to all APIs](../../README.md)
