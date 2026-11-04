# US Alzheimer's Disease Research Centers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 41](https://img.shields.io/badge/records-41-lightgrey) ![updated 2026-08-17](https://img.shields.io/badge/updated-2026--08--17-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

All 41 Alzheimer's Disease Research Centers (ADRCs) funded by the National Institute on Aging (NIA), as listed on Alzheimers.gov, in 25 states. The list includes 3 satellite sites and 1 exploratory center. ADRCs study Alzheimer's disease and related dementias, and many invite people to join research studies. Each row has the center name, host institution, street address, city, state, ZIP code, phone, second phone, website, general contact email, satellite and exploratory flags, the parent center of a satellite and a map point. Filter by the required `state`, then by city, center name, host institution, satellite or exploratory status or a coordinate range.

Most centers: California (8), New York and Pennsylvania (3 each), then Florida, Illinois, Massachusetts, North Carolina and Texas (2 each). Alzheimers.gov last updated the source page on 2026-08-17.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_alzheimers_disease_research_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 14 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 41 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `state` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

ZIP codes, phones and dates are text, so leading zeros stay, such as ZIP `06519`. `is_satellite` and `is_exploratory` hold the text `yes` or `no`. Only `latitude` and `longitude` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | Two letter state code, such as `CA` (8), `NY` (3), `PA` (3) or `*A` (15). Wildcards work, so `state=*` lists all 41 rows. |
| `center_name` | Optional | Yes | No | Center name as Alzheimers.gov lists it, such as `*Mayo*` (2) or `*Research Center*` (23). The two Mayo Clinic centers carry `(Jacksonville)` or `(Rochester)` to tell them apart. |
| `host_institution` | Optional | Yes | No | University, hospital or group that runs the center, such as `*University of California*` (4). Consortium centers list each member, separated by semicolons (6 rows). |
| `address` | Optional | No | No | Street address, often with the building or suite. For 6 consortium centers (Arizona, 1Florida, Michigan, Duke and UNC, Cleveland, South Texas) the address comes from the NACC directory because Alzheimers.gov gives none. |
| `city` | Optional | Yes | No | City, such as `New York` (3), `Chicago` (2), `Pittsburgh` (2) or `San*` (2). |
| `zip` | Optional | No | No | ZIP code, 5 digits or ZIP+4, such as `06519`. Text, so leading zeros are kept. |
| `phone` | Optional | No | No | Main phone number, such as `205-996-3679`. Text. Null on 1 row (Oregon). |
| `alt_phone` | Optional | No | No | Second phone number where the source gives one (3 rows: Emory, Wake Forest, South Texas). Text. |
| `website` | Optional | No | No | Center website. Null on 2 satellite rows that have none. |
| `email` | Optional | No | No | General contact email of the center (no personal addresses). Null on 17 rows. |
| `is_satellite` | Optional | No | No | Exact: `yes` (3, a satellite site of another center) or `no` (38). |
| `parent_center` | Optional | No | No | For satellite sites, the `center_name` of the main center they belong to. Null on 38 rows. |
| `is_exploratory` | Optional | No | No | Exact: `yes` (1, Vanderbilt, an exploratory center with a P20 grant) or `no` (40). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 29.506095 (San Antonio) to 47.604154 (Seattle). Example: `latitude=40..43&longitude=-75..-71` for the New York to Boston area (6). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -122.684656 (Portland) to -71.050533 (Charlestown). Use it with `latitude` to search a box, or `longitude=-125..-114` for the West Coast (10). |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from. See the notes. |
| `source` | Disallowed | No | No | Data source for the row, `https://www.alzheimers.gov/taking-action/national-research-centers` (National Institute on Aging, NIH). For 6 rows the address is from `https://naccdata.org/nacc-directory/` as well. |
| `updated` | Disallowed | No | No | Date the Alzheimers.gov page was last updated, `2026-08-17` on every row. Text. |

* Parameters are combined with AND. Text matches ignore case, so `is_satellite=YES` works the same as `is_satellite=yes`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on `geocode_quality`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source`.
* Wildcards work only on `state`, `center_name`, `host_institution` and `city`. A wildcard on another text field returns `[]`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `center_name` | string | Center name. |
| `host_institution` | string | Institution or institutions that run the center, separated by semicolons. |
| `address` | string | Street address. |
| `city` | string | City. |
| `state` | string | Two letter state code. |
| `zip` | string | ZIP code, 5 digits or ZIP+4. |
| `phone` | string | Main phone number. Null for 1 row. |
| `alt_phone` | string | Second phone number. Null for 38 rows. |
| `website` | string | Center website. Null for 2 rows. |
| `email` | string | General contact email. Null for 17 rows. |
| `is_satellite` | string | `yes` or `no`. |
| `parent_center` | string | Main center of a satellite site. Null for 38 rows. |
| `is_exploratory` | string | `yes` or `no`. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | Where the map point comes from. |
| `source` | string | Link to the source page. |
| `updated` | string | Date the source page was last updated, `YYYY-MM-DD`. |

Notes:

* Satellites: the 3 satellite sites are UC Davis East Bay (part of the UC Davis center), UC San Diego Hispanic Satellite and Outreach (part of the Shiley-Marcos center) and the Alzheimer's Outreach Program (part of the University of Pittsburgh center).
* Names: `(Jacksonville)` and `(Rochester)` were added to the two Mayo Clinic center names to tell them apart, so each name is unique.
* Emails: emails that were broken in the source were left null. Only general center addresses are kept.
* Most websites use the secure https address, and the current address where a site has moved. 4 websites are kept as Alzheimers.gov lists them because a secure link to them could not be confirmed.
* Map points: every row has a point. `geocode_quality` shows where it comes from, best first: `census_exact` (31, US Census geocoder exact match), `census_nonexact` (5, Census match with small differences), `arcgis_pointaddress` (3, ArcGIS address point), `arcgis_streetaddress` (1, ArcGIS street segment) and `arcgis_institution` (1, ArcGIS place match on the campus building).

## Examples

A wildcard on the host institution in one state. University of California centers. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_alzheimers_disease_research_centers" --data-urlencode "state=CA" --data-urlencode "host_institution=*University of California*" | jq -r '.[] | [.center_name, .city, .zip, .is_satellite] | @tsv'
```

```text
University of California Davis Alzheimer's Disease Center	Sacramento	95817-4540	no
UC Davis East Bay	Walnut Creek	94598	yes
Shiley-Marcos Alzheimer's Disease Research Center	La Jolla	92037-0948	no
Alzheimer's Disease Research Center UCSF	San Francisco	94143	no
```

Satellite sites in California, shown as a full row. This call returns 2 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_alzheimers_disease_research_centers" --data-urlencode "state=CA" --data-urlencode "is_satellite=yes"
```

The first row of the response:

```json
[
  {
    "__id": 5,
    "center_name": "UC Davis East Bay",
    "host_institution": "University of California, Davis",
    "address": "100 North Wiget Lane, Suite 150",
    "city": "Walnut Creek",
    "state": "CA",
    "zip": "94598",
    "phone": "925-357-6515",
    "alt_phone": null,
    "website": null,
    "email": null,
    "is_satellite": "yes",
    "parent_center": "University of California Davis Alzheimer's Disease Center",
    "is_exploratory": "no",
    "latitude": 37.924724,
    "longitude": -122.023647,
    "geocode_quality": "census_exact",
    "source": "https://www.alzheimers.gov/taking-action/national-research-centers",
    "updated": "2026-08-17"
  },
  ...
]
```

Range on latitude and longitude with the required wildcard. Centers in the New York to Boston area. This call returns 6 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_alzheimers_disease_research_centers" --data-urlencode "state=*" --data-urlencode "latitude=40..43" --data-urlencode "longitude=-75..-71" | jq -r '.[] | [.center_name, .city, .state, .zip] | @tsv'
```

```text
Yale University Alzheimer's Disease Center	New Haven	CT	06519
Boston University Alzheimer's Disease Center	Boston	MA	02118
Massachusetts Alzheimer's Disease Research Center	Charlestown	MA	02129
Columbia University Alzheimer's Disease Research Center	New York	NY	10032
Alzheimer's Disease Research Center, Icahn School of Medicine at Mount Sinai	New York	NY	10029
NYU Alzheimer's Disease Center	New York	NY	10016
```

The exploratory center:

```bash
curl -s "https://api.jael.ee/JLEE/us_alzheimers_disease_research_centers?state=*&is_exploratory=yes" | jq -r '.[0].center_name'
```

```text
Vanderbilt Alzheimer's Disease Research Center
```

## Data source

The center list, names, host institutions, addresses, phones, websites and emails come from the Alzheimers.gov page [Alzheimer's Disease Research Centers](https://www.alzheimers.gov/taking-action/national-research-centers), run by the National Institute on Aging, updated on 2026-08-17 and fetched on 2026-11-04. For 6 consortium centers with no address on that page, the address comes from the [NACC directory](https://naccdata.org/nacc-directory/). The address text was split into street, city, state and ZIP, second phone numbers were moved to `alt_phone`, broken email links were left null, websites were moved to their secure https address where one works, and satellite and exploratory flags were added. Map points come from the US Census Bureau geocoder and, where it found no match, ArcGIS World Geocoding. Check with the center before you visit.

Credit lines:

* Source: National Institute on Aging, Alzheimers.gov, Alzheimer's Disease Research Centers (https://www.alzheimers.gov/taking-action/national-research-centers), updated 17 August 2026, fetched 4 November 2026. US public domain. Alzheimers.gov says "Permission to use Alzheimers.gov materials is not required" (https://www.alzheimers.gov/about-us).
* Six consortium addresses from the NACC directory (https://naccdata.org/nacc-directory/).
* Coordinates: US Census Bureau geocoder (https://geocoding.geo.census.gov), public domain, and ArcGIS World Geocoding for 5 rows.
* NIH and NIA do not endorse this API. Not affiliated with Alzheimers.gov, NIH, NIA or the US government.

[Back to the API list](../../README.md)
