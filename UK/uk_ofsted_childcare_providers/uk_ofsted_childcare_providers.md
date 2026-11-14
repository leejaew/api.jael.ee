# Ofsted Childcare Providers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 60,007](https://img.shields.io/badge/records-60%2C007-lightgrey) ![updated 2026-06-30](https://img.shields.io/badge/updated-2026--06--30-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

60,007 active registered childcare providers in England as at 30 June 2026, from Ofsted management information: 27,990 on non-domestic premises, 24,556 childminders, 7,182 home childcarers, 204 on domestic premises and 75 childminders without domestic premises. Each record has the official provider identifier, local authority, region, maximum places, register flags and the latest published inspection outcomes where available. Childminder and other individual names and home addresses are not included. No field is required, so you can filter by local authority, type, region, name or a places range.

Coverage is England. There are no map points, because the official file has no coordinates. `provider_urn` is the stable key, and the API does not return a generated `__id`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_ofsted_childcare_providers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per provider. All matches are returned. |

## Query parameters

Required: none. Optional: 26 fields. Disallowed: 2 fields.

A call with no filters returns all 60,007 records. Partial filters accept `*`, such as `name=*Nursery*`, `region=*Midlands*` or `provider_type=*domestic*`. `early_years_register`, `compulsory_register` and `voluntary_register` are exact matches. `places` accepts a range in the form `min..max`, for example `places=100..550`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `provider_urn` | Optional | Yes | No | Official Ofsted provider identifier, unique and never blank, so it is the stable record key. Forms in this file are 7 digit numbers (27,900), 6 digit numbers (6,231), EY followed by 6 digits (25,738), and VC followed by 6 digits (138). Text, not a number. Partial wildcards allowed, such as `EY458067`, `229140` or `VC*` (138). |
| `name` | Optional | Yes | No | Official setting name for childcare on non-domestic premises, as Ofsted lists it, such as a nursery or pre-school title. Filled on 27,973 of 27,990 non-domestic rows. Blank (`null`) on 17 non-domestic rows where Ofsted already redacted the name, and on all 32,017 childminder, home childcarer and domestic-premises rows, including 75 childminders without domestic premises whose published names were dropped. Partial wildcards allowed, such as `*Nursery*` (8,606) or `*School*` (8,180). |
| `provider_type` | Optional | Yes | No | Official provider type as published: Childcare on non-domestic premises (27,990), Childminder (24,556), Home childcarer (7,182), Childcare on domestic premises (204) or Childminder without domestic premises (75). Every row in this file is Active as at 30 June 2026; cancelled or other statuses are not included. Partial wildcards allowed, such as `Childminder` or `*domestic*` (28,269). |
| `provider_subtype` | Optional | Yes | No | Official sub-type where Ofsted records one: Full day care (14,777), Out-of-school day care (8,486), Sessional day care (5,234) or Home childcarer (1,764). Blank (`null`) on 29,746 rows. Partial wildcards allowed, such as `Full day care` or `*school*` (8,486). |
| `registration_date` | Optional | Yes | No | Official registration date, converted from the published day/month/year form to YYYY-MM-DD text, from 1934-12-14 to 2026-06-30. Never blank. Partial wildcards allowed, such as `2026*` (3,548) or `1996-06-18`. |
| `register_combination` | Optional | Yes | No | Official combination of registers as published: ALL (33,900), VCR only (10,421), EYR only (8,090), EYR-CCR (4,645), CCR-VCR (2,815), CCR only (93) or EYR-VCR (43). Never blank. Partial wildcards allowed, such as `ALL` or `*EYR*` (12,778). |
| `early_years_register` | Optional | No | No | Official Early Years Register flag: Y (46,678) or N (13,329). Never blank. Exact match. |
| `compulsory_register` | Optional | No | No | Official Compulsory Childcare Register flag: Y (41,453) or N (18,554). Never blank. Exact match. |
| `voluntary_register` | Optional | No | No | Official Voluntary Childcare Register flag: Y (47,179) or N (12,828). Never blank. Exact match. |
| `local_authority` | Optional | Yes | No | Local authority as Ofsted lists it, such as Hertfordshire or Birmingham. Never blank. 154 values, including Not Recorded (25). The largest are Hertfordshire (1,937), Surrey (1,896), Hampshire (1,885), Kent (1,806) and Essex (1,712). The smallest named authorities are Isles of Scilly (2), City of London (9) and Rutland (30). Partial wildcards allowed, such as `Hertfordshire` or `*Sussex*` (1,469). |
| `constituency` | Optional | Yes | No | Parliamentary constituency as Ofsted lists it. Never blank. 544 values. This is area-level information that Ofsted publishes for childminders and other individual providers. Partial wildcards allowed, such as `*Hampstead*` (137) or `Birmingham*` (919). |
| `region` | Optional | Yes | No | English region as Ofsted lists it, such as London or South East. Never blank. 9 regions plus Not Recorded (25). The largest are London (13,528), South East (11,524) and East of England (7,351). The smallest named region is North East (1,736). Partial wildcards allowed, such as `London` or `*Midlands*` (9,108). |
| `ofsted_region` | Optional | Yes | No | Ofsted region as published. This groups North East with Yorkshire and the Humber as one value, North East, Yorkshire and the Humber (6,558). Other values match `region`, including Not Recorded (25). Partial wildcards allowed, such as `*Yorkshire*` (6,558) or `London`. |
| `deprivation_band` | Optional | Yes | No | Official deprivation band: Least deprived (12,600), Less deprived (12,816), Average (12,281), Deprived (11,863), Most deprived (10,427) or Unknown (20). Never blank. Partial wildcards allowed, such as `Most deprived` (10,427) or `*deprived*` (47,706). |
| `town` | Optional | Yes | No | Town of the non-domestic setting, as Ofsted lists it. Filled on 27,973 non-domestic rows. Blank (`null`) on 17 non-domestic rows where Ofsted already redacted the address, and on all childminder, home childcarer and domestic-premises rows. Partial wildcards allowed, such as `Manchester` (415) or `*upon*` (265). |
| `address` | Optional | Yes | No | Setting street address for childcare on non-domestic premises, built from the official address lines, town and postcode. Filled on 27,973 non-domestic rows. Blank (`null`) on 17 non-domestic rows where Ofsted already redacted the address, and on all childminder, home childcarer and domestic-premises rows, because those are home or individual addresses; Ofsted already withholds them for most of those rows, and the 75 published addresses for childminders without domestic premises were dropped. Partial wildcards allowed, such as `*School*` (8,324) or `*Lane*` (3,563). |
| `postcode` | Optional | Yes | No | UK postcode of the non-domestic setting, as published, with a space, such as B1 1AA. Text, not a number. Filled on 27,973 non-domestic rows. Blank (`null`) on the same rows as `address`. Partial wildcards allowed, such as `B1*` (187) or `*1AA` (22). |
| `places` | Optional | No | Yes | Official maximum places as an integer, 0 to 550, never blank. The 60,007 providers have 1,305,732 places in total. 13,936 rows are 0, including every home childcarer (7,182). Range form is `min..max` with two dots, such as `places=100..550` (1,721) or `places=0..0` (13,936). A hyphen range such as `100-550` returns HTTP 400. |
| `overall_effectiveness` | Optional | Yes | No | Latest Early Years Register full inspection overall effectiveness from the official OEIF/CIF field, as published: 1 (4,091, outstanding), 2 (27,823, good), 3 (214, requires improvement) or 4 (82, inadequate). Blank (`null`) on 27,790 rows with no such inspection. 7 official values are `00:00.0`, a source defect, and were left as published. This field is text, not a number, so those 7 values survive. Partial wildcards allowed, such as `1` or `4`. |
| `inspection_date` | Optional | Yes | No | Date of that OEIF/CIF full inspection, YYYY-MM-DD text. Filled on 32,217 rows, matching the overall effectiveness field except that the 7 defective grade rows still have a date. Dates in this field run to 2025. Partial wildcards allowed, such as `2025*` (6,573). |
| `safeguarding` | Optional | Yes | No | Official OEIF safeguarding is effective flag: Yes (31,104), No (64) or - (1,049). Blank (`null`) on 27,790 rows with no OEIF inspection. Partial wildcards allowed, such as `Yes` or `No`. |
| `ncor_overall` | Optional | Yes | No | Latest official outcome for an Early Years Register inspection with no children on roll, or no children present: Met (1,942) or Not met (with actions) (15). Blank (`null`) on 58,050 rows. Partial wildcards allowed, such as `Met` or `Not met*` (15). |
| `oosc_overall` | Optional | Yes | No | Latest official outcome for an Early Years Register out-of-school day care inspection: Met (2,729), Not met (with actions) (26) or Not met (enforcement) (5). Blank (`null`) on 57,247 rows. Partial wildcards allowed, such as `Met` or `*enforcement*` (5). |
| `cr_compliance` | Optional | Yes | No | Latest official Childcare Register compliance outcome: Met (4,971), Not met (with actions) (483) or Not met (enforcement) (22). Filled on 5,476 rows. Partial wildcards allowed, such as `Met` or `*actions*` (483). |
| `reif_visit_type` | Optional | Yes | No | Latest official Early Years Register inspection visit type under the newer REIF framework, filled on 3,590 rows: Early years register (full Inspection) (3,001), Early years register (Out-of-school day care) (382) or Early years register (No children on roll/No children present) (207). These rows have no OEIF overall effectiveness, because a provider is not given both in this file. Partial wildcards allowed, such as `*full*` (3,001) or `*Out-of-school*` (382). |
| `reif_inspection_date` | Optional | Yes | No | Date of that REIF inspection, YYYY-MM-DD text, filled on the same 3,590 rows. Partial wildcards allowed, such as `2026*` (2,977). |
| `source` | Disallowed | No | No | Ofsted, Childcare providers and inspections (management information), England, in all 60,007 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | As at date of the official providers file, YYYY-MM-DD (`2026-06-30`). The GOV.UK dataset page was last updated on 6 August 2026. The file's HTTP Last-Modified header is 31 July 2026 13:36:43 GMT (9:36 pm on 31 July 2026, Asia/Singapore time). Text. Not filterable. |

## Response fields

`places` is an integer. All other values are strings. Blank cells are returned as `null`. There is no `__id` field. Use `provider_urn` to identify a provider.

| Field | Type | Description |
|---|---|---|
| `provider_urn` | string | Official Ofsted provider identifier, unique on every row. |
| `name` | string | Official setting name for non-domestic childcare, or `null` when withheld or dropped. |
| `provider_type` | string | Official provider type, as published. |
| `provider_subtype` | string | Official sub-type, or `null` when Ofsted does not record one. |
| `registration_date` | string | Official registration date, YYYY-MM-DD. |
| `register_combination` | string | Official combination of registers, as published. |
| `early_years_register` | string | Official Early Years Register flag, Y or N. |
| `compulsory_register` | string | Official Compulsory Childcare Register flag, Y or N. |
| `voluntary_register` | string | Official Voluntary Childcare Register flag, Y or N. |
| `local_authority` | string | Local authority as Ofsted lists it. |
| `constituency` | string | Parliamentary constituency as Ofsted lists it. |
| `region` | string | English region as Ofsted lists it. |
| `ofsted_region` | string | Ofsted region as published. |
| `deprivation_band` | string | Official deprivation band. |
| `town` | string | Town of the non-domestic setting, or `null` when withheld or dropped. |
| `address` | string | Non-domestic street address, or `null` when withheld or dropped. |
| `postcode` | string | Non-domestic postcode, or `null` when withheld or dropped. |
| `places` | integer | Official maximum places, 0 to 550. |
| `overall_effectiveness` | string | Latest OEIF/CIF overall effectiveness (1, 2, 3 or 4), or `null`. |
| `inspection_date` | string | Date of that OEIF/CIF inspection, YYYY-MM-DD, or `null`. |
| `safeguarding` | string | Official OEIF safeguarding is effective flag, or `null`. |
| `ncor_overall` | string | Latest official no-children-on-roll inspection outcome, or `null`. |
| `oosc_overall` | string | Latest official out-of-school day care inspection outcome, or `null`. |
| `cr_compliance` | string | Latest official Childcare Register compliance outcome, or `null`. |
| `reif_visit_type` | string | Latest official REIF visit type, or `null`. |
| `reif_inspection_date` | string | Date of that REIF inspection, YYYY-MM-DD, or `null`. |
| `source` | string | Ofsted, Childcare providers and inspections (management information), England. |
| `source_as_of` | string | As at date of the official providers file, `2026-06-30`. |

## Examples

Longwood Nursery in Hertfordshire, looked up by official provider identifier EY563409. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ofsted_childcare_providers' --data-urlencode 'provider_urn=EY563409'
```

```json
[
  {
    "provider_urn": "EY563409",
    "name": "Longwood Nursery",
    "provider_type": "Childcare on non-domestic premises",
    "provider_subtype": "Full day care",
    "registration_date": "2018-07-23",
    "register_combination": "EYR only",
    "early_years_register": "Y",
    "compulsory_register": "N",
    "voluntary_register": "N",
    "local_authority": "Hertfordshire",
    "constituency": "Watford",
    "region": "East of England",
    "ofsted_region": "East of England",
    "deprivation_band": "Less deprived",
    "town": "Bushey",
    "address": "Longwood School, Bushey Hall Drive, Bushey, WD23 2QG",
    "postcode": "WD23 2QG",
    "places": 120,
    "overall_effectiveness": "2",
    "inspection_date": "2022-03-16",
    "safeguarding": "Yes",
    "ncor_overall": null,
    "oosc_overall": null,
    "cr_compliance": null,
    "reif_visit_type": null,
    "reif_inspection_date": null,
    "source": "Ofsted, Childcare providers and inspections (management information), England",
    "source_as_of": "2026-06-30"
  }
]
```

Childminders. This call returns 24,556 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ofsted_childcare_providers' --data-urlencode 'provider_type=Childminder'
```

Providers in London. This call returns 13,528 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ofsted_childcare_providers' --data-urlencode 'region=London'
```

Providers whose official name includes Nursery. This call returns 8,606 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ofsted_childcare_providers' --data-urlencode 'name=*Nursery*'
```

Providers with 100 to 550 places. This call returns 1,721 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ofsted_childcare_providers' --data-urlencode 'places=100..550'
```

Providers whose latest Early Years Register overall effectiveness is 1 (outstanding). This call returns 4,091 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ofsted_childcare_providers' --data-urlencode 'overall_effectiveness=1'
```

## Data source

Source: Ofsted, [Childcare providers and inspections (management information)](https://www.gov.uk/government/statistical-data-sets/childcare-providers-and-inspections-management-information), England, the providers file as at 30 June 2026. The [providers CSV](https://assets.publishing.service.gov.uk/media/6a6ca4eb0c36759b5ccaa2e0/Management_information_-_childcare_providers_and_inspections_-_childcare_providers_data_as_at_30_June_2026.csv) was last modified on 31 July 2026 at 1:36:43 pm UTC (9:36 pm on 31 July 2026, Asia/Singapore time). The GOV.UK dataset page was last updated on 6 August 2026. This is the most recent providers-level file on that page. The latest published official statistics edition there is as at 31 March 2026. Official statistics as at 31 August 2026 had been announced for November 2026 but were not yet on the page as of 15 November 2026.

Contains public sector information licensed under the Open Government Licence v3.0.

The data is used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). The information is Crown copyright. You may copy, share, adapt and use it for any purpose, including commercial use, if you use that attribution and link to the licence. If those conditions are not met, the rights granted under the licence end automatically. The licence does not cover personal data, or logos and other listed exemptions. The data comes with no warranty. This is a register of active providers and published inspection outcomes, not a recommendation, and it is not advice about any setting. Use of this data does not mean Ofsted endorses this API or its maker.

Childminder and other individual names and home addresses are not included. Ofsted already withholds name, street, town and postcode for childminders (24,556), home childcarers (7,182) and childcare on domestic premises (204), and publishes only area-level location (local authority, constituency, region, Ofsted region and deprivation band). Those official redactions were kept blank. The same area-level rule was applied to 75 childminders without domestic premises, whose names and published addresses were dropped. Registered person names were omitted. 17 non-domestic rows are already redacted by Ofsted for name and address, and were left blank. The official providers file has no phone, email or website columns.

Seven official `overall_effectiveness` values are `00:00.0` rather than 1, 2, 3 or 4, and were left as published. 25 rows have local authority, region and Ofsted region as Not Recorded. Provider URNs use four official forms, including a VC prefix on 138 rows.

This is a snapshot of the official providers file as at 30 June 2026. For the latest file, visit the [GOV.UK dataset page](https://www.gov.uk/government/statistical-data-sets/childcare-providers-and-inspections-management-information).

[Back to the API list](../../README.md)
