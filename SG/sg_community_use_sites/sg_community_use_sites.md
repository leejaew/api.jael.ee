# Singapore Community Use Sites

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 240](https://img.shields.io/badge/records-240-lightgrey)

State land sites in Singapore that can be used by the community, one row per site, with name, site area in square meters, managing agency (HDB or SLA), contact phone and email, and the site center point. Filter by agency, name or a range on site_area_sqm or the coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_community_use_sites` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 7 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `agency` | Required | Yes | No | Required. Allowed values: HDB (134); SLA (106). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the site name, mostly in capitals, such as SUNSET WAY; wildcards work. |
| `site_area_sqm` | Optional | No | Yes | Optional filter for the site area in square meters (assumed); from 281.3 to 137,526.5. |
| `phone` | Optional | No | No | Optional exact filter for the agency phone, +65 6490 1111 (HDB) or +65 6323 9829 (SLA). |
| `email` | Optional | Yes | No | Optional filter for the agency email, hdbmailbox@hdb.gov.sg or sla_enquiry@sla.gov.sg. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2702 to 1.4613. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6243 to 103.965. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The site name, mostly in capitals, such as SUNSET WAY. |
| `site_area_sqm` | number | The site area in square meters (assumed); from 281.3 to 137,526.5. |
| `agency` | string | The managing agency: HDB (134 rows) or SLA (106 rows). |
| `phone` | string | The agency phone, +65 6490 1111 (HDB) or +65 6323 9829 (SLA). |
| `email` | string | The agency email, hdbmailbox@hdb.gov.sg or sla_enquiry@sla.gov.sg. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

HDB sites with Tampines in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_community_use_sites?agency=HDB&name=*TAMPINES*"
```

Response (8 rows, first 2 shown):

```json
[
  {
    "__id": 1,
    "name": "STATE LAND AT TAMPINES CENTRAL 3",
    "site_area_sqm": 38014.0,
    "agency": "HDB",
    "phone": "+65 6490 1111",
    "email": "hdbmailbox@hdb.gov.sg",
    "location": {
      "latitude": 1.356781,
      "longitude": 103.942237
    }
  },
  {
    "__id": 74,
    "name": "SL at Tampines Street 92",
    "site_area_sqm": 2567.7,
    "agency": "HDB",
    "phone": "+65 6490 1111",
    "email": "hdbmailbox@hdb.gov.sg",
    "location": {
      "latitude": 1.344821,
      "longitude": 103.943691
    }
  }
]
```

Another call:

SLA sites with ROAD in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_community_use_sites?agency=SLA&name=*ROAD*"
```

Returns 35 rows.

## Data source

Community Use Sites, Singapore Land Authority and Housing and Development Board, data.gov.sg (https://data.gov.sg), dataset d_1f35a697b1a83fc771fa528cf76bcea4, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated February 14, 2025. Agency, phone, email and site area are read from the source description; latitude and longitude are the site center point; the area is assumed to be square meters.

[Back to all APIs](../../README.md)
