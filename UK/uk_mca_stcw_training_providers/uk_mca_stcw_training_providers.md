# UK MCA Approved Training Providers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1271](https://img.shields.io/badge/records-1271-lightgrey) ![updated 2026-11-05](https://img.shields.io/badge/updated-2026--11--05-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

Contains public sector information licensed under the Open Government Licence v3.0. Source: Maritime and Coastguard Agency, MCA approved training providers (ATP) lists, https://www.gov.uk/guidance/mca-approved-training-providers-atp. Map points: Contains OS data © Crown copyright and database right 2026. Contains Royal Mail data © Royal Mail copyright and database right 2026. Source: Office for National Statistics licensed under the Open Government Licence v.3.0. (ONS Postcode Directory, via postcodes.io).

1,271 approvals of training providers for 75 maritime courses on the UK Maritime and Coastguard Agency (MCA) approved training providers lists of 5 November 2026 (one list is dated 19 October 2026). Each record is one provider approved for one course, with the course, the training site address, UK county and postcode, phone, general email, website, an open to the public flag, and a map point on 659 rows. 781 rows are in the UK and 488 are in 25 other countries. 164 provider names appear, so one provider shows up once for each course and site. The courses cover STCW basic, advanced and refresher training, GMDSS radio, ECDIS, HELM, tanker, high voltage, security, yacht and workboat courses. No field is required, so you can filter by course, country, county, provider or postcode, or by a latitude and longitude box.

This is not an official MCA service, and it is not vetted or official advice from the MCA. The MCA lists change, so check the MCA page before you book a course. The MCA publishes each list as a PDF, and this API reads the tables out of those PDFs, so a few entries may differ from the PDF. The lists have no official row key. The API returns a generated `__id` that is unique on every row and is the stable record key.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_mca_stcw_training_providers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per provider and course approval. All matches are returned. |

## Query parameters

Required: none. Optional: 16 fields. Disallowed: 2 fields.

A call with no filters returns all 1,271 records (about 1.5 MB). Partial filters accept `*`, such as `provider=*Warsash*`, `postcode=SO14*` or `course_name=*Ammonia*`. `public_access` is an exact match, and a star there is read as a plain character, so it returns `[]`. `lat` and `lng` accept a range with two dots, lower number first, such as `lat=50..51&lng=-2..0`. A leading minus works, and a hyphen range such as `lng=-2-0` returns HTTP 400. Text filters ignore case. `source_url` and `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `course_code` | Optional | Yes | No | Short course code from the MCA list file name, such as EFA, PST, ECDIS or HELM-M, in 75 values. The largest are EFA (83), PST (72), PSSR (63). Never blank. Underscores in the file names were turned into hyphens (BASIC-OIL-CHEM, SV-AUX-1). Case is ignored. Partial wildcards allowed, such as HELM* or SV-*. |
| `course_name` | Optional | Yes | No | Course title as the MCA page lists it, such as Elementary First Aid or ECDIS, in 75 values. Never blank. Partial wildcards allowed, such as *Ammonia* (2), *Fire* or *Yacht*. |
| `course_group` | Optional | Yes | No | Heading the MCA page files the course under, such as STCW basic training (266), Updating STCW training (refresher) (193), STCW advanced training, Security training or GMDSS (71). 18 values. Never blank. Values contain spaces and some contain brackets or a slash. Partial wildcards allowed, such as STCW* or *tanker*. |
| `provider` | Optional | Yes | No | Training provider name as the MCA lists it, such as Warsash Maritime School (Solent University Southampton) or Bluewater (France). 164 distinct names, including 47 GMDSS rows where the MCA adds (Radio) to the name. Never blank. Four rows for one person trading alone were left out. Partial wildcards allowed, such as *Warsash* (56) or *RelyOn*. |
| `location` | Optional | Yes | No | The MCA Location column as published: a UK county for UK rows (such as Hampshire, 121), a country for overseas rows (such as India, 65), or Multi-Site (54) or Computer Based Learning (30) for peripatetic or online delivery. 62 values. Never blank. Use county or country for clean filters. Partial wildcards allowed. |
| `address` | Optional | Yes | No | Training site address as one line, as the MCA published it, such as Folnavon Close, Aberdeen Airport East Dyce, Aberdeen, Aberdeenshire AB21 7EG. Some entries are only a note such as Peripatetic Delivery or On board yachts. Blank (`null`) on 73 rows, including all 46 rows marked not open to the public. Spelling and case are the MCA's. Partial wildcards allowed, such as *Plymouth*. |
| `county` | Optional | Yes | No | UK county as listed in the MCA Location column, in 35 values; the largest are Hampshire (121), Aberdeenshire (51), Kent (43). Present on 703 UK rows. Blank (`null`) on Multi-Site, Computer Based Learning and overseas rows. Three Belfast rows that the MCA wrote as Ireland were set to Antrim to match the same provider's other rows. Partial wildcards allowed, such as *Yorkshire* (31). |
| `postcode` | Optional | Yes | No | UK postcode in the form SO14 5FL, read from the end of the UK address and put in capitals with one space. Present on 682 rows. Blank (`null`) on overseas rows, on peripatetic UK rows and where the MCA gave no full postcode. Partial wildcards allowed, such as SO14* (45) or AB*. |
| `country` | Optional | Yes | No | Country: United Kingdom (781) for UK rows, or the country name as the MCA published it for overseas rows, in 25 values such as India, France, Spain, South Africa and United States of America. Taiwan is written Taiwan, Republic of China as the MCA has it. Blank (`null`) on 2 rows where none could be read (Medaire Limited overseas and On Deck Aviation on board yachts). Blank countries on Bluewater (France) online rows and one HeliRiviera row were filled from the same provider's other rows or its address. Partial wildcards allowed. |
| `phone` | Optional | Yes | No | Training center or office phone as published, with the country code kept as written, such as 01224 794900 or +44 (0) 23 8201 5004. Present on 1110 rows. UK and overseas mobile numbers were dropped (131 numbers). Several lines in one cell are joined with a semicolon. Blank (`null`) where the source had none. Partial wildcards allowed, such as 01224* or +33*. |
| `email` | Optional | Yes | No | General booking or enquiry email as published, in lower case, such as info@wavetrain.co.uk. Present on 1164 rows. 28 cells that held an address naming a person were dropped. Blank (`null`) where the MCA gave none. Partial wildcards allowed, such as *@uhi.ac.uk*. |
| `website` | Optional | Yes | No | Provider website as published, such as https://www.hota.org/. Where the MCA wrote only www or a bare domain, http:// was added. Long links that wrapped across lines were joined. Present on 1211 rows. Links may be out of date. Partial wildcards allowed, such as *uhi.ac.uk*. |
| `public_access` | Optional | No | No | No (46) when the MCA entry says Not open to public, otherwise Yes (1225). Yes only means the list does not say the course is closed; it is not a promise that anyone can book. Never blank. Exact match, Yes or No. Rows marked No have no address, phone, email or website in the source. |
| `list_date` | Optional | Yes | No | Date of the MCA list file the row came from, YYYY-MM-DD text, taken from the file name: 2026-11-05 (1259) or 2026-10-19 (12, the Tanker Fire Fighting list, which the MCA did not reissue on 5 November). Never blank. Text, so no range; use a wildcard such as 2026-11* or 2026-10*. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees of the UK postcode center, from the ONS Postcode Directory through postcodes.io. Present on 659 rows, blank (`null`) on 612. Blank for overseas rows, for peripatetic UK rows, for the 15 Northern Ireland (BT) postcode rows, and for 8 rows whose postcode is no longer in the directory. It marks the postcode, not the building. Values run from 50.104174 to 60.169353. Range uses two dots, such as lat=50..51. A hyphen returns 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same UK postcode center. Present on the same 659 rows as lat. Values run from -7.358554 to 1.743661, so many are negative (west of Greenwich). Range uses two dots, min first, such as lng=-2..0. A hyphen range returns 400. |
| `source_url` | Disallowed | No | No | Link to the MCA PDF list the row came from, on assets.publishing.service.gov.uk, in all 1271 rows. One link per course. Not filterable. |
| `source` | Disallowed | No | No | Attribution text in all 1271 rows: Contains public sector information licensed under the Open Government Licence v3.0. Source: Maritime and Coastguard Agency, MCA approved training providers lists (gov.uk), parsed into rows. UK map points: Contains OS data © Crown copyright and database right 2026. Contains Royal Mail data © Royal Mail copyright and database right 2026. Source: Office for National Statistics licensed under the Open Government Licence v.3.0 (ONS Postcode Directory, via postcodes.io). Not filterable. |

## Response fields

`lat` and `lng` are numbers. `__id` is an integer. All other values are strings. Blank cells are returned as `null`. Every row has a unique `__id`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `course_code` | string | Short course code from the MCA file name, such as ECDIS or EFA. |
| `course_name` | string | Course title as the MCA lists it. |
| `course_group` | string | Heading the MCA files the course under. |
| `provider` | string | Training provider name. |
| `location` | string | MCA Location column: a UK county, a country, `Multi-Site` or `Computer Based Learning`. |
| `address` | string | Training site address as one line, or `null` on 73 rows. |
| `county` | string | UK county, or `null` on overseas, multi-site and online rows. |
| `postcode` | string | UK postcode, or `null` where none was given. |
| `country` | string | Country name, or `null` on 2 rows. |
| `phone` | string | Landline or office number, or `null`. |
| `email` | string | General email, or `null`. |
| `website` | string | Provider website, or `null`. |
| `public_access` | string | `Yes` or `No`. |
| `list_date` | string | Date of the MCA list, `2026-11-05` or `2026-10-19`. |
| `lat` | number | Latitude of the UK postcode centre, or `null`. |
| `lng` | number | Longitude of the UK postcode centre, or `null`. |
| `source_url` | string | Link to the MCA PDF list the row came from. |
| `source` | string | Credit text for the lists and the map points. |

## Examples

ECDIS training approvals in Hampshire. This call returns 2 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_mca_stcw_training_providers' --data-urlencode 'course_code=ECDIS' --data-urlencode 'county=Hampshire'
```

```json
{
  "__id": 248,
  "course_code": "ECDIS",
  "course_name": "ECDIS",
  "course_group": "STCW ECDIS/NAEST",
  "provider": "ECDIS Ltd",
  "location": "Hampshire",
  "address": "Bizz Space Solent Business Park, Whiteley, Fareham, Hampshire PO15 7FP",
  "county": "Hampshire",
  "postcode": "PO15 7FP",
  "country": "United Kingdom",
  "phone": "01489 559677",
  "email": "info@ecdis.org",
  "website": "http://www.ecdis.org/",
  "public_access": "Yes",
  "list_date": "2026-11-05",
  "lat": 50.880103,
  "lng": -1.23946,
  "source_url": "https://assets.publishing.service.gov.uk/media/6aec8196912134d4c0b52f79/ECDIS_05.11.26.pdf",
  "source": "Contains public sector information licensed under the Open Government Licence v3.0. Source: Maritime and Coastguard Agency, MCA approved training providers lists (gov.uk), parsed into rows. UK map points: Contains OS data © Crown copyright and database right 2026. Contains Royal Mail data © Royal Mail copyright and database right 2026. Source: Office for National Statistics licensed under the Open Government Licence v.3.0 (ONS Postcode Directory, via postcodes.io)."
}
```

ECDIS training approvals in India. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_mca_stcw_training_providers' --data-urlencode 'course_code=ECDIS' --data-urlencode 'country=India'
```

GMDSS radio training in Hampshire. This call returns 7 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_mca_stcw_training_providers' --data-urlencode 'course_group=GMDSS' --data-urlencode 'county=Hampshire'
```

Approvals for one provider, matched by part of the name. This call returns 56 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_mca_stcw_training_providers' --data-urlencode 'provider=*Warsash*'
```

ECDIS approvals in a map box along the south coast of England. The longitude values are negative, so the lower number comes first. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_mca_stcw_training_providers' --data-urlencode 'course_code=ECDIS' --data-urlencode 'lat=50..51' --data-urlencode 'lng=-2..0'
```

Entries marked not open to the public for the PSSR course. This call returns 2 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_mca_stcw_training_providers' --data-urlencode 'course_code=PSSR' --data-urlencode 'public_access=No'
```

## Data source

Contains public sector information licensed under the Open Government Licence v3.0. Source: Maritime and Coastguard Agency, MCA approved training providers (ATP) lists, https://www.gov.uk/guidance/mca-approved-training-providers-atp. Map points: Contains OS data © Crown copyright and database right 2026. Contains Royal Mail data © Royal Mail copyright and database right 2026. Source: Office for National Statistics licensed under the Open Government Licence v.3.0. (ONS Postcode Directory, via postcodes.io).

The records come from the [MCA approved training providers (ATP) page](https://www.gov.uk/guidance/mca-approved-training-providers-atp) on GOV.UK, which was last updated on 5 November 2026 and links 76 PDF files. 75 of them are course lists, one for each course, and they have 1,275 provider rows with 1,271 left after the privacy step below. The 76th file is a notice about India and Pakistan certificates and has no table. 74 of the course lists are dated 5 November 2026. The Tanker Fire Fighting list (12 rows) is dated 19 October 2026, and `list_date` shows which date each row has. Each PDF lists UK providers first, then providers outside the UK. `source_url` links to the PDF for the row. This API is a snapshot that is refreshed by hand. For the latest approvals, visit the [MCA page](https://www.gov.uk/guidance/mca-approved-training-providers-atp).

The GOV.UK page says that all of its content is available under the Open Government Licence v3.0, except where otherwise stated. The [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/) lets you copy, publish, adapt and use the information commercially and non-commercially, as long as you acknowledge the source and link to the licence. It does not cover personal data or third-party rights. The lists were parsed into rows, and the MCA did not produce or check this API. No MCA logo is used, and the MCA does not endorse this API.

Map points come from the ONS Postcode Directory through postcodes.io, and they mark the center of the UK postcode, not the building. Required credit: "Contains OS data © Crown copyright and database right 2026. Contains Royal Mail data © Royal Mail copyright and database right 2026. Source: Office for National Statistics licensed under the Open Government Licence v.3.0." 659 rows have a point. Postcodes that start with BT (Northern Ireland) have no point on purpose, because commercial use of the Northern Ireland postcode data needs a separate licence from Land and Property Services. That is 15 rows. 8 UK rows have a postcode that is no longer in the directory, and the rest of the 612 rows without a point are overseas, multi-site, online or on board, or have no full postcode. Overseas rows have no point. OpenStreetMap data was not used.

Personal data was left out. Four rows for one person trading alone were removed. 28 emails that name a person were removed. 131 mobile numbers, UK and overseas, were removed. Business names, general mailboxes and landline numbers were kept. 46 entries are marked Not open to public in the PDFs, and for those rows the address, phone, email and website are blank because the MCA merges those cells.

[Back to the API list](../../README.md)
