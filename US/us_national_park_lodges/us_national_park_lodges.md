# US National Park Lodges

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 92](https://img.shields.io/badge/records-92-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license public domain and facts](https://img.shields.io/badge/license-public%20domain%20%2B%20facts-green)

Lodges, inns, hotels, cabins, tent cabins, huts and hostels inside US National Park Service units, run by a park concessioner, as checked on 2026-10-30. There are 92 places to stay in 33 park units and 22 states and territories, from Yosemite (13) and Glacier and Yellowstone (9 each) to the US Virgin Islands. Each row has the lodge name, park name and code, unit type, state, operator, lodging type, status and a closure note, the usual season in words and as month numbers, a year round flag, room count where the operator gives one, phone, booking page, NPS lodging page and a map point. Filter by the required `state`, then by park, operator, lodging type, status or a month range.

14 lodges are marked `temporarily_closed`, so check `status` and `status_note` before you plan a trip. Seasons are the usual months and change from year to year.

> **Use of this data.** Facts come from National Park Service pages (public domain) and plain facts from operator websites (names, phones, months, room counts and links, with no text, photos or reviews). Map points come from OpenStreetMap. Not affiliated with or endorsed by the National Park Service or any lodge operator. Check with the operator before booking.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_national_park_lodges` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 16 fields. Disallowed: 5 fields.

`state` must be in every call. Use `state=*` to list all 92 lodges. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | 2 letter state or territory code where the lodge sits, such as `CA` (21), `WY` (16), `AZ` (10) or `MT` (9). 22 values, including `VI`. Wildcards work, so `state=*` lists all 92 lodges. |
| `lodge_name` | Optional | Yes | No | Name of the lodge, inn, hotel or cabin site, such as `*Lodge*` or `Old Faithful*`. One row per name. |
| `park_name` | Optional | Yes | No | Full name of the NPS unit, such as `Yosemite National Park` or `*Recreation Area*`. 33 units. |
| `park_code` | Optional | Yes | No | 4 letter NPS park code in lower case, such as `yose` (13), `glac` (9), `yell` (9) or `grca` (7). |
| `unit_type` | Optional | Yes | No | Kind of NPS unit: `national_park` (81), `national_recreation_area` (7), `parkway` (2), `national_river_and_recreation_area` (1) or `national_river` (1). |
| `operator` | Optional | Yes | No | Concession company that runs the lodge, such as `*Xanterra*` (20) or `*Aramark*` (23). 33 values. |
| `lodging_type` | Optional | Yes | No | Kind of lodging: `lodge` (31), `cabins` (23), `hotel` (11), `tent_cabins` (11), `motel` (7), `inn` (3), `ranch` (2), `chalet` (2), `hut` (1) or `hostel` (1). |
| `status` | Optional | Yes | No | Current status: `operating` (78) or `temporarily_closed` (14). See `status_note` for details. |
| `status_note` | Optional | Yes | No | Short note on access or a closure, such as `*closed*` or `*boat*`. Blank for 29 rows. |
| `season` | Optional | Yes | No | Usual open season in words, such as `*May*` or `year round` (29 rows). Dates change each year, so confirm with the operator. |
| `season_open` | Optional | No | Yes | Month number (1 to 12) the lodge usually opens, `1` when open all year. Example: `season_open=5..6`. Blank for 2 rows. |
| `season_close` | Optional | No | Yes | Month number (1 to 12) the lodge usually closes, `12` when open all year. Example: `season_close=9..10`. Blank for 2 rows. |
| `year_round` | Optional | Yes | No | Whether the lodge is usually open all year: `yes` (32) or `no` (60). |
| `phone` | Optional | Yes | No | Reservation phone number, such as `888-413-8869` or `888*`. Blank for 7 rows. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 18.352419 to 58.555473. Example: `latitude=36..38`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -155.77826 to -64.754697. Example: `longitude=-120..-119`. |
| `geocode_quality` | Optional | Yes | No | How the map point was placed: `osm_poi` (83, the lodge itself), `osm_place` (7, the named place or area) or `osm_nearby` (2, a nearby campground). |
| `rooms` | Disallowed | No | No | Number of rooms or units where the operator states it, from 10 to 385. Blank for 71 rows. |
| `booking_url` | Disallowed | No | No | Operator page for the lodge or for booking it (https). |
| `nps_url` | Disallowed | No | No | NPS lodging page for the park (https). |
| `source` | Disallowed | No | No | Source of the row. |
| `source_as_of` | Disallowed | No | No | Date the data was checked, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `lodge_name` | string | Name of the lodge, inn, hotel or cabin site. |
| `park_name` | string | Full name of the NPS unit. |
| `park_code` | string | 4 letter NPS park code, lower case. |
| `unit_type` | string | Kind of NPS unit. |
| `state` | string | 2 letter state or territory code. |
| `operator` | string | Concession company that runs the lodge. |
| `lodging_type` | string | Kind of lodging. |
| `status` | string | `operating` or `temporarily_closed`. |
| `status_note` | string | Note on access or a closure. Null for 29 rows. |
| `season` | string | Usual open season in words. |
| `season_open` | integer | Month the lodge usually opens. Null for 2 rows. |
| `season_close` | integer | Month the lodge usually closes. Null for 2 rows. |
| `year_round` | string | `yes` or `no`. |
| `rooms` | integer | Number of rooms or units. Null for 71 rows. |
| `phone` | string | Reservation phone number. Null for 7 rows. |
| `booking_url` | string | Operator page for the lodge or for booking it (https). |
| `nps_url` | string | NPS lodging page for the park (https). |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `geocode_quality` | string | `osm_poi`, `osm_place` or `osm_nearby`. |
| `source` | string | Source of the row. |
| `source_as_of` | string | Date the data was checked. |

Notes:

* 14 lodges are marked `temporarily_closed` as of 2026-10-30. Seven are at Grand Canyon South Rim and below it (El Tovar, Bright Angel, Kachina, Thunderbird, Maswik, Yavapai and Phantom Ranch), where overnight stays have been paused since August 31, 2026 after breaks in the Transcanyon Waterline. They are pending reopening: the operator says most South Rim stays from November 20, 2026 are going ahead for now, Phantom Ranch is closed through at least January 31, 2027, and Yavapai has no date yet. The other seven are White Wolf, Wawona, Vogelsang, Merced Lake and Glacier Point Ski Hut in Yosemite, Bearpaw in Sequoia, and North Cascades Lodge at Stehekin. `status_note` gives the reason for each.
* Seasons are typical months, not fixed dates. Two seasons run over the new year: Glacier Point Ski Hut (12 to 3) and Flamingo Winter Glamping Tents (11 to 4), so a plain month range on `season_open` or `season_close` will not catch them the way it catches summer lodges. Wawona Hotel and North Cascades Lodge at Stehekin have no season while closed.
* Map points come from OpenStreetMap. 83 mark the lodge itself, 7 mark the named place or area, and 2 (Lewis Mountain Cabins and Namakanipaio Camper Cabins) use the campground next door. Every point was checked to fall in the state shown.
* Only concession lodging is included. Private lodges on land inside a park (such as Kantishna or Panamint Springs), campgrounds and houseboats are left out. Grand Canyon North Rim lodge, which burned in 2025, is not listed.

## Examples

Two filters together. Lodges in Wyoming with "lake" in the name. This call returns 4 rows, 2 in Yellowstone and 2 in Grand Teton:

```bash
curl -sG "https://api.jael.ee/JLEE/us_national_park_lodges" --data-urlencode "state=WY" --data-urlencode "lodge_name=*lake*" | jq -r '.[] | [.lodge_name, .park_code, .status, .season] | @tsv'
```

```text
Lake Yellowstone Hotel and Cabins	yell	operating	mid May to early October
Lake Lodge Cabins	yell	operating	early June to early October
Jackson Lake Lodge	grte	operating	mid May to early October
Jenny Lake Lodge	grte	operating	June to early October
```

The first row in full:

```json
[
  {
    "__id": 48,
    "lodge_name": "Lake Yellowstone Hotel and Cabins",
    "park_name": "Yellowstone National Park",
    "park_code": "yell",
    "unit_type": "national_park",
    "state": "WY",
    "operator": "Yellowstone National Park Lodges (Xanterra)",
    "lodging_type": "hotel",
    "status": "operating",
    "status_note": null,
    "season": "mid May to early October",
    "season_open": 5,
    "season_close": 10,
    "year_round": "no",
    "rooms": null,
    "phone": "307-344-7311",
    "booking_url": "https://www.yellowstonenationalparklodges.com/lodgings/hotel/lake-yellowstone-hotel/",
    "nps_url": "https://www.nps.gov/yell/planyourvisit/lodging.htm",
    "latitude": 44.549964,
    "longitude": -110.400856,
    "geocode_quality": "osm_poi",
    "source": "NPS park lodging page; operator website; OpenStreetMap for coordinates",
    "source_as_of": "2026-10-30"
  }
]
```

A range on the opening month. California lodges that usually open in May or June. This call returns 5 rows. One of them, Bearpaw High Sierra Camp, is closed for 2026:

```bash
curl -sG "https://api.jael.ee/JLEE/us_national_park_lodges" --data-urlencode "state=CA" --data-urlencode "season_open=5..6" | jq -r '.[] | [.lodge_name, .park_code, .season, .season_open, .season_close, .status] | @tsv'
```

```text
Tuolumne Meadows Lodge	yose	early June to mid September	6	9	operating
Cedar Grove Lodge	seki	May to mid October	5	10	operating
Bearpaw High Sierra Camp	seki	summer	6	9	temporarily_closed
Manzanita Lake Camping Cabins	lavo	late May to mid October	5	10	operating
Drakesbad Guest Ranch	lavo	June to early fall	6	9	operating
```

Every lodge that is closed now. This call returns 14 rows, and the first three are El Tovar Hotel, Bright Angel Lodge and Kachina Lodge at Grand Canyon:

```bash
curl -sG "https://api.jael.ee/JLEE/us_national_park_lodges" --data-urlencode "state=*" --data-urlencode "status=temporarily_closed" | jq -r '.[] | [.lodge_name, .park_code, .state] | @tsv'
```

## Data source

The lodge list was built park by park from the National Park Service lodging pages (`https://www.nps.gov/<park code>/planyourvisit/`, one `nps_url` per park), which are US government works in the public domain. Each lodge was then checked on its operator's website for plain facts only: name, operator, phone, season months, room count, booking page and closure notes. No text, photos, prices or ratings were copied. Map points come from OpenStreetMap. Status, season month numbers and geocode fields are set by us, and can go out of date. Check with the operator before booking.

Credit lines:

* Source: National Park Service, park lodging pages (https://www.nps.gov), public domain. Lodge facts from concessioner websites (facts only).
* Coordinates: © OpenStreetMap contributors, ODbL (https://www.openstreetmap.org/copyright).
* Not affiliated with or endorsed by the National Park Service or any lodge operator. Check with the operator before booking.

[Back to the API list](../../README.md)
