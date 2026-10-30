# UK Freeports

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 68](https://img.shields.io/badge/records-68-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license OGL v3 and ODbL](https://img.shields.io/badge/license-OGL%20v3%20%2B%20ODbL-green)

Every UK Freeport site: 68 sites across all 12 Freeports in England, Scotland and Wales, as of October 2026. There are 47 tax sites and 21 customs sites, taken from the HMRC Freeport site maps and the regulations and orders that designated them. Each row has the Freeport, nation, site type and name, customs site number, local authority, designation date, statutory instrument, customs operator, an approximate map point and links to the map and the legislation. Filter by the required `nation`, then by Freeport, site type, site name, date, legislation or a coordinate range.

Rows by nation: England 45, Wales 12 and Scotland 11. Rows by site type: Tax site 47 (where Freeport tax reliefs apply) and Customs site 21 (where customs duty suspension applies).

Sites per Freeport:

| Freeport | Nation | Tax sites | Customs sites | Total |
|---|---|---|---|---|
| Anglesey Freeport | Wales | 5 | 0 | 5 |
| Celtic Freeport | Wales | 6 | 1 | 7 |
| East Midlands Freeport | England | 3 | 1 | 4 |
| Forth Green Freeport | Scotland | 4 | 1 | 5 |
| Freeport East | England | 3 | 2 | 5 |
| Humber Freeport | England | 3 | 1 | 4 |
| Inverness and Cromarty Firth Green Freeport | Scotland | 5 | 1 | 6 |
| Liverpool City Region Freeport | England | 3 | 7 | 10 |
| Plymouth and South Devon Freeport | England | 3 | 1 | 4 |
| Solent Freeport | England | 6 | 2 | 8 |
| Teesside Freeport | England | 3 | 1 | 4 |
| Thames Freeport | England | 3 | 3 | 6 |

> **Use of this data.** Map points are approximate and do not show legal site boundaries. The HMRC site maps and the legislation define each site. Not affiliated with or endorsed by HMRC or the Department for Business and Trade (DBT).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_freeports` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`nation`). Optional: 9 fields. Disallowed: 7 fields.

`nation` must be in every call. Use `nation=*` to list all 68 sites. Disallowed means the field is returned in the output but cannot be used as a filter.

Dates are text in the form `YYYY-MM-DD`, so filter them with a prefix wildcard, such as `2024*` for a year. Only `lat` and `long` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `nation` | Required | Yes | No | UK nation of the Freeport: `England` (45), `Wales` (12) or `Scotland` (11). Wildcards work, so `nation=*` lists all 68. |
| `freeport_name` | Optional | Yes | No | Name of the Freeport, such as `Liverpool City Region Freeport` (10), `Solent Freeport` (8) or `*Green Freeport*` (11). 12 Freeports. |
| `site_type` | Optional | Yes | No | Kind of site: `Tax site` (47) or `Customs site` (21). |
| `site_name` | Optional | Yes | No | Site name as on the HMRC map or in the order, such as `Tilbury` (1) or `*Customs Site*` (21). Customs sites are named `Customs Site No. 1` and so on. |
| `local_authority` | Optional | Yes | No | Local authority district at the map point, such as `Highland` (6) or `Thurrock` (5). 32 are used. Blank for the 3 sites with no point. |
| `designation_date` | Optional | Yes | No | For tax sites, the day the regulations came into force. For customs sites, the day the order was made. From 2021-11-19 to 2026-09-09, such as `2024*` (21) or `2026*` (3). |
| `legislation` | Optional | Yes | No | Statutory instrument that designated the site, such as `SI 2021/1194` (3) or `SI 2024*` (24). |
| `lat` | Optional | No | Yes | Approximate latitude of the site in decimal degrees (WGS 84), from 50.36621 to 57.70259. Blank for 3 sites. Example: `lat=51..52` (15). |
| `long` | Optional | No | Yes | Approximate longitude of the site in decimal degrees (WGS 84), from -5.01641 to 1.31298. Blank for 3 sites. Example: `long=-3..-2` (9). |
| `coord_note` | Optional | Yes | No | How the map point was placed, such as `*OSM*` (51), `*postcode*` (10), `*British National Grid*` (4) or `not placed*` (3). See the notes. |
| `customs_site_no` | Disallowed | No | No | Number of the customs site within its Freeport, 1 to 7, as text. Blank for the 47 tax sites. |
| `operator` | Disallowed | No | No | Customs site operator named in the designation order, such as `Forth Ports Limited`. Blank for tax sites. |
| `legislation_url` | Disallowed | No | No | Link to the statutory instrument on legislation.gov.uk. |
| `map_url` | Disallowed | No | No | Link to the HMRC site map PDF on GOV.UK. |
| `source_url` | Disallowed | No | No | GOV.UK page that publishes the site maps. |
| `source` | Disallowed | No | No | Source of the row. |
| `source_as_of` | Disallowed | No | No | Date the sources were read, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `freeport_name` | string | Name of the Freeport. |
| `nation` | string | `England`, `Wales` or `Scotland`. |
| `site_type` | string | `Tax site` or `Customs site`. |
| `site_name` | string | Site name as on the HMRC map or in the order. |
| `customs_site_no` | string | Customs site number, as text. Null for tax sites. |
| `local_authority` | string | Local authority district at the map point. Null for 3 rows. |
| `designation_date` | string | Designation date, `YYYY-MM-DD`. |
| `legislation` | string | Statutory instrument, such as `SI 2024/1035`. |
| `legislation_url` | string | Link to the statutory instrument on legislation.gov.uk. |
| `operator` | string | Customs site operator. Null for tax sites. |
| `lat` | number | Approximate latitude (WGS 84). Null for 3 rows. |
| `long` | number | Approximate longitude (WGS 84). Null for 3 rows. |
| `coord_note` | string | How the map point was placed. |
| `map_url` | string | Link to the HMRC site map PDF. |
| `source_url` | string | GOV.UK page that publishes the site maps. |
| `source` | string | Source of the row. |
| `source_as_of` | string | Date the sources were read. |

Notes:

* No source publishes coordinates, so every point is approximate and starts with `approximate:` in `coord_note`. Points mark a place in or next to the site, not its legal boundary. Many sites have several parcels, and only one point is given.
  * 51 points come from a named OpenStreetMap feature, such as a port, estate, business park or the nearest named place, checked against the HMRC map.
  * 10 come from a postcode printed on the customs site map, or from the operator office postcode where the map gives none.
  * 4 come from the center of a British National Grid box or frame printed on the site map.
  * 3 sites are not placed, so `lat`, `long` and `local_authority` are blank: Celtic Freeport Milford Haven (Puma), Humber Freeport Customs Site No. 1 and Teesside Freeport Customs Site No. 1.
* `local_authority` is a postcodes.io lookup of the map point. Liverpool City Region Customs Sites No. 5 and 6 read Liverpool, though the map labels the area Bootle. Humber Hull East reads East Riding of Yorkshire because the point is at Saltend.
* Thames Freeport Customs Site No. 1 shows DP World Logistics UK Ltd, the operator after SI 2024/1106 (from November 10, 2024). The first operator was P&O Ferrymasters.
* Some sites share a point, such as Freeport East Felixstowe and its Customs Site No. 1.

## Examples

Customs sites in Wales, shown as a full row. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_freeports" --data-urlencode "nation=Wales" --data-urlencode "site_type=Customs site"
```

```json
[
  {
    "__id": 12,
    "freeport_name": "Celtic Freeport",
    "nation": "Wales",
    "site_type": "Customs site",
    "site_name": "Customs Site No. 1",
    "customs_site_no": "1",
    "local_authority": "Neath Port Talbot",
    "designation_date": "2026-01-29",
    "legislation": "SI 2026/80",
    "legislation_url": "https://www.legislation.gov.uk/uksi/2026/80/contents/made",
    "operator": "Dyfed Steels Limited",
    "lat": 51.58624,
    "long": -3.79607,
    "coord_note": "approximate: postcode SA13 1RH (Talbot Wharf, Port Talbot) printed on the site map",
    "map_url": "https://assets.publishing.service.gov.uk/media/6968eb7d50a5ef67d031a7e4/Celtic-Freeport-Customs-Site-No.1.pdf",
    "source_url": "https://www.gov.uk/government/publications/map-of-celtic-freeport-customs-site",
    "source": "HMRC Freeport site maps (GOV.UK); designation regulations and orders (legislation.gov.uk)",
    "source_as_of": "2026-10-30"
  }
]
```

Wildcards on two fields. The customs sites of the Liverpool City Region Freeport. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_freeports" --data-urlencode "nation=England" --data-urlencode "site_type=Customs*" --data-urlencode "freeport_name=Liverpool*" | jq -r '.[] | [.site_name, .local_authority, .designation_date, .legislation, .operator] | @tsv'
```

```text
Customs Site No. 1	St. Helens	2022-12-07	SI 2022/1294	SSO International Forwarding Limited
Customs Site No. 2	Knowsley	2023-09-14	SI 2023/1000	Applied Nutrition Limited
Customs Site No. 3	Knowsley	2023-12-18	SI 2023/1401	Colloids Limited
Customs Site No. 4	Halton	2024-11-05	SI 2024/1103	SSO International Forwarding Limited
Customs Site No. 5	Liverpool	2024-11-05	SI 2024/1100	Wild Thang Limited
Customs Site No. 6	Liverpool	2024-11-05	SI 2024/1102	Wild Thang Limited
Customs Site No. 7	Warrington	2025-04-24	SI 2025/511	John K Philips Group Limited
```

A latitude and longitude range. Sites in a box over the Thames estuary and the Suffolk and Essex coast. This call returns 9 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_freeports" --data-urlencode "nation=*" --data-urlencode "lat=51.4..52" --data-urlencode "long=0..1.5" | jq -r '.[] | [.freeport_name, .site_type, .site_name, .local_authority, .lat, .long] | @tsv'
```

```text
Freeport East	Tax site	Felixstowe	East Suffolk	51.9557	1.31298
Freeport East	Tax site	Harwich	Tendring	51.94567	1.25404
Freeport East	Customs site	Customs Site No. 1	East Suffolk	51.9557	1.31298
Thames Freeport	Tax site	Dagenham	Barking and Dagenham	51.51852	0.15544
Thames Freeport	Tax site	London Gateway	Thurrock	51.50483	0.4924
Thames Freeport	Tax site	Tilbury	Thurrock	51.46147	0.34375
Thames Freeport	Customs site	Customs Site No. 1	Thurrock	51.51266	0.48117
Thames Freeport	Customs site	Customs Site No. 2	Thurrock	51.51715	0.47969
Thames Freeport	Customs site	Customs Site No. 3	Thurrock	51.51753	0.48672
```

## Data source

Freeports, sites, site names, customs site numbers and map links come from the HMRC Freeport site maps on GOV.UK. Designation dates, statutory instruments and customs operators come from the designation regulations and orders on legislation.gov.uk. All were read on 2026-10-30. Map points come from OpenStreetMap features, postcodes printed on the maps (looked up with postcodes.io) or British National Grid references printed on the maps. Local authorities come from postcodes.io lookups of each point. Points and `coord_note` are set by us, and the data can go out of date. Check the HMRC site map and the legislation for the legal extent of a site.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0 (https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), from HM Revenue and Customs (HMRC) and the Department for Business and Trade (DBT).
* Legislation from legislation.gov.uk (https://www.legislation.gov.uk), Crown copyright, under the Open Government Licence v3.0.
* Some map points © OpenStreetMap contributors, under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Postcode and local authority lookups by postcodes.io (https://postcodes.io):
  * Contains OS data © Crown copyright and database right.
  * Contains Royal Mail data © Royal Mail copyright and database right.
  * Source: Office for National Statistics, licensed under the Open Government Licence v3.0.
* Not affiliated with or endorsed by HMRC or the Department for Business and Trade.

[Back to the API list](../../README.md)
