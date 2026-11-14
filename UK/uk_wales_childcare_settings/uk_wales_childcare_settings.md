# Wales Childcare Settings

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 2,687](https://img.shields.io/badge/records-2%2C687-lightgrey) ![updated 2026-11-14](https://img.shields.io/badge/updated-2026--11--14-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

2,687 registered childcare and play settings in Wales from the Care Inspectorate Wales directory: 1,682 children's day care settings (1,149 full day care, 265 sessional, 249 out of school, 11 open access play and 8 creches) and 1,005 childminders. Each record has the official service identifier, local authority, town, maximum places, language, Flying Start and childcare offer flags, and inspection ratings where published. Childminder personal names and home street addresses are left out. No field is required, so you can filter by local authority, type, name, language or a places range.

The official type spelling is Childrens Day Care, without an apostrophe. There are no map points, because the official export has no coordinates. `service_urn` is the stable key, and the API does not return a generated `__id`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_wales_childcare_settings` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per setting. All matches are returned. |

## Query parameters

Required: none. Optional: 21 fields. Disallowed: 2 fields.

A call with no filters returns all 2,687 records. Partial filters accept `*`, such as `name=*Meithrin*`, `local_authority=*Gwent*` or `ratings=*Poor*`. `flying_start`, `childcare_offer`, `early_years_education` and `provides_care_during` are exact matches. `places` accepts a range in the form `min..max`, for example `places=100..297`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `service_urn` | Optional | Yes | No | Official Care Inspectorate Wales service identifier, unique and never blank, so it is the stable record key. Two official forms appear: `SIN-` followed by 8 digits, such as SIN-00000692 (1,280 rows), and the same form with a 4 letter suffix, such as SIN-00016462-WTJQ (1,407 rows). Text, not a number. Partial wildcards allowed, such as `SIN-00000692` or `SIN-00016*` (128). |
| `name` | Optional | Yes | No | Official setting name, as CIW lists it, such as a day nursery or Cylch Meithrin title. The source has no separate English and Welsh name columns. Blank (`null`) on all 1,005 childminder rows, because the official service name is a personal name and was dropped. Day care names are never blank; 27 day care names appear on more than one row (same title at more than one site). Partial wildcards allowed, such as `*Meithrin*` (347), `*Cylch*` (298) or `*Nursery*` (325). |
| `service_type` | Optional | Yes | No | Official service type as published: Childrens Day Care (1682) or Child Minder (1005). The official spelling is Childrens without an apostrophe. This table is only those two types; care homes and other CIW services were not included. Partial wildcards allowed, such as `Childrens Day Care` or `Child Minder`. |
| `service_sub_type` | Optional | Yes | No | Official sub-type for day care: Full Day Care (1149), Sessional Day Care (265), Out of School Care (249), Open Access Play Provision (11) or Creche (8). Blank (`null`) on all 1,005 childminder rows (the official file says None). Partial wildcards allowed, such as `Full Day Care` or `*School*` (249). |
| `local_authority` | Optional | Yes | No | Welsh local authority as CIW lists it, such as Cardiff or Rhondda Cynon Taf. 22 named authorities. The largest are Cardiff (303), Rhondda Cynon Taf (179), Carmarthenshire (175), Flintshire (163) and Caerphilly (163). The smallest are Merthyr Tydfil (29) and Blaenau Gwent (50). Blank (`null`) on 1 day care row (SIN-00016462-WTJQ), as published. Every childminder row has an authority. Partial wildcards allowed, such as `Cardiff` or `*Gwent*` (50). |
| `town` | Optional | Yes | No | Town or city as CIW lists it, such as Cardiff or Caernarfon. Never blank. For childminders this is the area-level location the official file publishes; no street is given. Partial wildcards allowed, such as `Cardiff` or `*well*` (53). |
| `address` | Optional | Yes | No | Setting street address for day care, built from the official address lines, town and postcode, such as a school or nursery site. Never blank on day care rows. Blank (`null`) on all 1,005 childminder rows: the official export already withholds street, postcode and UPRN for childminders, and no home address was added. Partial wildcards allowed, such as `*School*` (525) or `*Lane*` (143). |
| `postcode` | Optional | Yes | No | UK postcode of the day care setting, as published, usually with a space, such as CF10 1EP. Text, not a number. Filled on all 1,682 day care rows; 3 have no space, as published. Blank (`null`) on all childminder rows. Partial wildcards allowed, such as `CF10*` (10) or `*3PY`. |
| `uprn` | Optional | Yes | No | Official Unique Property Reference Number for the day care setting, as published, 8, 11 or 12 digits. Text, not a number, and not a map coordinate. Filled on 1,681 of 1,682 day care rows; blank (`null`) on SIN-00016437-WGMN, as published, and on all childminder rows. Partial wildcards allowed, such as `10013704778`. |
| `phone` | Optional | Yes | No | Official setting landline for day care, as CIW lists it. Filled on 893 day care rows. Blank (`null`) on 789 day care rows and on all 1,005 childminder rows. Day care mobile numbers published as the setting phone were omitted, and every childminder phone was omitted because those numbers belong to individuals. Partial wildcards allowed, such as `029*` (114) or `01407*` (11). |
| `website` | Optional | Yes | No | Setting website as CIW lists it, for day care only, such as a nursery site. Some official values have no http prefix. Filled on 685 day care rows. Blank (`null`) on all childminder rows. Partial wildcards allowed, such as `www.*` (477) or `*.gov.uk*` (4). |
| `language` | Optional | Yes | No | Main operating language as published: English (1950) or Welsh (418). Blank (`null`) on 319 rows. Partial wildcards allowed, such as `Welsh` or `English`. |
| `flying_start` | Optional | No | No | Official Flying Start provider flag: Yes (1114) or No (1534). Blank (`null`) on 39 rows. Exact match. |
| `childcare_offer` | Optional | No | No | Official flag for a setting registered to deliver the childcare offer: Yes (2331) or No (315). Blank (`null`) on 41 rows. Exact match. |
| `early_years_education` | Optional | No | No | Official flag for a setting that delivers early years part time education: Yes (509) or No (2139). Blank (`null`) on 39 rows. Exact match. |
| `places` | Optional | No | Yes | Maximum number of places as an integer, 3 to 297, never blank. The 2,687 settings have 74,004 places in total. Childminder places run from 3 to 18. Range form is `min..max` with two dots, such as `places=100..297` (59) or `places=3..6` (256). A hyphen range such as `100-297` returns HTTP 400. |
| `ratings` | Optional | Yes | No | Latest published inspection ratings as one official string, covering Well-being, Care and Development, Environment, and Leadership and Management, each Excellent, Good, Adequate or Poor, such as `Well-being: Good; Care and Development: Good; Environment: Good; Leadership and Management: Good`. Filled on 2,101 rows (1,257 day care and 844 childminders). 9 official strings repeat the four themes twice, and 1 more repeats three of the themes; those were left as published. Partial wildcards allowed, such as `*Poor*` (28) or `*Excellent*` (606). |
| `provides_care_during` | Optional | No | No | Official period of care: Term time and holidays (1782), Term time only (850) or Holidays only (45). Blank (`null`) on 10 rows. Exact match. |
| `provider_type` | Optional | Yes | No | Official legal form of the day care provider, such as Limited Company (586), Charitable Incorporated Organisation (416) or Local Authority (116). Blank (`null`) on all 1,005 childminders and on 319 day care rows that have no provider record in the official file. Partial wildcards allowed, such as `Limited Company` or `*Charit*` (569). |
| `company_number` | Optional | Yes | No | UK company number as published, for some day care providers. Text, not a number. 680 are 8 digits; 15 have a 2 letter prefix; 1 is a 3 character non-numeric value, left as published. Blank (`null`) on all childminders and on day care rows with no company number. Partial wildcards allowed, such as `0*` (445) or `CE*` (9). |
| `charity_number` | Optional | Yes | No | Charity number as published, for some day care providers. Text, not a number. Most are 7 digits. 2 official values have a trailing full stop, left as published. Blank (`null`) on all childminders and on day care rows with no charity number. Partial wildcards allowed, such as `1*` (470). |
| `source` | Disallowed | No | No | Care Inspectorate Wales, Register of childcare and play settings, Wales, in all 2,687 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the official directory export, YYYY-MM-DD (`2026-11-14`). The official DataExportDetails timestamp is 14 November 2026, 6:51:55 pm UTC (2:51 am on 15 November 2026, Asia/Singapore time). Text. Not filterable. |

## Response fields

`places` is an integer. All other values are strings. Blank cells are returned as `null`. There is no `__id` field. Use `service_urn` to identify a setting.

| Field | Type | Description |
|---|---|---|
| `service_urn` | string | Official CIW service identifier, unique on every row. |
| `name` | string | Official setting name, or `null` on childminder rows. |
| `service_type` | string | Official type: Childrens Day Care or Child Minder. |
| `service_sub_type` | string | Official day care sub-type, or `null` on childminder rows. |
| `local_authority` | string | Welsh local authority, or `null` on the one day care row published without one. |
| `town` | string | Town or city as CIW lists it. |
| `address` | string | Day care street address, or `null` on childminder rows. |
| `postcode` | string | Day care postcode, or `null` on childminder rows. |
| `uprn` | string | Official Unique Property Reference Number, or `null` when not published. |
| `phone` | string | Day care landline, or `null` when no landline was published. |
| `website` | string | Day care website, or `null` when none was published. |
| `language` | string | Main operating language, English or Welsh, or `null`. |
| `flying_start` | string | Official Flying Start flag, Yes or No, or `null`. |
| `childcare_offer` | string | Official childcare offer flag, Yes or No, or `null`. |
| `early_years_education` | string | Official early years education flag, Yes or No, or `null`. |
| `places` | integer | Maximum number of places, 3 to 297. |
| `ratings` | string | Latest published inspection ratings as one official string, or `null`. |
| `provides_care_during` | string | Official period of care, or `null`. |
| `provider_type` | string | Official legal form of the day care provider, or `null`. |
| `company_number` | string | UK company number as published, or `null`. |
| `charity_number` | string | Charity number as published, or `null`. |
| `source` | string | Care Inspectorate Wales, Register of childcare and play settings, Wales. |
| `source_as_of` | string | Date of the official directory export, `2026-11-14`. |

## Examples

7-2-11 Club in Hawarden, looked up by official service identifier SIN-00000692. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_childcare_settings' --data-urlencode 'service_urn=SIN-00000692'
```

```json
[
  {
    "service_urn": "SIN-00000692",
    "name": "7-2-11 Club",
    "service_type": "Childrens Day Care",
    "service_sub_type": "Out of School Care",
    "local_authority": "Flintshire",
    "town": "Hawarden",
    "address": "Hawarden Village Church in Wales Primary School, Cross Tree Lane, Hawarden, CH5 3PY",
    "postcode": "CH5 3PY",
    "uprn": "10013704778",
    "phone": null,
    "website": "hawardenvillage.net",
    "language": "English",
    "flying_start": "No",
    "childcare_offer": "Yes",
    "early_years_education": "No",
    "places": 60,
    "ratings": "Well-being: Excellent; Care and Development: Good; Environment: Good; Leadership and Management: Good",
    "provides_care_during": "Term time and holidays",
    "provider_type": null,
    "company_number": null,
    "charity_number": null,
    "source": "Care Inspectorate Wales, Register of childcare and play settings, Wales",
    "source_as_of": "2026-11-14"
  }
]
```

Children's day care settings, using the official spelling Childrens Day Care. This call returns 1,682 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_childcare_settings' --data-urlencode 'service_type=Childrens Day Care'
```

Settings in Cardiff. This call returns 303 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_childcare_settings' --data-urlencode 'local_authority=Cardiff'
```

Settings whose official name includes Meithrin. This call returns 347 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_childcare_settings' --data-urlencode 'name=*Meithrin*'
```

Settings whose main operating language is Welsh. This call returns 418 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_childcare_settings' --data-urlencode 'language=Welsh'
```

Settings with 100 to 297 places. This call returns 59 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_childcare_settings' --data-urlencode 'places=100..297'
```

## Data source

Source: Care Inspectorate Wales, [Register of childcare and play settings](https://digital.careinspectorate.wales/api/DataExport), from the official directory export dated 14 November 2026 (DataExportDetails timestamp 14 November 2026, 6:51:55 pm UTC, which is 2:51 am on 15 November 2026, Asia/Singapore time). Export details are at [DataExportDetails](https://digital.careinspectorate.wales/api/DataExportDetails). The CIW [Our data](https://www.careinspectorate.wales/our-data) page names the licence and links to it.

Contains public sector information licensed under the Open Government Licence v3.0.

The data is used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). You may copy, share, adapt and use the information for any purpose, including commercial use, if you use that attribution and link to the licence. If those conditions are not met, the rights granted under the licence end automatically. The licence does not cover personal data, or logos and other listed exemptions. The data comes with no warranty. This is a register of registered settings and published ratings, not a recommendation, and it is not advice about any setting. Use of this data does not mean Care Inspectorate Wales endorses this API or its maker.

Childminder personal names and home street addresses are left out. The official export already withholds street, postcode and UPRN for childminders and publishes only town and local authority; that area-level location was kept, and no home address was added. Childminder phones and websites were also omitted, because they belong to individuals. For day care settings, organization and setting names, site addresses, official landlines and websites were kept. Personal names of responsible individuals were omitted. Day care mobile numbers published as the setting phone were omitted. Emails are not in this API.

One day care row has a blank local authority (SIN-00016462-WTJQ). One day care row has a blank UPRN (SIN-00016437-WGMN). Three day care postcodes have no space. Nine official rating strings repeat the four themes twice, and one more repeats three of the themes. Two charity numbers have a trailing full stop, and one company number is a 3 character non-numeric value. Those rows were left as published.

This is a snapshot of the official directory export dated 14 November 2026. For the latest file, visit the [CIW data export](https://digital.careinspectorate.wales/api/DataExport).

[Back to the API list](../../README.md)
