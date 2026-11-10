# Northern Ireland Care Services

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,545](https://img.shields.io/badge/records-1%2C545-lightgrey) ![updated 2026-11-11](https://img.shields.io/badge/updated-2026--11--11-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

Every care home, home care agency, day care setting, independent hospital, clinic and dental practice on the Regulation and Quality Improvement Authority (RQIA) register as at 3 August 2026: 1,545 services. Each record has the service type, address, postcode, phone, approved places where published, care categories as Yes or No flags, the registered provider when it is an organization, and the last inspection date. Choose a service type, then narrow by town, postcode, name, care category or a range of places.

Services by type: dental treatment in independent hospitals (367), domiciliary care agencies (343), nursing homes (243), residential homes (225), day care settings (150), independent hospitals (116), nursing agencies (69), independent medical agencies (13), independent clinics (11), adult placement agencies (5) and voluntary adoption agencies (3).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_ni_care_services` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per service. All matches are returned. |

## Query parameters

Required: 1 field (`service_type`). Optional: 30 fields. Disallowed: 2 fields.

`service_type` must be in every call. It accepts wildcards, such as `Residential*` or `*Dental*`. `Nursing*` also matches Nursing Agency (NA), and `Independent Hospital (IH)*` also matches dental practices. Partial filters accept `*`, such as `name=*Lodge*` or `postcode=BT9*`. `max_places` accepts ranges with two dots, such as `40..100`. All other fields are exact match strings. The text date `updated` and the `source` field cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A call without `service_type`, or a filter on a disallowed field, returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `service_type` | Required | Yes | No | Required. RQIA service type as published. Values: Independent Hospital (IH) - Dental Treatment (367); Domiciliary Care Agency (DCA) (343); Nursing (NH) (243); Residential (RC) (225); Day Care Setting (DCS) (150); Independent Hospital (IH) (116); Nursing Agency (NA) (69); Independent Medical Agency (IMA) (13); Independent Clinic (IC) (11); Adult Placement Agency (APA) (5); Voluntary Adoption Agency (VAA) (3). Use the full value, or a wildcard such as Residential* or *Dental*. |
| `service_id` | Optional | No | No | RQIA service ID as text, taken from the brackets after the name in the source. 4 to 6 digits; 412 IDs keep a leading zero, such as 021098. Unique. Exact match. |
| `name` | Optional | Yes | No | Service name as published, with the ID removed. Some dental and clinic names include the owner's surname because that is the trading name. Partial wildcards allowed, such as *Lodge* or *Supported Living*. |
| `address_line1` | Optional | Yes | No | First address line. All caps source values changed to title case. Blank in 2 rows. Partial wildcards allowed, such as *Road*. |
| `address_line2` | Optional | Yes | No | Second address line, often a street or area. Blank in 1,022 rows. Partial wildcards allowed. |
| `town` | Optional | Yes | No | Town as published, all caps values changed to title case. 159 distinct values; blank in 3 rows. Belfast has 372 rows. Both Londonderry (53) and Derry (23) appear as published. 37 rows are agency offices outside Northern Ireland, such as London (12). Partial wildcards allowed, such as Belfast or Newtown*. |
| `postcode` | Optional | Yes | No | Postcode in upper case with one space, such as BT9 7AS. 1,508 rows are BT postcodes; 37 are agency offices in Great Britain or Ireland. Partial wildcards allowed, such as BT9* or BT4*. |
| `phone` | Optional | No | No | Main landline as text in UK format, such as 028 9024 5729. Mobile numbers were removed (89 rows had only a mobile), so phone is blank in 101 rows. Where two numbers were given, the first landline is kept; extensions and menu options were dropped. 4 numbers are kept as published because they are not standard length or are Irish (+353). Exact match. |
| `max_places` | Optional | No | Yes | Maximum approved places as a number. Present in 1,019 rows, blank in 526. Nursing homes 4 to 92 (10,691 places in total); residential homes 1 to 92 (5,306); day care settings 6 to 120; dental practices 1 to 11. 27 agencies and clinics show 0. Bounds 0..120. Range uses two dots, such as 40..100. |
| `categories` | Optional | Yes | No | Raw RQIA categories of care, comma separated, with the service prefix kept, such as NH-I, NH-DE, RC-LD(E), DCS-PH or PT(L). Blank in 875 rows (most dental practices, home care agencies and nursing agencies). Partial wildcards allowed, such as *DE* or *LD(E)*. Codes: I old age; E over 65 but not old age; DE dementia; MP mental disorder excluding learning disability or dementia; LD learning disability; PH physical disability other than sensory impairment; (E) means over 65; A alcohol dependence; D drug dependence; TI terminally ill; SI sensory impairment; AH acute hospital with overnight beds; AH(DS) day surgery only; H(A) and H(C) adult and children's hospice; PT(L) class 3B or 4 lasers; PT(IL) intense light; PT(E) endoscopy; PT(IVF) in vitro fertilisation; PT(HBO) hyperbaric oxygen; PD private doctor; PD(IMA) independent medical agency. |
| `old_age` | Optional | No | No | Yes if any category is I (old age not in any other category), else No. Yes in 319 rows (298 care homes). Exact match. |
| `over_65` | Optional | No | No | Yes if any category is E (over 65 but not in the old age category), else No. Yes in 9 rows. Exact match. |
| `dementia` | Optional | No | No | Yes if any category is DE, else No. Yes in 248 rows (230 care homes). Exact match. |
| `mental_health` | Optional | No | No | Yes if any category is MP or MP(E) (mental disorder excluding learning disability or dementia), else No. Yes in 115 rows (96 care homes). Exact match. |
| `learning_disability` | Optional | No | No | Yes if any category is LD or LD(E), else No. Yes in 159 rows (126 care homes). Exact match. |
| `physical_disability` | Optional | No | No | Yes if any category is PH or PH(E), else No. Yes in 256 rows (232 care homes). Exact match. |
| `sensory_impairment` | Optional | No | No | Yes if any category is SI, else No. Yes in 22 rows. Exact match. |
| `terminally_ill` | Optional | No | No | Yes if any category is TI, else No. Yes in 82 rows (81 care homes). Exact match. |
| `alcohol_dependence` | Optional | No | No | Yes if any category is A (past or present alcohol dependence), else No. Yes in 25 rows. Exact match. |
| `drug_dependence` | Optional | No | No | Yes if any category is D (past or present drug dependence), else No. Yes in 4 rows. Exact match. |
| `acute_hospital` | Optional | No | No | Yes if a category is AH (acute hospital with overnight beds), else No. Yes in 3 rows. Exact match. |
| `day_surgery` | Optional | No | No | Yes if a category is AH(DS) (acute hospital, day surgery only), else No. Yes in 20 rows. Exact match. |
| `hospice` | Optional | No | No | Yes if a category is H(A) or H(C) (adult or children's hospice), else No. Yes in 6 rows. Exact match. |
| `lasers` | Optional | No | No | Yes if a category is PT(L) (class 3B or class 4 lasers), else No. Yes in 93 rows. Exact match. |
| `intense_light` | Optional | No | No | Yes if a category is PT(IL) (intense light sources), else No. Yes in 51 rows. Exact match. |
| `endoscopy` | Optional | No | No | Yes if a category is PT(E), else No. Yes in 8 rows. Exact match. |
| `ivf` | Optional | No | No | Yes if a category is PT(IVF) (in vitro fertilisation), else No. Yes in 2 rows. Exact match. |
| `hyperbaric_oxygen` | Optional | No | No | Yes if a category is PT(HBO), else No. Yes in 1 row. Exact match. |
| `private_doctor` | Optional | No | No | Yes if a category is PD or PD(IMA) (private doctor or independent medical agency), else No. Yes in 51 rows. Exact match. |
| `provider` | Optional | Yes | No | Registered provider when it is a company, trust, charity or other body, as published. Blank in 150 rows where the provider is a named person (mostly dental practices). Where a person trades under a business name (t/a), only the business name is kept (6 rows). 649 distinct values. Partial wildcards allowed, such as *HSC Trust* or *Ltd*. |
| `last_inspected` | Optional | Yes | No | Date of the last RQIA inspection as text YYYY-MM-DD, from 2018-07-16 to 2026-07-31. Never blank. Partial wildcards allowed, such as 2026* or 2026-07*. |
| `source` | Disallowed | No | No | Source name and license. Not filterable. |
| `updated` | Disallowed | No | No | Build date 2026-11-11, as text. Not filterable. |

## Response fields

Blank source cells are returned as `null`. `max_places` is a number. All other values are strings.

| Field | Type | Description |
|---|---|---|
| `service_id` | string | RQIA service ID as text. 4 to 6 digits; 412 IDs keep a leading zero. |
| `name` | string | Service name as published, with the ID removed. |
| `service_type` | string | RQIA service type as published. |
| `address_line1` | string | First address line. Null in 2 rows. |
| `address_line2` | string | Second address line, often a street or area. Null in 1,022 rows. |
| `town` | string | Town as published. Null in 3 rows. |
| `postcode` | string | Postcode with one space. |
| `phone` | string | Main landline in UK format. Null in 101 rows. |
| `max_places` | number | Maximum approved places. Null in 526 rows. |
| `categories` | string | Raw RQIA categories of care, comma separated. Null in 875 rows. |
| `old_age` | string | Yes or No. Yes in 319 rows. |
| `over_65` | string | Yes or No. Yes in 9 rows. |
| `dementia` | string | Yes or No. Yes in 248 rows. |
| `mental_health` | string | Yes or No. Yes in 115 rows. |
| `learning_disability` | string | Yes or No. Yes in 159 rows. |
| `physical_disability` | string | Yes or No. Yes in 256 rows. |
| `sensory_impairment` | string | Yes or No. Yes in 22 rows. |
| `terminally_ill` | string | Yes or No. Yes in 82 rows. |
| `alcohol_dependence` | string | Yes or No. Yes in 25 rows. |
| `drug_dependence` | string | Yes or No. Yes in 4 rows. |
| `acute_hospital` | string | Yes or No. Yes in 3 rows. |
| `day_surgery` | string | Yes or No. Yes in 20 rows. |
| `hospice` | string | Yes or No. Yes in 6 rows. |
| `lasers` | string | Yes or No. Yes in 93 rows. |
| `intense_light` | string | Yes or No. Yes in 51 rows. |
| `endoscopy` | string | Yes or No. Yes in 8 rows. |
| `ivf` | string | Yes or No. Yes in 2 rows. |
| `hyperbaric_oxygen` | string | Yes or No. Yes in 1 row. |
| `private_doctor` | string | Yes or No. Yes in 51 rows. |
| `provider` | string | Registered provider when it is an organization. Null in 150 rows. |
| `last_inspected` | string | Date of the last RQIA inspection, YYYY-MM-DD. |
| `source` | string | RQIA Registered Services, OpenDataNI, OGL v3.0, as at 2026-08-03. |
| `updated` | string | Build date 2026-11-11. |

## Data notes

* The source has no map points. No geocoding was added. OpenStreetMap data and Northern Ireland postcode centroids were not used.
* Current Manager was dropped, because every row named a person. Conditions was dropped, because the free text often names managers, owners or clinicians.
* Provider is null in 150 rows where the registered provider is a named person, mostly dental practices and small clinics. In 6 rows where a person trades under a business name, only the business name is kept.
* Mobile numbers were removed (89 rows had only a mobile, so phone is null in 101 rows). Where two numbers were given, the first landline is kept.
* Some service names include an owner's surname because that is the trading name of the practice. These are kept as published.
* The register is a monthly snapshot, so new, closed or changed services may differ from the live RQIA register. This is not the official register; check the RQIA website before relying on a registration.
* Children's homes are not in the source.
* `max_places` is null for most agencies and some clinics (526 rows), and 0 for 27.
* Town values are as published, so both Londonderry (53) and Derry (23) appear. 37 rows are agency offices outside Northern Ireland.
* Categories of care were split into 19 Yes or No flags, with the raw codes kept in `categories`.
* The 468 care homes (243 nursing, 225 residential) have 15,997 approved places between them (10,691 nursing, 5,306 residential).
* Changes from the source: shorter field names, the RQIA ID moved out of the name into `service_id` (leading zeros kept), stray and non breaking spaces trimmed, curly apostrophes made straight, all caps addresses and towns changed to title case, one Irish Eircode given its standard space, inspection dates written as YYYY-MM-DD text, phones set to UK format, and personal data removed as above.

## Examples

Residential homes with 40 to 100 approved places. This call returns 34 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_care_services' --data-urlencode 'service_type=Residential*' --data-urlencode max_places=40..100
```

First record:

```json
[
  {
    "service_id": "10055",
    "name": "Adelaide House",
    "service_type": "Residential (RC)",
    "address_line1": "24-26 Adelaide Park",
    "address_line2": null,
    "town": "Belfast",
    "postcode": "BT9 6FX",
    "phone": "028 9066 9362",
    "max_places": 45,
    "categories": "RC-MP(E), RC-DE, RC-I",
    "old_age": "Yes",
    "over_65": "No",
    "dementia": "Yes",
    "mental_health": "Yes",
    "learning_disability": "No",
    "physical_disability": "No",
    "sensory_impairment": "No",
    "terminally_ill": "No",
    "alcohol_dependence": "No",
    "drug_dependence": "No",
    "acute_hospital": "No",
    "day_surgery": "No",
    "hospice": "No",
    "lasers": "No",
    "intense_light": "No",
    "endoscopy": "No",
    "ivf": "No",
    "hyperbaric_oxygen": "No",
    "private_doctor": "No",
    "provider": "Presbyterian Council of Social Witness",
    "last_inspected": "2026-04-30",
    "source": "RQIA Registered Services, OpenDataNI, OGL v3.0, as at 2026-08-03",
    "updated": "2026-11-11"
  }
]
```

Nursing homes in Belfast that cover dementia. This call returns 22 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_care_services' --data-urlencode 'service_type=Nursing (NH)' --data-urlencode town=Belfast --data-urlencode dementia=Yes
```

Dental practices in Belfast. This call returns 88 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_care_services' --data-urlencode 'service_type=*Dental*' --data-urlencode town=Belfast
```

## Data source

[RQIA Registered Services](https://admin.opendatani.gov.uk/dataset/rqia-regulated-services) on OpenDataNI, current register excluding children's homes, file opendata.xlsx last updated 3 August 2026. Inspections in the file run to 31 July 2026.

Contains public sector information licensed under the [Open Government Licence v3.0 (OGL v3)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Credit: RQIA Registered Services, OpenDataNI, as at 3 August 2026. See also the [OpenDataNI terms](https://www.opendatani.gov.uk/terms-and-conditions). You may copy, share, change and use it for business if you credit the source and link the license. Logos and personal data are not covered. The license gives no warranty, and this API makes no claim of official status or endorsement by RQIA or Health and Social Care (HSC). This is not the official register; check the RQIA website before relying on a registration.
