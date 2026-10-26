# Singapore Student Care Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 336](https://img.shields.io/badge/records-336-lightgrey)

Registered student care centres in Singapore from the Ministry of Social and Family Development, one row per centre, with address parts, phone, email, operator type, audit grade, monthly fee in Singapore dollars, enrolment and coordinates. Filter by name, postal code, audit_status, business_profile, or a range on monthly_fee or enrolment.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_student_care_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 17 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Optional filter for the centre name, such as a brand or school it sits in; wildcards work. |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 80 or 259C; text, so wildcards work. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Punggol Field; wildcards work. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01 or #B1; blank when the source has none. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 47; blank when the source has none. |
| `address[building]` | Optional | Yes | No | Optional filter for the host building, often a school such as Fuchun Primary School; wildcards work; blank for many rows. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay. |
| `phone` | Optional | No | No | Optional exact filter for the phone number, written like +65 6333 5084. |
| `email` | Optional | Yes | No | Optional filter for the contact email; some rows hold two emails separated by a comma, so wildcards help. |
| `business_profile` | Optional | No | No | Optional exact filter for the operator type: Commercial Companies (312 rows) or Social Service Agency (SSA) (24 rows). |
| `audit_status` | Optional | No | No | Optional exact filter for the audit grade: Grade A, Grade B, Grade B- or Grade D; blank for 9 rows. |
| `scfa` | Optional | No | No | Optional exact filter for the fee assistance flag; every row is Y. |
| `monthly_fee` | Optional | No | Yes | Optional filter for the monthly fee in Singapore dollars; a number from 215 to 850. |
| `enrolment` | Optional | No | Yes | Optional filter for the number of enrolled children; a number from 3 to 397. |
| `centre_id` | Optional | No | Yes | Optional filter for the centre ID, a unique number from 23001 to 23337. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2747 to 1.4567. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6876 to 103.963. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | The centre name, such as a brand or school it sits in. |
| `address.block` | string | The block or house number, such as 80 or 259C; text, so wildcards work. |
| `address.street` | string | The street name, such as Punggol Field. |
| `address.level` | string | The floor, such as #01 or #B1; blank when the source has none. |
| `address.unit` | string | The unit number, such as 47; blank when the source has none. |
| `address.building` | string | The host building, often a school such as Fuchun Primary School. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay. |
| `phone` | string | The phone number, written like +65 6333 5084. |
| `email` | string | The contact email; some rows hold two emails separated by a comma, so wildcards help. |
| `business_profile` | string | The operator type: Commercial Companies (312 rows) or Social Service Agency (SSA) (24 rows). |
| `audit_status` | string | The audit grade: Grade A, Grade B, Grade B- or Grade D; blank for 9 rows. |
| `scfa` | string | The fee assistance flag; every row is Y. |
| `monthly_fee` | number | The monthly fee in Singapore dollars; a number from 215 to 850. |
| `enrolment` | integer | The number of enrolled children; a number from 3 to 397. |
| `centre_id` | integer | The centre ID, a unique number from 23001 to 23337. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Grade A centres run by social service agencies.

```bash
curl -s "https://api.jael.ee/JLEE/sg_student_care_centres?audit_status=Grade%20A&business_profile=Social%20Service%20Agency%20%28SSA%29"
```

Response (18 rows, first 2 shown):

```json
[
  {
    "name": "RAFFLES STUDENT CARE (TANJONG KATONG) PTE. LTD.",
    "address": {
      "block": "10",
      "street": "Seraya Road",
      "level": null,
      "unit": null,
      "building": null,
      "postalcode": "437259"
    },
    "phone": "+65 8793 3420",
    "email": "tk@rafflesstudentcare.com",
    "business_profile": "Social Service Agency (SSA)",
    "audit_status": "Grade A",
    "scfa": "Y",
    "monthly_fee": 265.9,
    "enrolment": 62,
    "centre_id": 23199,
    "location": {
      "latitude": 1.304985,
      "longitude": 103.9
    }
  },
  {
    "name": "Big Heart Student Care @ Woodlands Ring",
    "address": {
      "block": "11",
      "street": "Woodlands Ring Road",
      "level": null,
      "unit": null,
      "building": "Woodlands Ring Primary School",
      "postalcode": "738240"
    },
    "phone": "+65 9022 1242",
    "email": "woodlandsring@shgstudentcare.com.sg, bigheart_administrator@shgstudentcare.com.sg",
    "business_profile": "Social Service Agency (SSA)",
    "audit_status": "Grade A",
    "scfa": "Y",
    "monthly_fee": 290.0,
    "enrolment": 145,
    "centre_id": 23201,
    "location": {
      "latitude": 1.434852,
      "longitude": 103.797414
    }
  }
]
```

Another call:

Centres with YMCA in the name.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_student_care_centres?name=*YMCA*"
```

Returns 23 rows.

## Data source

Student care centres registered with the Ministry of Social and Family Development, data.gov.sg (https://data.gov.sg), dataset d_77e6e0d58ce4743dab1f26dfcbbeb6f4, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated September 23, 2026.

[Back to all APIs](../../README.md)
