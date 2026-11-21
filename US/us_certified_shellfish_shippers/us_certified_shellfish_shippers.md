# US Certified Shellfish Shippers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 2,554](https://img.shields.io/badge/records-2%2C554-lightgrey) ![updated 2026-11-21](https://img.shields.io/badge/updated-2026--11--21-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

U.S. Food and Drug Administration, Interstate Certified Shellfish Shippers List, November 2026. The contents of fda.gov are in the public domain and may be republished freely; credit to the FDA is appreciated but not required (see the [FDA website policies](https://www.fda.gov/about-fda/about-website/website-policies)). Source: [Interstate Certified Shellfish Shippers List](https://www.fda.gov/food/federal-state-local-tribal-and-territorial-cooperative-human-food-programs/interstate-certified-shellfish-shippers-list). Map points: U.S. Census Bureau 2024 National Gazetteer, also public domain. The data was reformatted: U.S. firms only, certificate dates converted to YYYY-MM-DD, a few wrapped PDF rows cleaned, phones and emails left out, and city or county map points added.

The 2,554 U.S. firms on the FDA Interstate Certified Shellfish Shippers List for November 2026, with firm name, city, state, certificate number, activity symbols, expiration date and a Census Gazetteer city or county point on 2,120 rows. Every call needs `address[state]`, and `address[state]=*` returns all 2,554. Foreign shippers are not included. No certificate in this extract had expired as of 21 November 2026.

This API is not affiliated with or endorsed by the U.S. Food and Drug Administration, and it is not an official source of certificate status. The list is a monthly snapshot, so check the [current Interstate Certified Shellfish Shippers List](https://www.fda.gov/food/federal-state-local-tribal-and-territorial-cooperative-human-food-programs/interstate-certified-shellfish-shippers-list) before relying on a certificate.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_certified_shellfish_shippers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per firm. All matches are returned. |

## Query parameters

Required: 1 field (`address[state]`). Optional: 9 fields. Disallowed: 1 field.

`address[state]` is required. A call without it returns HTTP 400. Use `address[state]=*` to return all 2,554 records, or a code such as `address[state]=MD`. City, state, latitude and longitude are queried with brackets: `address[state]`, `address[city]`, `location[latitude]`, `location[longitude]`. Partial filters accept `*`, such as `name=*OYSTER*`, `address[city]=*BEACH*`, `activities=*AQ*` or `expires=2026*`. `geoq` and `source_as_of` are exact matches, and a star there is read as a plain character, so it returns `[]`. All filters ignore case. `location[latitude]` and `location[longitude]` accept a range with two dots, lower number first, such as `location[latitude]=38..42`. A hyphen range such as `location[latitude]=38-42` returns HTTP 400.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `address[state]` | Required | Yes | No | U.S. state or DC code, 39 values on every row (none is blank): MD (234); WA (219); NY (217); VA (194); CA (191); MA (174); LA (139); ME (135); NC (121); NJ (116); FL (114); TX (80); PA (75); CT (61); RI (52); SC (48); IL (43); GA (40); AL (36); MS (32); NH (30); OR (29); AK (19); DE (18); NV (18); OH (17); CO (16); TN (15); MI (13); KY (10); UT (10); WI (8); NM (6); IN (5); OK (5); VT (5); HI (4); WV (4); DC (1). There is no row in PR, GU, VI or AS in this edition. Every call must include `address[state]`, and a call without it returns HTTP 400. Partial wildcards allowed, so `address[state]=*` returns all 2,554 rows, and `N*` returns NC, NH, NJ, NM, NV and NY (508). Case ignored. |
| `name` | Optional | Yes | No | Firm name as the November 2026 list prints it, such as 3 MEN AND A BOAT. Never blank. 2,355 distinct names. Partial wildcards allowed, such as `*OYSTER*`. |
| `address[city]` | Optional | Yes | No | City as the list prints it, such as CODEN, BOSTON or CAPE MAY COURT HOUSE, including typographical errors in the PDF (ORANEG BEACH, BIN SECOUR, POINT PLESANT BEACH), which are kept as published. Never blank. Partial wildcards allowed, such as `*BEACH*`. |
| `cert_number` | Optional | Yes | No | State certificate number as text, such as 05 or 2928, unique within a state (state plus number is unique on all 2,554 rows). Leading zeros are kept. Exact or partial match, such as `cert_number=1`. |
| `activities` | Optional | Yes | No | Certification activity symbols from the list, comma separated with no spaces, such as `SS`, `SP,PHP` or `SS,AQ,WS`. The symbols are SS (shellstock shipper), SP (shucker-packer), RP (repacker), RS (reshipper), DP (depuration processor), AQ (aquaculture), WS (wet storage) and PHP (post harvest processor). Partial wildcards allowed, so `activities=*AQ*` returns every row that includes aquaculture (323), and `activities=SS` is an exact value and does not include `SS,AQ`. |
| `expires` | Optional | Yes | No | Certificate expiration date as text, YYYY-MM-DD, from 2026-12-30 to 2028-01-31, never blank. Converted from the list MM/DD/YYYY. No certificate in this U.S. extract had expired as of 21 November 2026. Partial wildcards allowed, such as `2026*`. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees of the Census Gazetteer place (or county, when the list names a county), present on 2,120 rows, null on 434 where the city is not in the 2024 National Gazetteer of places or counties (many are small coastal communities or New England towns). Range filter with two dots, lower number first, such as `38..42`. A hyphen range returns 400. |
| `location[longitude]` | Optional | No | Yes | Longitude of the same point, all west of Greenwich (negative), null on the same 434 rows. Range filter with two dots, such as `-76..-70`. |
| `geoq` | Optional | No | No | How the point was matched, exact value: `city` (2,081, a 2024 Census Gazetteer place, including New York City borough names mapped to New York city, and typographical aliases such as ORANEG BEACH to Orange Beach for the match only) or `county` (39, the city field is a county or parish, such as St. Bernard Parish, Louisiana). Null on 434 rows with no point. Filtering on null is not possible. Exact match, such as `geoq=city`. |
| `source_as_of` | Optional | No | No | Date this extract was taken from the November 2026 FDA list, `2026-11-21`, the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: FDA Interstate Certified Shellfish Shippers List, November 2026, public domain, with the Census Gazetteer credit for map points. Returned in output only. |

## Response fields

`location.latitude` and `location.longitude` are numbers, or `null`. `__id` is an integer. All other values are strings. Blank cells are returned as `null`: `location` is an object with both coordinates `null`, and `geoq` is `null`, on 434 rows. City and state are nested under `address`; latitude and longitude are nested under `location`. The API returns a generated `__id` that is unique on every row and is the stable record key (it will change if the table is reloaded).

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `name` | string | Firm name as the list prints it. |
| `address` | object | `city` and `state` as the list prints them. |
| `cert_number` | string | State certificate number as text, so leading zeros remain. |
| `activities` | string | Activity symbols, comma separated with no spaces. |
| `expires` | string | Expiration date, YYYY-MM-DD. |
| `location` | object | `latitude` and `longitude` of the Census Gazetteer point, or both `null` when there is no point. |
| `geoq` | string | `city`, `county`, or `null` when there is no point. |
| `source` | string | Credit line for the FDA list and the Census Gazetteer credit for map points. |
| `source_as_of` | string | Date this extract was taken from the November 2026 list, `2026-11-21`. |

## Data notes

* The records are the U.S. firms on the November 2026 Interstate Certified Shellfish Shippers List PDF (https://www.fda.gov/media/196813/download). 133 foreign rows (Canada, Mexico, Korea, the Netherlands, New Zealand, Spain and others) were left out. No U.S. territory (PR, GU, VI, AS) appears in this edition. Officials and contact pages at the end of the PDF, which include personal names, phones and emails, were not imported.
* No U.S. certificate in this extract had expired as of 21 November 2026 (the earliest U.S. date is 2026-12-30). Some Canadian rows expire on 30 November 2026, and those were already dropped as foreign.
* A few U.S. rows that the PDF text layer wrapped or merged were cleaned so each firm has a name, city and numeric certificate number (PESCADOR city, two Cape May Court House wraps, five Pennsylvania certificate fields printed as PA2928RS, and a truncated BREMERTON ENVIRONMENTAL LABORATORIES name). City typographical errors such as ORANEG BEACH are kept as published.
* Activity symbols stay in one field so a partial filter such as `*AQ*` or `*PHP*` works. An exact value such as `activities=SS` does not match `SS,AQ`.
* `lat` and `lng` come from the U.S. Census Bureau 2024 National Gazetteer of places, plus counties when the city field is a county or parish, public domain. 2,081 rows match a place and 39 match a county. 434 have no safe match, including New England towns that are not in the place gazetteer and small communities such as Coden, Alabama. New York City borough names were mapped to New York city for the match only. City typographical errors were used only for the match. The point is the city or county, not a street address.
* The data is a snapshot of the November 2026 edition, pulled on 21 November 2026, the `source_as_of` value.

## Examples

3 MEN AND A BOAT in Coden, Alabama, which has no Gazetteer point. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_certified_shellfish_shippers' --data-urlencode 'address[state]=AL' --data-urlencode 'name=3 MEN AND A BOAT'
```

```json
[
  {
    "__id": 2574,
    "name": "3 MEN AND A BOAT",
    "address": {
      "city": "CODEN",
      "state": "AL"
    },
    "cert_number": "29",
    "activities": "SP",
    "expires": "2027-09-30",
    "location": {
      "latitude": null,
      "longitude": null
    },
    "geoq": null,
    "source": "U.S. Food and Drug Administration, Interstate Certified Shellfish Shippers List, November 2026, https://www.fda.gov/food/federal-state-local-tribal-and-territorial-cooperative-human-food-programs/interstate-certified-shellfish-shippers-list. Public domain (U.S. government work). Map points: U.S. Census Bureau 2024 National Gazetteer, public domain.",
    "source_as_of": "2026-11-21"
  }
]
```

The one certified shipper in the District of Columbia, with a city point. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_certified_shellfish_shippers' --data-urlencode 'address[state]=DC'
```

```json
[
  {
    "__id": 2878,
    "name": "OCEANPRO PROFISH",
    "address": {
      "city": "WASHINGTON",
      "state": "DC"
    },
    "cert_number": "52",
    "activities": "SS",
    "expires": "2027-07-31",
    "location": {
      "latitude": 38.904247,
      "longitude": -77.016517
    },
    "geoq": "city",
    "source": "U.S. Food and Drug Administration, Interstate Certified Shellfish Shippers List, November 2026, https://www.fda.gov/food/federal-state-local-tribal-and-territorial-cooperative-human-food-programs/interstate-certified-shellfish-shippers-list. Public domain (U.S. government work). Map points: U.S. Census Bureau 2024 National Gazetteer, public domain.",
    "source_as_of": "2026-11-21"
  }
]
```

A firm whose certificate includes depuration, aquaculture and wet storage, looked up by the exact activity string. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_certified_shellfish_shippers' --data-urlencode 'address[state]=*' --data-urlencode 'activities=DP,AQ,WS'
```

```json
[
  {
    "__id": 3774,
    "name": "SPINNEY CREEK",
    "address": {
      "city": "ELIOT",
      "state": "ME"
    },
    "cert_number": "271",
    "activities": "DP,AQ,WS",
    "expires": "2027-03-31",
    "location": {
      "latitude": null,
      "longitude": null
    },
    "geoq": null,
    "source": "U.S. Food and Drug Administration, Interstate Certified Shellfish Shippers List, November 2026, https://www.fda.gov/food/federal-state-local-tribal-and-territorial-cooperative-human-food-programs/interstate-certified-shellfish-shippers-list. Public domain (U.S. government work). Map points: U.S. Census Bureau 2024 National Gazetteer, public domain.",
    "source_as_of": "2026-11-21"
  }
]
```

## Data source

U.S. Food and Drug Administration, Interstate Certified Shellfish Shippers List, November 2026. Source: [Interstate Certified Shellfish Shippers List](https://www.fda.gov/food/federal-state-local-tribal-and-territorial-cooperative-human-food-programs/interstate-certified-shellfish-shippers-list), November 2026 PDF at https://www.fda.gov/media/196813/download. FDA website policies say the contents of fda.gov are not copyrighted and may be republished freely, and that credit to the U.S. Food and Drug Administration as the source is appreciated but not required. Map points: U.S. Census Bureau 2024 National Gazetteer of places and counties, public domain. This data was changed from the source as described in the notes above. The FDA logo is not used, and the FDA does not endorse this API.
