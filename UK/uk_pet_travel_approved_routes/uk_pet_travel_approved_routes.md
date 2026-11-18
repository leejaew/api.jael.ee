# UK Pet Travel Approved Routes

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 3410](https://img.shields.io/badge/records-3410-lightgrey) ![updated 2026-11-17](https://img.shields.io/badge/updated-2026--11--17-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

Contains public sector information licensed under the Open Government Licence v3.0. Source: Animal and Plant Health Agency (APHA), [Pet travel: approved air, sea and rail carriers and routes](https://www.gov.uk/government/publications/pet-travel-approved-air-sea-and-rail-carriers-and-routes), GOV.UK, page last updated 17 November 2026. Airport map points: OurAirports (public domain).

3,410 approvals of carriers and routes for bringing a pet into Great Britain, from the APHA lists on GOV.UK. 3,379 records are air approvals, one for each airline or charter operator and UK airport, with the animal handler where the page names one and an airport map point. 31 records are sea and rail approvals, one for each company and route, with the country and port the route starts from. The air page lists 59 commercial airlines and 554 charter operators, and the sea and rail page lists ferries, cruises and rail. No field is required, so you can filter by carrier, UK airport, UK port, handler or country, or by a latitude and longitude box.

This is not an official APHA or UK government service, and it has no official status. The lists change, and APHA says to check with your travel company if you want to make sure they are still approved. Check the GOV.UK page and your carrier before you travel. The page also says you must follow the pet travel rules and should ask your travel company about any extra rules. The page says you can only use these carriers and routes to bring your pet to England or Scotland, and that there are no approved routes to Wales. The air page still lists Cardiff for 57 charter operators, so those rows are kept and marked with `wales_flag` = `yes`. The page also says you do not have to use an approved carrier or route if you travel to England, Scotland or Wales from the UK countries, the Channel Islands, the Isle of Man or the Republic of Ireland. The lists have no row key. The generated `__id` is unique within the current data, and it can change when the data is refreshed.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_pet_travel_approved_routes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per carrier and UK airport, or per company and route. All matches are returned. |

## Query parameters

Required: none. Optional: 15 fields. Disallowed: 2 fields.

A call with no filters returns all 3,410 records (about 3 MB). Partial filters accept `*`, such as `carrier=*Brittany*`, `uk_airport=London*` or `handler=*Animal Air Care*`. `mode`, `carrier_type`, `service`, `from_country` and `wales_flag` are exact matches, and a star there is read as a plain character, so it returns `[]`. `lat` and `lng` accept a range with two dots, lower number first, such as `lat=51..52` or `lng=-2..-1`. Longitude values run from -4.43 to 0.94, so a western range starts with the lower negative number. A hyphen range such as `lat=51-52` returns HTTP 400. Text filters ignore case. `source_url` and `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `mode` | Optional | No | No | How the pet travels: air (3,379 rows), sea (29) or rail (2). Never blank. Exact match, such as mode=air, case ignored. |
| `carrier_type` | Optional | No | No | Kind of approved carrier: airline (100, scheduled commercial airlines), charter (3,279, charter and private jet operators), sea (29, ferry and cruise companies) or rail (2). Never blank. Exact match. |
| `service` | Optional | No | No | The section of the source page the row comes from: Commercial airline (100), Charter airline (3,279), Ferry (23), Cruise (6) or Rail (2). Never blank. Exact match, such as service=Ferry. It separates ferries from cruises, which carrier_type does not. |
| `carrier` | Optional | Yes | No | Name of the approved airline, charter operator, ferry, cruise or rail company as the page prints it, 626 distinct names (613 air carriers). Never blank. A carrier has one row for each UK airport or route it is approved for, so Paradox Jets Ltd and Brittany Ferries have many. Partial wildcards allowed, such as *Brittany* or *Aviation*. |
| `uk_airport` | Optional | Yes | No | UK airport the carrier is approved to fly pets into, 26 names, such as Biggin Hill, Farnborough or London Heathrow. The largest are Biggin Hill (400), Farnborough (364), London Stansted (242), London Luton (231), Manchester (222). Present on all 3,379 air rows, blank (null) on sea and rail rows. A few names the page spells differently are unified (Stansted becomes London Stansted); the original text is in notes. Partial wildcards allowed, such as London* or *Stansted*. |
| `uk_port` | Optional | Yes | No | UK port or station where the sea or rail route ends, such as Dover, Portsmouth, Southampton or Folkestone (Cheriton). Present on the 31 sea and rail rows, blank (null) on air rows. Partial wildcards allowed, such as Port*. |
| `handler` | Optional | Yes | No | Animal handling agent or terminal the page names next to an airport, such as Animal Air Care (AAC), Heathrow Animal Reception Centre (HARC), Animal Reception Centre (ARC), Pets on Jets, Inflite, XLR, Ravenair, Signature or Skypets. Present on 659 air rows, blank (null) on the rest. The abbreviations AAC, HARC and ARC are written out with the code in brackets. Partial wildcards allowed, such as *Animal Air Care* (204). |
| `from_country` | Optional | No | No | Country the pet travels from, as the sea and rail tables give it: France, Netherlands, Spain, USA, Falkland Islands, Germany, UK, or Various for cruises that call from many places. Present on 31 sea and rail rows. Blank (null) on all air rows because the air page does not give a departure country. Exact match, such as from_country=France. |
| `from_place` | Optional | Yes | No | Departure port or station on the route, such as Calais, St Malo or New York. Present on 27 of the 31 sea and rail rows; blank (null) for the cruise rows that list only a destination and for the Eurostar row. Blank (`null`) on all air rows. Partial wildcards allowed. |
| `route` | Optional | Yes | No | Route text from the source page, such as Calais to Dover or To Southampton. Present on the 31 sea and rail rows, blank (null) on air rows. Partial wildcards allowed, such as *to Dover. |
| `wales_flag` | Optional | No | No | yes on the 57 air rows for Cardiff, otherwise no. The source page says "There are no approved routes to Wales", yet it still lists Cardiff for these charter carriers, so the rows are kept and flagged. Never blank. Exact match, wales_flag=yes or no. |
| `notes` | Optional | Yes | No | Short notes from the page, such as Vehicle passengers only, Winter only, Ship: Queen Mary 2, and for airports the original spelling when it was unified (Listed on the page as Stansted). Present on 72 rows, blank (null) on the rest. Partial wildcards allowed, such as *only*. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84, usually 6 decimals; the 400 Biggin Hill rows carry 14) of the UK airport, taken from the OurAirports open data (public domain), not from the page. It is the airport reference point, not the handler or terminal. Present on 3,373 air rows, blank (null) on sea and rail rows and on the 6 Sheffield rows (the page does not say which airport that is). Values run from 50.78 to 55.95. A number. Range filter with two dots, min..max, such as lat=51..52. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same airport point. Present on the same rows. Values run from -4.43 to 0.94, so write the lower number first, such as lng=-2..-1. A number. A hyphen range returns 400. |
| `source_as_of` | Optional | Yes | No | Date the GOV.UK page was last updated, 2026-11-17, the same on every row. Text, YYYY-MM-DD. Partial wildcards allowed, such as 2026*. No range on dates. |
| `source_url` | Disallowed | No | No | Not filterable. The page the row came from, either the air page or the sea and rail page on GOV.UK. Returned in output only. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: Contains public sector information licensed under the Open Government Licence v3.0, with the APHA page name and the OurAirports credit for map points. Returned in output only. |

## Response fields

`lat` and `lng` are numbers. `__id` is an integer. All other values are strings. Blank cells are returned as `null`. Every row has a unique `__id`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. It is generated, and it can change when the data is refreshed, so do not store it. |
| `mode` | string | `air`, `sea` or `rail`. |
| `carrier_type` | string | `airline`, `charter`, `sea` or `rail`. |
| `service` | string | `Commercial airline`, `Charter airline`, `Ferry`, `Cruise` or `Rail`. |
| `carrier` | string | Airline, charter operator, ferry, cruise or rail company name as the page prints it. |
| `uk_airport` | string | UK airport for an air approval, or `null` on sea and rail rows. |
| `uk_port` | string | UK port or station for a sea or rail route, or `null` on air rows. |
| `handler` | string | Animal handler or terminal named next to the airport, or `null` on 2,751 rows. |
| `from_country` | string | Country the pet travels from for sea and rail routes, or `null` on air rows. |
| `from_place` | string | Departure port or station, or `null`. |
| `route` | string | Route text from the page, or `null` on air rows. |
| `wales_flag` | string | `yes` on the 57 Cardiff rows, otherwise `no`. |
| `notes` | string | Short note from the page, or `null`. |
| `lat` | number | Latitude of the UK airport, or `null`. |
| `lng` | number | Longitude of the UK airport, or `null`. |
| `source_url` | string | Link to the GOV.UK page the row came from. |
| `source` | string | Credit text for the lists and the airport points. |
| `source_as_of` | string | Date the GOV.UK page was last updated, `2026-11-17`. |

## Examples

Commercial airlines approved for London Heathrow. This call returns 51 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'uk_airport=London Heathrow' --data-urlencode 'carrier_type=airline'
```

```json
{
  "__id": 3411,
  "mode": "air",
  "carrier_type": "airline",
  "service": "Commercial airline",
  "carrier": "Aeromexico",
  "uk_airport": "London Heathrow",
  "uk_port": null,
  "handler": "Heathrow Animal Reception Centre (HARC)",
  "from_country": null,
  "from_place": null,
  "route": null,
  "wales_flag": "no",
  "notes": null,
  "lat": 51.470748,
  "lng": -0.459909,
  "source_url": "https://www.gov.uk/government/publications/pet-travel-approved-air-sea-and-rail-carriers-and-routes/approved-air-routes-for-pet-travel",
  "source": "Contains public sector information licensed under the Open Government Licence v3.0. Source: Animal and Plant Health Agency (APHA), Pet travel: approved air, sea and rail carriers and routes (gov.uk), page last updated 17 November 2026. Airport map points: OurAirports (public domain).",
  "source_as_of": "2026-11-17"
}
```

Approvals for London Gatwick that name Animal Air Care as the handler. This call returns 137 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'uk_airport=London Gatwick' --data-urlencode 'handler=*Animal Air Care*'
```

Approvals for one ferry company, matched by part of the name. This call returns 11 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'carrier=*Brittany*'
```

Ferry routes from France that end at Dover. This call returns 4 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'from_country=France' --data-urlencode 'uk_port=Dover'
```

Cruise approvals. This call returns 6 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'service=Cruise'
```

The Cardiff rows that the page lists even though it says there are no approved routes to Wales. This call returns 57 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'wales_flag=yes'
```

Air approvals for airports in a map box over north west England. The longitude values are negative, so the lower number comes first. This call returns 337 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_pet_travel_approved_routes' --data-urlencode 'lat=53..54' --data-urlencode 'lng=-3..-2'
```

## Data source

Contains public sector information licensed under the Open Government Licence v3.0. Source: Animal and Plant Health Agency (APHA), [Pet travel: approved air, sea and rail carriers and routes](https://www.gov.uk/government/publications/pet-travel-approved-air-sea-and-rail-carriers-and-routes), GOV.UK, page last updated 17 November 2026. Airport map points: OurAirports (public domain).

The records come from two pages on GOV.UK: [Approved air routes for pet travel](https://www.gov.uk/government/publications/pet-travel-approved-air-sea-and-rail-carriers-and-routes/approved-air-routes-for-pet-travel) and [Pet travel sea routes and companies you can use](https://www.gov.uk/government/publications/pet-travel-approved-air-sea-and-rail-carriers-and-routes/pet-travel-sea-routes-and-companies-you-can-use). The GOV.UK footer says that all content is available under the Open Government Licence v3.0, except where otherwise stated, and it carries the Crown copyright line. The [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/) lets you copy, publish, adapt and use the information commercially and non-commercially, as long as you acknowledge the source and link to the licence. It does not cover personal data or third-party rights, and it does not let you suggest any official status or that APHA endorses you. APHA and the UK government did not produce or check this API, and no logo is used. This API is a snapshot that is refreshed by hand. The source page notes that the lists are updated when APHA is told of changes.

The air page has two tables. Commercial airlines (59 rows) transport pets in the hold of the plane, and charter airlines (554 rows) transport pets in the carriage of the plane, as the page puts it. Each airline row lists several airports in one cell, and every airport has its own record here. The page has 3,380 airport entries. One cell was missing a comma, which makes 3,381 records, and 2 exact repeats were removed (CTR Atmospherica Aviation at Leeds Bradford, and Luxaviation SA at Birmingham), leaving 3,379. Where an airport is spelled in different ways on the page, one name is used (Stansted, Stanstead and London Stansted all become London Stansted), and `notes` keeps the original spelling. Text in brackets next to an airport is moved to `handler`. The abbreviations AAC, HARC and ARC are written out in full with the code in brackets. 659 air records name a handler. Carrier names are kept as the page prints them.

The sea and rail page has 28 route rows. Some rows list several companies, such as Calais to Dover with three, so each company has its own record. That gives 23 ferry, 6 cruise and 2 rail records. Stena Line, Holland BV is kept as one carrier exactly as printed, and it may be a single company. Cunard New York to Southampton is on both the ferry table (Queen Mary 2) and the cruise table, so it has two records. `from_country` is the page's own wording, such as USA, UK or Various. For the Eurostar row the page gives the UK as the starting country and lists London St Pancras International to Amsterdam, Brussels, Lille, Paris and Rotterdam, so `uk_port` is London St Pancras International and `notes` has the original text.

Airport map points come from [OurAirports](https://ourairports.com/data/), whose terms say: "All data is released to the Public Domain, and comes with no guarantee of accuracy or fitness for use." Each point is the airport reference point matched by ICAO code, not the handler or terminal. 3,373 air rows have a point. The 6 Sheffield rows have none, because the page does not say which airport Sheffield means. Sea and rail rows have no point. No OpenStreetMap data was used, and no geocoding was run.

The source lists company names only. No personal data was found or left out.

[Back to the API list](../../README.md)
