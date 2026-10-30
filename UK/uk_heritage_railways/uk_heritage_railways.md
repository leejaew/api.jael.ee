# UK Heritage Railways

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 166](https://img.shields.io/badge/records-166-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license facts, CC0 and OGL v3](https://img.shields.io/badge/license-facts%20%2B%20CC0%20%2B%20OGL%20v3-green)

Heritage railways, tramways, cliff lifts and railway museums in the United Kingdom and the Isle of Man: 166 sites from the Heritage Railway Association map of members open to the public, as of October 2026. Each row has the site name, type, nation, local council area, town, postcode, track gauge, website, map point and Wikidata ID. Filter by the required `nation`, then by type, gauge, name, council, town, postcode or a coordinate range.

Rows by nation: England 124, Scotland 17, Wales 16, Isle of Man 6 and Northern Ireland 3. The Isle of Man is a Crown Dependency, not part of the UK. Its 6 sites are included because they are in the same HRA member list. Rows by type: railway 122, museum 30, cliff_railway 7 and tramway 7.

This is a list of sites, not of opening times or status. Many sites open only on some days or in some seasons, so check with the site before you travel.

> **Use of this data.** Site facts come from the Heritage Railway Association member map. No HRA text was copied, and this API is not affiliated with or endorsed by the Heritage Railway Association or any railway. Gauges and IDs come from Wikidata, and council areas from postcodes.io.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_heritage_railways` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`nation`). Optional: 9 fields. Disallowed: 4 fields.

`nation` must be in every call. Use `nation=*` to list all 166 sites. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `nation` | Required | Yes | No | Nation: `England` (124), `Scotland` (17), `Wales` (16), `Isle of Man` (6) or `Northern Ireland` (3). The Isle of Man is a Crown Dependency, not part of the UK, and is listed since it is in the same member list. Wildcards work, so `nation=*` lists all 166. |
| `name` | Optional | Yes | No | Name of the railway, tramway, lift or museum as listed by the Heritage Railway Association, such as `*Valley*` (21). One row per name. |
| `type` | Optional | Yes | No | Kind of site: `railway` (122), `museum` (30), `cliff_railway` (7) or `tramway` (7). A museum is a museum or steam center. `*tram*` matches tramways only. |
| `local_authority` | Optional | Yes | No | Local council area of the site postcode, such as `Gwynedd` (7) or `*Devon*` (3). Isle of Man rows say `Isle of Man`. |
| `town` | Optional | Yes | No | Town or village of the site from its address, such as `Porthmadog` (2). |
| `postcode` | Optional | Yes | No | Site postcode, such as `LL49 9NF` or `TN*` (6). Blank for 5 rows with no valid postcode. |
| `gauge` | Optional | Yes | No | Track gauge class: `standard` (73), `narrow` (51) or `broad` (1). Standard is 1,435 mm, narrow is less and broad is more. Blank for 41 rows, mostly museums, tramways and lifts. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 50.13401 to 57.458. Example: `lat=51..52` (39). |
| `long` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from -7.64345 to 1.25521. Example: `long=-4..-3` (28). |
| `wikidata_id` | Optional | Yes | No | Wikidata item ID, such as `Q4667224`. Blank for 5 rows with no clear match. |
| `gauge_mm` | Disallowed | No | No | Track gauge in millimeters where Wikidata gives one clear value, such as `597` or `1435`. Blank for 123 rows. |
| `website` | Disallowed | No | No | Official website (https), checked October 2026. Blank for 2 rows. |
| `source` | Disallowed | No | No | Sources of the row. |
| `source_as_of` | Disallowed | No | No | Date the sources were read, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Name of the railway, tramway, lift or museum. |
| `type` | string | `railway`, `museum`, `cliff_railway` or `tramway`. |
| `nation` | string | England, Scotland, Wales, Northern Ireland or Isle of Man. |
| `local_authority` | string | Local council area of the site postcode. |
| `town` | string | Town or village of the site. |
| `postcode` | string | Site postcode. Null for 5 rows. |
| `gauge` | string | `standard`, `narrow` or `broad`. Null for 41 rows. |
| `gauge_mm` | string | Track gauge in millimeters, as text. Null for 123 rows. |
| `website` | string | Official website (https). Null for 2 rows. |
| `lat` | number | Latitude in decimal degrees (WGS 84), 5 decimals. |
| `long` | number | Longitude in decimal degrees (WGS 84), 5 decimals. |
| `wikidata_id` | string | Wikidata item ID. Null for 5 rows. |
| `source` | string | Sources of the row. |
| `source_as_of` | string | Date the sources were read. |

Notes:

* This is a list of sites, not of opening status. Scarborough Spa Cliff Lift has been closed for repairs since August 21, 2026, and is still listed. Check with the site before you travel.
* Map points come from the HRA member map and mark the main site or station. A railway with many stations has one point.
* `local_authority` comes from the postcode. Isle of Man rows use `Isle of Man`. The 5 rows with no valid postcode (Bala Lake Railway, Douglas Bay Horse Tramway, Manx Electric Railway, Snaefell Mountain Railway and Volks Electric Railway) still have a council area and town.
* The only `broad` gauge site is Downpatrick & County Down Railway in Northern Ireland, on the Irish gauge of 1,600 mm.
* Names keep their punctuation, such as `Bo'ness & Kinneil Railway`. Use a wildcard such as `name=bo*` if quoting is hard.

## Examples

A wildcard on type. Tramways in Wales. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_heritage_railways" --data-urlencode "nation=Wales" --data-urlencode "type=*tram*"
```

```json
[
  {
    "__id": 58,
    "name": "Great Orme Tramway",
    "type": "tramway",
    "nation": "Wales",
    "local_authority": "Conwy",
    "town": "Llandudno",
    "postcode": "LL30 2NB",
    "gauge": "narrow",
    "gauge_mm": "1067",
    "website": "https://greatormetramway.co.uk/",
    "lat": 53.32723,
    "long": -3.83559,
    "wikidata_id": "Q1412842",
    "source": "Heritage Railway Association member map; Wikidata; postcodes.io",
    "source_as_of": "2026-10-30"
  }
]
```

Three filters together. Narrow gauge railways in Wales. This call returns 8 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_heritage_railways" --data-urlencode "nation=Wales" --data-urlencode "type=railway" --data-urlencode "gauge=narrow" | jq -r '.[] | [.name, .town, .gauge_mm] | @tsv'
```

```text
Bala Lake Railway	Llanuwchllyn	
Corris Railway	Corris	686
Fairbourne Railway	Fairbourne	
Ffestiniog & Welsh Highland Railways	Porthmadog	597
Llanberis Lake Railway	Llanberis	597
Talyllyn Railway	Tywyn	686
Welsh Highland Heritage Railway	Porthmadog	
Welshpool & Llanfair Light Railway	Llanfair Caereinion	762
```

A latitude and longitude range. Sites in England in a box over south Devon. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_heritage_railways" --data-urlencode "nation=England" --data-urlencode "lat=50..50.7" --data-urlencode "long=-4..-3" | jq -r '.[] | [.name, .type, .town, .postcode, .lat, .long] | @tsv'
```

```text
Babbacombe Cliff Railway	cliff_railway	Torquay	TQ1 3LF	50.48155	-3.518
Dartmouth Steam Railway	railway	Paignton	TQ4 6AF	50.43474	-3.56486
South Devon Railway	railway	Buckfastleigh	TQ11 0DZ	50.48282	-3.76893
```

## Data source

The list of sites, their names, types, addresses, websites and map points comes from the Heritage Railway Association map of member sites open to the public, read on 2026-10-30. Only facts were taken. No HRA descriptions, photos or other text were copied. Each site was matched to Wikidata for its item ID and, where Wikidata gives one clear value, its track gauge. Council areas come from postcodes.io lookups of each site postcode. No OpenStreetMap data is used. Type, gauge class and town are set by us, and the data can go out of date. Check with the site before you travel.

Credit lines:

* Site facts from the Heritage Railway Association member map (https://www.hra.uk.com/directory), facts only. Not affiliated with or endorsed by the Heritage Railway Association or any member railway.
* Wikidata (https://www.wikidata.org), CC0.
* Postcode lookups by postcodes.io (https://postcodes.io):
  * Contains OS data © Crown copyright and database right.
  * Contains Royal Mail data © Royal Mail copyright and database right.
  * Source: Office for National Statistics, licensed under the Open Government Licence v3.0.

[Back to the API list](../../README.md)
