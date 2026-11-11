# Community Diagnostic Centres and Surgical Hubs

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 285](https://img.shields.io/badge/records-285-lightgrey) ![updated 2026-11-11](https://img.shields.io/badge/updated-2026--11--11-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

285 Community Diagnostic Centres and elective surgical hubs in England, from the NHS Fit for the Future map last modified 15 October 2026. Each record has the site type, name, postcode, local authority, region, Integrated Care Board and a map point. Choose a site type, then narrow by name, postcode, area, board or a latitude and longitude box.

Sites by type: CDC (165), a Community Diagnostic Centre for tests such as imaging, bloods and endoscopy; Surgical hub (120), a site for planned operations. Regions: South East (32 CDC, 17 hubs); North West (25, 17); Yorkshire and The Humber (19, 16); London (14, 21); West Midlands (16, 19); South West (21, 11); East of England (20, 9); East Midlands (14, 5); North East (4, 5).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_nhs_cdcs_surgical_hubs` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per site. All matches are returned. |

## Query parameters

Required: 1 field (`site_type`). Optional: 8 fields. Disallowed: 2 fields.

`site_type` must be in every call. Use `CDC` or `Surgical hub`. Partial filters accept `*`, such as `name=*orthopaedic*` or `postcode=LS*`. `lat` and `lng` accept ranges with two dots, such as `lat=53.7..53.9`. `icb_code` is exact match only. Text filters ignore case, so `region=yorkshire*` also matches Yorkshire and The Humber. `source` and `source_date` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A call without `site_type`, or a filter on a disallowed field, returns HTTP 400. A hyphen in a `lat` or `lng` range also returns HTTP 400; use two dots. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `site_type` | Required | No | No | Required. Kind of site, as normalized from the map layers. Values: CDC (165), a Community Diagnostic Centre; Surgical hub (120), a site for planned operations. Exact match only. Text match ignores case. |
| `name` | Optional | Yes | No | Site name as published on the NHS Fit for the Future map, with spaces trimmed. 285 unique names. Two South West Ambulatory Orthopaedic Centre (SWAOC) Elective Hub sites in Exeter are told apart by postcode in brackets: (EX2 7LE) and (EX2 5DS). One Community Diagnostic Centre is published only as NN1 1EE, which is also its postcode. Partial wildcards allowed, such as Amersham*, *Hospital* or *orthopaedic*. |
| `postcode` | Optional | Yes | No | Nearest postcode from postcodes.io reverse geocoding of the map pin, in upper case with one space, such as HP7 0JD. Never blank. 257 distinct values; some neighboring sites share a postcode. Partial wildcards allowed, such as LS* or EX2*. |
| `local_authority` | Optional | Yes | No | Local authority (ONS administrative district) from postcodes.io, in 183 values. Never blank. Official names are kept, including Herefordshire, County of, Kingston upon Hull, City of, Bristol, City of and St. Helens. Leeds has 5 sites (3 CDCs). Partial wildcards allowed, such as Leeds or *Yorkshire*. |
| `region` | Optional | Yes | No | English region from postcodes.io (former Government Office Region). Never blank. Values: South East (32 CDC, 17 hubs); North West (25, 17); Yorkshire and The Humber (19, 16); London (14, 21); West Midlands (16, 19); South West (21, 11); East of England (20, 9); East Midlands (14, 5); North East (4, 5). Use the full value, including The in Yorkshire and The Humber, or a wildcard such as Yorkshire*. |
| `icb_code` | Optional | No | No | Official 9-character GSS code for the NHS Integrated Care Board, from postcodes.io codes.icb, such as E54000054. Never blank. 36 codes, all starting E54. Exact match. |
| `icb_name` | Optional | Yes | No | Official Integrated Care Board name from postcodes.io, such as NHS West Yorkshire Integrated Care Board. Never blank. 36 names, one for each icb_code. Partial wildcards allowed, such as *West Yorkshire* or *Devon*. |
| `lat` | Optional | No | Yes | Latitude of the map pin in decimal degrees (WGS84), from 50.121713 to 55.184126. This is the published pin, not the postcode centre. Range uses two dots, such as lat=53.7..53.9. A hyphen returns 400. |
| `lng` | Optional | No | Yes | Longitude of the map pin in decimal degrees (WGS84), from -5.54271 to 1.728588. This is the published pin, not the postcode centre. Range uses two dots, such as lng=-1.7..-1.4. A hyphen returns 400. |
| `source` | Disallowed | No | No | NHS England, NHS Fit for the Future, in all 285 rows. Not filterable. |
| `source_date` | Disallowed | No | No | Date of the source page as text YYYY-MM-DD. 2026-10-15 in all 285 rows, the last modified date on the NHS Fit for the Future campaign page. Not filterable. |

## Response fields

`__id` is an integer. `lat` and `lng` are numbers. All other values are strings. No field is blank.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. The source map has no official stable site ID. |
| `site_type` | string | CDC or Surgical hub. |
| `name` | string | Site name as published. 285 unique names. |
| `postcode` | string | Nearest postcode to the map pin, with one space. |
| `local_authority` | string | Local authority from postcodes.io. 183 values. |
| `region` | string | English region from postcodes.io. 9 values. |
| `icb_code` | string | Integrated Care Board GSS code, such as E54000054. 36 values. |
| `icb_name` | string | Integrated Care Board name. 36 values. |
| `lat` | number | Latitude of the published map pin, in decimal degrees. |
| `lng` | number | Longitude of the published map pin, in decimal degrees. |
| `source` | string | NHS England, NHS Fit for the Future. |
| `source_date` | string | Source page date 2026-10-15. |

## Data notes

* The map points are the pins published on the campaign map, not the postcode centre. Postcode, local authority, region and Integrated Care Board fields come from the nearest postcode to each pin, found with postcodes.io (Office for National Statistics Postcode Directory). Every pin matched a postcode in England. No OpenStreetMap data was used.
* Hours and services are not included. Check with the provider, as those details can change between map updates.
* Two South West Ambulatory Orthopaedic Centre (SWAOC) Elective Hub sites in Exeter are told apart by postcode in the name: (EX2 7LE) and (EX2 5DS).
* Two pins look misplaced on the source map and were left as published. St Michael's Elective Surgical Hub (Royal Cornwall) shares the St Austell pin at PL25 4PF. Edgware Community Hospital Elective Surgical Hub geocodes to BN21 2UD in Eastbourne, in the South East.
* One CDC is published only as NN1 1EE, which is also its postcode in West Northamptonshire.
* Three name variants that sat on the same pin as another site were dropped, and the clearer published name at each pin was kept.

## Examples

Community Diagnostic Centres in a Leeds area box. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_nhs_cdcs_surgical_hubs' --data-urlencode 'site_type=CDC' --data-urlencode 'lat=53.7..53.9' --data-urlencode 'lng=-1.7..-1.4'
```

First record:

```json
[
  {
    "__id": 3,
    "site_type": "CDC",
    "name": "Armley Moor Health Centre CDC",
    "postcode": "LS12 3HD",
    "local_authority": "Leeds",
    "region": "Yorkshire and The Humber",
    "icb_code": "E54000054",
    "icb_name": "NHS West Yorkshire Integrated Care Board",
    "lat": 53.796228,
    "lng": -1.593909,
    "source": "NHS England, NHS Fit for the Future",
    "source_date": "2026-10-15"
  }
]
```

The other two records are Beeston Village Health Centre CDC (LS11 8AB) and Leeds CDC (LS14 6ZN).

Surgical hubs whose name includes orthopaedic. This call returns 20 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_nhs_cdcs_surgical_hubs' --data-urlencode 'site_type=Surgical hub' --data-urlencode 'name=*orthopaedic*'
```

The Amersham Hospital CDC. This call returns 1 record, postcode HP7 0JD:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_nhs_cdcs_surgical_hubs' --data-urlencode 'site_type=CDC' --data-urlencode 'name=Amersham*'
```

## Data source

[NHS Fit for the Future, NHS Services in Your Area](https://nhsfitforthefuture.campaign.gov.uk/nhs-services-in-your-area/), page last modified 15 October 2026. The list is refreshed monthly against the campaign map.

Contains public sector information licensed under the [Open Government Licence v3.0 (OGL v3)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Source: NHS England, NHS Fit for the Future (https://nhsfitforthefuture.campaign.gov.uk/nhs-services-in-your-area/), page last modified 15 October 2026.

Postcode, local authority, region and Integrated Care Board fields are from [postcodes.io](https://postcodes.io) (Office for National Statistics Postcode Directory). Contains OS data © Crown copyright and database right; Contains Royal Mail data © Royal Mail copyright and database right; Contains National Statistics data © Crown copyright and database right.

This list is not an official NHS directory and is not endorsed by NHS England. No NHS logo is used. Hours and services are not included; check with the provider, as those details can change between map updates.
