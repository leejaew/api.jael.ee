# NZ International Student Providers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 962](https://img.shields.io/badge/records-962-lightgrey) ![updated 2026-11-02](https://img.shields.io/badge/updated-2026--11--02-orange)

Every New Zealand education provider that NZQA lists as a signatory to the Code of Practice for the Pastoral Care of International Students: 962 providers, one row per provider. Each row has the provider name and number, provider type, school type and authority for schools, NZQA category, number of sites, main address, region, general phone, email and website, and a map point. Filter by the required `region`, then by name, type, town, school type, authority, category or a coordinate range.

Rows by type: School 765, Private Training Establishment 171, Polytechnic 17, University 8 and Wānanga 1. Rows by region: Auckland 372, Canterbury 124, Wellington 91, Waikato 82, Bay of Plenty 69, Otago 57, Manawatū-Whanganui 43, Hawke's Bay 28, Northland 24, Nelson 18, Tasman 15, Taranaki 15, Southland 12, Marlborough 5, West Coast 5 and Gisborne 2. The data comes from NZQA and the Ministry of Education, both fetched on 2026-11-02.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_international_student_providers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 18 fields. Disallowed: 3 fields.

`region` must be in every call. Use `region=*` to list all 962 rows. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | Regional council region of the provider's main site, such as `Auckland` (372), `Canterbury` (124), `Wellington` (91) or `Waikato` (82). Wildcards work, so `region=*` lists all 962 rows, and `Manawat*` (43) or `Hawke*` (28) save typing the macron in `Manawatū-Whanganui` or the apostrophe in `Hawke's Bay`. Schools use the Ministry of Education region. Other providers use the region of their map point. |
| `provider_name` | Optional | Yes | No | Provider name as NZQA lists it, such as `Massey University` (1), `*Grammar*` (7) or `*College*` (202). `New Zealand Institute of Skills and Technology` is on 6 rows, one for each division, told apart by `trading_as` and `provider_number`. |
| `trading_as` | Optional | Yes | No | Trading name NZQA shows, such as `NorthTec` or `*English*` (7). Null for 872 rows. |
| `provider_number` | Optional | No | No | NZQA education organisation number, such as `6683`. Unique for each row. For schools it is also the Ministry of Education school number. Exact match only. |
| `provider_type` | Optional | No | No | Provider type: `School` (765), `Private Training Establishment` (171), `Polytechnic` (17), `University` (8) or `Wānanga` (1). Exact match only. |
| `school_type` | Optional | Yes | No | Ministry of Education school type, such as `Contributing` (213), `Full Primary` (143), `Intermediate` (71), `Composite` (55) or `Secondary*` (274, all secondary types). Null for non-schools and 2 schools not in the Ministry directory. |
| `authority` | Optional | Yes | No | School authority: `State` (571), `State: Integrated` (131), `Private: Fully Registered` (57), `Charter School` (3) or `Private: Provisionally Registered` (1). `Private*` (58) matches both private kinds. Null for non-schools. |
| `nzqa_category` | Optional | No | No | NZQA provider category, from `1` (highest confidence) to `4`: `1` (102), `2` (62), `3` (7) or `4` (2). Null for schools and providers NZQA has not put in a category. Exact match only. |
| `site_count` | Optional | No | Yes | Number of delivery sites NZQA lists for the provider, from 1 to 99. Example: `site_count=2..99` (128 rows). |
| `other_site_towns` | Optional | Yes | No | Towns of the provider's other delivery sites, separated by semicolons, such as `*Auckland*` (31). Some entries are offshore or online venues as NZQA lists them. Null when there is only one site. |
| `street_address` | Optional | Yes | No | Street address of the main site, such as `*Queen Street*` (8). Schools use the Ministry of Education street address. Null for 48 rows where NZQA gives only a PO Box or Private Bag, including all 8 universities. |
| `suburb` | Optional | Yes | No | Suburb of the main site, such as `Takapuna` (11). Null for 354 rows. |
| `town_city` | Optional | Yes | No | Town or city of the main site, such as `Auckland` (314), `Christchurch` (86), `Wellington` (50) or `Queenstown` (8). A few rows give a suburb such as `Grafton` or `Newtown`, as NZQA lists it. |
| `postcode` | Optional | Yes | No | Four digit postcode, such as `1010` (31), kept as text so leading zeros stay, as in `0622`. For schools it comes from the Ministry of Education mailing address and is null when that is a PO Box or Private Bag. Null for 346 rows. |
| `phone` | Optional | Yes | No | Main phone number in international form, such as `+64 9*` (368). Null for 3 rows. |
| `email` | Optional | Yes | No | General email of the organisation, such as `*@*.school.nz` (676). Only office or role addresses are kept, so it is null for 106 rows. |
| `website` | Optional | Yes | No | Website, always https, such as `*.ac.nz*` (127). Null for 8 rows. |
| `latitude` | Optional | No | Yes | Latitude of the main site in decimal degrees (WGS84), from -46.45314 to -35.121322. Example: `latitude=-44..-43`. Check `geocode_quality` before you use the point. |
| `longitude` | Optional | No | Yes | Longitude of the main site in decimal degrees (WGS84), from 168.016613 to 178.009033. Example: `longitude=172..173`. |
| `geocode_quality` | Disallowed | No | No | Where the point came from: `moe_directory` (762, the Ministry of Education school point), `address` (117, OpenStreetMap match on the street number), `street` (24, the street but not the number), `named_campus` (8, the university campus by name) or `town_centroid` (51, the center of the town or suburb only). |
| `source` | Disallowed | No | No | Data source and credit for the row. |
| `updated` | Disallowed | No | No | Date the data was fetched, `YYYY-MM-DD` (`2026-11-02` for every row). |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `region` returns HTTP 400 with `Your request is missing the following required parameters: region. You did not supply any.`
* A filter on `geocode_quality`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source` (or the field you used).

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. IDs start at 963, not 1. |
| `provider_name` | string | Provider name as NZQA lists it. |
| `trading_as` | string | Trading name, or null. |
| `provider_number` | string | NZQA education organisation number (for schools, also the Ministry of Education school number). |
| `provider_type` | string | `School`, `Private Training Establishment`, `Polytechnic`, `University` or `Wānanga`. |
| `school_type` | string | Ministry of Education school type, or null. |
| `authority` | string | School authority, or null. |
| `nzqa_category` | string | NZQA category `1` to `4`, or null. |
| `site_count` | integer | Number of delivery sites NZQA lists. |
| `other_site_towns` | string | Towns of the other sites, separated by semicolons, or null. |
| `street_address` | string | Street address of the main site, or null. |
| `suburb` | string | Suburb of the main site, or null. |
| `town_city` | string | Town or city of the main site. |
| `region` | string | Regional council region. |
| `postcode` | string | Four digit postcode, or null. |
| `phone` | string | Main phone number, or null. |
| `email` | string | General email of the organisation, or null. |
| `website` | string | Website (https), or null. |
| `latitude` | number | Latitude of the main site in decimal degrees. |
| `longitude` | number | Longitude of the main site in decimal degrees. |
| `geocode_quality` | string | Where the map point came from. |
| `source` | string | Data source and credit. |
| `updated` | string | Date the data was fetched. |

## Notes

* **One row per provider.** NZQA lists 2,014 sites for these providers. Each row is one provider with its main site, the number of sites in `site_count` and the other towns in `other_site_towns`. Use `provider_number` or `__id` as the row key, since a name can repeat.
* **Where the fields come from.** Name, number, type, category, sites, phone, email and website come from NZQA. For the 763 schools found in the Ministry of Education Schools Directory, school type and authority come from the Ministry, and so do the address and point for 762 of them. For all other rows the address comes from NZQA's main site.
* **How good the points are.** 762 school points come from the Ministry. The rest were found with OpenStreetMap Nominatim from the NZQA address: 117 on the street number, 24 on the street, 8 on the university campus and 51 only at the center of the town or suburb. Do not use a `town_centroid` point as a campus pin.
* **Region is the regional council.** It is not NZQA's own region list. A few schools sit in a different NZQA region, such as Thames South School, which NZQA lists under Bay of Plenty but which is in the Waikato region.
* **No age or level scope.** The source does not say whether a provider takes students under 18, so the API does not either.
* **Contacts are general only.** No personal names are kept, and emails that looked personal were left out.
* **Check before you rely on a row.** Code signatories change. Check the provider on the NZQA site before you rely on a row.

## Examples

Universities in one region. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_international_student_providers" --data-urlencode "region=Otago" --data-urlencode "provider_type=University"
```

```json
[
  {
    "__id": 1728,
    "provider_name": "University of Otago",
    "trading_as": null,
    "provider_number": "7007",
    "provider_type": "University",
    "school_type": null,
    "authority": null,
    "nzqa_category": null,
    "site_count": 1,
    "other_site_towns": null,
    "street_address": null,
    "suburb": null,
    "town_city": "Dunedin",
    "region": "Otago",
    "postcode": null,
    "phone": "+64 3 479 1100",
    "email": "university@otago.ac.nz",
    "website": "https://www.otago.ac.nz",
    "latitude": -45.86467,
    "longitude": 170.513997,
    "geocode_quality": "named_campus",
    "source": "NZQA Find Education Organisations, Code of Practice signatories (CC BY 4.0); point: OpenStreetMap Nominatim, data (c) OpenStreetMap contributors (ODbL)",
    "updated": "2026-11-02"
  }
]
```

Schools in Auckland. This call returns 278 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_international_student_providers" --data-urlencode "region=Auckland" --data-urlencode "provider_type=School" | jq length
```

Category 1 private training establishments in all regions. This call returns 96 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_international_student_providers" --data-urlencode "region=*" --data-urlencode "provider_type=Private Training Establishment" --data-urlencode "nzqa_category=1" | jq -r '.[] | [.provider_name, .town_city, .region] | @tsv'
```

A box on the map around Christchurch and Banks Peninsula. This call returns 106 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_international_student_providers" --data-urlencode "region=*" --data-urlencode "latitude=-44..-43" --data-urlencode "longitude=172..173" | jq length
```

## Data source

The provider list comes from the New Zealand Qualifications Authority (NZQA) [Find Education Organisations](https://www.nzqa.govt.nz/providers/index.do) search, filtered to signatories of the Code of Practice and its provider detail pages, fetched on 2026-11-02. School type, authority, school addresses and school points come from the Ministry of Education [Schools Directory](https://catalogue.data.govt.nz/dataset/c1923d33-e781-46c9-9ea1-d9b850082be4) file dated 2026-11-02, also shown on [Education Counts](https://www.educationcounts.govt.nz/directories/list-of-nz-schools). Both are published under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), which lets anyone copy, share and adapt the data, including for commercial use, as long as the source is credited, a link to the license is given and changes are noted. NZQA's [copyright page](https://www2.nzqa.govt.nz/about-this-site/copyright/) says "NZQA materials on this website are licensed under the Creative Commons by Attribution 4.0 International Licence." Changes: one row per provider built from the site list, personal names and personal emails removed, phones put in +64 form, websites set to https, regions set to regional councils and map points added. The data is not guaranteed to be complete or accurate.

Credit lines:

* Source: New Zealand Qualifications Authority, Find Education Organisations (https://www.nzqa.govt.nz/providers/index.do). Copyright NZQA, licensed under CC BY 4.0.
* Source: Ministry of Education, Schools Directory (https://catalogue.data.govt.nz/dataset/c1923d33-e781-46c9-9ea1-d9b850082be4), licensed under CC BY 4.0.
* Map points not from the Ministry: © OpenStreetMap contributors, through Nominatim, available under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not endorsed by NZQA or the Ministry of Education. No NZQA or Ministry logos are used.

[Back to the API list](../../README.md)
