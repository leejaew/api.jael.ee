# UK Rail Station Usage

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 2589](https://img.shields.io/badge/records-2589-lightgrey) ![period 2024-25](https://img.shields.io/badge/period-2024--25-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

How many people used each of the 2,589 railway stations in Great Britain from April 2024 to March 2025, from the Office of Rail and Road (ORR). Each row has entries and exits by ticket type (full price, reduced price and season), the total, the national rank, interchanges, the main station people travel to or from, the CRS and NLC codes, region, the company that runs the station, and a map point from the Department for Transport NaPTAN dataset. Filter by the required `region`, then by name, operator, CRS code, or a range on usage, rank or location.

The busiest station is London Liverpool Street with 98,015,658 entries and exits. The 10 busiest stations are all in London. Clapham Junction has the most interchanges (20,404,411). Most stations: South East (432), Scotland (363) and London (340). ORR published these figures on 2025-12-04.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_rail_station_usage` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 19 fields. Disallowed: 5 fields.

`region` must be in every call. Use `region=*` to list all 2,589 stations. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | Region of England, or Scotland or Wales: `South East` (432), `Scotland` (363), `London` (340), `North West` (337), `Wales` (223), `East of England` (215), `Yorkshire and The Humber` (187), `South West` (170), `West Midlands` (156), `East Midlands` (108) or `North East` (58). Wildcards work, so `region=*` lists all 2,589 stations and `region=*East*` gives 813. |
| `station_name` | Optional | Yes | No | Station name as ORR lists it, such as `London Liverpool Street`, `London*` (17) or `*Parkway` (23). Some names have the area in brackets, such as `Stratford (London)`. |
| `crs_code` | Optional | No | No | 3 letter CRS code used on tickets and timetables, such as `LST` (London Liverpool Street) or `EDB` (Edinburgh). Unique per row. Exact match only. |
| `nlc` | Optional | No | No | 4 digit National Location Code used in rail ticketing, such as `6965` or `0506` (Amersham). Text, so the leading zero stays. Exact match only. |
| `station_facility_owner` | Optional | Yes | No | Company that runs the station, one of 31, such as `Northern Trains` (468), `ScotRail` (360), `TfW Rail` (248), `Network Rail` (20, the largest stations) or `Govia*` (236). |
| `station_group` | Optional | Yes | No | ORR station group for towns where tickets cover several stations, such as `LONDON BR` (20) or `MANCHESTER BR` (4). Null for 2,486 rows not in a group. |
| `entries_exits_full` | Optional | No | Yes | Entries and exits on full price tickets, from 0 to 35,286,688. Null for 3 rows with no usage. |
| `entries_exits_reduced` | Optional | No | Yes | Entries and exits on reduced price tickets, such as railcards and off peak fares, from 14 to 50,038,262. Null for 3 rows. |
| `entries_exits_season` | Optional | No | Yes | Entries and exits on season tickets, from 0 to 12,690,708. Null for 3 rows. |
| `entries_exits_all` | Optional | No | Yes | Total entries and exits, from 68 to 98,015,658. Example: `entries_exits_all=10000000..200000000` (50). Null for 3 rows. |
| `rank` | Optional | No | Yes | National rank by total entries and exits, from 1 to 2586. Example: `rank=1..10` (10). Null for 3 rows. |
| `interchanges` | Optional | No | Yes | Times people changed trains at the station, from 0 to 20,404,411. Example: `interchanges=1000000..30000000` (53). Null for 3 rows. |
| `main_origin_destination` | Optional | Yes | No | Station most often travelled to or from, such as `London Euston` (34) or `Tottenham Court Road` (7). Null for 3 rows. |
| `main_od_journeys` | Optional | No | Yes | Journeys in the year between this station and its main origin or destination, from 24 to 8,699,014. Example: `main_od_journeys=1000000..10000000` (139). Null for 3 rows. |
| `data_source_adjustments` | Optional | Yes | No | ORR codes for extra data or changes used in the estimate, often several in one cell, such as `*CS*` (298). See the ORR methodology for each code. Null for 1,353 rows. |
| `local_ticketing_data` | Optional | No | No | Local transport body whose ticket sales were added, such as `Strathclyde` (186), `Merseyside` (93) or `Greater Manchester` (90). Null for 2,038 rows. Exact match only. |
| `additional_information` | Optional | Yes | No | ORR notes on events that changed usage, such as closures or new stations, for example `*closed*` (22) or `*suspended*` (3). Null for 2,500 rows. |
| `naptan_atco_code` | Optional | No | No | NaPTAN stop code for the station, such as `9100LIVST`, for joining with other UK transport data. Exact match only. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS84), from 50.121674 to 58.590177. Example: `latitude=57..60` (55). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS84), from -5.839078 to 1.749708. Example: `latitude=51.49..51.53&longitude=-0.15..-0.07` (18, central London). |
| `quality_limitations` | Disallowed | No | No | ORR note on known limits of the estimate, given for 11 rows: 8 stations in Wales that may be overestimated and the 3 Heathrow stations. |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from: `naptan_crs` (2,583) or `naptan_crs_grid` (6). See the notes below. |
| `period` | Disallowed | No | No | Year the counts cover, `2024-25` (April 2024 to March 2025) for every row. |
| `source` | Disallowed | No | No | Data source and credit for the row: ORR and DfT NaPTAN, both under the Open Government Licence v3.0. |
| `updated` | Disallowed | No | No | Date ORR published the figures, `YYYY-MM-DD` (`2025-12-04` for every row). |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `region` returns HTTP 400 with `Your request is missing the following required parameters: region. You did not supply any.`
* A filter on `quality_limitations`, `geocode_quality`, `period`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: geocode_quality` (or the field you used).

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `station_name` | string | Station name. |
| `crs_code` | string | 3 letter CRS code. |
| `nlc` | string | 4 digit National Location Code. |
| `region` | string | Region of England, or Scotland or Wales. |
| `station_facility_owner` | string | Company that runs the station. |
| `station_group` | string | ORR station group, if any. |
| `entries_exits_full` | integer | Entries and exits on full price tickets. |
| `entries_exits_reduced` | integer | Entries and exits on reduced price tickets. |
| `entries_exits_season` | integer | Entries and exits on season tickets. |
| `entries_exits_all` | integer | Total entries and exits. |
| `rank` | integer | National rank by total entries and exits. |
| `interchanges` | integer | Times people changed trains at the station. |
| `main_origin_destination` | string | Station most often travelled to or from. |
| `main_od_journeys` | integer | Journeys to or from the main origin or destination. |
| `data_source_adjustments` | string | ORR codes for extra data used. |
| `local_ticketing_data` | string | Local ticket data added, if any. |
| `quality_limitations` | string | ORR note on limits of the estimate. |
| `additional_information` | string | ORR notes on events that changed usage. |
| `naptan_atco_code` | string | NaPTAN stop code. |
| `latitude` | number | Latitude in decimal degrees. |
| `longitude` | number | Longitude in decimal degrees. |
| `geocode_quality` | string | Where the map point comes from. |
| `period` | string | Year the counts cover. |
| `source` | string | Data source and credit. |
| `updated` | string | Date ORR published the figures. |

## Notes

* **Entries and exits.** ORR says these figures are estimates of the number of entries, exits and interchanges at each station, based mainly on ticket sales. They are not gate counts.
* **Stations with no usage.** Altnabreac, Stanlow and Thornton, and Teesside Airport had their mainline services suspended for the whole year, so ORR recorded no entries or exits. Their counts, rank and main origin or destination are null, and `additional_information` starts with a note that says so.
* **Codes.** ORR stores NLC as a number, so 16 codes had 3 digits in the source (for example Amersham `506`). They were padded to the usual 4 digits (`0506`).
* **Map points.** Every station was matched to its NaPTAN rail station point by CRS code (`naptan_crs`, 2,583 rows). For 6 stations NaPTAN gives only a British National Grid point (easting and northing), which was turned into latitude and longitude (`naptan_crs_grid`): Barking Riverside, Bond Street, Canary Wharf, Custom House, Tottenham Court Road and Woolwich.
* **Next update.** ORR lists 19 November 2026 as the date of its next release, the 2025-26 figures (April 2025 to March 2026). This API covers 2024-25 until then.

## Examples

One station by CRS code. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_rail_station_usage" --data-urlencode "region=*" --data-urlencode "crs_code=LST"
```

```json
[
  {
    "__id": 1459,
    "station_name": "London Liverpool Street",
    "crs_code": "LST",
    "nlc": "6965",
    "region": "London",
    "station_facility_owner": "Network Rail",
    "station_group": "LONDON BR",
    "entries_exits_full": 35286688,
    "entries_exits_reduced": 50038262,
    "entries_exits_season": 12690708,
    "entries_exits_all": 98015658,
    "rank": 1,
    "interchanges": 6534549,
    "main_origin_destination": "Tottenham Court Road",
    "main_od_journeys": 8699014,
    "data_source_adjustments": "Group, Same OD, EL, CPAY",
    "local_ticketing_data": null,
    "quality_limitations": null,
    "additional_information": null,
    "naptan_atco_code": "9100LIVST",
    "latitude": 51.517991,
    "longitude": -0.081426,
    "geocode_quality": "naptan_crs",
    "period": "2024-25",
    "source": "Office of Rail and Road, Estimates of station usage, Table 1410 (April 2024 to March 2025), Open Government Licence v3.0; coordinates from DfT NaPTAN, Open Government Licence v3.0",
    "updated": "2025-12-04"
  }
]
```

Stations in one region with a name wildcard. This call returns 2 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_rail_station_usage" --data-urlencode "region=Scotland" --data-urlencode "station_name=Glasgow*" | jq -r '.[] | [.station_name, .crs_code, .entries_exits_all, .rank] | @tsv'
```

```text
Glasgow Central	GLC	25293930	17
Glasgow Queen Street	GLQ	14950752	30
```

The 10 busiest stations, with a range on rank. This call returns 10 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_rail_station_usage" --data-urlencode "region=*" --data-urlencode "rank=1..10" | jq -r 'sort_by(.rank) | .[] | [.rank, .station_name, .crs_code, .entries_exits_all] | @tsv'
```

```text
1	London Liverpool Street	LST	98015658
2	London Waterloo	WAT	70389546
3	London Paddington	PAD	69897084
4	Tottenham Court Road	TCR	68133826
5	London Bridge	LBG	54685648
6	London Victoria	VIC	53784358
7	Stratford (London)	SRA	51473518
8	Farringdon	ZFD	50171290
9	Bond Street	BDS	42752242
10	London Euston	EUS	40248974
```

Stations where more than 10 million people changed trains. This call returns 2 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_rail_station_usage" --data-urlencode "region=*" --data-urlencode "interchanges=10000000..30000000" | jq -r '.[] | [.station_name, .interchanges] | @tsv'
```

```text
Clapham Junction	20404411
Whitechapel	13770478
```

A box on the map in central London. This call returns 18 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_rail_station_usage" --data-urlencode "region=*" --data-urlencode "latitude=51.49..51.53" --data-urlencode "longitude=-0.15..-0.07" | jq length
```

## Data source

Station usage comes from the Office of Rail and Road [Estimates of station usage](https://dataportal.orr.gov.uk/statistics/usage/estimates-of-station-usage/), Table 1410, Passenger entries and exits and interchanges by station, April 2024 to March 2025, published 4 December 2025 and fetched on 2026-11-03. Map points come from the Department for Transport [National Public Transport Access Nodes (NaPTAN)](https://naptan.api.dft.gov.uk/) dataset, rail stations only, fetched on 2026-11-03. Both are published under the [Open Government Licence v3.0 (OGL v3.0)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), which lets anyone copy, adapt and use the data, including for commercial use, as long as the source is credited. Changes: columns renamed, numbers stored without commas, `[z]` cells and note marks removed, 3 digit NLC codes padded to 4 digits, a note added for the 3 stations with no usage, and map points added from NaPTAN by CRS code, with 6 British National Grid points turned into latitude and longitude. The data is not guaranteed to be complete or accurate.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0. Source: Office of Rail and Road, Estimates of station usage (https://dataportal.orr.gov.uk).
* Map points: Department for Transport, National Public Transport Access Nodes (NaPTAN) (https://naptan.api.dft.gov.uk), licensed under the Open Government Licence v3.0.
* Not endorsed by the Office of Rail and Road or the Department for Transport. No ORR or DfT logos are used.

[Back to the API list](../../README.md)
