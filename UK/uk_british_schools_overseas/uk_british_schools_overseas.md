# British Schools Overseas

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 294](https://img.shields.io/badge/records-294-lightgrey) ![updated 2026-11-18](https://img.shields.io/badge/updated-2026--11--18-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

Contains public sector information licensed under the Open Government Licence v3.0. Source: Department for Education, Get Information about Schools. Links: [Get Information about Schools](https://get-information-schools.service.gov.uk/), [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).

All 294 open schools that the Department for Education (DfE) register Get Information about Schools (GIAS) lists as British schools overseas, in 62 countries and territories, from the GIAS public download of 18 November 2026. The United Arab Emirates has 92, Qatar 22, Spain 18, Kuwait 14 and Romania 14. Each record has the school name, address, website, phone, the inspecting body, the date of the last inspection and a link to its report, the pupil age range, and sixth form, nursery and boarding details. No field is required, so you can filter by country, town, name, inspectorate, last inspection date (such as `2026*`) or age range.

This is not an official DfE or UK government service, and the DfE does not endorse it. British schools overseas accreditation is voluntary: a school listed here has joined the voluntary inspection scheme and met the British schools overseas standards in an inspection, as the [gov.uk list](https://www.gov.uk/government/publications/british-schools-overseas-inspection-reports) puts it, and accreditation lasts for 3 years. Schools that are not in the scheme are not in this list, and the list does not rate schools or compare them.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_british_schools_overseas` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per school. All matches are returned. |

## Query parameters

Required: none. Optional: 22 fields. Disallowed: 1 field.

A call with no filters returns all 294 records (about 290 KB). Partial filters accept `*`, such as `name=*GEMS*`, `town=Dub*`, `website=*.ae*` or `last_inspection=2026*`. `urn`, `postcode`, `country`, `country_src`, `phone`, `inspectorate`, `report_url`, `gender`, `boarders`, `sixth_form`, `nursery` and `source_as_of` are exact matches, and a star there is read as a plain character, so it returns `[]`. All filters ignore case. `age_low` and `age_high` accept a range with two dots, lower number first, such as `age_high=16..18`. A hyphen range such as `age_low=3-5` returns HTTP 400. The dates `last_inspection` and `open_date` are text, so use wildcards on them, not ranges.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `urn` | Optional | No | No | Unique Reference Number from Get Information about Schools, a 6 digit number such as 132290, kept as text. Unique on every row and never blank, so it is the record key. Exact match, such as urn=132290. |
| `name` | Optional | Yes | No | School name as published, such as Dubai British School or Doha College. Never blank. Partial wildcards allowed, such as *British*, King* or *GEMS*. |
| `address_1` | Optional | Yes | No | First address line (street or PO Box) as published, such as Po Box 37828. Address text is in English or romanized and the capitalization follows the source. Blank (`null`) on 1 row. Partial wildcards allowed, such as *PO Box*. |
| `address_2` | Optional | Yes | No | Second address line (locality or district) as published. Blank (`null`) on 73 rows. Partial wildcards allowed. |
| `address_3` | Optional | Yes | No | Third address line as published, such as Emirates Hills. Blank (`null`) on 146 rows. Partial wildcards allowed, such as *Emirates Hills*. |
| `town` | Optional | Yes | No | Town or city as the source prints it, such as Dubai, Doha or Cairo. It can also hold a postal code or a region (61 values contain digits, such as Madrid 28761). Blank (`null`) on 4 rows. Partial wildcards allowed, such as *Dubai*. |
| `postcode` | Optional | No | No | Postcode, only where the source has one: 4 rows, in Guernsey (2), Isle of Man (1) and Tunisia (1). Blank (`null`) on 290 rows. Exact match. |
| `country` | Optional | No | No | Country or territory in English, 62 values, such as United Arab Emirates (92), Qatar (22), Spain (18) or Hong Kong. Never blank. Filled by this API on 166 rows where the source has none, see country_src. Exact match, such as country=Qatar, case ignored. |
| `country_src` | Optional | No | No | How country was set: gias (128, the source country field), address_text (98, a country name or abbreviation such as UAE appears in the address), city_name (67, no country in the address but the city, such as Dubai or Madrid, names it) or website_tld (1, from a .il website). Never blank. Exact match, such as country_src=gias. |
| `website` | Optional | Yes | No | School website as the source prints it, often without https://, such as www.dubaibritishschool.ae. Blank (`null`) on 24 rows. Partial wildcards allowed, such as *.ae* or *kingscollegeschools*. |
| `phone` | Optional | No | No | School telephone number as published, digits only with the country code and sometimes a leading 00, such as 97143619361. These are school office lines. Blank (`null`) on 157 rows. Exact match. |
| `inspectorate` | Optional | No | No | Body that inspected the school for British schools overseas accreditation: Penta International (159), Independent Schools Inspectorate (76), Education Development Trust (56) or Cambridge Education (2). Blank (`null`) on 1 row. Exact match, such as inspectorate=Penta International. |
| `last_inspection` | Optional | Yes | No | Date of the last inspection visit, YYYY-MM-DD text, from 2016-10-27 to 2026-07-04. Never blank. Partial wildcards work on text dates, such as 2025* or 2025-1*. No range filter on dates. |
| `report_url` | Optional | No | No | Link to the inspection report published by the inspectorate or the school. This API does not host reports. Links that the source prints without https:// have it added. Blank (`null`) on 1 row. Exact match. |
| `age_low` | Optional | No | Yes | Lowest statutory age of pupils, an integer from 0 to 14. Blank (`null`) on 1 row. Range filter with two dots, min..max, such as age_low=0..2. |
| `age_high` | Optional | No | Yes | Highest statutory age of pupils, an integer from 6 to 19. Blank (`null`) on 1 row. Range filter with two dots, such as age_high=18..19. A hyphen range returns 400. |
| `gender` | Optional | No | No | Pupil gender: Mixed (289) or Girls (1). Blank (`null`) on 4 rows. Exact match. |
| `boarders` | Optional | No | No | No boarders (34 rows) or blank (`null`) on 260 rows, where the source does not say. Blank does not mean the school has boarders. Exact match. |
| `sixth_form` | Optional | No | No | Has a sixth form (208) or Does not have a sixth form (76). Blank (`null`) on 10 rows. Exact match. |
| `nursery` | Optional | No | No | Has Nursery Classes (243) or No Nursery Classes (36). Blank (`null`) on 15 rows. Exact match. |
| `open_date` | Optional | Yes | No | Date the school opened in the register, YYYY-MM-DD text, from 1925-01-01. Blank (`null`) on 65 rows, where the source has no date or its 1900-01-01 placeholder. Partial wildcards allowed, such as 2019*. No range filter on dates. |
| `source_as_of` | Optional | No | No | Date of the GIAS download file this copy was built from, 2026-11-18, the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: Contains public sector information licensed under the Open Government Licence v3.0. Source: Department for Education, Get Information about Schools. Returned in output only. |

## Response fields

`age_low` and `age_high` are integers. All other values are strings. Blank cells are returned as `null`. There is no `__id`: `urn` is the key.

| Field | Type | Description |
|---|---|---|
| `urn` | string | Unique Reference Number from Get Information about Schools. Unique on every record. |
| `name` | string | School name. |
| `address_1` | string | First address line, or `null` on 1 record. |
| `address_2` | string | Second address line, or `null`. |
| `address_3` | string | Third address line, or `null`. |
| `town` | string | Town or city as the register prints it, or `null` on 4 records. |
| `postcode` | string | Postcode, or `null` on 290 records. |
| `country` | string | Country or territory in English. Never `null`. |
| `country_src` | string | `gias`, `address_text`, `city_name` or `website_tld`: where `country` came from. |
| `website` | string | School website, or `null` on 24 records. |
| `phone` | string | School telephone number, digits only, or `null` on 157 records. |
| `inspectorate` | string | Inspecting body, or `null` on 1 record. |
| `last_inspection` | string | Date of the last inspection visit, `YYYY-MM-DD`. |
| `report_url` | string | Link to the inspection report, or `null` on 1 record. |
| `age_low` | integer | Lowest age of pupils, or `null` on 1 record. |
| `age_high` | integer | Highest age of pupils, or `null` on 1 record. |
| `gender` | string | `Mixed` or `Girls`, or `null` on 4 records. |
| `boarders` | string | `No boarders`, or `null` where the register does not say (260 records). |
| `sixth_form` | string | `Has a sixth form` or `Does not have a sixth form`, or `null` on 10 records. |
| `nursery` | string | `Has Nursery Classes` or `No Nursery Classes`, or `null` on 15 records. |
| `open_date` | string | Opening date, `YYYY-MM-DD`, or `null` on 65 records. |
| `source` | string | Credit line for the register. |
| `source_as_of` | string | Date of the GIAS download file, `2026-11-18`. |

## Data notes

* Accreditation is voluntary. The records are the schools that GIAS holds as British schools overseas, and the [gov.uk list of accredited schools](https://www.gov.uk/government/publications/british-schools-overseas-inspection-reports) is the DfE page for the scheme. Check it and the school before you rely on a record.
* `report_url` points to the inspection report on the inspectorate's or the school's own site. The reports are not hosted here and a link can stop working. Three links that the register prints without `https://` have it added.
* `country` is filled from the address on 166 of the 294 records where the register has no country. `country_src` says how: `gias` (128, the register value), `address_text` (98, a country name or abbreviation such as UAE in the address), `city_name` (67, the address names a city such as Dubai or Madrid but no country) or `website_tld` (1, from a .il website). All 294 values agree with the country on the gov.uk list, except two Hong Kong schools that the list files under China. Guernsey, Jersey and the Isle of Man are named as places.
* `boarders` is `No boarders` on 34 records and blank on 260. Blank means the register does not say, not that the school has boarders.
* There are no coordinates. The register has none for these schools, and no open official source covers addresses in 62 countries.
* `town` is printed as the register has it and 61 values include a postcode or other digits, such as `Madrid 28761`. The register has no opening date for 65 schools, and its 1900-01-01 placeholder is shown as `null`.
* The head teacher's name and title in the register are not included. `phone` is the school office number.
* `last_inspection` follows the register. In 24 cases it differs from the date on the gov.uk list, by 1 day to about 5.5 years, and the register is mostly the older one.
* The data is refreshed from the GIAS public download. This copy is from the file dated 2026-11-18, the `source_as_of` value. No GIAS web pages were scraped.

## Examples

Schools in the United Arab Emirates whose oldest pupils are 16 to 18. This call returns 68 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'country=united arab emirates' --data-urlencode 'age_high=16..18'
```

```json
{
  "urn": "130892",
  "name": "Dubai British School",
  "address_1": "Po Box 37828",
  "address_2": "Springs 3",
  "address_3": "Emirates Hills",
  "town": "Dubai",
  "postcode": null,
  "country": "United Arab Emirates",
  "country_src": "city_name",
  "website": "www.dubaibritishschool.ae",
  "phone": "97143619361",
  "inspectorate": "Education Development Trust",
  "last_inspection": "2024-10-17",
  "report_url": "https://edtlive.b-cdn.net/livenew/media/p2rfadrr/dubai-british-school-emirates-hills-bso-report-oct-24_final.pdf",
  "age_low": 3,
  "age_high": 18,
  "gender": "Mixed",
  "boarders": null,
  "sixth_form": "Has a sixth form",
  "nursery": "Has Nursery Classes",
  "open_date": "2005-09-01",
  "source": "Contains public sector information licensed under the Open Government Licence v3.0. Source: Department for Education, Get Information about Schools.",
  "source_as_of": "2026-11-18"
}
```

Schools last inspected in 2026. This call returns 50 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'last_inspection=2026*'
```

Schools with GEMS in the name. This call returns 16 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'name=*GEMS*'
```

Schools in Spain inspected by the Independent Schools Inspectorate. This call returns 7 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'country=Spain' --data-urlencode 'inspectorate=Independent Schools Inspectorate'
```

Schools whose country comes from the address text. This call returns 98 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'country_src=address_text'
```

Schools that take children from age 2 or younger. This call returns 76 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'age_low=0..2'
```

Dubai schools with a sixth form. This call returns 31 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_british_schools_overseas' --data-urlencode 'town=*Dubai*' --data-urlencode 'sixth_form=Has a sixth form'
```

## Data source

Contains public sector information licensed under the Open Government Licence v3.0. Source: Department for Education, Get Information about Schools. Source: Department for Education, [Get Information about Schools](https://get-information-schools.service.gov.uk/), public download `edubasealldata20261118.csv`, records with the establishment type British schools overseas and status Open. The GIAS footer says that all content is available under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/) except where otherwise stated.

The records were cross checked against the [gov.uk list of accredited British schools overseas](https://www.gov.uk/government/publications/british-schools-overseas-inspection-reports), which has 302 schools. All 294 records match a school on that list. Eight listed schools are not in the GIAS British schools overseas type and are not included: three Channel Islands schools that GIAS holds as offshore schools, and five schools that GIAS lists under other names or does not list.

The data is as the register publishes it, with the changes in the notes above, and it can contain errors. One example is that the Royal Grammar School Guildford in Qatar carries the report link of the Dubai school in the register. Check the school and the inspection report before you use a record for a decision.

[Back to the API list](../../README.md)
