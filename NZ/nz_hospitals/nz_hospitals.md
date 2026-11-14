# New Zealand Certified Hospitals

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 164](https://img.shields.io/badge/records-164-lightgrey) ![updated 2026-11-13](https://img.shields.io/badge/updated-2026--11--13-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

164 hospitals certified by the Ministry of Health / Manatū Hauora in New Zealand, as listed on 13 November 2026. 89 are public and 75 are private. Each record has the premises name, legal name, certificate name, official service type, sector, certified services, total beds, a website when the source lists one, the premises address, the former DHB name, the certification period, the certificate end date and the auditor. No field is required, so you can filter by name, city, service, bed count or certificate end date.

The list covers 64 towns and cities and 20 former DHB areas. Bed counts run from 3 to 1,187. 23 hospitals have 100 beds or more.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_hospitals` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per hospital. All matches are returned. |

## Query parameters

Required: none. Optional: 17 fields. Disallowed: 2 fields.

A call with no filters returns all 164 records. Partial filters accept `*`, such as `premises_name=*Hospice*`, `service_types=*Maternity*` or `postcode=0*`. `certification_service_type` and `sector` are exact match. `total_beds` accepts a range in the form `min..max`, for example `100..1187`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `premises_name` | Optional | Yes | No | Official premises name, unique on every row, such as Auckland City Hospital or Te Whatu Ora - Waikato Hospital. Partial wildcards allowed, such as `*Hospice*` (19). |
| `legal_name` | Optional | Yes | No | Legal entity that holds the certificate, such as Health New Zealand (Auckland district). 79 distinct values. All 89 public hospitals use a Health New Zealand legal name. Partial wildcards allowed, such as `*Health New Zealand*` (89). |
| `certificate_name` | Optional | Yes | No | Certificate name as the Ministry lists it, such as Auckland District Health Board. 91 distinct values. Partial wildcards allowed. |
| `certification_service_type` | Optional | No | No | Official list label. `Public Hospital` (89) or `NGO Hospital` (75). Exact match. NGO Hospital is the official label on the private hospital list. |
| `sector` | Optional | No | No | `public` (89) or `private` (75). Exact match. |
| `service_types` | Optional | Yes | No | Certified services as a comma list, as the Ministry writes them. Official spelling is `Childrens health` (no apostrophe). Counts by service: Medical (96), Surgical (68), Maternity (60), Mental health (38), Childrens health (24), Psychogeriatric (9), Geriatric (8), Rest home care (2), Physical (2), and one each of Dementia care, Psychiatric, Intellectual and Sensory. Partial wildcards allowed, such as `*Maternity*` (60). A search for `*Geriatric*` also matches Psychogeriatric, so it returns 13 rows, not 8. |
| `total_beds` | Optional | No | Yes | Integer bed count, from 3 to 1187. Never blank. Range only, both bounds required, for example `100..1187` (23) or `3..20` (89). A hyphen range such as `100-1187` returns HTTP 400. |
| `website` | Optional | Yes | No | Premises website as the source lists it, or `null` on 136 rows. Operator office websites are not included. Partial wildcards allowed. |
| `address_other` | Optional | Yes | No | Extra premises location text, such as a building or floor, or `null` on 146 rows. Partial wildcards allowed. |
| `address` | Optional | Yes | No | Street address of the premises, such as 2 Park Road. Never blank. Partial wildcards allowed, such as `*Park Road*`. |
| `suburb` | Optional | Yes | No | Suburb, or `null` on 56 rows. Partial wildcards allowed, such as `Grafton` or `*Parnell*`. |
| `town_city` | Optional | Yes | No | Town or city, 64 distinct values. Largest counts: Auckland (37), Hamilton (13), Christchurch (7), Wellington (6), Nelson (6). Partial wildcards allowed. Alexandra Hospital is in Nelson (Richmond), not the town of Alexandra. |
| `postcode` | Optional | Yes | No | Postcode, kept as text so leading zeros stay in place. 23 values start with 0, including `0110` (2). Never blank. Partial wildcards allowed, such as `0*` (23) or `0110` (2). |
| `dhb_name` | Optional | Yes | No | Former district health board title as the Ministry still lists it, 20 values. This is not the current Health New Zealand district name. Counts: Waikato District Health Board (20), Auckland District Health Board (19), Southern District Health Board (17), Canterbury District Health Board (16), Waitemata District Health Board (13), Counties Manukau District Health Board (12), Nelson Marlborough District Health Board (12), Northland District Health Board (10), Capital and Coast District Health Board (9), Hawke's Bay District Health Board (7), MidCentral District Health Board (6), then 3 each for Bay of Plenty, Hutt Valley, South Canterbury, Taranaki and Whanganui, and 2 each for Lakes, Tairawhiti, Wairarapa and West Coast. Partial wildcards allowed, such as `*Waikato*` (20) or `*Auckland*` (19). |
| `certification_period` | Optional | Yes | No | Period text as the Ministry lists it: `No change` (73), `36` (52), `48` (26), `Other` (11), `12` (1) and `24` (1). Partial wildcards allowed. |
| `cert_end_date` | Optional | Yes | No | Certificate end date as YYYY-MM-DD text, such as `2026-12-01`. Counts by year: 2026 (6), 2027 (61), 2028 (30), 2029 (59), 2030 (8). Partial wildcards allowed, such as `2026*` (6) or `2027*` (61). |
| `auditor` | Optional | Yes | No | Auditor organization: The DAA Group Limited (149), BSI Group New Zealand Ltd (14) or Quality Health Authority (1). These are organizations, not people. Partial wildcards allowed, such as `*BSI*` (14). |
| `source` | Disallowed | No | No | Ministry of Health, HealthCert certified hospital providers, New Zealand, in all 164 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the snapshot, YYYY-MM-DD (`2026-11-13`). Text. Not filterable. |

## Response fields

There is no `__id` field. `total_beds` is an integer. All other values are strings, or `null` when the source has no value.

| Field | Type | Description |
|---|---|---|
| `premises_name` | string | Official premises name, unique on every row. |
| `legal_name` | string | Legal entity that holds the certificate. |
| `certificate_name` | string | Certificate name as the Ministry lists it. |
| `certification_service_type` | string | `Public Hospital` or `NGO Hospital`. |
| `sector` | string | `public` or `private`. |
| `service_types` | string | Certified services as a comma list, as the Ministry writes them. |
| `total_beds` | integer | Bed count, from 3 to 1187. |
| `website` | string | Premises website, or `null` on 136 rows. |
| `address_other` | string | Extra premises location text, or `null` on 146 rows. |
| `address` | string | Street address of the premises. |
| `suburb` | string | Suburb, or `null` on 56 rows. |
| `town_city` | string | Town or city. |
| `postcode` | string | Postcode, kept as text. |
| `dhb_name` | string | Former district health board title as the Ministry still lists it. |
| `certification_period` | string | Period text as the Ministry lists it. |
| `cert_end_date` | string | Certificate end date, YYYY-MM-DD. |
| `auditor` | string | Auditor organization. |
| `source` | string | Ministry of Health, HealthCert certified hospital providers, New Zealand. |
| `source_as_of` | string | Date of the snapshot, `2026-11-13`. |

## Examples

Auckland City Hospital, by official premises name. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_hospitals' --data-urlencode 'premises_name=Auckland City Hospital'
```

```json
[
  {
    "premises_name": "Auckland City Hospital",
    "legal_name": "Health New Zealand (Auckland district)",
    "certificate_name": "Auckland District Health Board",
    "certification_service_type": "Public Hospital",
    "sector": "public",
    "service_types": "Medical, Childrens health, Maternity, Surgical",
    "total_beds": 1187,
    "website": null,
    "address_other": null,
    "address": "2 Park Road",
    "suburb": "Grafton",
    "town_city": "Auckland",
    "postcode": "1023",
    "dhb_name": "Auckland District Health Board",
    "certification_period": "No change",
    "cert_end_date": "2026-12-01",
    "auditor": "The DAA Group Limited",
    "source": "Ministry of Health, HealthCert certified hospital providers, New Zealand",
    "source_as_of": "2026-11-13"
  }
]
```

Hospitals with 100 beds or more. This call returns 23 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_hospitals' --data-urlencode 'total_beds=100..1187'
```

Public hospitals. This call returns 89 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_hospitals' --data-urlencode 'sector=public'
```

Hospitals certified for maternity. This call returns 60 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_hospitals' --data-urlencode 'service_types=*Maternity*'
```

## Data source

Source: Ministry of Health / Manatū Hauora, HealthCert certified public and private hospital providers. The official lists are the [certified public hospital providers](https://www.health.govt.nz/regulation-legislation/certification-of-health-care-services/certified-providers/public-hospitals) page and the [certified private hospital providers](https://www.health.govt.nz/regulation-legislation/certification-of-health-care-services/certified-providers/private-hospitals) page. The list files used for this snapshot are also published at [LegalEntitySummaryPublicHospital.csv](https://www.health.govt.nz/system/files/LegalEntitySummaryPublicHospital.csv) and [LegalEntitySummaryNGOHospital.csv](https://www.health.govt.nz/system/files/LegalEntitySummaryNGOHospital.csv). Those lists are free to use on the Ministry website.

The Ministry of Health [copyright page](https://www.health.govt.nz/about-this-site/copyright) licenses this content for reuse under a [Creative Commons Attribution 4.0 International Licence](https://creativecommons.org/licenses/by/4.0/), except photographs and graphic work, the Ministry logo, other logos, emblems and trade marks, and content supplied by third parties. For content covered by that licence you may copy and redistribute the material in any medium or format, and adapt, remix, transform and build upon it. You must give appropriate credit, provide a link to the licence, and say if changes were made. You may not suggest that the Ministry of Health, Health New Zealand or Manatū Hauora endorses this API or its maker. Do not reproduce the Ministry logo.

This API combines the two official lists, renames columns, reformats certificate dates to YYYY-MM-DD, adds a public or private sector tag, omits operator office addresses and adds a source date. Premises websites from the source are kept. There are no map points, because the official lists have no coordinates.

This is a snapshot from 13 November 2026. Certificates, services and bed counts can change. `dhb_name` still uses the former district health board titles, as the Ministry lists them. Some units share a campus and are listed separately: Auckland City Hospital and Auckland DHB X 3 Units - Mental Health, both at 2 Park Road, Grafton; the public unit Te Whatu Ora - Auckland City Hospital - Birthcare (Leased space) and the private Birthcare Auckland, both at 20 Titoki Street, Parnell; and the public Waterford Birthing Centre and the private Waterford Birth Centre, both at 27 Tisdall Terrace, Hamilton. Alexandra Hospital is at Gilbert Street, Richmond, 7020, in the Nelson area, not in the town of Alexandra. This data is not medical advice. For the latest list, visit the Ministry pages linked above.

[Back to the API list](../../README.md)
