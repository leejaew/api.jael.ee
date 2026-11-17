# Australia AMEP Class Locations

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 303](https://img.shields.io/badge/records-303-lightgrey) ![updated 2026-11-10](https://img.shields.io/badge/updated-2026--11--10-orange) ![license CC BY 3.0 AU](https://img.shields.io/badge/license-CC%20BY%203.0%20AU-green)

Source: Australian Government Department of Home Affairs, Adult Migrant English Program (AMEP) providers and locations, page last updated 10 November 2026, https://immi.homeaffairs.gov.au/settling-in-australia/amep/find-a-class/providers-and-locations. Creative Commons Attribution 3.0 Australia. Map points: Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement.

303 Adult Migrant English Program class sites across Australia from the Department of Home Affairs find-a-class list. There are 13 providers in all 8 states and territories: Victoria (108), New South Wales (87), Queensland (52), Western Australia (23), South Australia (19), the ACT (7), Tasmania (4) and the Northern Territory (3). Each record is one venue, with suburb, postcode, provider, venue name, street address, phone, the provider's AMEP web page, a general email for some providers, and a map point on 267 rows. No field is required, so you can filter by state, provider, postcode, or a latitude and longitude box.

This is not an official Department of Home Affairs service. Class sites, phone numbers and rules for new clients change, so call the provider before you go. 32 rows have `callfirst` set to `yes` because the list tells new clients to call or contact another office before visiting in person. The list has no official site ID. The API returns a generated `__id` that is unique on every row and is the stable record key.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_amep_class_locations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per class site. All matches are returned. |

## Query parameters

Required: none. Optional: 15 fields. Disallowed: 1 field.

A call with no filters returns all 303 records. Partial filters accept `*`, such as `provider=*TAFE*`, `address=*Station Road*` or `postcode=30*`. `state`, `geoq` and `callfirst` are exact matches, and a star there is read as a plain character, so it returns `[]`. `lat` and `lng` accept a range with two dots, written with the lower number first, such as `lat=-38..-37&lng=144..146`. A leading minus works. A hyphen range such as `lat=-34-33` returns HTTP 400. Text filters ignore case. `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `state` | Optional | No | No | Australian state or territory code, 8 values: VIC (108 rows), NSW (87), QLD (52), WA (23), SA (19), ACT (7), TAS (4) and NT (3). Never blank. Exact match, such as `state=NSW`, and case does not matter. Leave it out to search the whole country, for example with a latitude and longitude box. |
| `suburb` | Optional | Yes | No | Suburb or town as the Home Affairs list gives it, such as Liverpool, Craigieburn or Alice Springs. 243 values. Never blank. Several sites can share a suburb (Melton, Richmond and Werribee have 6 each). A few sites are listed under a nearby suburb, so it can differ from the locality at the end of `address`. Partial wildcards allowed, such as `*Hill*` or `Mount*`. |
| `postcode` | Optional | Yes | No | Four digit Australian postcode as text, such as 2170 or 3064. Never blank. 233 values. Text, so use a wildcard for a block of postcodes: `30*` returns the Victorian postcodes that start with 30, such as 3000 (50 rows). Leading zeros are kept (the 3 Northern Territory postcodes start with 0). Partial wildcards allowed. |
| `provider` | Optional | Yes | No | AMEP provider that runs the class, 13 names: TAFE NSW (66), Melbourne Polytechnic (61), TAFE Queensland (52), Learning for Employment (37), Navitas Skilled Futures (31), TAFE SA (19), and 7 smaller ones. Regional TAFE Queensland labels and Learning for Employment (LfE) were merged into one name each. Never blank. Walkamin, Queensland names no provider in the source, so it was set to TAFE Queensland, the only provider in that state, and `note` says so. Partial wildcards allowed, such as `*TAFE*` or `Navitas*`. |
| `venue` | Optional | Yes | No | Host organization or building lines above the street address, such as Coffs Harbour Education Campus (CHEC) or Kangan Institute; Craigieburn Sports Stadium, joined with semicolons. Present on 189 rows, blank (`null`) on 114 where the list gives only an address. The Melbourne AMEP brand line was dropped. Partial wildcards allowed. |
| `address` | Optional | Yes | No | Full one line address as listed, with suburb, state and postcode at the end, such as Ground Floor, 54 Benjamin Way, Belconnen ACT 2617. Never blank. Some are corners or place descriptions, such as Corner Poictiers & Macauley Streets. Partial wildcards allowed, such as `*Station Road*`. |
| `phone` | Optional | Yes | No | Site or provider enquiry number as text, such as 02 8234 1490 or 1300 308 233. Present on 300 rows. Several numbers in one cell are joined with a semicolon. Seven mobile numbers (three 04 numbers and four SMS lines) were left out, which leaves 3 rows blank (`null`). Partial wildcards allowed, such as `03 9*`. |
| `website` | Optional | Yes | No | The provider's AMEP page, as linked under its heading on the Home Affairs list, such as https://www.melbourneamep.com.au/. One link per provider, so rows from the same provider share it, except that Navitas Skilled Futures has two links (its 3 Tasmanian rows use a different page). Present on all 303 rows. Some links use http. Links can go out of date. Partial wildcards allowed, such as `*tafe*`. |
| `email` | Optional | Yes | No | General enquiry email of the provider as linked on the list, in lower case, such as amep@tafensw.edu.au. The list gives 5 addresses (TAFE NSW, TAFE Queensland, Navitas Skilled Futures, STEPS Group and TasTAFE), which are on 153 rows. Blank (`null`) on the other 150, where the provider publishes only a phone or website. No personal addresses. Partial wildcards allowed, such as `*@tafe*`. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (GDA2020, 6 decimals) of the venue, matched against the Geoscape G-NAF address file, not taken from the source page. Present on 267 rows, blank (`null`) on 36 where no safe match was found. All values are negative because Australia is south of the equator, from -42.88 to -12.37. A number. Range filter with two dots, lower number first, such as `-34..-33`. See `geoq` for how close the point is. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same point. Present on the same 267 rows as `lat`. All values are positive, from 114.62 to 153.58. A number. Range filter with two dots, such as `150..152`. Put `lat` in the same call to make a box. |
| `geoq` | Optional | No | No | How the map point was matched. `address` (220 rows): a G-NAF address with that street number, or the middle of a numbered range. `near` (15): the nearest numbered address, at most 6 numbers away, or nearby numbers within 0.4 km. `street` (14): the middle of a street whose addresses lie within 0.4 km. `corner` (18): the midpoint of two named streets at most 150 m apart. Blank (`null`) on the 36 rows with no point. Exact match only, such as `geoq=address`. |
| `callfirst` | Optional | No | No | `yes` (32 rows) when the list tells new clients to call or contact another office before visiting in person: 31 Victorian sites and Midland in Western Australia. Otherwise `no` (271). Never blank. Exact match only. The instruction itself is in `note`. |
| `note` | Optional | Yes | No | Short text carried from the list or added when the data was built: the call first instruction (for example New clients please call 03 8746 1000 or visit 239 Station Road Melton before attending this site in person), the Balga line All enquiries to be directed to: Mirrabooka Office 08 9207 4268, the Midland line about contacting the Balga office, the Petersham registration line, Also known as Mooloolaba campus, Listed in the source as AMES Australia/ TAFE SA, and the Walkamin line about the provider. Present on 37 rows, blank (`null`) on the rest. Partial wildcards allowed, such as `*call*`. |
| `source_as_of` | Optional | Yes | No | Date the Home Affairs page says it was last updated, 2026-11-10, as text in YYYY-MM-DD form. It is the same on every row (the page was fetched on 2026-11-18). Partial wildcards allowed, such as `2026-11*`. |
| `source` | Disallowed | No | No | Credit text, the same on every row: Australian Government Department of Home Affairs, AMEP providers and locations, with the page link, Creative Commons Attribution 3.0 Australia, and the G-NAF credit line for the map points. Text. Not filterable. |

## Response fields

`lat` and `lng` are numbers. `__id` is an integer. All other values are strings. Blank cells are returned as `null`. Every row has a unique `__id`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `state` | string | State or territory code: VIC, NSW, QLD, WA, SA, ACT, TAS or NT. |
| `suburb` | string | Suburb or town as listed. |
| `postcode` | string | Four digit postcode, as text. |
| `provider` | string | AMEP provider name. |
| `venue` | string | Host organization or building lines, or `null` when none. |
| `address` | string | One line street address with suburb, state and postcode. |
| `phone` | string | Enquiry phone number or numbers, or `null` on 3 rows. |
| `website` | string | The provider's AMEP page. |
| `email` | string | General provider email, or `null` on 150 rows. |
| `lat` | number | Latitude in decimal degrees, or `null` when there is no map point. |
| `lng` | number | Longitude in decimal degrees, or `null` when there is no map point. |
| `geoq` | string | `address`, `near`, `street` or `corner`, or `null` when there is no map point. |
| `callfirst` | string | `yes` or `no`. |
| `note` | string | Call first instruction or other short note, or `null`. |
| `source_as_of` | string | Date of the Home Affairs page, `2026-11-10`. |
| `source` | string | Credit text for the list and the map points. |

## Examples

Sites where new clients are asked to contact another office first, in Western Australia. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_amep_class_locations' --data-urlencode 'state=WA' --data-urlencode 'callfirst=yes'
```

```json
{
  "__id": 298,
  "state": "WA",
  "suburb": "Midland",
  "postcode": "6056",
  "provider": "North Metropolitan TAFE",
  "venue": "Midland Campus",
  "address": "Corner Lloyd Street and Eddie Barron Drive, Midland WA 6056",
  "phone": "08 9207 4268",
  "website": "http://www.northmetrotafe.wa.edu.au/courses/study-types/amep",
  "email": null,
  "lat": -31.877899,
  "lng": 116.019864,
  "geoq": "corner",
  "callfirst": "yes",
  "note": "08 9207 4268 (Contact the Balga office before visiting this campus)",
  "source": "Australian Government Department of Home Affairs, AMEP providers and locations (https://immi.homeaffairs.gov.au/settling-in-australia/amep/find-a-class/providers-and-locations), Creative Commons Attribution 3.0 Australia. Coordinates: Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement.",
  "source_as_of": "2026-11-10"
}
```

Melbourne Polytechnic sites in Victoria. This call returns 61 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_amep_class_locations' --data-urlencode 'state=VIC' --data-urlencode 'provider=Melbourne Polytechnic'
```

Sites in postcodes that start with 30 (all in Victoria). This call returns 50 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_amep_class_locations' --data-urlencode 'postcode=30*'
```

Sites in a map box that covers Melbourne and the area around it. The latitude values are negative, so the lower number comes first. This call returns 81 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_amep_class_locations' --data-urlencode 'lat=-38..-37' --data-urlencode 'lng=144..146'
```

Sites with a G-NAF street address match, as opposed to a nearby or corner point. This call returns 220 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_amep_class_locations' --data-urlencode 'geoq=address'
```

## Data source

Source: Australian Government Department of Home Affairs, Adult Migrant English Program (AMEP) providers and locations, page last updated 10 November 2026, https://immi.homeaffairs.gov.au/settling-in-australia/amep/find-a-class/providers-and-locations. Creative Commons Attribution 3.0 Australia. Map points: Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement.

The records come from the [AMEP providers and locations page](https://immi.homeaffairs.gov.au/settling-in-australia/amep/find-a-class/providers-and-locations) on the Department of Home Affairs website. The page shows Last updated 10/11/2026 10:08, which is `source_as_of`, and it was fetched on 18 November 2026. It has one tab for each state and territory, with a heading for each provider and a table of suburb, address and telephone. All 303 sites on the page are in this API: NSW 87, Victoria 108, Queensland 52, Western Australia 23, South Australia 19, ACT 7, Tasmania 4 and the Northern Territory 3. This API is a snapshot that is refreshed by hand. For the latest list, visit the [Home Affairs page](https://immi.homeaffairs.gov.au/settling-in-australia/amep/find-a-class/providers-and-locations).

The Department's [copyright page](https://www.homeaffairs.gov.au/access-and-accountability/using-our-website/copyright-and-disclaimer) says: "All material presented on this website is provided under a Creative Commons Attribution 3.0 Australia licence, with the exception of: the Commonwealth Coat of Arms ... our logo ... materials specifically not provided under a Creative Commons attribution 3.0 Australia licence ... content supplied by third parties." It asks you to attribute the material as Australian Government Department of Home Affairs. The licence is [CC BY 3.0 AU](https://creativecommons.org/licenses/by/3.0/au/). This API reformats the list (one row per site, provider names tidied, mobile numbers and some text left out, map points added). It is not endorsed by the Department, and no Department logo is used.

Map points come from the Geoscape Geocoded National Address File (G-NAF), November 2026 release, used under the Open G-NAF End User Licence Agreement. Required credit: "Incorporates or developed using G-NAF © Geoscape Australia licensed by the Commonwealth of Australia under the Open Geo-coded National Address File (G-NAF) End User Licence Agreement." The licence says you must not use G-NAF to generate an address or a list of addresses for sending mail, unless you check each address against another source. This API is for finding class sites, not for building a mailing list. 36 rows have no map point because no safe match was found. Points with `geoq` of `near`, `street` or `corner` can be a few hundred meters from the building. OpenStreetMap data was not used.

Phone numbers are the numbers the list shows for each site. Seven mobile numbers were left out. Email addresses are the general provider addresses linked on the page, and there are no personal addresses.

[Back to the API list](../../README.md)
