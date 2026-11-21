# UK Fulfilment Houses

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 600](https://img.shields.io/badge/records-600-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

Contains public sector information licensed under the Open Government Licence v3.0. Source: HM Revenue and Customs, [Fulfilment House Due Diligence Scheme registered businesses list](https://www.gov.uk/government/publications/fulfilment-house-due-diligence-scheme-registered-businesses-list), last updated 30 October 2026. Map points: postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right. Contains Royal Mail data Crown copyright and database right. Contains National Statistics data Crown copyright and database right.

The 600 UK businesses on the HMRC Fulfilment House Due Diligence Scheme list last updated 30 October 2026, one row per registered business, with name, FHDDS number, address, country and a postcode map point on 598 rows. Every call needs `country`, and `country=*` returns all 600.

This is not an official HMRC check, and it is not affiliated with or endorsed by HM Revenue and Customs. Registration status changes, so confirm a business on the [GOV.UK Fulfilment House Due Diligence Scheme registered businesses list](https://www.gov.uk/government/publications/fulfilment-house-due-diligence-scheme-registered-businesses-list) before you rely on a record.

Six rows are privacy trimmed: the four listings named as a person (Brian Duncan Dowie, Jane Hamlyn, Timothy Boon, and Susana Fiorillo, published here under the trading name DSI Fulfilment) and two company rows whose published address was a Flat (Andreev Services Ltd in Sutton, Ideal Logistics Ltd in London). Those rows keep only the name, town and postcode district. The street, county and full postcode are left out, `privacy_trimmed` is `yes`, and the map point is the postcode district centroid (`geoq=outcode`).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_fulfilment_houses` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per business. All matches are returned. |

## Query parameters

Required: 1 field (`country`). Optional: 16 fields. Disallowed: 1 field.

`country` is required. A call without it returns HTTP 400. Use `country=*` to return all 600 records, or a value such as `country=England`. Street, city, county, postcode, latitude and longitude are queried with brackets: `address[street]`, `address[city]`, `address[county]`, `address[postalcode]`, `location[latitude]`, `location[longitude]`. Partial filters accept `*`, such as `name=*Logistics*`, `address[city]=*mouth*` or `postcode_district=BT*`. `geoq`, `privacy_trimmed` and `source_as_of` are best used as exact matches; a star on `geoq` or `source_as_of` is read as a plain character, so it returns `[]`. All filters ignore case. `location[latitude]` and `location[longitude]` accept a range with two dots, lower number first, such as `location[latitude]=51..52`. A hyphen range such as `location[latitude]=51-52` returns HTTP 400. Filtering on a blank (`null`) value is not possible: a parameter with no value, such as `legal_name=`, is ignored.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `country` | Required | Yes | No | UK country, 4 values on every row (none is blank): England (568), Scotland (16), Wales (12), Northern Ireland (4). Taken from postcodes.io where a point was found, otherwise from the postcode area (BT is Northern Ireland, and a London address with no postcode is England). Every call must include `country`, and a call without it returns HTTP 400. Partial wildcards allowed, so `country=*` returns all 600 rows, and `*land` returns England, Scotland and Northern Ireland (588). Case ignored. |
| `name` | Optional | Yes | No | Business name as published, or the trading-as name when the row was listed as a person trading as a firm (DSI Fulfilment). Never blank. 599 distinct values: Elovate appears twice, at York and Winsford, with different FHDDS numbers. Partial wildcards allowed, such as `*Logistics*`. |
| `legal_name` | Optional | Yes | No | Registered name before a trading-as clause, where the source prints one. Blank (`null`) on the 4 privacy trimmed person rows. The two privacy trimmed company rows keep their company legal name. Partial wildcards allowed. |
| `trading_name` | Optional | Yes | No | Name after "trading as" in the source, when present. Present on 64 rows, including DSI Fulfilment; blank (`null`) on the rest. Partial wildcards allowed. |
| `fhdds_number` | Optional | Yes | No | FHDDS registration number as the list prints it, such as XTFH00000101187. Unique on 598 rows. One row is blank (`null`): Hemisphere Freight Services Ltd. One is printed with a doubled prefix, XKXKFH00000100795 (International Cargo Logistic Limited). Text. Partial wildcards allowed, such as `XTFH*` or `X*` (599). |
| `address[street]` | Optional | Yes | No | Street and building line, such as Field View, Castledon Road, Downham. Blank (`null`) on 14 rows: the 6 privacy trimmed rows and 8 listings that had no street line in the source. Partial wildcards allowed. |
| `address[city]` | Optional | Yes | No | Town or city, such as Billericay, London or Cupar. Never blank. Partial wildcards allowed, such as `*mouth*`. |
| `address[county]` | Optional | Yes | No | County as printed at the end of the address, when present, such as Leicestershire. Blank (`null`) on 411 rows, including the 6 privacy trimmed rows. Partial wildcards allowed. |
| `address[postalcode]` | Optional | Yes | No | Postcode as the list prints it, text, such as CM11 1LH. On the 6 privacy trimmed rows this is only the postcode district (KY14, W1F, BH10, BN6, SM1, N16). Blank (`null`) on Giraf Logistics (UK) Ltd, which has no postcode. A few published postcodes have typos (ME33 7AN, S053 4DP, C717 0HL, LA3 IRQ, CT12 5GI, S051 0HR). Partial wildcards allowed, such as `CM11*`. |
| `postcode_district` | Optional | Yes | No | Outward postcode (district), such as CM11 or BT17, present on 599 rows (null on Giraf Logistics (UK) Ltd). Text. Partial wildcards allowed, such as `BT*`. |
| `admin_district` | Optional | Yes | No | Local authority district from postcodes.io, such as Chelmsford or City of Edinburgh. Blank (`null`) on 12 rows with no full postcode match, including Merlin Fulfilment Ltd and Giraf Logistics (UK) Ltd. Partial wildcards allowed, such as `*Cardiff*`. |
| `region` | Optional | Yes | No | English region from postcodes.io, such as South East or East of England. Present on 549 rows. Blank (`null`) on all 16 Scotland, 12 Wales and 4 Northern Ireland rows, and on 19 England rows (outcode-only matches and the 2 rows with no point). Partial wildcards allowed, such as `*West*`. |
| `privacy_trimmed` | Optional | Yes | No | `yes` on the 6 privacy trimmed rows described above, `no` on the other 594. Use `privacy_trimmed=yes` or `no`. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees of the postcode (or a terminated postcode, or an outcode centroid), present on 598 rows, null on 2 (Giraf Logistics (UK) Ltd has no postcode; Merlin Fulfilment Ltd has the published typo S051 0HR, which is not a real postcode and was not forced to SO51, because that would place the site in Hampshire instead of Bury St Edmunds). Range filter with two dots, lower number first, such as `51..52`. A hyphen range returns 400. |
| `location[longitude]` | Optional | No | Yes | Longitude of the same point, west (negative) or east (positive) of Greenwich, null on the same 2 rows. Range filter with two dots, such as `-4..-2`. |
| `geoq` | Optional | No | No | How the point was matched, exact value: `postcode` (580), `terminated_postcode` (10, a retired postcode still in postcodes.io) or `outcode` (8: the 6 privacy trimmed rows plus LE10 8ER and NG2 3RD, which are not in the current directory). Null on 2 rows with no point. Filtering on null is not possible. Exact match, such as `geoq=postcode`. |
| `source_as_of` | Optional | No | No | Date the GOV.UK list was last updated, `2026-10-30`, the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: HMRC FHDDS list under the Open Government Licence v3.0, with the postcodes.io and ONS or OS credit for map points. Returned in output only. |

## Response fields

`location.latitude` and `location.longitude` are numbers, or `null`. `__id` is an integer. All other values are strings. Blank cells are returned as `null`. Street, city, county and postcode are nested under `address`; latitude and longitude are nested under `location`. The API returns a generated `__id` that is unique on every row and is the stable record key (it will change if the table is reloaded). `fhdds_number` is not a complete unique key: one row is blank and one is printed with a doubled prefix.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `name` | string | Business name as published, or the trading-as name on a privacy trimmed person row. |
| `legal_name` | string | Registered name before a trading-as clause, or `null` on the 4 privacy trimmed person rows. |
| `trading_name` | string | Name after "trading as", or `null` when the source prints none. |
| `fhdds_number` | string | FHDDS registration number as the list prints it, or `null` on one row. |
| `address` | object | `street`, `city`, `county` and `postalcode` as published or, on privacy trimmed rows, only city and the postcode district. |
| `postcode_district` | string | Outward postcode, or `null` when there is no postcode. |
| `country` | string | `England`, `Scotland`, `Wales` or `Northern Ireland`. |
| `admin_district` | string | Local authority district from postcodes.io, or `null`. |
| `region` | string | English region from postcodes.io, or `null`. |
| `location` | object | `latitude` and `longitude` of the postcode, terminated postcode or outcode point, or both `null` when there is no point. |
| `geoq` | string | `postcode`, `terminated_postcode`, `outcode`, or `null` when there is no point. |
| `privacy_trimmed` | string | `yes` or `no`. |
| `source` | string | Credit line for the HMRC list and the postcodes.io or ONS credit for map points. |
| `source_as_of` | string | Date the GOV.UK list was last updated, `2026-10-30`. |

## Data notes

* The records are the 600 businesses on the HMRC Fulfilment House Due Diligence Scheme registered businesses list on GOV.UK, last updated 30 October 2026 (8 HTML pages). The extract was taken on 21 November 2026.
* Six rows are privacy trimmed, as described above: 4 person-named rows and 2 company rows whose published address was a Flat. They keep name, town and postcode district only, and they are geocoded at outcode level.
* Data issues kept as published: Hemisphere Freight Services Ltd has no FHDDS number; International Cargo Logistic Limited is printed as XKXKFH00000100795; several postcodes have typos. LE10 8ER and NG2 3RD are not in the current postcode directory, so they use the district centroid. S051 0HR (Merlin Fulfilment Ltd, Saxham Business Park, Bury St Edmunds) is not a real postcode and was not mapped, because treating it as SO51 would put it in Hampshire. Giraf Logistics (UK) Ltd has no postcode and no point.
* Map points come from postcodes.io (ONS Postcode Directory), not from OpenStreetMap. 580 current postcodes, 10 terminated postcodes, 8 outcode centroids, and 2 rows with no point.
* This is a snapshot of the list as last updated on 30 October 2026, the `source_as_of` value. Check the GOV.UK list before relying on a record, as firms are added and removed.

## Examples

123PL Limited in Billericay, looked up by its FHDDS number. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_fulfilment_houses' --data-urlencode 'country=*' --data-urlencode 'fhdds_number=XTFH00000101187'
```

```json
[
  {
    "__id": 601,
    "name": "123PL Limited",
    "legal_name": "123PL Limited",
    "trading_name": null,
    "fhdds_number": "XTFH00000101187",
    "address": {
      "street": "Field View, Castledon Road, Downham",
      "city": "Billericay",
      "county": null,
      "postalcode": "CM11 1LH"
    },
    "postcode_district": "CM11",
    "country": "England",
    "admin_district": "Chelmsford",
    "region": "East of England",
    "location": {
      "latitude": 51.624264,
      "longitude": 0.500566
    },
    "geoq": "postcode",
    "privacy_trimmed": "no",
    "source": "HM Revenue and Customs, Fulfilment House Due Diligence Scheme registered businesses list, https://www.gov.uk/government/publications/fulfilment-house-due-diligence-scheme-registered-businesses-list. Contains public sector information licensed under the Open Government Licence v3.0. Map points from postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right.",
    "source_as_of": "2026-10-30"
  }
]
```

DSI Fulfilment, one of the 6 privacy trimmed rows: the source named a person trading as that firm, so the row keeps the trading name, town and postcode district only, with an outcode map point. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_fulfilment_houses' --data-urlencode 'country=*' --data-urlencode 'name=DSI Fulfilment'
```

```json
[
  {
    "__id": 1097,
    "name": "DSI Fulfilment",
    "legal_name": null,
    "trading_name": "DSI Fulfilment",
    "fhdds_number": "XBFH00000100738",
    "address": {
      "street": null,
      "city": "Bournemouth",
      "county": null,
      "postalcode": "BH10"
    },
    "postcode_district": "BH10",
    "country": "England",
    "admin_district": "Bournemouth, Christchurch and Poole",
    "region": null,
    "location": {
      "latitude": 50.759541,
      "longitude": -1.892783
    },
    "geoq": "outcode",
    "privacy_trimmed": "yes",
    "source": "HM Revenue and Customs, Fulfilment House Due Diligence Scheme registered businesses list, https://www.gov.uk/government/publications/fulfilment-house-due-diligence-scheme-registered-businesses-list. Contains public sector information licensed under the Open Government Licence v3.0. Map points from postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right.",
    "source_as_of": "2026-10-30"
  }
]
```

Merlin Fulfilment Ltd in Bury St Edmunds, whose published postcode S051 0HR is not a real postcode, so there is no map point. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_fulfilment_houses' --data-urlencode 'country=England' --data-urlencode 'name=Merlin Fulfilment Ltd'
```

```json
[
  {
    "__id": 944,
    "name": "Merlin Fulfilment Ltd",
    "legal_name": "Merlin Fulfilment Ltd",
    "trading_name": null,
    "fhdds_number": "XDFH00000100667",
    "address": {
      "street": "Unit 1 Saxham Business Park",
      "city": "Bury St Edmunds",
      "county": "Suffolk",
      "postalcode": "S051 0HR"
    },
    "postcode_district": "S051",
    "country": "England",
    "admin_district": null,
    "region": null,
    "location": {
      "latitude": null,
      "longitude": null
    },
    "geoq": null,
    "privacy_trimmed": "no",
    "source": "HM Revenue and Customs, Fulfilment House Due Diligence Scheme registered businesses list, https://www.gov.uk/government/publications/fulfilment-house-due-diligence-scheme-registered-businesses-list. Contains public sector information licensed under the Open Government Licence v3.0. Map points from postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right.",
    "source_as_of": "2026-10-30"
  }
]
```

## Data source

Contains public sector information licensed under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Source: HM Revenue and Customs, [Fulfilment House Due Diligence Scheme registered businesses list](https://www.gov.uk/government/publications/fulfilment-house-due-diligence-scheme-registered-businesses-list), last updated 30 October 2026. Map points from postcodes.io, which uses the ONS Postcode Directory: Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right. This data was changed from the source as described in the notes above. The HMRC and GOV.UK logos are not used, and HMRC does not endorse this API.
