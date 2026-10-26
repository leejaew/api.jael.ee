# Malaysia Public Bus Terminals

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 228](https://img.shields.io/badge/records-228-lightgrey)

Bus terminals, bus stations and public transport hubs in Malaysia: 162 Peninsular Malaysia records from the 2013 APAD terminal directory plus 66 OpenStreetMap terminals added for Sabah, Sarawak and newer or missing stations. Fields are name, operator, address, state, coordinates and yes or no facility flags; filter by state, city or any facility, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/my_bus_terminals` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 15 fields. Disallowed: 6 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `address[state]` | Required | Yes | No | Required. Allowed values: Selangor (34); Johor (32); Perak (32); Pahang (22); Kedah (16); Kelantan (15); Kuala Lumpur (13); Sarawak (12); Penang (11); Sabah (11); Negeri Sembilan (10); Terengganu (7); Melaka (6); Perlis (6); Putrajaya (1). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the terminal name as published, such as Terminal Bersepadu Selatan, Pudu Sentral or Kuching Sentral; spelling follows the source, including a few typos. |
| `address[street]` | Optional | Yes | No | Optional filter for the street or address line; for the 2013 APAD rows this is the free text address with the postcode and town removed when they could be split, so some rows hold only a landmark; blank for 29 rows. |
| `address[city]` | Optional | Yes | No | Optional filter for the town or city, such as Kuala Lumpur, Johor Bahru, Kuching or Tapah; blank for 37 rows. |
| `address[postalcode]` | Optional | No | No | Optional exact filter for the 5 digit postal code as text, such as 50100 or 02100; blank for 45 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2401 to 6.6614; blank for 7 rows. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 100.1309 to 118.6092; blank for 7 rows. |
| `toilet` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded a toilet in 2013; 118 yes, 39 no, 71 blank. |
| `shop` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded a shop or kiosk in 2013; 117 yes, 34 no, 77 blank. |
| `oku_facilities` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded facilities for disabled people (OKU) in 2013; 22 yes, 135 no, 71 blank. |
| `aircond` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded an air conditioned waiting area in 2013; 40 yes, 115 no, 73 blank. |
| `cctv` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded CCTV in 2013; 27 yes, 132 no, 69 blank. |
| `security` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded security staff in 2013; 38 yes, 120 no, 70 blank. |
| `help_point` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded a help point or information counter in 2013; 33 yes, 126 no, 69 blank. |
| `public_phone` | Optional | No | No | Optional exact filter, yes or no: whether APAD recorded a public phone in 2013; 119 yes, 41 no, 68 blank. |
| `operator` | Disallowed | No | No | The operator or authority named in OpenStreetMap, such as Perak Transit Berhad, Majlis Perbandaran Sandakan or MBPJ; blank for 217 of 228 rows because APAD does not list one. |
| `operating_hours` | Disallowed | No | No | The opening hours text published by APAD in 2013, such as 24 hours or 7.00am-7.00pm; blank for 139 rows, including all OpenStreetMap rows. |
| `coordinate_source` | Disallowed | No | No | Where the coordinates came from: APAD directory (XLSX) (111 rows), OpenStreetMap (72 rows) or APAD terminal list (CSV) (38 rows); blank for 7 rows with no coordinates. |
| `coordinate_ref` | Disallowed | No | No | The OpenStreetMap element used, such as w122345016 (w for way, n for node, r for relation); blank for the 156 APAD rows. |
| `data_source` | Disallowed | No | No | The row origin: APAD terminal directory, collected 2013 (162 rows; Peninsular Malaysia only) or OpenStreetMap contributors (ODbL); address by Nominatim reverse geocode (66 rows, including all Sabah and Sarawak rows). |
| `apad_id` | Disallowed | No | No | The APAD record number as text, such as 366; blank for the 66 OpenStreetMap rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The terminal name as published, such as Terminal Bersepadu Selatan, Pudu Sentral or Kuching Sentral; spelling follows the source, including a few typos. |
| `operator` | string | The operator or authority named in OpenStreetMap, such as Perak Transit Berhad, Majlis Perbandaran Sandakan or MBPJ; blank for 217 of 228 rows because APAD does not list one. |
| `address.street` | string | The street or address line; for the 2013 APAD rows this is the free text address with the postcode and town removed when they could be split, so some rows hold only a landmark; blank for 29 rows. |
| `address.city` | string | The town or city, such as Kuala Lumpur, Johor Bahru, Kuching or Tapah; blank for 37 rows. |
| `address.state` | string | Required. Allowed values: Selangor (34); Johor (32); Perak (32); Pahang (22); Kedah (16); Kelantan (15); Kuala Lumpur (13); Sarawak (12); Penang (11); Sabah (11); Negeri Sembilan (10); Terengganu (7); Melaka (6); Perlis (6); Putrajaya (1). Use * to list all rows. |
| `address.postalcode` | string | The 5 digit postal code as text, such as 50100 or 02100; blank for 45 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `operating_hours` | string | The opening hours text published by APAD in 2013, such as 24 hours or 7.00am-7.00pm; blank for 139 rows, including all OpenStreetMap rows. |
| `toilet` | string | Optional exact filter, yes or no: whether APAD recorded a toilet in 2013; 118 yes, 39 no, 71 blank. |
| `shop` | string | Optional exact filter, yes or no: whether APAD recorded a shop or kiosk in 2013; 117 yes, 34 no, 77 blank. |
| `oku_facilities` | string | Optional exact filter, yes or no: whether APAD recorded facilities for disabled people (OKU) in 2013; 22 yes, 135 no, 71 blank. |
| `aircond` | string | Optional exact filter, yes or no: whether APAD recorded an air conditioned waiting area in 2013; 40 yes, 115 no, 73 blank. |
| `cctv` | string | Optional exact filter, yes or no: whether APAD recorded CCTV in 2013; 27 yes, 132 no, 69 blank. |
| `security` | string | Optional exact filter, yes or no: whether APAD recorded security staff in 2013; 38 yes, 120 no, 70 blank. |
| `help_point` | string | Optional exact filter, yes or no: whether APAD recorded a help point or information counter in 2013; 33 yes, 126 no, 69 blank. |
| `public_phone` | string | Optional exact filter, yes or no: whether APAD recorded a public phone in 2013; 119 yes, 41 no, 68 blank. |
| `coordinate_source` | string | Where the coordinates came from: APAD directory (XLSX) (111 rows), OpenStreetMap (72 rows) or APAD terminal list (CSV) (38 rows); blank for 7 rows with no coordinates. |
| `coordinate_ref` | string | The OpenStreetMap element used, such as w122345016 (w for way, n for node, r for relation); blank for the 156 APAD rows. |
| `data_source` | string | The row origin: APAD terminal directory, collected 2013 (162 rows; Peninsular Malaysia only) or OpenStreetMap contributors (ODbL); address by Nominatim reverse geocode (66 rows, including all Sabah and Sarawak rows). |
| `apad_id` | string | The APAD record number as text, such as 366; blank for the 66 OpenStreetMap rows. |

## Example

Perak terminals that APAD recorded with a toilet.

```bash
curl -sg "https://api.jael.ee/JLEE/my_bus_terminals?address[state]=Perak&toilet=yes"
```

Response (21 rows, first 2 shown):

```json
[
  {
    "__id": 126,
    "name": "HENTIAN GERIK",
    "operator": null,
    "address": {
      "street": "Hentian Bas Gerik, Jalan Intan, Pekan Gerik",
      "city": "Gerik",
      "state": "Perak",
      "postalcode": "33300"
    },
    "location": {
      "latitude": 5.43055,
      "longitude": 101.12915
    },
    "operating_hours": "6.30am - 9.30pm",
    "toilet": "yes",
    "shop": "yes",
    "oku_facilities": "no",
    "aircond": "yes",
    "cctv": "no",
    "security": "no",
    "help_point": "no",
    "public_phone": "yes",
    "coordinate_source": "APAD directory (XLSX)",
    "coordinate_ref": null,
    "data_source": "APAD terminal directory, collected 2013",
    "apad_id": "173"
  },
  {
    "__id": 127,
    "name": "Hentian Bas Pengkalan Hulu",
    "operator": null,
    "address": {
      "street": null,
      "city": "PENGKALAN HULU",
      "state": "Perak",
      "postalcode": "33100"
    },
    "location": {
      "latitude": 5.707499,
      "longitude": 101.000613
    },
    "operating_hours": null,
    "toilet": "yes",
    "shop": "yes",
    "oku_facilities": "no",
    "aircond": "no",
    "cctv": "no",
    "security": "no",
    "help_point": "no",
    "public_phone": "yes",
    "coordinate_source": "APAD directory (XLSX)",
    "coordinate_ref": null,
    "data_source": "APAD terminal directory, collected 2013",
    "apad_id": "172"
  }
]
```

Another call:

Sarawak terminals with Sentral in the name.

```bash
curl -sg "https://api.jael.ee/JLEE/my_bus_terminals?name=*Sentral*&address[state]=Sarawak"
```

Returns 3 rows.

## Data source

Terminal lists from the Land Public Transport Agency (APAD) on data.gov.my, [CC BY 4.0](https://developer.data.gov.my/faq), credit APAD and data.gov.my. Added coordinates and addresses: [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), Open Database License.

[Back to all APIs](../../README.md)
