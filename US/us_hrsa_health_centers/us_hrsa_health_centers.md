# US HRSA Health Centers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 19537](https://img.shields.io/badge/records-19537-lightgrey) ![updated 2026-11-05](https://img.shields.io/badge/updated-2026--11--05-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

The 19,537 active sites of US community health centers from HRSA (the Health Resources and Services Administration). 18,570 sites belong to FQHCs (Federally Qualified Health Centers, the community clinics funded by HRSA) and 967 to FQHC look-alikes, which meet the same standards without the federal grant. Each row has the site name, the health center that runs it, the address, phone, website, setting (such as school or clinic), type (permanent, seasonal or mobile van), weekly hours, coordinates and HRSA ids. Filter by the required `state`, then by city, name, setting, type, ZIP code, county, hours or coordinates.

The list covers the 50 states, DC, Puerto Rico, Guam, the U.S. Virgin Islands, American Samoa, the Northern Mariana Islands, the Marshall Islands, the Federated States of Micronesia and Palau (59 values in `state`). California has the most sites (3,189), then New York (915) and Texas (854).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_hrsa_health_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per site |

## Query parameters

Required: 1 field (`state`). Optional: 18 fields. Disallowed: 10 fields.

`state` must be in every call. Use `state=*` to list all 19,537 sites. Disallowed means the field is returned in the output but cannot be used as a filter. Without `state` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

`operating_hours_per_week`, `lat` and `lng` are numbers and take a range, such as `operating_hours_per_week=40..168`. Every other field is text, including ZIP codes, phone numbers, ids and dates.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | Full state or territory name, such as `Ohio` (702), `California` (3,189), `Puerto Rico`, `Guam`, `District of Columbia` or `Federated States of Micronesia`. Wildcards work, so `state=*` lists all 19,537 sites and `state=New*` gives 1,419. For the 677 sites whose address HRSA keeps private, this is the state of the health center that runs the site. |
| `site_name` | Optional | Yes | No | Site name as HRSA lists it, such as `Tri-County Community Health Center` or `Mobile Optometry Van`. Wildcards work, so `site_name=*School*` finds sites with School in the name. |
| `health_center_name` | Optional | Yes | No | Health center that runs the site (the grantee or look-alike), mostly in capitals as HRSA gives it, such as `ALLIANCE MEDICAL CENTER, INC`. Wildcards work, such as `health_center_name=*ALLIANCE*`. |
| `health_center_type` | Optional | No | No | `FQHC` (18,570 sites) or `Look-Alike` (967 sites). |
| `site_type` | Optional | No | No | What the site does: `Service Delivery Site` (16,688), `Administrative/Service Delivery Site` (1,512) or `Administrative` (1,337, offices that may not see patients). |
| `city` | Optional | Yes | No | City of the site, such as `Columbus` or `Los Angeles`. Wildcards work, such as `city=San*`. Blank where the address is private. |
| `state_code` | Optional | No | No | Two letter postal code, such as `OH`, `CA` or `PR`. Same rule as `state` for private addresses. |
| `address_public` | Optional | No | No | `Yes` when HRSA publishes the site's location, `No` for the 677 sites whose address, city and ZIP code HRSA keeps private, such as domestic violence shelters. |
| `zip` | Optional | Yes | No | ZIP code as text, often ZIP+4 such as `02919-7130`. Use a wildcard to match the five digit ZIP code, such as `zip=43215*` (6 in Ohio). |
| `county` | Optional | Yes | No | County, such as `Franklin County` or `Lane County`. Wildcards work, such as `county=Los Angeles*`. Blank on 671 sites. |
| `location_setting` | Optional | Yes | No | Where the site is: `All Other Clinic Types` (13,996), `School` (4,831), `Unknown` (531), `Hospital` (100), `Transitional Care in Carceral Setting` (38), `Nursing Home` (24), `Domestic Violence` (14) or `Correctional Facility` (3). Wildcards work, such as `location_setting=*Carceral*`. |
| `location_type` | Optional | No | No | `Permanent` (15,638), `Seasonal` (2,476) or `Mobile Van` (1,423). |
| `operator_type` | Optional | No | No | Who runs the site: `Health Center/Applicant` (19,186), `Subrecipient` (198) or `Contractor` (153). |
| `operating_hours_per_week` | Optional | No | Yes | Hours the site is open each week, 0 to 168, as a number. Use a range such as `operating_hours_per_week=40..168`. Blank on 790 sites. |
| `operating_schedule` | Optional | No | No | `Full-Time`, `Part-Time` or `Unknown` (most sites). |
| `operating_calendar` | Optional | No | No | `Year-Round`, `Seasonal` or `Unknown` (most sites). |
| `lat` | Optional | No | Yes | Latitude from HRSA geocoding, as a number. Use a range such as `lat=40..40.1`. Blank on 679 sites, including all private addresses. |
| `lng` | Optional | No | Yes | Longitude from HRSA geocoding, as a number (negative in the US). Use a range such as `lng=-83.1..-82.9`. Blank on 679 sites. |
| `site_address` | Disallowed | No | No | Street address of the site. Blank where HRSA keeps the address private (see `address_public`). Not filterable. |
| `phone` | Disallowed | No | No | Site phone number as text, such as `401-519-1940`. A few carry an extension. Not filterable. |
| `website` | Disallowed | No | No | Site or health center web address as HRSA lists it, sometimes without the `https://` part. Blank on 8,692 sites. Not filterable. |
| `added_to_scope` | Disallowed | No | No | Date HRSA added the site to the health center's scope, `YYYY-MM-DD`, such as `2009-03-01`. Not filterable. |
| `health_center_number` | Disallowed | No | No | HRSA grant or look-alike number of the health center, such as `H80CS12879` or `LALCS40180`. Not filterable. |
| `bhcmis_id` | Disallowed | No | No | HRSA BHCMIS organization id as text, such as `01E00040`. Not filterable. |
| `bphc_site_number` | Disallowed | No | No | HRSA site number, unique for each row, such as `BPS-H80-008326`. Not filterable. |
| `site_npi` | Disallowed | No | No | The site's NPI (National Provider Identifier) as text, where HRSA lists one. Not filterable. |
| `medicare_billing_number` | Disallowed | No | No | The site's FQHC Medicare billing number as text, where HRSA lists one. Not filterable. |
| `source` | Disallowed | No | No | Data source: HRSA Health Center Service Delivery and Look-Alike Sites (`https://data.hrsa.gov/data/download`), a US government work in the public domain. Not filterable. |
| `updated` | Disallowed | No | No | Date of the HRSA file, `YYYY-MM-DD` (`2026-11-05`). Not filterable. |

* Parameters are combined with AND. Text matches ignore case, so `state=ohio` works the same as `state=Ohio`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on a disallowed field returns HTTP 400 with `Request attempts to filter on disallowed parameters: phone`.
* A range written with a hyphen, such as `60-168`, returns HTTP 400 with `Value '60-168' is not a double`. Use two dots.

## Response fields

| Field | Type | Description |
|---|---|---|
| `site_name` | string | Site name as HRSA lists it. |
| `health_center_name` | string | Health center that runs the site. |
| `health_center_type` | string | `FQHC` or `Look-Alike`. |
| `site_type` | string | Service delivery, administrative or both. |
| `site_address` | string | Street address. Null on 678 sites. |
| `city` | string | City. Null on 678 sites. |
| `state` | string | Full state or territory name. |
| `state_code` | string | Two letter postal code. |
| `address_public` | string | `Yes` or `No`. |
| `zip` | string | ZIP code or ZIP+4. Null on 685 sites. |
| `county` | string | County. Null on 671 sites. |
| `phone` | string | Site phone number. |
| `website` | string | Web address. Null on 8,692 sites. |
| `location_setting` | string | Setting, such as School or Hospital. |
| `location_type` | string | Permanent, Seasonal or Mobile Van. |
| `operator_type` | string | Health Center/Applicant, Subrecipient or Contractor. |
| `operating_hours_per_week` | number | Weekly hours, such as `40.0`. Null on 790 sites. |
| `operating_schedule` | string | Full-Time, Part-Time or Unknown. |
| `operating_calendar` | string | Year-Round, Seasonal or Unknown. |
| `added_to_scope` | string | Date added to scope, `YYYY-MM-DD`. |
| `lat` | number | Latitude. Null on 679 sites. |
| `lng` | number | Longitude. Null on 679 sites. |
| `health_center_number` | string | HRSA grant or look-alike number. |
| `bhcmis_id` | string | HRSA BHCMIS organization id. |
| `bphc_site_number` | string | HRSA site number, unique for each row. |
| `site_npi` | string | Site NPI. Null on 13,397 sites. |
| `medicare_billing_number` | string | FQHC Medicare billing number. Null on 10,800 sites. |
| `source` | string | Data source name and link. |
| `updated` | string | Date of the HRSA file, `YYYY-MM-DD`. |

Notes:

* No `__id` field. Use `bphc_site_number` as the key for each site.
* HRSA keeps the location of 677 sites private, such as domestic violence shelters and some school and mobile sites. These rows have `address_public=No` and no address, city, ZIP code or coordinates. HRSA gives no state for them, so `state` and `state_code` come from the health center that runs the site. List them with `state=*&address_public=No`.
* The 14 domestic violence sites all had the same stand in coordinates in the HRSA file, so `lat` and `lng` are null for them.

## Examples

School sites in one city. School based sites in Columbus, Ohio. This call returns 3 rows; the first is shown:

```bash
curl -s "https://api.jael.ee/JLEE/us_hrsa_health_centers?state=Ohio&city=Columbus&location_setting=School"
```

```json
[
  {
    "site_name": "OSU Total Health Care Center Ohio Deaf and Blind Education Services",
    "health_center_name": "OHIO STATE UNIVERSITY, THE",
    "health_center_type": "FQHC",
    "site_type": "Service Delivery Site",
    "site_address": "500 Morse Rd",
    "city": "Columbus",
    "state": "Ohio",
    "state_code": "OH",
    "address_public": "Yes",
    "zip": "43214-1899",
    "county": "Franklin County",
    "phone": "614-685-9994",
    "website": null,
    "location_setting": "School",
    "location_type": "Permanent",
    "operator_type": "Health Center/Applicant",
    "operating_hours_per_week": 40.0,
    "operating_schedule": "Unknown",
    "operating_calendar": "Unknown",
    "added_to_scope": "2026-07-14",
    "lat": 40.06464209,
    "lng": -83.00525205,
    "health_center_number": "H80CS33660",
    "bhcmis_id": "05E01269",
    "bphc_site_number": "BPS-H80-042030",
    "site_npi": null,
    "medicare_billing_number": null,
    "source": "HRSA Health Center Service Delivery and Look-Alike Sites (https://data.hrsa.gov/data/download)",
    "updated": "2026-11-05"
  }
]
```

Range on weekly hours. Ohio sites open 100 hours a week or more. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_hrsa_health_centers" --data-urlencode "state=Ohio" --data-urlencode "operating_hours_per_week=100..168" | jq -r '.[] | [.site_name, .city, .operating_hours_per_week] | @tsv'
```

```text
Gallia Adult Crisis Stabilization Unit	Gallipolis	168.0
Health & Wellness at the Glick Recovery Campus	Cleveland	168.0
SHELTERHOUSE-Barron Center for Men	Cincinnati	168.0
Hopewell Health Centers - Athens Respite	Athens	168.0
```

Wildcard on ZIP code. Ohio sites in ZIP code 43215 (downtown Columbus). This call returns 6 rows:

```bash
curl -s "https://api.jael.ee/JLEE/us_hrsa_health_centers?state=Ohio&zip=43215*" | jq -r '.[] | [.site_name, .zip, .location_type] | @tsv'
```

```text
Southeast Inc., Long Street Admin	43215-2815	Permanent
Franklin Station Health Center	43215-2775	Permanent
PrimaryOne Health 240 Parsons Avenue, Suite 233	43215-5331	Permanent
Health Center at Faith Mission	43215-2641	Permanent
Southeast Inc., Long Street	43215-2815	Permanent
Mobile Coach	43215-2815	Mobile Van
```

## Data source

The sites come from the HRSA file [Health Center Service Delivery and Look-Alike Sites](https://data.hrsa.gov/data/download), dated November 5, 2026 and fetched on November 6, 2026. HRSA refreshes the file every day. Check with the health center before you visit, because hours and sites can change.

Credit lines:

* Source: HRSA. Data from the Health Resources and Services Administration (HRSA) file Health Center Service Delivery and Look-Alike Sites (https://data.hrsa.gov/data/download), dated November 5, 2026, fetched November 6, 2026. HRSA lists no usage limitations for this file. HRSA does not endorse this API or the people who run it. Changes: picked 29 of 55 columns (congressional, senator, FIPS, HHS region and internal code columns left out), renamed columns, shortened health center type to FQHC or Look-Alike, dates as YYYY-MM-DD, county Not Determined left blank, state filled from the health center for the 677 sites whose address HRSA keeps private, and the placeholder coordinates on the 14 domestic violence sites left blank; no change to the substance. US federal government works are in the public domain (17 U.S.C. 105).

[Back to the API list](../../README.md)
