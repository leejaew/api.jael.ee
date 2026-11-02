# UK Student Visa Sponsors

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1291](https://img.shields.io/badge/records-1291-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange)

Every school, college and university on the UK Home Office register of sponsors licensed for the Student and Child Student visa routes: 1,291 rows for 932 sponsors, one row per sponsor, town and route. Each row has the sponsor name, town, other sites, sponsor type, licence rating, route, any action plan and a map point for the town. Filter by the required `sponsor_name`, then by town, sponsor type, rating, route, action plan or a coordinate range.

Rows by type: Independent school 889, Higher Education Institution (HEI) 192, Private provider 107, Publicly funded college 67, Overseas Higher Education Institution 28 and Embedded college offering pathway courses 8. Rows by route: Student 752 and Child Student 539. Most rows: London (222), Oxford (26) and Cambridge (21). The data comes from the Home Office register file dated 2026-10-30.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_student_visa_sponsors` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`sponsor_name`). Optional: 8 fields. Disallowed: 4 fields.

`sponsor_name` must be in every call. Use `sponsor_name=*` to list all 1,291 rows. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `sponsor_name` | Required | Yes | No | Sponsor name as the Home Office lists it, such as `The King's School` (6) or `*Academy*` (18). Wildcards work, so `sponsor_name=*` lists all 1,291 rows and `*University*` gives 173. |
| `town_city` | Optional | Yes | No | The sponsor's main town as the Home Office lists it, such as `London` (222), `Oxford` (26) or `Brighton*` (12). Some rows give a county, a region or `Various` (15) instead of a town. |
| `additional_locations` | Optional | Yes | No | Other sites or embedded colleges the Home Office lists for the sponsor, such as `*Embedded College*` (17). Null for 1,254 rows. |
| `sponsor_type` | Optional | No | No | Sponsor type: `Independent school` (889), `Higher Education Institution (HEI)` (192), `Private provider` (107), `Publicly funded college` (67), `Overseas Higher Education Institution` (28) or `Embedded college offering pathway courses` (8). Exact match only. |
| `status` | Optional | No | No | Licence rating: `Student Sponsor` (928), `Student Sponsor - Track Record` (299) or `Probationary Sponsor` (64). Exact match only. |
| `route` | Optional | No | No | Visa route the licence covers: `Student` (752) or `Child Student` (539). Exact match only. |
| `immigration_compliance` | Optional | No | No | `Subject To Action Plan` (16) when the Home Office has put the sponsor on an action plan. Null for all other rows. Exact match only. |
| `latitude` | Optional | No | Yes | Latitude of the town center in decimal degrees (WGS84), from 50.15522 to 57.648789. Example: `latitude=55..56`. Null for 27 rows. |
| `longitude` | Optional | No | Yes | Longitude of the town center in decimal degrees (WGS84), from -6.770362 to 1.576433. Example: `longitude=-4.5..-3`. Null for 27 rows. |
| `geocode_quality` | Disallowed | No | No | How the map point was found: `town_centroid` (1,254), `near_town_centroid` (7), `station` (2), `address` (1), or no point for `not_a_town` (24), `ambiguous_town` (2) and `not_found` (1). See the notes below. |
| `geocode_match` | Disallowed | No | No | The place the geocoder matched, for checking the point. |
| `source` | Disallowed | No | No | Data source and credit for the row: UK Home Office, Register of licensed sponsors: students. |
| `updated` | Disallowed | No | No | Date of the Home Office register file, `YYYY-MM-DD` (`2026-10-30` for every row). |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `sponsor_name` returns HTTP 400 with `Your request is missing the following required parameters: sponsor_name. You did not supply any.`
* A filter on `geocode_quality`, `geocode_match`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source` (or the field you used).

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `sponsor_name` | string | Sponsor name. |
| `town_city` | string | Main town as the Home Office lists it. |
| `additional_locations` | string | Other sites or embedded colleges. |
| `sponsor_type` | string | Sponsor type. |
| `status` | string | Licence rating. |
| `route` | string | `Student` or `Child Student`. |
| `immigration_compliance` | string | `Subject To Action Plan` or null. |
| `latitude` | number | Latitude of the town center in decimal degrees. |
| `longitude` | number | Longitude of the town center in decimal degrees. |
| `geocode_quality` | string | How the map point was found. |
| `geocode_match` | string | The place the geocoder matched. |
| `source` | string | Data source and credit. |
| `updated` | string | Date of the Home Office register file. |

## Notes

* **One row per sponsor, town and route.** A sponsor licensed for both routes has two rows, one for `Student` and one for `Child Student`. Names also repeat across towns: `The King's School` has rows in Canterbury, Chester, Ely and Harpenden. The source has no sponsor ID, so use `__id` as the row key.
* **Points are town centers, not campuses.** The register gives only a town, so each point is the center of that town from OpenStreetMap Nominatim (`town_centroid`). For towns given as `Nr` or `Near` a place, the point is that place (`near_town_centroid`, 7 rows). `South Croydon` uses the station (`station`, 2 rows) and the one street address given as a town, `566 Chiswick High Road`, uses that address (`address`, 1 row). 27 rows have no point: 24 where the town is a county, a region or `Various` (`not_a_town`), 2 for `Mayfield`, which could be more than one town (`ambiguous_town`), and 1 for `Hope Valley` (`not_found`). Do not use any point as a campus pin.
* **Home Office text kept as is.** Names, towns and other text are kept the way the Home Office wrote them, typos and case included, such as `LONDON` next to `London`, `Berwick Upon Twed`, `Faringdon.` and both `Blandford` and `Blandford Forum`. Only extra spaces at the start or end of a value were trimmed.
* **Case insensitive matching.** Text filters ignore case, so `town_city=London` also returns the 7 rows that say `LONDON`.
* **What a rating means.** The register lists sponsors and their rating on the day of the file. It does not say which courses a sponsor offers. Check the sponsor and the Home Office guidance before you rely on a row.

## Examples

Sponsors with a wildcard on the name in one town. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_student_visa_sponsors" --data-urlencode "sponsor_name=*Academy*" --data-urlencode "town_city=Oxford"
```

```json
[
  {
    "__id": 170,
    "sponsor_name": "CAE Oxford Aviation Academy",
    "town_city": "Oxford",
    "additional_locations": null,
    "sponsor_type": "Private provider",
    "status": "Student Sponsor",
    "route": "Student",
    "immigration_compliance": null,
    "latitude": 51.752013,
    "longitude": -1.25785,
    "geocode_quality": "town_centroid",
    "geocode_match": "Oxford, Oxfordshire, England, United Kingdom",
    "source": "UK Home Office, Register of licensed sponsors: students",
    "updated": "2026-10-30"
  }
]
```

One name in several towns. This call returns 6 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_student_visa_sponsors" --data-urlencode "sponsor_name=The King's School" | jq -r '.[] | [.sponsor_name, .town_city, .route, .status] | @tsv'
```

```text
The King's School	Canterbury	Child Student	Student Sponsor
The King's School	Canterbury	Student	Student Sponsor
The King's School	Chester	Child Student	Student Sponsor
The King's School	Ely	Child Student	Student Sponsor
The King's School	Ely	Student	Student Sponsor
The King's School	Harpenden	Child Student	Student Sponsor
```

Sponsors on an action plan. This call returns 16 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_student_visa_sponsors" --data-urlencode "sponsor_name=*" --data-urlencode "immigration_compliance=Subject To Action Plan" | jq -r '.[] | [.sponsor_name, .town_city, .route] | @tsv'
```

A box on the map around Edinburgh and Glasgow. This call returns 41 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_student_visa_sponsors" --data-urlencode "sponsor_name=*" --data-urlencode "latitude=55..56" --data-urlencode "longitude=-4.5..-3" | jq length
```

## Data source

The sponsor list comes from the UK Home Office [Register of licensed sponsors: students](https://www.gov.uk/government/publications/register-of-licensed-sponsors-students), file `SP_-_Student_and_Child_Student_Web_Register_-_2026-10-30.csv`, published on 2026-10-30 and fetched on 2026-11-02. It is published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited. Changes: columns renamed, extra spaces trimmed, and town map points added from OpenStreetMap Nominatim. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: UK Home Office, Register of licensed sponsors: students (https://www.gov.uk/government/publications/register-of-licensed-sponsors-students).
* Map points: © OpenStreetMap contributors, through Nominatim, available under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not endorsed by the UK Home Office. No Home Office logos are used.

[Back to the API list](../../README.md)
