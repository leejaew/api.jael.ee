# UK SELT Test Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1361](https://img.shields.io/badge/records-1361-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license OGL v3 + ODbL](https://img.shields.io/badge/license-OGL%20v3%20%2B%20ODbL-green)

Every test centre approved by the UK Home Office for Secure English Language Tests (SELT), the English tests used for UK visa and immigration applications: 1,361 centres, 84 in the UK and 1,277 in 141 other countries and territories, from the two GOV.UK lists dated 30 October 2026. The centres are run by the 5 approved providers. Each row has the provider, centre name, country, city, address, availability, an in or outside the UK flag and a map point. Filter by the required `country`, then by city, centre name, provider, in or outside the UK or a coordinate range.

Rows by provider: IELTS SELT Consortium 547, PSI 330, LanguageCert 237, Pearson 227 and Trinity College London 20 (UK only). Most centres: China (147), India (145), United Kingdom (84), Pakistan (66) and the Philippines (37). Most centres in one city: Beijing and Bangkok (17 each), then London, Dhaka and Lahore (15 each).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_selt_test_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`country`). Optional: 8 fields. Disallowed: 3 fields.

`country` must be in every call. Use `country=*` to list all 1,361 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `country` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `country` | Required | Yes | No | Country or territory as the Home Office lists it, such as `United Kingdom` (84), `China` (147), `India` (145), `United*` (107) or `*stan` (91). Wildcards work, so `country=*` lists all 1,361 rows. See the notes for names that follow the source. |
| `provider` | Optional | No | No | Exact name of the approved test provider: `IELTS SELT Consortium` (547, IELTS run by the British Council and IDP), `PSI` (330, Skills for English), `LanguageCert` (237), `Pearson` (227, PTE Academic UKVI and PTE Home) or `Trinity College London` (20, UK only). |
| `centre_name` | Optional | Yes | No | Centre name as the provider lists it, such as `*IDP*` (125) or `BC *` (252, British Council centres). 25 names appear more than once, each time at a different venue. |
| `city` | Optional | Yes | No | City or town, such as `London` (15 in the United Kingdom), `New Delhi` (12) or `Kuala*`. Some rows give a region instead of a city, such as `Goa`, `Bali`, `Sabah` or `Pampanga`. |
| `address` | Optional | No | No | Address of the test venue as the provider lists it. UK addresses end with the postcode. |
| `availability` | Optional | No | No | Availability as the Home Office lists it. `Available` on every row. |
| `in_uk` | Optional | No | No | Exact: `Yes` (84, on the list of test centres in the UK) or `No` (1,277, on the list of test centres outside the UK). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from -37.818196 to 64.11135. Example: `latitude=51.3..51.7&longitude=-0.5..0.3` for the London area (20). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -123.113952 to 178.453203. Use it with `latitude` to search a box. |
| `geocode_quality` | Disallowed | No | No | How exact the map point is. See the notes. |
| `source` | Disallowed | No | No | Link to the GOV.UK file the row comes from: the list of centres in the UK or the list outside the UK. |
| `updated` | Disallowed | No | No | Date of the source lists, `2026-10-30` on every row. Text. |

* Parameters are combined with AND. Text matches ignore case, so `in_uk=yes` works the same as `in_uk=Yes`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `country` returns HTTP 400 with `Your request is missing the following required parameters: country. You did not supply any.`
* A filter on `geocode_quality`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source`.
* Wildcards work only on `country`, `centre_name` and `city`. A wildcard on another text field returns `[]`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `provider` | string | Approved test provider. |
| `centre_name` | string | Centre name as the provider lists it. |
| `country` | string | Country or territory. |
| `city` | string | City or town, sometimes a region. |
| `address` | string | Address of the test venue. |
| `availability` | string | Availability, `Available`. |
| `in_uk` | string | `Yes` or `No`. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | How exact the map point is. |
| `source` | string | Link to the GOV.UK source file. |
| `updated` | string | Date of the source lists, `YYYY-MM-DD`. |

Notes:

* Countries: names follow the source, for example `USA`, `Ivory Coast`, `Macau` and `Occupied Palestinian Territories`. `Cyprus`, `Cyprus (North)` and `Cyprus (South)` are three values, so use `Cyprus*` to find all 10. Guernsey, Jersey and the Isle of Man are on the list of centres outside the UK, so their `in_uk` is `No`.
* Names: centre names and addresses are kept as the providers wrote them, typing errors included, such as `IDP Kuala Lumour`.
* Map points: every row has a point, but points are approximate, so check the address before you rely on one. `geocode_quality` shows how exact it is, best first: `uk_postcode` (82, UK postcode centre from postcodes.io), `address` (392, the building or place found in OpenStreetMap), `street` (174, the street found), `area` (308, the district or neighbourhood found) and `city_centroid` (405, the middle of the city, used when the address could not be found).
* The Home Office updates these lists from time to time. Check with the test provider before you book.

## Examples

A wildcard on the city in one country. Test centres in Kuala Lumpur, Malaysia. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_selt_test_centres" --data-urlencode "country=Malaysia" --data-urlencode "city=Kuala*"
```

Response excerpt (first of 7 rows):

```json
[
  {
    "__id": 809,
    "provider": "IELTS SELT Consortium",
    "centre_name": "BC Kuala Lumpur - Classroom",
    "country": "Malaysia",
    "city": "Kuala Lumpur",
    "address": "Classroom 1-20, West Block, Wisma Golden Eagle Realty, Jalan Ampang, 50450 Kuala Lumpur, Malaysia",
    "availability": "Available",
    "in_uk": "No",
    "latitude": 3.159272,
    "longitude": 101.708971,
    "geocode_quality": "address",
    "source": "https://assets.publishing.service.gov.uk/media/6ae859c6b87d58a0053ada49/Copy_of_List_of_approved_test_centres_outside_the_UK_from_October_2026_30.10.2026.ods",
    "updated": "2026-10-30"
  },
  ...
]
```

The same call as a short list:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_selt_test_centres" --data-urlencode "country=Malaysia" --data-urlencode "city=Kuala*" | jq -r '.[] | [.provider, .centre_name] | @tsv'
```

```text
IELTS SELT Consortium	BC Kuala Lumpur - Classroom
IELTS SELT Consortium	BC Kuala Lumpur - Exam Hall
IELTS SELT Consortium	IDP Kuala Lumour
LanguageCert	LC - Kuala Lumpur
PSI	Kuala Lumpur - Arcnet English Language Training
Pearson	AUG Student Services
Pearson	Pearson Professional Centers-Kuala Lumpur
```

A wildcard on the country. All three Cyprus values. This call returns 10 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_selt_test_centres" --data-urlencode "country=Cyprus*" | jq -r '.[] | [.country, .city, .provider] | @tsv'
```

One provider in one country. IELTS centres in China. This call returns 86 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_selt_test_centres" --data-urlencode "country=China" --data-urlencode "provider=IELTS SELT Consortium" | jq length
```

Range on latitude and longitude with the required wildcard. Centres in the London area. This call returns 20 rows, and the first three are in Croydon:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_selt_test_centres" --data-urlencode "country=*" --data-urlencode "latitude=51.3..51.7" --data-urlencode "longitude=-0.5..0.3" | jq -r '.[] | [.provider, .centre_name, .city] | @tsv'
```

```text
LanguageCert	LC - London South (Croydon)	Croydon
Pearson	Pearson Professional Centers-Croydon, UK	Croydon
Trinity College London	London South - Croydon	Croydon
...
```

## Data source

The centre lists come from the UK Home Office page [Prove your English language abilities with a secure English language test (SELT)](https://www.gov.uk/guidance/prove-your-english-language-abilities-with-a-secure-english-language-test-selt), last updated on 2 November 2026 and fetched on 4 November 2026: the [list of approved test centres in the UK](https://assets.publishing.service.gov.uk/media/6ae859afb87d58a0053ada48/Copy_of_List_of_approved_test_centres_in_the_UK_from_October_2026_30.10.2026.ods) and the [list of approved test centres outside the UK](https://assets.publishing.service.gov.uk/media/6ae859c6b87d58a0053ada49/Copy_of_List_of_approved_test_centres_outside_the_UK_from_October_2026_30.10.2026.ods), both dated 30 October 2026. They are published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited.

Changes: the two lists were merged with an `in_uk` column, columns renamed, extra spaces trimmed, the country names GHANA, Sri lanka and morocco capitalised, 3 rows corrected where the country or city held the wrong value (Guayaquil listed as a country, a Jakarta address listed under Malaysia and Turkey listed as a city), 5 misspelled city names corrected (Doula to Douala, Abidjian to Abidjan, Zebugg to Haz-Zebbug, Gurjat to Gujrat and Hammemet to Hammamet) and map points added. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: UK Home Office, Prove your English language abilities with a secure English language test (SELT), lists of approved test centres in and outside the UK dated 30 October 2026 (https://www.gov.uk/guidance/prove-your-english-language-abilities-with-a-secure-english-language-test-selt).
* UK map points: postcodes.io (https://postcodes.io), using Office for National Statistics and Ordnance Survey data licensed under the Open Government Licence v3.0. Contains OS data © Crown copyright and database right 2026.
* Other map points: © OpenStreetMap contributors, through Nominatim and Photon, available under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not endorsed by the Home Office. No Home Office logos are used.

[Back to the API list](../../README.md)
