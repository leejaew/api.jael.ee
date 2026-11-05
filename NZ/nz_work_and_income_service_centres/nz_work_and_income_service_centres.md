# New Zealand Work and Income Service Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 122](https://img.shields.io/badge/records-122-lightgrey) ![updated 2026-08-26](https://img.shields.io/badge/updated-2026--08--26-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

All 122 Work and Income service centres in New Zealand, run by the Ministry of Social Development, as listed on the 11 regional Find a service centre pages of workandincome.govt.nz (pages updated May to August 2026), each with its street and postal address, opening hours and a map point. Filter by the required `region`, then by centre name, address, or a latitude and longitude range.

Most centres: Auckland (29), Waikato (13) and Bay of Plenty (12). The Southern region has 10. Every centre has coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_work_and_income_service_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 5 fields. Disallowed: 5 fields.

`region` must be in every call. Use `region=*` to list all 122 centres. Disallowed means the field is returned in the output but cannot be used as a filter. Without `region` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges. Macrons are kept in names, so use a wildcard if you are unsure of the spelling.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | Work and Income region: Auckland (29), Waikato (13), Bay of Plenty (12), Southern (10), Taranaki, King Country and Whanganui (10), Canterbury (9), Wellington (9), East Coast (8), Nelson, Marlborough and West Coast (8), Central (7) or Northland (7). Wildcards work, so `region=*` lists all 122 rows, and `Taranaki*` or `Nelson*` avoid typing the commas. |
| `name` | Optional | Yes | No | Service centre name as Work and Income lists it, such as `Kāpiti`, `Ōtara` or `Dunedin*` (2). Wildcards work and case is ignored. Macrons are kept, so use a wildcard if unsure (for example `*tara` finds Ōtara). Every name is unique. |
| `street_address` | Optional | Yes | No | Street address as published, with lines joined by commas, such as `*Christchurch*` (7) or `*Corner*` (8, centres given as a street corner). Wildcards work. Many addresses give only the street and number, without the town. |
| `postal_address` | Optional | Yes | No | Postal address, such as `PO Box*` (88) or `Private Bag*` (31). For 3 centres (Shirley, Tākaka and Dunedin South) it is a street address. Wildcards work. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from -46.406546 to -35.097973. Use a range, for example `latitude=-37.1..-36.7&longitude=174.5..175.0` for the Auckland area (23). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 168.348479 to 178.318589. Use a range with latitude to search a box. |
| `directions` | Disallowed | No | No | Extra directions published for 8 centres, such as Opposite the tennis courts. Blank for the rest. |
| `hours` | Disallowed | No | No | Opening hours. Each regional page says most service centres are open 8:30am to 5pm Monday to Friday and 9:30am to 5pm on Wednesdays, so 120 rows carry that general text; Kaikōura and Ohakune list their own hours. Hours are not confirmed for each centre; check the source page before visiting. |
| `geocode_quality` | Disallowed | No | No | How exact the map point is, best first: `address` (98), `street_corner` (6), `place` (4) or `street` (14). Points are approximate. |
| `source` | Disallowed | No | No | The Work and Income regional page the row comes from. |
| `updated` | Disallowed | No | No | Date the regional source page was last updated, `YYYY-MM-DD`, from 2026-05-21 to 2026-08-26. Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `region` returns HTTP 400 with `Your request is missing the following required parameters: region. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.
* Names with macrons must be URL encoded, which `curl --data-urlencode` does for you.

## Response fields

| Field | Type | Description |
|---|---|---|
| `name` | string | Service centre name. Unique key. |
| `region` | string | Work and Income region. |
| `street_address` | string | Street address, lines joined by commas. |
| `postal_address` | string | Postal address. |
| `directions` | string | Extra directions. Null on most rows. |
| `hours` | string | Opening hours text. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | How exact the map point is. |
| `source` | string | Link to the regional source page. |
| `updated` | string | Source page date, `YYYY-MM-DD`. |

## Examples

Exact name. Kāpiti in Wellington. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_work_and_income_service_centres" --data-urlencode "region=Wellington" --data-urlencode "name=Kāpiti"
```

```json
[
  {
    "name": "Kāpiti",
    "region": "Wellington",
    "street_address": "Shop 915, Coastlands Shopping Mall, Rimu Road, Paraparaumu",
    "postal_address": "Private Bag 60602, Paraparaumu 5254",
    "directions": null,
    "hours": "8:30am to 5pm Monday to Friday, except Wednesday 9:30am to 5pm",
    "latitude": -40.917257,
    "longitude": 175.004036,
    "geocode_quality": "place",
    "source": "https://www.workandincome.govt.nz/about-work-and-income/contact-us/find-a-service-centre/wellington",
    "updated": "2026-08-26"
  }
]
```

Auckland region. This call returns 29 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_work_and_income_service_centres?region=Auckland" | jq length
```

Name wildcard. Dunedin. This call returns 2 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_work_and_income_service_centres" --data-urlencode "region=*" --data-urlencode "name=Dunedin*" | jq -r '.[].name'
```

Street address wildcard. Christchurch. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_work_and_income_service_centres" --data-urlencode "region=*" --data-urlencode "street_address=*Christchurch*" | jq length
```

Map box over Auckland. This call returns 23 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/nz_work_and_income_service_centres" --data-urlencode "region=*" --data-urlencode "latitude=-37.1..-36.7" --data-urlencode "longitude=174.5..175.0" | jq length
```

## Data source

The centres come from the Ministry of Social Development, Work and Income, [Find a service centre](https://www.workandincome.govt.nz/about-work-and-income/contact-us/find-a-service-centre), 11 regional pages last updated between 21 May 2026 and 26 August 2026, fetched on 5 November 2026. Crown copyright, licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license, which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Changes: the 11 regional lists were merged with a region column, address lines were joined with commas, spaces were tidied, the name Highland park was capitalised as Highland Park, the general opening hours from each page were copied to centres without their own hours, and map points were added. The data is not guaranteed to be complete or accurate. Check hours with the centre before you visit.

Credit lines:

* Source: Ministry of Social Development, Work and Income, Find a service centre (https://www.workandincome.govt.nz/about-work-and-income/contact-us/find-a-service-centre), 11 regional pages last updated between 21 May 2026 and 26 August 2026, fetched 5 November 2026. Crown copyright, licensed under the Creative Commons Attribution 4.0 International license (https://creativecommons.org/licenses/by/4.0/).
* Map points from OpenStreetMap contributors via Photon and Overpass (ODbL, https://www.openstreetmap.org/copyright).
* Not endorsed by the Ministry of Social Development.

[Back to the API list](../../README.md)
