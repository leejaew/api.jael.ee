# US FMC Ocean Intermediaries

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 9,846](https://img.shields.io/badge/records-9%2C846-lightgrey) ![updated 2026-11-06](https://img.shields.io/badge/updated-2026--11--06-orange) ![license CC0 1.0](https://img.shields.io/badge/license-CC0%201.0-green)

9,846 ocean freight forwarders and NVOCCs (non vessel operating common carriers) licensed or registered with the US Federal Maritime Commission, on the list dated November 6, 2026. Records cover 86 countries. Each record has the company name, trade names, office address, phone, license type, license number, renewal date, surety company, bond amounts, and a map point for 4,656 of the US offices. Choose a country, then narrow by name, trade name, city, state, type, license status, renewal date, bond amount or a lat and lng box.

Top countries: United States (5,161), China (2,092), India (586), Korea, South (379), Hong Kong (210), Vietnam (200) and Taiwan (194). Types: NVOCC (6,563), NVOCC and Freight Forwarder (2,609) and Freight Forwarder (674). All 674 freight forwarder only rows are in the US.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_fmc_ocean_intermediaries` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per firm. All matches are returned. |

## Query parameters

Required: 1 field (`country`). Optional: 23 fields. Disallowed: 2 fields.

`country` must be in every call. Use the exact name in title case, such as `United States` or `Korea, South`. Partial filters accept `*`, such as `name=*LOGISTICS*` or `city=Taipei*`. Bond amounts, `lat` and `lng` accept ranges with two dots, such as `bond_amount=125000..150000` or `lat=33.6..34.2`. All other fields are exact match strings. The text date `updated` and the `source` field cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A call without `country`, or a filter on a disallowed field, returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `country` | Required | No | No | Required exact country name in title case, 86 values. Top: United States (5,161); China (2,092); India (586); Korea, South (379); Hong Kong (210); Vietnam (200); Taiwan (194); Canada (110); Japan (98); Singapore (78); Pakistan (72); Italy (68); United Kingdom (48); Germany (40). South Korea is Korea, South as the FMC writes it. |
| `org_number` | Optional | No | No | FMC organization number as 6 digit text, such as 000142. Unique in every row. Exact match. |
| `name` | Optional | Yes | No | Company name as published by the FMC, mostly in capitals, with stray spaces trimmed. 60 rows where the licensee is a person show Individual licensee (name withheld). Partial wildcards allowed, such as *LOGISTICS* or MAERSK*. |
| `trade_names` | Optional | Yes | No | Trade names (doing business as names) from the NVOCC and freight forwarder records, joined with " \| " when there is more than one. Blank in 8,296 rows. Partial wildcards allowed, such as *EXPRESS*. |
| `address_line1` | Optional | Yes | No | First street line. All caps source values changed to title case. Blank in 60 rows (individual licensees, street withheld). Partial wildcards allowed. |
| `address_line2` | Optional | Yes | No | Second street line, often a suite or district. Blank in 3,814 rows. Partial wildcards allowed. |
| `city` | Optional | Yes | No | City as published, in title case. Outside the US it often includes a postal code or province, such as Shanghai 200080 or Ningbo, Zhejiang, so use a wildcard like Shanghai* or *Angeles*. 3,428 distinct values, none blank. |
| `state` | Optional | Yes | No | Two letter US state or territory code, such as CA (1,332), FL (1,141), NY (606), NJ (449), TX (419), IL (246), PR (43). Blank for almost all rows outside the US and for 2 US rows. Partial wildcards allowed. |
| `postal_code` | Optional | Yes | No | ZIP or postal code as text. Present for 5,156 US rows and 15 others; most non US rows put the code in city instead. Partial wildcards allowed, such as 900*. |
| `phone` | Optional | No | No | Main office phone as text, as published, with any leading plus sign removed (international numbers start with the country code, such as 852 2322 4030). Only the first number is kept when the source lists more than one. Numbers that look like mobile phones were removed, and phone is blank in 1,341 rows. Exact match. |
| `oti_type` | Optional | No | No | License type. Exact match. Values: NVOCC (6,563); NVOCC and Freight Forwarder (2,609); Freight Forwarder (674). All freight forwarder rows are US based. |
| `nvocc` | Optional | No | No | Yes when the firm is a non vessel operating common carrier (NVOCC), else No. Exact match. Yes (9,172); No (674). |
| `freight_forwarder` | Optional | No | No | Yes when the firm is an ocean freight forwarder, else No. Exact match. Yes (3,283); No (6,563). |
| `license_number` | Optional | No | No | FMC license number as 6 digit text. Blank in 4,621 rows (foreign NVOCCs that are registered and bonded but not licensed). A few license numbers appear on two org numbers. Exact match. |
| `licensed` | Optional | No | No | Yes when the row has a license number, else No. Exact match. Yes (5,225; 5,159 in the US and 66 abroad); No (4,621; all but 2 outside the US). |
| `renewal_date` | Optional | Yes | No | License or registration renewal date as YYYY-MM-DD text. Use wildcards, such as 2027* or 2027-01*. By year: 2026 (161), 2027 (2,821), 2028 (4,113), 2029 (2,679), 2030 (61), 2032 (5); 3 earlier and 3 blank. 75 rows show a date before the list date but are still on the active list. |
| `nvocc_surety` | Optional | Yes | No | Surety company on the NVOCC bond, in title case, such as American Alternative Insurance Corporation (2,908). Blank when the firm is not an NVOCC. Partial wildcards allowed. |
| `nvocc_bond_amount` | Optional | No | Yes | NVOCC bond amount in US dollars. 75000 (4,540, licensed US NVOCCs), 150000 (4,626, mostly foreign registered NVOCCs), a few others. Blank when not an NVOCC. Range uses two dots, such as 100000..150000. |
| `ff_surety` | Optional | Yes | No | Surety company on the freight forwarder bond, in title case. Blank when the firm is not a freight forwarder. Partial wildcards allowed. |
| `ff_bond_amount` | Optional | No | Yes | Freight forwarder bond amount in US dollars, almost always 50000 (3,276). Blank when not a freight forwarder. Range uses two dots, such as 50000..75000. |
| `bond_amount` | Optional | No | Yes | Total of the NVOCC and freight forwarder bonds in US dollars, present in every row. Values: 150000 (4,628); 125000 (2,599); 75000 (1,940); 50000 (672); a few between 85000 and 210000. Range uses two dots, such as 75000..150000. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees from the US Census Bureau Geocoder, for US rows only. Present in 4,656 of 5,161 US rows; blank for all other countries and for US rows the geocoder could not place. Bounds 13.496493..61.221433 (Guam to Alaska). Range uses two dots, such as 33.6..34.2. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees from the same geocoder. Present in 4,656 rows. Bounds -157.997998..144.827098. Range uses two dots, such as -118.5..-117.9. |
| `coord_source` | Optional | No | No | Where the map point came from. Exact match. Values: US Census Geocoder, exact (4,100); US Census Geocoder, non exact (556); blank (5,190). |
| `source` | Disallowed | No | No | Source name and license. Not filterable. |
| `updated` | Disallowed | No | No | FMC list date 2026-11-06, as text. Not filterable. |

## Response fields

Blank source cells are returned as `null`. `nvocc_bond_amount`, `ff_bond_amount` and `bond_amount` are integers. `lat` and `lng` are numbers. All other values are strings.

| Field | Type | Description |
|---|---|---|
| `org_number` | string | FMC organization number as 6 digit text. Unique in every row. |
| `name` | string | Company name as published. 60 individual licensees show Individual licensee (name withheld). |
| `trade_names` | string | Trade names, joined with " \| " when there is more than one. Null in 8,296 rows. |
| `address_line1` | string | First street line. Null in 60 rows. |
| `address_line2` | string | Second street line, often a suite or district. Null in 3,814 rows. |
| `city` | string | City as published. Outside the US it often includes a postal code or province. |
| `state` | string | Two letter US state or territory code. Null for almost all rows outside the US. |
| `postal_code` | string | ZIP or postal code as text. Null in most non US rows. |
| `country` | string | Country name in title case. 86 values. |
| `phone` | string | Main office phone as text. Null in 1,341 rows. |
| `oti_type` | string | NVOCC, NVOCC and Freight Forwarder, or Freight Forwarder. |
| `nvocc` | string | Yes or No. |
| `freight_forwarder` | string | Yes or No. |
| `license_number` | string | FMC license number as 6 digit text. Null in 4,621 rows. |
| `licensed` | string | Yes or No. |
| `renewal_date` | string | License or registration renewal date, YYYY-MM-DD. Null in 3 rows. |
| `nvocc_surety` | string | Surety company on the NVOCC bond. Null when the firm is not an NVOCC. |
| `nvocc_bond_amount` | number | NVOCC bond amount in US dollars. Null when the firm is not an NVOCC. |
| `ff_surety` | string | Surety company on the freight forwarder bond. Null when the firm is not a freight forwarder. |
| `ff_bond_amount` | number | Freight forwarder bond amount in US dollars. Null when the firm is not a freight forwarder. |
| `bond_amount` | number | Total of the NVOCC and freight forwarder bonds in US dollars. Present in every row. |
| `lat` | number | Latitude in decimal degrees, US rows only. Null in 5,190 rows. |
| `lng` | number | Longitude in decimal degrees, US rows only. Null in 5,190 rows. |
| `coord_source` | string | US Census Geocoder, exact or US Census Geocoder, non exact. Null in 5,190 rows. |
| `source` | string | US Federal Maritime Commission, OTI List (CC0 1.0). |
| `updated` | string | FMC list date 2026-11-06. |

## Data notes

* 60 licensees are people, not companies (59 in the US and 1 in India). For these rows the name shows Individual licensee (name withheld), the street lines and phone are blank, and 5 trade names that repeat the person's name were removed. City, state, postal code, license and bond details stay.
* The names and titles of the qualifying individuals (QI 1 to QI 3), fax numbers, bond numbers and bond effective dates were dropped.
* Only the first phone number is kept when two or more are listed (83 rows). Numbers that a phone number library classes as mobile for that country were removed, which blanked the phone in 1,251 company rows (China 694, India 306, Vietnam 68, Hong Kong 28, Singapore 13, others 142). A leading plus sign was removed from 390 numbers. US numbers cannot be told apart from mobiles and were kept. In all, phone is null in 1,341 rows. No rows were dropped.
* Map points come only from the US Census Bureau Geocoder (batch address service, Public_AR_Current benchmark, run November 11, 2026, Asia/Singapore), a US government service whose output is in the public domain. Points were matched on the published street, city, state and ZIP. Rows outside the US have no point. No OpenStreetMap or other Open Database License data was used, and no point was guessed.
* 4,656 of 5,161 US rows (90.2%) have a point: US Census Geocoder, exact (4,100) or US Census Geocoder, non exact (556). 505 US rows are blank: 413 the geocoder could not match, 32 ties between two places, 59 individual licensees whose street is withheld, and 1 where the matched state did not agree with the listed state. None of the 4,685 rows outside the US has a point. Bounds: latitude 13.496493..61.221433; longitude -157.997998..144.827098 (Guam and the Northern Mariana Islands to Alaska and Puerto Rico). Non exact matches may sit at the street or ZIP level rather than the building.
* The list covers active OTIs only; revoked or surrendered firms are not in it. Company names, cities and street lines outside the US are as entered by the firms and often mix the postal code into the city. A few license numbers appear on two org numbers (15 cases), as in the source. 2 rows are listed with country United States but have addresses in China; they are kept as published. 75 rows have a renewal date before the list date but are still on the active list.
* Changes from the source: shorter field names; stray spaces trimmed; all caps addresses, cities and surety names changed to title case (company names and trade names kept as published); country names in title case, with 3 rows spelled UNTED STATES corrected to United States and 8 rows of CHINA with a trailing space trimmed; renewal dates changed from 30-Jun-2029 style to 2029-06-30; the two NVOCC and two freight forwarder trade name fields joined into `trade_names`; yes or no fields added for `nvocc`, `freight_forwarder` and `licensed`; `bond_amount` added as the total of the two bonds.

## Examples

Firms in Singapore whose published name includes LOGISTICS. This call returns 20 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_fmc_ocean_intermediaries' --data-urlencode 'country=Singapore' --data-urlencode 'name=*LOGISTICS*'
```

First record:

```json
[
  {
    "org_number": "019956",
    "name": "RENUS LOGISTICS PTE LTD",
    "trade_names": null,
    "address_line1": "46A Tanjong Penjuru, #04-01/03",
    "address_line2": "Global Trade Logistics Centre",
    "city": "Singapore 609040",
    "state": null,
    "postal_code": null,
    "country": "Singapore",
    "phone": "65-6264-3988",
    "oti_type": "NVOCC",
    "nvocc": "Yes",
    "freight_forwarder": "No",
    "license_number": null,
    "licensed": "No",
    "renewal_date": "2026-02-18",
    "nvocc_surety": "Southwest Marine And General Insurance Company",
    "nvocc_bond_amount": 150000,
    "ff_surety": null,
    "ff_bond_amount": null,
    "bond_amount": 150000,
    "lat": null,
    "lng": null,
    "coord_source": null,
    "source": "US Federal Maritime Commission, OTI List (CC0 1.0)",
    "updated": "2026-11-06"
  }
]
```

US offices in a Los Angeles area box. This call returns 783 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_fmc_ocean_intermediaries' --data-urlencode 'country=United States' --data-urlencode 'lat=33.6..34.2' --data-urlencode 'lng=-118.5..-117.9'
```

Licensed firms in Canada. This call returns 21 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_fmc_ocean_intermediaries' --data-urlencode 'country=Canada' --data-urlencode 'licensed=Yes'
```

## Data source

[FMC Ocean Transportation Intermediary (OTI) List](https://www2.fmc.gov/OTI/reports/OTI_List.xlsx), "List as of: 2026-11-06", downloaded November 11, 2026, Asia/Singapore, from the [US Federal Maritime Commission](https://www.fmc.gov/). The [FMC data catalog](https://www.fmc.gov/data.json) marks the licensed OTI freight forwarder and NVOCC lists (entries FMC-001 and FMC-002) with the [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) public domain dedication. As a work of the US government it is also not subject to copyright in the US ([17 U.S.C. § 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)). You may copy, change, share and use it for business with no conditions.

Map points come from the [US Census Bureau Geocoder](https://geocoding.geo.census.gov/geocoder/), a US government service whose output is in the public domain.

Credit: US Federal Maritime Commission, OTI List (CC0 1.0), list date 2026-11-06. Map points: US Census Bureau Geocoder. This API is not made, approved or endorsed by the Federal Maritime Commission, and the FMC name and seal are not used to suggest any link. Check a firm's current status with the FMC before you rely on it; licenses lapse and renew between list dates.

