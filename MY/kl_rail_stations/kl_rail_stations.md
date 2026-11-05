# Klang Valley Rapid Rail Stations

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 162](https://img.shields.io/badge/records-162-lightgrey) ![updated 2026-11-05](https://img.shields.io/badge/updated-2026--11--05-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

All 162 unique Rapid Rail Klang Valley stations from Prasarana's GTFS static feed (LRT Ampang, Kelana Jaya, Sri Petaling and Shah Alam lines; MRT Kajang and Putrajaya lines; KL Monorail; BRT Sunway), with station codes, line codes, an interchange flag, a step free access flag and a map point. Filter by the required `categories`, then by name, line, interchange or a latitude and longitude range.

Rows by exact category alone: LRT 83, MRT 55, BRT 7 and MRL 8. Stations that serve an LRT line (including LRT and MRT interchanges): 91. Interchange stations (more than one line code): 20.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/kl_rail_stations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`categories`). Optional: 9 fields. Disallowed: 2 fields.

`categories` must be in every call. Use `categories=*` to list all 162 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `categories` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `categories` | Required | Yes | No | Mode category: `LRT`, `MRT`, `MRL` (monorail), `BRT`, or a joined value such as `LRT;MRT` for interchanges. Wildcards work, so `categories=*` lists all 162 rows, and `categories=*LRT*` matches every station that serves an LRT line (91). Exact `LRT` alone is 83; exact `MRT` alone is 55; `BRT` is 7; `MRL` alone is 8. |
| `station_code` | Optional | Yes | No | GTFS stop id codes at this station, joined with semicolons when a station has more than one platform code, such as `AG18`, `*KJ13*` or `AG7;KJ13;SP7` (Masjid Jamek). Wildcards work. Text. |
| `name` | Optional | Yes | No | Station name in title case, such as `*Sentral*` (6), `KL Sentral` or `Masjid Jamek`. Wildcards work. |
| `name_upper` | Optional | Yes | No | Original GTFS `stop_name` in uppercase, such as `KL SENTRAL` or `*SENTRAL*`. Wildcards work. |
| `lines` | Optional | Yes | No | Short line codes joined with semicolons: AGL (18), KJL (37), SPL (29), KGL (29), PYL (36), MRL (11), BRT (7), SAL (20). Example `*KJL*` or `KJL;SPL` (Putra Heights). Wildcards work. |
| `line_names` | Optional | Yes | No | Full line names joined with semicolons, such as `*Kelana Jaya*` or `*Kajang*`. Wildcards work. |
| `interchange` | Optional | No | No | Exact: `yes` (20) if the station serves more than one line code, `no` (142) if it serves only one. |
| `accessible` | Optional | No | No | Exact: `yes` (162) if the GTFS feed marks the stop as OKU (step free) accessible. All rows are `yes` in this feed. |
| `latitude` | Optional | No | Yes | Latitude of the station in decimal degrees (WGS 84), from 2.9313 to 3.231793. Use a range, for example `latitude=3.1..3.2` (82). |
| `longitude` | Optional | No | Yes | Longitude of the station in decimal degrees (WGS 84), from 101.440556 to 101.790278. Use a range, for example `longitude=101.69..101.71`. |
| `source` | Disallowed | No | No | Data source and credit for the row. |
| `updated` | Disallowed | No | No | Date this feed copy was fetched, `YYYY-MM-DD` (`2026-11-05`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `categories` returns HTTP 400 with `Your request is missing the following required parameters: categories. You did not supply any.`
* A filter on `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source`.
* Wildcards work only on `categories`, `station_code`, `name`, `name_upper`, `lines` and `line_names`. A wildcard on another text field returns `[]`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `station_code` | string | GTFS stop id codes, joined with semicolons when needed. |
| `name` | string | Station name in title case. |
| `name_upper` | string | Original GTFS stop name in uppercase. |
| `lines` | string | Short line codes joined with semicolons. |
| `line_names` | string | Full line names joined with semicolons. |
| `categories` | string | Mode categories joined with semicolons. |
| `interchange` | string | `yes` or `no`. |
| `accessible` | string | `yes` if step free (OKU). |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `source` | string | Credit line for the feed. |
| `updated` | string | Date this feed copy was fetched, `YYYY-MM-DD`. |

Notes:

* Interchange stations that appear on more than one line in the GTFS feed are one row, with joined codes and line names.
* Kajang Line stops that used route id `MRT` in the feed are mapped to line code `KGL`.
* data.gov.my also publishes a live GTFS API for the same feed. This API is a station level view that is easier to query by name, line and category.

## Examples

Exact station name. KL Sentral on the monorail. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/kl_rail_stations" --data-urlencode "categories=*" --data-urlencode "name=KL Sentral"
```

```json
[
  {
    "__id": 64,
    "station_code": "MR1",
    "name": "KL Sentral",
    "name_upper": "KL SENTRAL",
    "lines": "MRL",
    "line_names": "KL Monorail Line",
    "categories": "MRL",
    "interchange": "no",
    "accessible": "yes",
    "latitude": 3.132852,
    "longitude": 101.687817,
    "source": "Prasarana Malaysia, Rapid Rail KL GTFS static feed (data.gov.my), Creative Commons Attribution 4.0 International (CC BY 4.0)",
    "updated": "2026-11-05"
  }
]
```

A wildcard on the name. Every station with Sentral in the name. This call returns 6 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/kl_rail_stations" --data-urlencode "categories=*" --data-urlencode "name=*Sentral*" | jq -r '.[] | [.name, .lines, .categories] | @tsv'
```

```text
Dato' Menteri - Sa Sentral	SAL	LRT
KL Sentral	MRL	MRL
KL Sentral - Redone	KJL	LRT
KWASA Sentral	KGL	MRT
Putrajaya Sentral	PYL	MRT
Sri Damansara Sentral	PYL	MRT
```

Exact LRT only (no MRT interchanges). This call returns 83 rows:

```bash
curl -s "https://api.jael.ee/JLEE/kl_rail_stations?categories=LRT" | jq length
```

Stations that serve any LRT line. This call returns 91 rows:

```bash
curl -s "https://api.jael.ee/JLEE/kl_rail_stations?categories=*LRT*" | jq length
```

Range on latitude and longitude. Stations near KLCC. This call returns 10 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/kl_rail_stations" --data-urlencode "categories=*" --data-urlencode "latitude=3.14..3.16" --data-urlencode "longitude=101.69..101.71" | jq length
```

## Data source

The stations come from Prasarana Malaysia Berhad, Rapid Rail KL GTFS static feed via [data.gov.my](https://developer.data.gov.my/realtime-api/gtfs-static), fetched on 5 November 2026. The GTFS zip has no `feed_info.txt` publish date. Licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Changes: stops with the same station name were joined into one row with semicolon separated codes and lines; title case names were added; the interchange flag was derived from multiple line codes; Kajang Line stops that used route id `MRT` in the feed were mapped to `KGL`. The data is not guaranteed to be complete or accurate.

Credit lines:

* Source: Prasarana Malaysia Berhad, Rapid Rail KL GTFS static feed via data.gov.my (https://developer.data.gov.my/realtime-api/gtfs-static), fetched 5 November 2026. Licensed under Creative Commons Attribution 4.0 International (https://creativecommons.org/licenses/by/4.0/).
* data.gov.my also publishes a live GTFS API for the same feed.
* No logos. Not endorsed by Prasarana or the Malaysian government.

[Back to the API list](../../README.md)
