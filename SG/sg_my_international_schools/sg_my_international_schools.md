# International Schools in Singapore and Malaysia

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 126](https://img.shields.io/badge/records-126-lightgrey)

International and private schools in Singapore and in three Malaysian cities: Kuala Lumpur, Johor Bahru and Penang. Each row is one school with its curriculum, address, phone and website.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_my_international_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 9 fields. Disallowed: 3 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city` | Required | Yes | No | Required. Allowed values: Singapore (46); Kuala Lumpur (40); Johor Bahru (26); Penang (14). Use * to list all rows. |
| `country` | Optional | No | No | `Singapore` or `Malaysia`. |
| `curriculum` | Optional | No | No | Curriculum label such as `British`, `American`, `IB`, `IB,British` or `mixed`. Exact match. |
| `tag` | Optional | No | No | Lowercase version of `curriculum`, for example `ib,british`. |
| `name` | Optional | Yes | No | School name. |
| `address[street]` | Optional | Yes | No | Street address. |
| `address[city]` | Optional | Yes | No | Local city or town, which can be more specific than `city`. |
| `address[state]` | Optional | No | No | State, or `Singapore`. |
| `address[postalcode]` | Optional | No | No | Postal code, as a number. Leading zeros are not kept. |
| `phone` | Optional | No | No | Phone number with country code. Null when unknown. |
| `website` | Disallowed | No | No | School website. |
| `source` | Disallowed | No | No | Where the row came from, such as a ministry list or the school site. |
| `sourceupdated` | Disallowed | No | No | Date the row was last checked, in ISO 8601 format. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | School name. |
| `country` | string | `Singapore` or `Malaysia`. |
| `city` | string | Main city grouping: `Singapore`, `Kuala Lumpur`, `Johor Bahru` or `Penang`. |
| `curriculum` | string | Curriculum the school offers. More than one is joined with a comma, for example `IB,British`. `mixed` means a mix of several. |
| `tag` | string | Lowercase version of `curriculum`. |
| `address.street` | string | Street address. |
| `address.city` | string | Local city or town, which can be more specific than `city`. |
| `address.state` | string | State, or `Singapore`. |
| `address.postalcode` | integer | Postal code, as a number. Leading zeros are not kept. |
| `phone` | string, null | Phone number with country code. Null when unknown. |
| `website` | string | School website. |
| `source` | string | Where the row came from, such as a ministry list or the school site. |
| `sourceupdated` | string | Date the row was last checked, in ISO 8601 format. |

## Example

International schools in Penang.

```bash
curl -s "https://api.jael.ee/JLEE/sg_my_international_schools?country=Malaysia&city=Penang"
```

Response (14 rows, first 2 shown):

```json
[
  {
    "name": "Assumption International Secondary School",
    "country": "Malaysia",
    "city": "Penang",
    "curriculum": "British",
    "tag": "british",
    "address": {
      "street": "No. 2077 Jalan Kulim, No. 1682 Jalan Kulim, Gereja Jalan Kulim Bukit Mertajam",
      "city": "Penang",
      "state": "Pulau Pinang",
      "postalcode": 14000
    },
    "phone": "+60 12 500 6830",
    "website": "https://assumption.edu.my",
    "source": "EduSwasta school profile https://eduswasta.my/school/assumption-international-secondary-school-bukit-mertajam/ (aggregator claiming SMIPS/KPM registration). Official SMIPS search https://smips.moe.gov.my/utama.cfm?cari=1 was unreachable from collector on 2026-10-03; verify registration on SMIPS / school site before relying on this row.",
    "sourceupdated": "2026-10-03T00:00:00+00:00"
  },
  {
    "name": "Dalat International School",
    "country": "Malaysia",
    "city": "Penang",
    "curriculum": "American",
    "tag": "american",
    "address": {
      "street": "Jalan Tanjung Bungah",
      "city": "Tanjung Bungah",
      "state": "Pulau Pinang",
      "postalcode": 11200
    },
    "phone": "+60 4 685 2100",
    "website": "https://dalat.org",
    "source": "https://dalat.org/; American college-prep; contact address on home page; SMIPS https://smips.moe.gov.my/ unreachable (TLS/connect fail) as of 2026-10-03; BPSWasta https://www.moe.gov.my/bpswasta points to SMIPS 3.0 (no public roster download)",
    "sourceupdated": "2026-10-03T00:00:00+00:00"
  }
]
```

Another call:

Schools of any city with American in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_my_international_schools?city=*&name=*American*"
```

Returns 3 rows.

[Back to all APIs](../../README.md)
