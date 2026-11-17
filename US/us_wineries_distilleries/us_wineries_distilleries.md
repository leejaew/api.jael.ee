# US Wineries and Distilleries

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 23555](https://img.shields.io/badge/records-23555-lightgrey) ![updated 2026-11-10](https://img.shields.io/badge/updated-2026--11--10-orange) ![license US Gov work](https://img.shields.io/badge/license-US%20Gov%20work-green)

Source: Alcohol and Tobacco Tax and Trade Bureau (TTB), FOIA List of Permittees, files dated 10 November 2026, https://www.ttb.gov/public-information/foia/list-of-permittees. US government work, not copyrighted (17 U.S.C. 105). Map points added with the US Census Bureau Geocoder.

23,555 federal permits for wineries and distilleries in the 50 states and DC: 18,002 Wine Producer permits and 5,553 Distilled Spirits Plant permits. Each record has the permit number, the business name, the premises address, the county and a map point on 88% of rows. The largest states are California (6,886), Washington (1,654), Texas (1,345), New York (1,205) and Oregon (1,154). `state` is required, so you can filter further by permit type, city, county, ZIP, name or a latitude and longitude box.

This is not an official TTB or Treasury service. A permit is not a license to sell, and many of these premises are not open to visitors. The permit number is the record key and is unique on every row, so the response has no `__id`.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_wineries_distilleries` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per permit. All matches are returned. |

## Query parameters

Required: 1 field (`state`). Optional: 12 fields. Disallowed: 2 fields.

A call without `state` returns HTTP 400. The biggest answer, `state=CA`, returns 6,886 records (about 3.9 MB). Partial filters accept `*`, such as `operating_name=*Distilling*`, `county=*sonoma*` or `zip=972*`. `state`, `permit_type`, `new_permit` and `geo_quality` are exact matches, and a star there is read as a plain character, so it returns `[]`. `latitude` and `longitude` accept a range with two dots, such as `latitude=40.5..41&longitude=-74.5..-73.5`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `state` | Required | No | No | Required. Two-letter postal code of the permit premises: 50 states and DC (51 values, no territories). The largest are CA (6,886 rows), WA (1,654), TX (1,345), NY (1,205) and OR (1,154); the smallest are DE (22), MS (24) and DC (25). Exact match only, and case does not matter, so `state=ny` works. A call without state returns HTTP 400. The value is the premises state from the address, not always the state in the permit number. |
| `permit_number` | Optional | Yes | No | TTB permit number, the official key for one permit, such as AK-W-15001 (a Wine Producer permit) or AK-S-15000 (a Distilled Spirits Plant permit). Never blank and unique on all 23,555 rows. The two letters at the start are normally the state that issued the permit and match the premises state, but 2 rows differ because the permit holder gave a premises address in another state. The middle part is W for wine (17,987 rows), S for spirits (5,553) or WS for the 15 combined permits. Partial wildcards allowed, such as `CA-W-*` or `*-S-2*`. |
| `permit_type` | Optional | No | No | Kind of federal permit. Exactly two values: Wine Producer (18,002 rows, from the TTB Wine Producer and Blender list) or Distilled Spirits Plant (5,553 rows, from the TTB Spirits Producers and Bottlers list). Never blank. Exact match only; use `permit_type=Wine Producer` or `permit_type=Distilled Spirits Plant`. A business that holds both permits appears as two rows. |
| `owner` | Optional | Yes | No | Legal name of the business that holds the permit, as TTB lists it in capitals, such as E. & J. GALLO WINERY (25 rows) or JACKSON FAMILY WINES, INC. (28 rows). Blank (`null`) on 1,590 rows where TTB lists a private person, a couple or a family trust, and we left the name out to protect privacy; those rows still show the business name in operating_name. 37 rows where a limited partnership listed its general partners were cut down to the business name. Present on 21,965 rows. Partial wildcards allowed, such as `*GALLO*` or `*LLC`. |
| `operating_name` | Optional | Yes | No | Trade name the business operates under (the doing business as name), such as RING OF FIRE MEADERY, Ole Smoky Distillery or Roy Piper Wines, in the case TTB gave it. Blank (`null`) on 7,418 rows where TTB gave no separate name; in that case the owner is the best name to use. We also cleared 27 rows where the name only repeated a private person's name. Present on 16,137 rows. Partial wildcards allowed, such as `*Distilling*` or `*Cellars`. |
| `street` | Optional | Yes | No | Street address of the permit premises as TTB lists it in capitals, such as 178 E BUNNELL AVE. Blank (`null`) on 56 rows where TTB gave none. Some entries are rural routes, suites or building letters, and a few premises share one address (1160 HOPPER AVE appears 54 times). Partial wildcards allowed, such as `*MAIN ST*` or 1160 HOPPER*. |
| `city` | Optional | Yes | No | City of the permit premises as TTB lists it, in capitals, such as NAPA (818 rows), PASO ROBLES (473) or HEALDSBURG (318). 5,360 distinct names. Never blank. Names repeat across states, so combine it with the required state filter. Case does not matter in filters. Partial wildcards allowed, such as `SAN*` or `*SPRINGS`. |
| `zip` | Optional | Yes | No | Premises ZIP code as text, such as 99603 or 94558 (722 rows). Never blank. 8,120 distinct values. Leading zeros are kept (Massachusetts has 01890), which is why this is text and not a number. 3 rows carry a nine digit ZIP in the form `12345-6789`. Partial wildcards allowed, such as `972*` or `945*`. |
| `county` | Optional | Yes | No | County of the premises as the permit holder typed it, in capitals, such as NAPA (1,365 rows) or SONOMA (1,120). TTB warns that the permit holder enters this by hand, so it can differ from the real county, can be misspelled (KENAI PENINSULA BUROUGH) or can be blank. Blank (`null`) on 4,114 rows. We kept it as published. Partial wildcards allowed, such as `*SONOMA*` or `SANTA*`. |
| `new_permit` | Optional | No | No | Whether TTB marked the permit as newly issued in its latest weekly list (under 7 days old at that time): yes (28 rows) or no (23,527 rows). Never blank. Exact match only. This is a snapshot flag from 10 November 2026 and does not update by itself. |
| `latitude` | Optional | No | Yes | Latitude of the premises in decimal degrees, WGS 84, with 6 decimals, from the US Census Geocoder on the street, city, state and ZIP columns, such as 59.639792. Blank (`null`) on 2,756 rows (11.7%): 56 rows without a street, 2,687 where the geocoder found no match or a tie, and 13 where we dropped a match that fell in another state or far outside its ZIP. Filled on 20,799 rows (88.3%): 88.0% of wine rows and 89.1% of spirits rows. A number. Range filter with two dots, min..max, such as `38.2..38.6`. Hyphens return 400. |
| `longitude` | Optional | No | Yes | Longitude of the premises in decimal degrees, WGS 84, with 6 decimals, such as -151.542032. All values are negative (the United States is west of Greenwich). Blank (`null`) on the same 2,756 rows as latitude. A number. Range filter with two dots, min..max, such as `-122.6..-122.2` (put latitude in the same call to make a box). Hyphens return 400. |
| `geo_quality` | Optional | No | No | How the map point was matched: exact (15,812 rows), where the Census Geocoder matched the street address exactly, or approximate (4,987 rows), where it matched a similar address on the same street (for example a different suffix or suite) and the point can be off by a block or more. Blank (`null`) on the 2,756 rows with no point. Exact match only, such as `geo_quality=exact`. |
| `source` | Disallowed | No | No | Source credit text: Alcohol and Tobacco Tax and Trade Bureau, FOIA List of Permittees (Wine Producer and Blender, Spirits Producers and Bottlers). The same on every row. Text. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the TTB files, YYYY-MM-DD (2026-11-10). It is the Last-Modified date of the CSV and JSON files; the TTB page itself says data as of November 16, 2026, and TTB posts updates weekly. Text, not a date value. Not filterable. |

## Response fields

`latitude` and `longitude` are numbers. All other values are strings. Blank cells are returned as `null`.

| Field | Type | Description |
|---|---|---|
| `permit_number` | string | TTB permit number, the official key. Unique on every row. |
| `permit_type` | string | `Wine Producer` or `Distilled Spirits Plant`. |
| `owner` | string | Business that holds the permit, or `null` when TTB lists a private person, couple or trust, or when it gave none. |
| `operating_name` | string | Trade name, or `null` when TTB gave none. |
| `street` | string | Premises street address, or `null` on 56 rows. |
| `city` | string | Premises city. |
| `state` | string | Two-letter premises state code, 50 states and DC. |
| `zip` | string | Premises ZIP code, as text. |
| `county` | string | County as the permit holder typed it, or `null` when blank. |
| `new_permit` | string | `yes` or `no`. |
| `latitude` | number | Latitude in decimal degrees, or `null` when there is no map point. |
| `longitude` | number | Longitude in decimal degrees, or `null` when there is no map point. |
| `geo_quality` | string | `exact` or `approximate`, or `null` when there is no map point. |
| `source` | string | Alcohol and Tobacco Tax and Trade Bureau, FOIA List of Permittees (Wine Producer and Blender, Spirits Producers and Bottlers). |
| `source_as_of` | string | Date of the TTB files, `2026-11-10`. |

## Examples

Wine and spirits businesses with Cellars in the name in Healdsburg, California. This call returns 22 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_wineries_distilleries' --data-urlencode 'state=CA' --data-urlencode 'city=Healdsburg' --data-urlencode 'operating_name=*Cellars*'
```

```json
{
  "permit_number": "CA-W-15802",
  "permit_type": "Wine Producer",
  "owner": "DIAMOND RIDGE ESTATE WINERY AND VINEYARDS, INC.",
  "operating_name": "MALM CELLARS",
  "street": "119 W NORTH ST",
  "city": "HEALDSBURG",
  "state": "CA",
  "zip": "95448",
  "county": "SONOMA",
  "new_permit": "no",
  "latitude": 38.611107,
  "longitude": -122.87481,
  "geo_quality": "exact",
  "source": "Alcohol and Tobacco Tax and Trade Bureau, FOIA List of Permittees (Wine Producer and Blender, Spirits Producers and Bottlers)",
  "source_as_of": "2026-11-10"
}
```

Distilled spirits plants in Kentucky. This call returns 207 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_wineries_distilleries' --data-urlencode 'state=KY' --data-urlencode 'permit_type=Distilled Spirits Plant'
```

Permits in Sonoma County, California (any spelling that contains sonoma). This call returns 1,144 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_wineries_distilleries' --data-urlencode 'state=CA' --data-urlencode 'county=*sonoma*'
```

Permits in Oregon ZIP codes that start with 972. This call returns 111 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_wineries_distilleries' --data-urlencode 'state=OR' --data-urlencode 'zip=972*'
```

Permits in a map box around New York City. This call returns 86 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_wineries_distilleries' --data-urlencode 'state=NY' --data-urlencode 'latitude=40.5..41' --data-urlencode 'longitude=-74.5..-73.5'
```

Texas businesses with Distilling in the operating name. This call returns 32 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_wineries_distilleries' --data-urlencode 'state=TX' --data-urlencode 'operating_name=*Distilling*'
```

## Data source

Source: Alcohol and Tobacco Tax and Trade Bureau (TTB), FOIA List of Permittees, files dated 10 November 2026, https://www.ttb.gov/public-information/foia/list-of-permittees. US government work, not copyrighted (17 U.S.C. 105). Map points added with the US Census Bureau Geocoder.

The records come from two CSV files on the [TTB List of Permittees page](https://www.ttb.gov/public-information/foia/list-of-permittees): the Wine Producer and Blender Permit List (18,130 rows) and the Spirits Producers and Bottlers List (5,581 rows). Both files are dated 10 November 2026, which is `source_as_of`. The page itself says the data is as of 16 November 2026, and TTB posts updates weekly. This API is a snapshot that is refreshed by hand. For the latest list, visit the [TTB page](https://www.ttb.gov/public-information/foia/list-of-permittees). The list covers only Wine Producer and Distilled Spirits Plant permits. It does not cover brewers, importers, wholesalers or tobacco businesses, and it leaves out Puerto Rico and other territories.

The list is a work of the US government, and [17 U.S.C. 105](https://www.law.cornell.edu/uscode/text/17/105) says copyright protection is not available for such a work. The TTB [website policy](https://www.ttb.gov/about-ttb/privacy-policy) says you may copy its text and images freely, apart from official seals, names and symbols, but credit must be given to TTB. You can use this data for any purpose, including commercial use, if you credit TTB. No TTB or Treasury seal, name or logo is used here. The data comes with no warranty, and TTB does not endorse this API or its maker.

Owners: 156 permits where the owner is a private person, a couple or a family trust and TTB gave no business name were left out. On 1,590 more rows the owner name is blank (`null`) for the same reason, and the business name is in `operating_name`. A name is kept only when it carries a business word or a legal form such as LLC, INC, WINERY or DISTILLERY. This test uses keywords, so a few real businesses may have a blank owner. A business name can still contain a family name. TTB publishes the premises address of each licensed business, and some premises may be a home.

County: TTB says the county is typed by the permit holder, so it may not match the real county of the premises. It can be misspelled or blank (`null` on 4,114 rows). It is kept as published.

Map points: the US Census Bureau Geocoder placed each street address (run on 18 November 2026). 20,799 rows (88.3%) have a point: 15,812 are `exact` and 4,987 are `approximate`, which means the geocoder matched a similar address on the same street and the point can be a block or more off. 2,756 rows have no point (`null`): 2,471 had no match, 216 were ties, 56 have no street, and 13 matches were dropped because they fell in another state or far from the row's ZIP code. No point was filled in by hand or from another source.

[Back to the API list](../../README.md)
