# New Zealand Tertiary Providers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 430](https://img.shields.io/badge/records-430-lightgrey) ![updated 2026-11-02](https://img.shields.io/badge/updated-2026--11--02-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

430 tertiary education providers in New Zealand from the Ministry of Education Education Counts directory, as listed on 2 November 2026. The list has 400 private training establishments, 17 institutes of technology and polytechnics (including Te Pukenga), 8 universities, 3 wananga and 2 other providers. Each record has the official institution number, name, telephone, website, type, definition, funding type, authority, street address, postal address, territorial authority and regional council. No field is required, so you can filter by region, city, type, funding or name.

This is the Education Counts tertiary providers directory. It is a different list from the NZQA Code of Practice signatories in [`nz_international_student_providers`](../nz_international_student_providers/nz_international_student_providers.md). The directory spells Wananga without a macron.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_tertiary_providers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per provider. All matches are returned. |

## Query parameters

Required: none. Optional: 17 fields. Disallowed: 2 fields.

A call with no filters returns all 430 records. Partial filters accept `*`, such as `name=*University*`, `city=*Wellington*` or `website=*ac.nz*`. `type`, `funding_type`, `authority` and `regional_council` are exact match, using the full official wording. Text filters ignore case. No field accepts a range. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `institution_number` | Optional | Yes | No | Official unique number assigned to each institution, such as `7001` (University of Auckland) or `6006` (Ara Institute of Canterbury). Never blank. All 430 values are unique. 420 numbers have four digits and 10 have five digits. Kept as text. Partial wildcards allowed, such as `7001` (1) or `70*` (27, the eight universities and 19 private training establishments numbered in the 7000s). |
| `name` | Optional | Yes | No | Full name of the institution as the directory lists it, such as University of Otago or Te Wananga O Aotearoa. Never blank. 17 names include `Proposed PTE`. Some of those are placeholders such as Proposed PTE 6031, and some already have a trading name, such as Institute of Data (Proposed PTE). Partial wildcards allowed, such as `*University*` (8), `*Wananga*` (5) or `*Proposed PTE*` (17). Matching is case insensitive. |
| `telephone` | Optional | Yes | No | Institution telephone as the directory lists it, kept as text, including area codes, `0800` and `0508` numbers, and one Cook Islands number (`+682 29357`). Blank on 71 rows. New Zealand mobile numbers were omitted. Partial wildcards allowed, such as `09*` (132) or `0800*` (26). |
| `website` | Optional | Yes | No | Institution website as the directory lists it, such as `http://www.otago.ac.nz` or `https://toiohomai.ac.nz/`. Blank on 51 rows. Two broken values that began `http://https//` were corrected to `https://`. Other addresses were left as published, including those that still use `http`. Partial wildcards allowed, such as `*ac.nz*` (130). |
| `type` | Optional | No | No | Institution type in five official values: Private Training Establishment (400), Institute of Technology/Polytechnic (17, including Te Pukenga - New Zealand Institute of Skills and Technology and Te Pukenga - BCITO, Connexis, Competenz, ServiceIQ, MITO, Careerforce, Primary ITO, HITO, EarnLearn), University (8), Wananga (3: Te Wananga O Aotearoa, Te Wananga O Raukawa and Te Whare Wananga O Awanuiarangi) or Other Tertiary Education Provider (2: Hagley Community College YG and Tokomairiro High School YG). Exact match, using that full wording. |
| `definition` | Optional | Yes | No | Further description of the type. `Not Applicable` on 150 rows, including all universities and all institutes of technology and polytechnics. Other counts: First Aid/Safety (33), English Language School (33), Business Management (32), Trades and Services (30), Travel Agency Training/Tourism/Hospitality/Retail (20), Agriculture/Horticulture/Science (16), Aviation (15), Health Therapy (15), Computer Skills (13), General (12), Theological (12), Hairdressing/Beauty Therapy/Fashion (9), Teacher Education (9), Recreation/Fitness (7), Counselling/Social Work (5), Art/Design/Craft (5), Security Industry (3), Early Childhood Education & Care / NannyTraining (3), Dance/Drama (3), Mass Communication (2), and one each of Film and Television, Equine and Maori Studies. Wording follows the source, including `NannyTraining`. Partial wildcards allowed, such as `*English Language*` (33). |
| `funding_type` | Optional | No | No | Funding category in four official values: Miscellaneous (Non-funded) (218), Student Achievement Component (SAC) funding (178), Training for Designated Groups funding (includes Youth Guarantee) (30) or Non-funded (Voluntary SDR) (4). Exact match, using that full wording. |
| `authority` | Optional | No | No | Ownership, operation or registration status in four official values: Privately Owned Tertiary Institution (363), Public Tertiary Institution (28), Tertiary institution owned by a Trust (22) or Tertiary institution owned by an Incorporated Society (17). Exact match, using that full wording. |
| `street` | Optional | Yes | No | Street line of the location address, such as 24 Princes Street (University of Auckland) or Ngatangiia Campus. Blank on 22 rows, including all 17 proposed PTEs. Partial wildcards allowed. |
| `suburb` | Optional | Yes | No | Suburb of the location address. Blank on 142 rows. Partial wildcards allowed, such as `*Te Aro*` (5). |
| `city` | Optional | Yes | No | Town or city of the location address. Largest counts: Auckland (148), Christchurch (35). Blank on 23 rows. Matching is case insensitive. Partial wildcards allowed, such as `*Wellington*` (31). |
| `postal_address_1` | Optional | Yes | No | First line of the postal address, such as a PO Box, Private Bag or rural delivery number. Blank on 19 rows. Partial wildcards allowed, such as `PO Box*` (266). |
| `postal_address_2` | Optional | Yes | No | Second line of the postal address, often a suburb. Blank on 211 rows. Partial wildcards allowed. |
| `postal_address_3` | Optional | Yes | No | Third line of the postal address, often a town or city. Blank on 21 rows. Partial wildcards allowed. |
| `postcode` | Optional | Yes | No | Postcode for the postal address, kept as text so leading zeros stay in place. This is the postal postcode from the directory, not always the street address postcode. Blank on 60 rows. 38 values start with 0. Partial wildcards allowed, such as `1010` (8) or `80*` (6). |
| `territorial_authority` | Optional | Yes | No | Territorial authority (city or district council), with Auckland local boards written as `Auckland -` followed by the board name, such as Auckland - Otara-Papatoetoe. Blank on 30 rows. Trailing spaces in the source were trimmed. Partial wildcards allowed, such as `*Auckland*` (153) or `*Queenstown-Lakes*` (6). |
| `regional_council` | Optional | No | No | Regional council as the directory lists it, including the word Region. Counts: Auckland Region (153), Canterbury Region (46), Wellington Region (41), Waikato Region (36), Bay of Plenty Region (31), Manawatu-Whanganui Region (23), Otago Region (19), Northland Region (11), Nelson Region (10), Hawke's Bay Region (9), Taranaki Region (9), Southland Region (3), West Coast Region (3), Tasman Region (3), Gisborne Region (3). Blank on 30 rows, including the 17 proposed PTEs, Cook Islands Tertiary Training Institute, and a few providers with incomplete addresses. There are no Marlborough Region rows in this snapshot. Exact match, using that full wording. |
| `source` | Disallowed | No | No | Ministry of Education, Education Counts, Tertiary providers directory, in all 430 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the directory snapshot, YYYY-MM-DD (`2026-11-02`). Text. Not filterable. |

## Response fields

`__id` is an integer. All other values are strings, or `null` when the directory has no value, or when a New Zealand mobile telephone number was omitted. `institution_number` is the official public key.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `institution_number` | string | Official institution number, unique on every row. |
| `name` | string | Full name of the institution as the directory lists it. |
| `telephone` | string | Institution telephone, or `null` on 71 rows. |
| `website` | string | Institution website, or `null` on 51 rows. |
| `type` | string | Official institution type. |
| `definition` | string | Further description of the type, or `Not Applicable`. |
| `funding_type` | string | Official funding category. |
| `authority` | string | Official ownership, operation or registration status. |
| `street` | string | Street line of the location address, or `null` on 22 rows. |
| `suburb` | string | Suburb, or `null` on 142 rows. |
| `city` | string | Town or city, or `null` on 23 rows. |
| `postal_address_1` | string | First line of the postal address, or `null` on 19 rows. |
| `postal_address_2` | string | Second line of the postal address, or `null` on 211 rows. |
| `postal_address_3` | string | Third line of the postal address, or `null` on 21 rows. |
| `postcode` | string | Postal postcode, kept as text, or `null` on 60 rows. |
| `territorial_authority` | string | Territorial authority, or `null` on 30 rows. |
| `regional_council` | string | Regional council, or `null` on 30 rows. |
| `source` | string | Ministry of Education, Education Counts, Tertiary providers directory. |
| `source_as_of` | string | Date of the snapshot, `2026-11-02`. |

## Examples

University of Auckland, by official institution number. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_tertiary_providers' --data-urlencode 'institution_number=7001'
```

```json
[
  {
    "__id": 82,
    "institution_number": "7001",
    "name": "University of Auckland",
    "telephone": "09-3737999",
    "website": "http://www.auckland.ac.nz",
    "type": "University",
    "definition": "Not Applicable",
    "funding_type": "Student Achievement Component (SAC) funding",
    "authority": "Public Tertiary Institution",
    "street": "24 Princes Street",
    "suburb": "Auckland Central",
    "city": "Auckland",
    "postal_address_1": "Private Bag 92019",
    "postal_address_2": null,
    "postal_address_3": "Auckland",
    "postcode": "1142",
    "territorial_authority": "Auckland - Waitemata",
    "regional_council": "Auckland Region",
    "source": "Ministry of Education, Education Counts, Tertiary providers directory",
    "source_as_of": "2026-11-02"
  }
]
```

The eight universities, by official type. This call returns 8 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_tertiary_providers' --data-urlencode 'type=University'
```

Providers in the Auckland Region. This call returns 153 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_tertiary_providers' --data-urlencode 'regional_council=Auckland Region'
```

Proposed private training establishments. This call returns 17 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_tertiary_providers' --data-urlencode 'name=*Proposed PTE*'
```

## Data source

Source: Ministry of Education, Education Counts, [Tertiary providers directory](https://www.educationcounts.govt.nz/directories/list-of-tertiary-providers). The listing page, checked on 15 November 2026, says "Latest directory update: November 2026". The official file used for this snapshot is [tertiary-providers-directory-02112026.csv](https://www.educationcounts.govt.nz/__data/assets/file/0005/62573/tertiary-providers-directory-02112026.csv). The CSV itself is titled "Directory for Tertiary Education Providers" and says the report data was last updated on 2 November 2026. The list is free to use on the Education Counts website.

Copyright material on the Education Counts website is protected by copyright owned by the Ministry of Education, on behalf of the Crown, or its licensors. Unless indicated otherwise, that material is licensed for reuse under a [Creative Commons Attribution 4.0 International licence](https://creativecommons.org/licenses/by/4.0/). You may copy, distribute and adapt the material, as long as you attribute it to the Ministry of Education and Education Counts and abide by the other licence terms. This licence does not apply to logos, emblems and trade marks, to the website design, or to photography and imagery. Those items may not be reused without express permission. You must give appropriate credit, provide a link to the licence, and say if changes were made. You may not suggest that the Ministry of Education or Education Counts endorses this API or its maker. No ministry logos or emblems are used.

This API omits New Zealand mobile telephone numbers, corrects two broken website prefixes that began `http://https//` (QMS Audits New Zealand Ltd, institution 20599, and JNP Aviation Training, institution 6697), trims extra spaces, including trailing spaces on 153 Auckland territorial authority values, leaves out the Fax column (blank on every source row) and adds a source date. Other websites, addresses and names were left as published. There are no map points, because the directory has no coordinates.

This is a snapshot from 2 November 2026. Education Counts updates the directory from time to time. All 430 official records are included, sorted by institution number, including 17 proposed PTEs (some with only a placeholder name and no address) and Cook Islands Tertiary Training Institute, which has no New Zealand region. Some address fields are blank in the official file. For the latest list, visit the Education Counts page linked above.

[Back to the API list](../../README.md)
