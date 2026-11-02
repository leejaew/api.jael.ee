# UK Further Education Colleges

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 222](https://img.shields.io/badge/records-222-lightgrey) ![updated 2026-11-02](https://img.shields.io/badge/updated-2026--11--02-orange)

All 222 open colleges in England on the Department for Education (DfE) Get Information about Schools register: 208 further education colleges and 14 sixth form centres. Each row has the DfE URN, UKPRN, college type, age range, address, local authority, region, parliamentary constituency, website, phone and a map point. Filter by the required `region`, then by name, town, postcode, local authority, college type or a coordinate or age range.

Rows by type: General Further Education College 152, Sixth Form College (General) 21, Sixth Form College (Voluntary Aided) 13, Specialist Designated College 11, Land-Based College 9, and 3 in smaller groups, plus 13 sixth form centres marked `Not applicable` and 1 blank. Most colleges: North West (37), London (35) and South East (35). The data comes from the DfE download dated 2026-11-02.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_further_education_colleges` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 22 fields. Disallowed: 4 fields.

`region` must be in every call. Use `region=*` to list all 222 colleges. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | English region as DfE lists it: `North West` (37), `London` (35), `South East` (35), `Yorkshire and the Humber` (26), `West Midlands` (24), `South West` (20), `East of England` (19), `North East` (13) or `East Midlands` (13). Wildcards work, so `region=*` lists all 222 colleges and `region=North*` gives 50. |
| `urn` | Optional | No | No | DfE Unique Reference Number, such as `130401` (City Lit). Unique per row. Exact match only. |
| `ukprn` | Optional | No | No | UK Provider Reference Number, such as `10001463`. Exact match only. Null for 1 row (JMF6 Abingdon), where DfE gives none. |
| `name` | Optional | Yes | No | College name as DfE lists it, such as `*Sixth Form*` (22) or `*College*` (193). |
| `fe_type` | Optional | No | No | Further education type: `General Further Education College` (152), `Sixth Form College (General)` (21), `Sixth Form College (Voluntary Aided)` (13), `Specialist Designated College` (11), `Land-Based College` (9), `Sixth Form College (Voluntary Controlled)` (1) or `Art, Design and Performing Arts College` (1). `Not applicable` for 13 sixth form centres and null for 1. Exact match only. |
| `establishment_type` | Optional | No | No | `Further education` (208) or `Sixth form centres` (14). Exact match only. |
| `religious_character` | Optional | No | No | `Does not apply` (193) or `Roman Catholic` (13). Null for 16 rows where DfE gives none. Exact match only. |
| `gender` | Optional | No | No | Who the college admits. `Mixed` for 221 rows; null for 1 (Hartpury College). Exact match only. |
| `statutory_low_age` | Optional | No | Yes | Youngest age the college takes, from 14 to 19 (`16` for 192 rows, `14` for 27). Example: `statutory_low_age=14..15` (27). |
| `statutory_high_age` | Optional | No | Yes | Oldest age the college takes, from 18 to 99, where `99` means no upper limit (193 rows). Example: `statutory_high_age=18..24` (29). |
| `street` | Optional | Yes | No | First address line as DfE lists it. Null for 7 rows. |
| `locality` | Optional | Yes | No | Second address line, such as a district. Null for most rows. |
| `address3` | Optional | Yes | No | Third address line, given for 24 rows. |
| `town` | Optional | Yes | No | Town, such as `London` (21) or `Leeds` (4). Null for 6 rows with no address. |
| `county` | Optional | Yes | No | County as DfE lists it, such as `*Yorkshire*` (17). Null for 54 rows. |
| `postcode` | Optional | Yes | No | Postcode, such as `LS*` (3) or `B*` (30). Null for 6 rows with no address. |
| `local_authority` | Optional | Yes | No | Local authority, 127 in all, such as `Leeds` (3), `Hampshire` (8) or `*Yorkshire*` (5). |
| `parliamentary_constituency` | Optional | Yes | No | Parliamentary constituency, such as `*Leeds*` (3). Null for 6 rows with no address. |
| `website` | Optional | Yes | No | College website, given for 169 rows. Null for 53 rows. See the notes below. |
| `telephone` | Optional | Yes | No | Phone number as DfE lists it, digits only with the leading 0, as text, such as `0113*`. Null for 9 rows. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS84), from 50.262803 to 54.986229. Example: `latitude=51.3..51.7`. Null for 6 rows. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS84), from -5.100783 to 1.743636. Example: `longitude=-0.5..0.3`. Null for 6 rows. |
| `geocode_note` | Optional | No | No | How the map point was found: `easting_northing` (211), `postcode_postcodes_io` (5) or `no_location` (6). See the notes below. Exact match only. |
| `opened_date` | Disallowed | No | No | Date the college opened, `YYYY-MM-DD`, given for 24 rows. |
| `last_changed` | Disallowed | No | No | Date DfE last changed the record, `YYYY-MM-DD`. |
| `source` | Disallowed | No | No | Data source and credit for the row: Department for Education, Get Information about Schools. |
| `updated` | Disallowed | No | No | Date of the DfE download file, `YYYY-MM-DD` (`2026-11-02` for every row). |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `region` returns HTTP 400 with `Your request is missing the following required parameters: region. You did not supply any.`
* A filter on `opened_date`, `last_changed`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source` (or the field you used).

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `urn` | integer | DfE Unique Reference Number. |
| `ukprn` | integer | UK Provider Reference Number. Null for 1 row. |
| `name` | string | College name. |
| `fe_type` | string | Further education type. |
| `establishment_type` | string | `Further education` or `Sixth form centres`. |
| `religious_character` | string | Religious character, if any. |
| `gender` | string | Who the college admits. |
| `statutory_low_age` | integer | Youngest age taken. |
| `statutory_high_age` | integer | Oldest age taken, `99` for no limit. |
| `street` | string | First address line. |
| `locality` | string | Second address line. |
| `address3` | string | Third address line. |
| `town` | string | Town. |
| `county` | string | County. |
| `postcode` | string | Postcode. |
| `local_authority` | string | Local authority. |
| `region` | string | English region. |
| `parliamentary_constituency` | string | Parliamentary constituency. |
| `website` | string | College website. |
| `telephone` | string | Phone number with the leading 0. |
| `latitude` | number | Latitude in decimal degrees. |
| `longitude` | number | Longitude in decimal degrees. |
| `geocode_note` | string | How the map point was found. |
| `opened_date` | string | Date the college opened. |
| `last_changed` | string | Date DfE last changed the record. |
| `source` | string | Data source and credit. |
| `updated` | string | Date of the DfE download file. |

## Notes

* **Which rows.** Only rows in the DfE group `Colleges` with the status `Open` are kept. Schools, academies and universities are not in this API. Head teacher names in the source are left out.
* **Sixth form centres.** The 14 sixth form centres are joint sixth forms run by groups of schools. Most have `fe_type` set to `Not applicable`, and several have no address in the source.
* **Map points.** DfE gives a British National Grid point (easting and northing) for most rows, and these were turned into latitude and longitude (`easting_northing`, 211 rows). For 5 rows with no grid point but a postcode, the point is the postcode center from postcodes.io (`postcode_postcodes_io`), not the door. 6 rows have no address, postcode or point in the source (`no_location`), so their coordinates are null: URNs 132913, 132916, 132942, 134821, 150214 and 152679.
* **Websites.** Links that were checked to load over https were stored with `https://` (158 rows). 11 links could not be checked and are kept as DfE lists them, some with no scheme and some with the plain http scheme.
* **DfE values kept as is.** Names, addresses and phone numbers are kept the way DfE wrote them. 4 phone numbers have only 10 digits in the source. DfE uses `1900-01-01` as a stand in for an unknown open date; those were removed, so `opened_date` is given for only 24 rows.

## Examples

Colleges in one region with a postcode wildcard. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_further_education_colleges" --data-urlencode "region=Yorkshire and the Humber" --data-urlencode "postcode=LS*"
```

```json
[
  {
    "__id": 70,
    "urn": 130542,
    "ukprn": 10003855,
    "name": "Leeds College of Building",
    "fe_type": "General Further Education College",
    "establishment_type": "Further education",
    "religious_character": "Does not apply",
    "gender": "Mixed",
    "statutory_low_age": 16,
    "statutory_high_age": 99,
    "street": "North Street",
    "locality": null,
    "address3": null,
    "town": "Leeds",
    "county": "West Yorkshire",
    "postcode": "LS2 7QT",
    "local_authority": "Leeds",
    "region": "Yorkshire and the Humber",
    "parliamentary_constituency": "Leeds Central and Headingley",
    "website": "https://www.lcb.ac.uk",
    "telephone": "01132226000",
    "latitude": 53.802946,
    "longitude": -1.535223,
    "geocode_note": "easting_northing",
    "opened_date": null,
    "last_changed": "2025-06-25",
    "source": "Department for Education, Get Information about Schools",
    "updated": "2026-11-02"
  },
  ...
]
```

Land based colleges across all regions. This call returns 9 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_further_education_colleges" --data-urlencode "region=*" --data-urlencode "fe_type=Land-Based College" | jq -r '.[] | [.name, .town, .region] | @tsv'
```

```text
Capel Manor College	Enfield	London
Bishop Burton College	Beverley	Yorkshire and the Humber
Askham Bryan College	York	Yorkshire and the Humber
Reaseheath College	Nantwich	North West
Plumpton College	Lewes	South East
Sparsholt College Hampshire	Winchester	South East
Myerscough College	Preston	North West
Moulton College	Northampton	East Midlands
Hartpury College	Gloucester	South West
```

A box on the map in central London. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_further_education_colleges" --data-urlencode "region=London" --data-urlencode "latitude=51.5..51.55" --data-urlencode "longitude=-0.15..-0.05" | jq -r '.[] | [.name, .postcode, .latitude, .longitude] | @tsv'
```

```text
City Lit	WC2B 4BA	51.514902	-0.120124
WM College	NW1 1TR	51.535236	-0.136041
Workers' Educational Association	EC2A 4XW	51.524196	-0.080978
Capital City College	WC1X 8RA	51.526904	-0.118793
```

A range on the oldest age taken. This call returns 29 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_further_education_colleges" --data-urlencode "region=*" --data-urlencode "statutory_high_age=18..24" | jq length
```

## Data source

The college list comes from the Department for Education [Get Information about Schools](https://get-information-schools.service.gov.uk/Downloads) public download `edubasealldata20261102.csv`, fetched on 2026-11-02. It is published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited. The site says "All content is available under the Open Government Licence v3.0, except where otherwise stated." Changes: filtered to open colleges, head teacher names removed, columns renamed, British National Grid points turned into latitude and longitude, 5 points added from postcode centers, the `1900-01-01` stand in open dates removed, and website links set to https where checked. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: Department for Education, Get Information about Schools (https://get-information-schools.service.gov.uk).
* Postcode points: postcodes.io (https://postcodes.io), from the ONS Postcode Directory. Source: Office for National Statistics, licensed under the Open Government Licence v3.0. Contains OS data © Crown copyright and database right.
* Not endorsed by the Department for Education. No DfE logos are used.

[Back to the API list](../../README.md)
