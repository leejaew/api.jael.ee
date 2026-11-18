# US J-1 Exchange Visitor Sponsors

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 4133](https://img.shields.io/badge/records-4133-lightgrey) ![updated 2026-11-18](https://img.shields.io/badge/updated-2026--11--18-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

4,133 sponsor designations for the J-1 Exchange Visitor Program, from the U.S. Department of State BridgeUSA Designated Sponsor List, read on 18 November 2026. Each record is one sponsor in one exchange category, with the sponsor name, address, phone, website, the sending countries it listed, and a map point on 2,919 rows. 1,259 sponsors appear across 15 categories (such as Research Scholar, Short-Term Scholar, Student, College/University, Professor, Intern and Au Pair) and 54 states and territories. No field is required, so you can filter by category, state, sponsor name, country or a latitude and longitude box.

This API is not affiliated with or endorsed by the U.S. Department of State, and it is not an official source of J-1 information. Designations change, so confirm a sponsor on the [State Department list](https://j1visa.state.gov/participants/how-to-apply/sponsor-search/) before you apply. A sponsor can place participants anywhere in the United States, whatever address is listed, so the address and map point show the sponsor office, not where a program runs. The sending countries come from a separate State Department table that is labeled 2024, and it covers only 2,452 of the 4,133 rows (59%). A blank `countries` value does not mean a sponsor works with no countries. A sponsor and category pair has one row, except 27 pairs that have two rows because the list shows two addresses. The list has no official row key, so use the generated `__id` as the record key.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_j1_exchange_sponsors` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per sponsor and category. All matches are returned. |

## Query parameters

Required: none. Optional: 16 fields. Disallowed: 1 field.

A call with no filters returns all 4,133 records (about 4 MB). Partial filters accept `*`, such as `sponsor=*University of California*`, `zip=021*` or `countries=*Mexico*`. Category values can contain a comma and a slash, such as `Student, College/University`, so URL encode them, or use `category=*Student*`. `state`, `has_website`, `countries_year` and `geoq` are exact matches, and a star there is read as a plain character, so it returns `[]`. `country_count`, `lat` and `lng` accept a range with two dots, lower number first, such as `country_count=100..250` or `lat=40..41`. Every longitude in this data is negative, so write the lower number first, such as `lng=-74.5..-73.5`. A hyphen range such as `lat=40-41` returns HTTP 400. Text filters ignore case. `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `category` | Optional | Yes | No | Exchange Visitor category the sponsor is designated for, as the State Department list gives it, 15 values: Research Scholar (863), Short-Term Scholar (833), Student, College/University (785), Professor (750), Specialist (441), Intern (110), and 9 smaller ones such as Au Pair (14) and Alien Physician (1). Never blank. The values contain commas and slashes. Partial wildcards allowed, such as *Student* (850) or Research*. |
| `sponsor` | Optional | Yes | No | Sponsor organization name as listed, such as Stanford University or CIEE, Inc., 1,259 distinct names. Never blank. One sponsor has one row for each category it holds (CIEE, Inc. has 10). 27 sponsor and category pairs have two rows because the list shows two addresses. Partial wildcards allowed, such as *University of California* (60) or *Intrax*. |
| `street` | Optional | Yes | No | Street part of the listed address, kept as published in the case the State Department used (often capitals), such as 2137 EMBASSY DR. Can include a suite or office line before the street. 202 rows list a PO box. Never blank. Partial wildcards allowed, such as *Broadway*. |
| `city` | Optional | Yes | No | City as listed, 632 values, such as Boston or LANCASTER (case as published; filters ignore case). Never blank. Partial wildcards allowed, such as New*. |
| `state` | Optional | No | No | Two-letter USPS code of the listed address: 50 states, DC (203), Puerto Rico (5), Guam (4) and the US Virgin Islands (1), 54 values. The largest are CA (435), NY (424), DC (203), MA (200), TX (198). Never blank. Exact match only, such as state=CA, case ignored. |
| `zip` | Optional | Yes | No | Five digit ZIP code as text, such as 17603 or 02138. Leading zeros are kept. Never blank. Partial wildcards allowed, such as 021* (62) or 100*. A ZIP plus 4 never appears. |
| `phone` | Optional | Yes | No | Sponsor office phone as published, written as (717) 560-2840, with ext. 99 added when the list gives an extension. Present on all rows. These are organization numbers from the State Department list. Partial wildcards allowed, such as (202)*. |
| `website` | Optional | Yes | No | Sponsor website, the plain address taken from the link on the State Department page, with no HTML, such as http://amhomelife.org. Present on 3,851 rows, blank (null) on 282. 13 rows list two addresses, joined with a semicolon. Links can be out of date. Partial wildcards allowed, such as *edu*. |
| `has_website` | Optional | No | No | yes (3,851 rows) when website has a value, otherwise no (282). Never blank. Exact match, yes or no. |
| `countries` | Optional | Yes | No | Sending countries the sponsor listed for that category, as one text value with the names separated by a semicolon and a space, in alphabetical order, such as Argentina; Austria; Brazil. Present on 2,452 rows, blank (null) on 1,681. This comes from a separate State Department table labeled 2024, joined on sponsor name and category (Student, College/University joins the student degree, non-degree and intern programs). A blank means the sponsor was not in that table for that category, not that it takes no countries. Names follow the State Department spelling, such as Burma, Macedonia and Bahamas, The. Up to 191 countries in one cell. Partial wildcards allowed, such as *Mexico* (588). |
| `country_count` | Optional | No | Yes | Number of countries in countries, a whole number from 1 to 191 (median 5). Blank (null) when countries is blank. An integer. Range filter with two dots, min..max, such as 100..250 (8 rows). An exact value works too, such as country_count=1 (504 rows). |
| `countries_year` | Optional | No | No | 2024 on the 2,452 rows that have countries, the year in the State Department table title (Designated Sponsor List By Country 2024). Blank (null) on the rest. Text, exact match. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84, 6 decimals) of the listed street address, from the US Census Bureau Geocoder. Present on 2,919 rows (70.6%), blank (null) on 1,214: PO boxes and rooms without a street, and addresses the geocoder could not match or tied. Values run from 18.40 to 61.19. A number. Range filter with two dots, min..max, such as 40..41. A hyphen range returns 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same point. Present on the same 2,919 rows. All values are negative (west of Greenwich), from -157.86 to -66.05, so a range is written lower number first, such as lng=-74.5..-73.5 (put lat in the same call for a map box). A number. A hyphen range returns 400. |
| `geoq` | Optional | No | No | How the map point was matched: exact (2,374 rows), where the Census Geocoder matched the street address and the ZIP code agrees, or approximate (545), where it matched a similar address or a different ZIP on the same street and the point can be a block or more off. Blank (null) on the 1,214 rows with no point. Exact match only, such as geoq=exact. |
| `source_as_of` | Optional | Yes | No | Date the State Department pages were read, 2026-11-18, text YYYY-MM-DD. The pages show no last updated date, so this is the fetch date. Same on every row. Partial wildcards allowed, such as 2026-11*. |
| `source` | Disallowed | No | No | Credit text in all 4,133 rows: U.S. Department of State, Bureau of Educational and Cultural Affairs, BridgeUSA Designated Sponsor List and Designated Sponsor List By Country 2024, with page links, US government work in the public domain (17 U.S.C. 105), Census Geocoder for map points, and a note that this is not affiliated with the State Department. Not filterable. |

## Response fields

`lat` and `lng` are numbers, and `country_count` and `__id` are integers. All other values are strings. Blank cells are returned as `null`. Every row has a unique `__id`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. It is generated and can change when the data is refreshed. |
| `category` | string | Exchange Visitor category the sponsor is designated for, such as `Research Scholar` or `Au Pair`. |
| `sponsor` | string | Sponsor organization name as listed. |
| `street` | string | Street part of the listed address, as published. |
| `city` | string | City of the listed address. |
| `state` | string | Two-letter USPS code of the listed address. |
| `zip` | string | Five digit ZIP code as text. Leading zeros are kept. |
| `phone` | string | Sponsor office phone, written as `(717) 560-2840`, with `ext.` added when the list gives one. |
| `website` | string | Sponsor website address with no HTML, or `null` on 282 rows. Two addresses are joined with a semicolon on 13 rows. |
| `has_website` | string | `yes` or `no`. |
| `countries` | string | Sending countries for that sponsor and category, separated by a semicolon and a space, or `null` on 1,681 rows. From a table labeled 2024. |
| `country_count` | integer | Number of countries in `countries`, or `null`. |
| `countries_year` | string | `2024` when `countries` has a value, otherwise `null`. |
| `lat` | number | Latitude of the listed street address from the Census Geocoder, or `null` on 1,214 rows. |
| `lng` | number | Longitude of the same point, or `null`. Always negative. |
| `geoq` | string | Match quality of the point, `exact` or `approximate`, or `null` when there is no point. |
| `source` | string | Credit text for the lists and the map points. |
| `source_as_of` | string | Date the lists were read, `2026-11-18`. |

## Examples

Au Pair sponsors in California. This call returns 5 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'category=Au Pair' --data-urlencode 'state=CA'
```

```json
{
  "__id": 463,
  "category": "Au Pair",
  "sponsor": "Cultural Homestay International",
  "street": "255 W END AVE",
  "city": "SAN RAFAEL",
  "state": "CA",
  "zip": "94901",
  "phone": "(415) 459-5397",
  "website": "http://www.chinet.org/",
  "has_website": "yes",
  "countries": "Algeria; Argentina; Belarus; Brazil; China; Colombia; Dominican Republic; Ecuador; France; French Southern and Antarctic Lands; Germany; Italy; Mexico; Namibia; Netherlands; Peru; South Africa; Spain; Swaziland; Taiwan; Ukraine; United Kingdom; Venezuela; Zimbabwe",
  "country_count": 24,
  "countries_year": "2024",
  "lat": 37.974497,
  "lng": -122.543433,
  "geoq": "exact",
  "source": "U.S. Department of State, Bureau of Educational and Cultural Affairs, BridgeUSA (J-1 visa) Designated Sponsor List (https://j1visa.state.gov/participants/how-to-apply/sponsor-search/) and Designated Sponsor List By Country 2024 (https://j1visa.state.gov/sponsors/sponsor-by-country/?program=all). US government work, public domain (17 U.S.C. 105). Map points: US Census Bureau Geocoder. Not affiliated with the U.S. Department of State.",
  "source_as_of": "2026-11-18"
}
```

Intern sponsors that listed Singapore as a sending country. This call returns 16 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'category=Intern' --data-urlencode 'countries=*Singapore*'
```

Camp Counselor sponsors in New York. This call returns 7 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'category=Camp Counselor' --data-urlencode 'state=NY'
```

Sponsors with Intrax in the name. This call returns 7 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'sponsor=*Intrax*'
```

Sponsor offices in a map box around New York City. The longitude values are negative, so the lower number comes first. This call returns 187 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'lat=40.6..40.9' --data-urlencode 'lng=-74.1..-73.9'
```

Sponsors that listed 100 to 250 sending countries. This call returns 8 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'country_count=100..250'
```

California sponsors with no website. This call returns 24 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_j1_exchange_sponsors' --data-urlencode 'has_website=no' --data-urlencode 'state=CA'
```

## Data source

U.S. Department of State, Bureau of Educational and Cultural Affairs, BridgeUSA Designated Sponsor List ([sponsor search](https://j1visa.state.gov/participants/how-to-apply/sponsor-search/)) and Designated Sponsor List By Country 2024 ([sponsors by country](https://j1visa.state.gov/sponsors/sponsor-by-country/?program=all)). This is a United States government work and is in the public domain ([17 U.S.C. 105](https://www.law.cornell.edu/uscode/text/17/105)). The State Department copyright page says: "Unless a copyright is indicated, information on State Department websites is in the public domain and may be copied and distributed without permission. Citation of the U.S. State Department as source of the information is appreciated." No State Department seal, logo or insignia is used. This API is not affiliated with or endorsed by the State Department.

The sponsor list has 4,133 rows, and all of them are here. The table on the page loads its rows from the site and reports 4,133 records. The sending country table has 3,071 rows of program, sponsor and countries, and its title says 2024. The six student programs in that table (Student Non-Degree, Masters, Doctorate, Bachelors, Associate and Student Intern) were merged into the `Student, College/University` category, and the other program names match the sponsor list one to one. A row joins on the sponsor name and category after case, punctuation and endings such as Inc and LLC are ignored. Near matches were not joined, because near names were often different schools. Many Professor, Specialist and Short-Term Scholar sponsors are not in the 2024 table, so 1,681 rows have no countries. 115 entries in the 2024 table match no current sponsor and were not used. Country names that contain a comma, such as Bahamas, The, are kept whole.

Map points come from the U.S. Census Bureau Geocoder batch service (benchmark Public_AR_Current), run on 18 November 2026. The Census Geocoder output is a US government service and its output is in the public domain. 2,919 rows (70.6%) have a point. `geoq` is `exact` on 2,374 rows, where the geocoder matched the street address and the ZIP code agrees. It is `approximate` on 545 rows, where it matched a similar address or a different ZIP on the same street, so the point can be a block or more off. 1,214 rows have no point, because the address is a PO box or a room without a street number, or the geocoder found no match or a tie. OpenStreetMap data was not used.

Phone numbers are the sponsor office numbers on the list. All 1,259 sponsor names are organizations, and no personal data was found, so nothing was left out. Street text is shown as published, often in capitals. Website links on the State Department page can be out of date. The `source_as_of` date is the day the pages were read, because the pages show no update date.

[Back to the API list](../../README.md)
