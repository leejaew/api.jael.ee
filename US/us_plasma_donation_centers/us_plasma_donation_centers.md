# US Plasma Donation Centers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,216](https://img.shields.io/badge/records-1%2C216-lightgrey) ![updated 2026-11-15](https://img.shields.io/badge/updated-2026--11--15-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

1,216 active plasmapheresis centers in 46 U.S. states from the FDA CBER Blood Establishment Registration database as of 15 November 2026. Each record has the FDA Establishment Identifier, legal and applicant names, address, phone, ownership, establishment types and a map point where one was found. Reporting official names, emails, phones and mailing addresses are not included. No field is required, so you can filter by state, city, ZIP, legal name, applicant, FEI, district office, ownership type or a latitude and longitude box.

Coverage is 46 U.S. states. There are no rows for Alaska, Hawaii, New Hampshire, Vermont or the District of Columbia, and none for U.S. territories. The largest states are TX (189), FL (92), CA (66), GA (54) and OH (54). `fei` is unique and never blank, and it is the stable record key. The API does not return a generated `__id`. Two sites have no map point. A `geo_match` of `city` is the Census point for the city, not the door.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_plasma_donation_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per center. All matches are returned. |

## Query parameters

Required: none. Optional: 20 fields. Disallowed: 2 fields.

A call with no filters returns all 1,216 records. Partial filters accept `*`, such as `legal_name=*CSL*`, `zip=770*` or `establishment_types=*COLLECTION FACILITY*`. `lat` and `lng` accept a range with two dots, such as `lat=40..41&lng=-75..-73`. `state`, `district_office`, `ownership_type`, `last_registration_year` and `geo_match` are exact matches. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `fei` | Optional | Yes | No | FDA Establishment Identifier as text, never blank and unique on all 1,216 rows, so it is the stable key. 1,054 values are 10 digits, such as 3023673069, and 162 are 7 digits, such as 1073354. Partial wildcards allowed, such as `fei=3023673069` (1). |
| `license_number` | Optional | Yes | No | Applicant biologics license number as text, such as 1823 (CSL Plasma, 305 rows) or 1640 (BioLife Plasma Services, 237 rows). Present on 1,195 rows. Blank (`null`) on 21 rows that also have no applicant name. Only 27 distinct numbers; a license is shared by every site under that applicant, so it is not a unique key. Two Continental Services Group rows use the 3-digit value 439. Partial wildcards allowed. |
| `applicant_name` | Optional | Yes | No | Name of the licensed applicant, such as CSL Plasma, Inc. (305) or BioLife Plasma Services L.P. (237). Blank (`null`) on the same 21 rows as `license_number`. Usually the same as `legal_name`; 41 rows differ, often only in punctuation. Partial wildcards allowed, such as `*CSL*` (305). |
| `legal_name` | Optional | Yes | No | Legal name of the establishment as FDA lists it. Never blank. 41 distinct values. The largest are CSL Plasma, Inc. (305, plus one official typo CSL PLasma, Inc.), Biomat USA, Inc. (252, plus one without a final period), BioLife Plasma Services L.P. (242), Octapharma Plasma, Inc. and close official variants (184), KEDPLASMA LLC (71) and Join Parachute, LLC (58). Official punctuation varies, such as Octapharma Plasma, Inc without a final period (25) or ABO Holdings,Inc with no space. Partial wildcards allowed, such as `*CSL*` (306) or `*BioLife*` (242). |
| `other_names` | Optional | Yes | No | Other trade or doing business as names, when FDA lists them, separated by a space, a pipe and a space. Present on 344 rows; 186 of those have more than one name. Blank (`null`) on 872 rows. Use this to find Grifols sites that are legally named Biomat USA: `other_names=*Grifols*` returns 84. Partial wildcards allowed, such as `*Grifols*` (84), `*Talecris*` (60) or `*Parachute*` (40). |
| `address` | Optional | Yes | No | First address line as FDA lists it, such as 1430 Quintard Avenue. Never blank. Each value is unique in this snapshot. Partial wildcards allowed, such as `*Suite*` (32). |
| `address_2` | Optional | Yes | No | Second address line when FDA gives one, usually a suite, such as Suite E. Present on 210 rows. Blank (`null`) on 1,006 rows. Partial wildcards allowed, such as `*Suite*` (174). |
| `city` | Optional | Yes | No | City as FDA lists it, such as Houston or San Antonio. Never blank. 687 distinct cities. Matching is case insensitive, so `city=Houston` with `state=TX` returns 19. Official spelling varies (Fort Worth and Ft. Worth, St. Louis and Saint Louis). One Louisiana row is uppercase MONROE. Partial wildcards allowed. |
| `state` | Optional | No | No | Two letter U.S. state code. Present in 46 states. There are no rows for Alaska, Hawaii, New Hampshire, Vermont or the District of Columbia, and none for U.S. territories. The largest are TX (189), FL (92), CA (66), GA (54) and OH (54). Never blank. Exact match. |
| `zip` | Optional | Yes | No | ZIP as text from the FDA listing, so leading zeros remain, such as 06906 or 02155-4901. Never blank. 1,096 are 5 digits, 119 are ZIP+4, and one official value is the defective 37421-349 (Chattanooga, TN). Partial wildcards allowed, such as `zip=770*` (19, Houston area) or `0*` (28). |
| `phone` | Optional | Yes | No | Phone as FDA lists it, not normalized. 825 use 999-999-9999, 270 use (999) 999-9999, and others use spaces, no separators, or an extension such as 619-768-7888 ext. 1003. Blank (`null`) on 5 rows. One official number is missing a digit: (317) 83-3839 in Greenwood, IN. Four numbers are shared by more than one site under the same operator. Partial wildcards allowed, such as `214-*` (5). |
| `district_office` | Optional | No | No | FDA district office that covers the site. Exact match. Never blank. 18 values: Dallas (228), Atlanta (131), New Orleans (92), Florida (92), Detroit (87), Los Angeles (69), Kansas City (66), Cincinnati (65), Denver (57), Minneapolis (53), Chicago (52), San Francisco (45), Philadelphia (42), Baltimore (42), Seattle (41), New York (26), New Jersey (16) and New England (12). |
| `ownership_type` | Optional | No | No | Official ownership class. Exact match. Never blank. Values: CORPORATION: PROFIT (957), PARTNERSHIP (244), OTHER: LLC (10), CORPORATION: NON-PROFIT (3) and SINGLE PROPRIETORSHIP (2). |
| `establishment_types` | Optional | Yes | No | Official establishment type labels, always including PLASMAPHERESIS CENTER, separated by a space, a pipe and a space when FDA also lists another type. Values: PLASMAPHERESIS CENTER only (1,173), that type plus COLLECTION FACILITY (38), that type plus COMMUNITY (NON-HOSPITAL) BLOOD BANK (3), and all three together (2). Partial wildcards allowed, such as `*COLLECTION FACILITY*` (40) or `*COMMUNITY*` (5). |
| `last_registration_year` | Optional | No | No | Last annual registration year as text, 2026 (463) or 2027 (753). Never blank. Exact match. |
| `duns` | Optional | Yes | No | Nine-digit establishment DUNS as text, so leading zeros remain (370 start with 0). Never blank, but not unique: 12 numbers are shared across 32 rows, including 000000000 on two Biomat USA rows. Partial wildcards allowed. |
| `cfn` | Optional | Yes | No | Central File Number as text, 7 digits when present. Present on 219 rows, each unique. On all 162 seven-digit FEI rows, CFN equals FEI. Blank (`null`) on 997 rows. Partial wildcards allowed. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 25.469721 (Homestead, FL) to 48.790876 (Bellingham, WA). Blank (`null`) on 2 rows with no Census match. Use a range, for example `lat=40..41&lng=-75..-73` for the New York area (20). A hyphen such as `lat=40-41` returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -123.121503 (Eugene, OR) to -68.749449 (Brewer, ME). Blank (`null`) on the same 2 rows. Use a range with `lat` to search a box, or `lng=-125..-114` for the West Coast (124). A hyphen returns HTTP 400. |
| `geo_match` | Optional | No | No | How the map point was found: `street` (1,139 rows, a U.S. Census street address match), `city` (75 rows, the Census point for the city because the street did not match) or blank (`null`, 2 rows, no point). Exact match, such as `geo_match=street` for the most precise points. |
| `source` | Disallowed | No | No | U.S. FDA CBER Blood Establishment Registration database, in all 1,216 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date the FDA eBER records were read, YYYY-MM-DD (`2026-11-15`). Text. Not filterable. |

## Response fields

`lat` and `lng` are numbers. All other values are strings. Blank cells are returned as `null`. There is no `__id` field. Use `fei` to identify a center.

| Field | Type | Description |
|---|---|---|
| `fei` | string | FDA Establishment Identifier, unique on every row. |
| `license_number` | string | Applicant biologics license number, or `null` when FDA lists none. |
| `applicant_name` | string | Name of the licensed applicant, or `null` when FDA lists none. |
| `legal_name` | string | Legal name of the establishment as FDA lists it. |
| `other_names` | string | Other trade or doing business as names, or `null` when FDA lists none. |
| `address` | string | First address line as FDA lists it. |
| `address_2` | string | Second address line, or `null` when FDA lists none. |
| `city` | string | City as FDA lists it. |
| `state` | string | Two letter U.S. state code. |
| `zip` | string | ZIP as text, so leading zeros remain. |
| `phone` | string | Phone as FDA lists it, or `null` on 5 rows. |
| `district_office` | string | FDA district office that covers the site. |
| `ownership_type` | string | Official ownership class. |
| `establishment_types` | string | Official establishment type labels. |
| `last_registration_year` | string | Last annual registration year, 2026 or 2027. |
| `duns` | string | Nine-digit establishment DUNS as text. |
| `cfn` | string | Central File Number, or `null` when FDA lists none. |
| `lat` | number | Latitude in decimal degrees (WGS 84), or `null` when there is no map point. |
| `lng` | number | Longitude in decimal degrees (WGS 84), or `null` when there is no map point. |
| `geo_match` | string | `street`, `city`, or `null` when there is no map point. |
| `source` | string | U.S. FDA CBER Blood Establishment Registration database. |
| `source_as_of` | string | Date the FDA eBER records were read, `2026-11-15`. |

## Examples

LFB American Plasma in Anniston, Alabama, looked up by FDA Establishment Identifier 3023673069. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_plasma_donation_centers' --data-urlencode 'fei=3023673069'
```

```json
[
  {
    "fei": "3023673069",
    "license_number": "2254",
    "applicant_name": "LFB American Plasma, LLC",
    "legal_name": "LFB American Plasma, LLC",
    "other_names": null,
    "address": "1430 Quintard Avenue",
    "address_2": null,
    "city": "Anniston",
    "state": "AL",
    "zip": "36201",
    "phone": "256-403-0666",
    "district_office": "New Orleans",
    "ownership_type": "CORPORATION: PROFIT",
    "establishment_types": "PLASMAPHERESIS CENTER",
    "last_registration_year": "2026",
    "duns": "119072552",
    "cfn": null,
    "lat": 33.662756,
    "lng": -85.826918,
    "geo_match": "street",
    "source": "U.S. FDA CBER Blood Establishment Registration database",
    "source_as_of": "2026-11-15"
  }
]
```

Centers whose legal name includes CSL. This call returns 306 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_plasma_donation_centers' --data-urlencode 'legal_name=*CSL*'
```

Centers in a map box around New York. This call returns 20 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_plasma_donation_centers' --data-urlencode 'lat=40..41' --data-urlencode 'lng=-75..-73'
```

Centers in Texas. This call returns 189 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_plasma_donation_centers' --data-urlencode 'state=TX'
```

Centers in Houston, Texas. This call returns 19 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_plasma_donation_centers' --data-urlencode 'state=TX' --data-urlencode 'city=Houston'
```

Centers whose other names include Grifols. This call returns 84 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_plasma_donation_centers' --data-urlencode 'other_names=*Grifols*'
```

## Data source

The records come from the public [eBER search](https://www.accessdata.fda.gov/scripts/cber/CFAppsPub/Index.cfm), queried for EstablishmentType=PLASMAPHERESIS CENTER, EstablishmentStatus=ACTIVE and Country=UNITED STATES. The search listed 1,216 establishments. All 1,216 detail records were read on 15 November 2026, Asia/Singapore time. `source_as_of` is `2026-11-15` on every row. Inactive establishments and non-U.S. sites were not requested. For the latest list, visit the [eBER search](https://www.accessdata.fda.gov/scripts/cber/CFAppsPub/Index.cfm).

Source: U.S. FDA CBER Blood Establishment Registration database.

This is a United States government work and is in the public domain ([17 U.S.C. 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)). FDA [website policies](https://www.fda.gov/about-fda/about-website/website-policies) say the contents of the FDA website are in the public domain and may be reused freely. Credit to the U.S. Food and Drug Administration as the source is appreciated but not required. Commercial reuse is allowed. No FDA logo is used. This use does not mean the U.S. Food and Drug Administration endorses this API or its maker.

Reporting official names, emails, phones and mailing addresses were omitted. Legal names and phones are kept as FDA lists them, including official punctuation variants and a few defective ZIP and phone values: ZIP 37421-349 at the BioLife site on Gunbarrel Road, Chattanooga, TN; phone (317) 83-3839 at the BioLife site in Greenwood, IN; DUNS 000000000 on two Biomat USA rows (Surprise, AZ and Stamford, CT); and legal name punctuation variants such as CSL PLasma, Inc., Octapharma Plasma, Inc without a final period, Octapharma Plasma. Inc., BPC Plasma, Inc, and ABO Holdings,Inc.

Map points come from the [U.S. Census Bureau geocoder](https://geocoding.geo.census.gov) only. 1,139 rows are a street address match. 75 rows use the Census point for the listing city (`geo_match=city`); that is a city point, not the door. Two sites have no point: ABO Holdings, Inc. at Cherry Hill Commons, 500 Route 38, Units 13-14, Cherry Hill, NJ 08002, and KEDPLASMA LLC at 20 Northpointe Parkway, Suite 160, Amherst, NY 14228.

This table is a directory of registered establishments. It is not medical advice.

[Back to the API list](../../README.md)
