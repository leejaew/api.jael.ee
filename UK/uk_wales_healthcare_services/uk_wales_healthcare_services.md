# UK Wales Healthcare Services

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,674](https://img.shields.io/badge/records-1%2C674-lightgrey) ![updated 2026-11-21](https://img.shields.io/badge/updated-2026--11--21-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

Contains public sector information licensed under the Open Government Licence v3.0. Source: Healthcare Inspectorate Wales, [service directory](https://www.hiw.org.uk/), extract of 21 November 2026. Copyright and licence: [HIW Crown copyright notice](https://www.hiw.org.uk/ccrown-copyright-2026). The HIW logo is not used. Map points: postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right. Contains Royal Mail data Crown copyright and database right. Contains National Statistics data Crown copyright and database right.

The 1,674 healthcare services in Wales on the Healthcare Inspectorate Wales directory as of 21 November 2026, with type, address, sector, last inspection date and a postcode map point on every row (648 GP surgeries, 574 dental practices, 236 clinics, 193 hospitals, 12 medical agencies with no physical premises and 11 hospices). Every call needs `establishment_type`, and `establishment_type=*` returns all 1,674.

This API is not affiliated with or endorsed by Healthcare Inspectorate Wales, and it is not an official source of registration status. The directory is a snapshot, so check the [HIW website](https://www.hiw.org.uk/) before relying on a record.

Twelve rows are medical agencies with no physical premises. They are listed with `establishment_type` `Medical agencies (with no physical premises)` and `has_premises=no`. They still have a registered address or PO Box. Bracketed doctor names such as (Dr V Jayaraman) were removed from 6 service names. Practice titles that themselves start with Dr, such as Dr Anwar's Surgery, were kept as published, because they are the registered service names.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_wales_healthcare_services` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per service. All matches are returned. |

## Query parameters

Required: 1 field (`establishment_type`). Optional: 15 fields. Disallowed: 1 field.

`establishment_type` is required. A call without it returns HTTP 400. Use `establishment_type=*` to return all 1,674 records, or a value such as `establishment_type=GP Surgeries`. Street, city, postcode, latitude and longitude are queried with brackets: `address[street]`, `address[city]`, `address[postalcode]`, `location[latitude]`, `location[longitude]`. Partial filters accept `*`, such as `name=*Dental*`, `services=*Laser*`, `address[postalcode]=CF*` or `last_inspection=2026*`. `geoq`, `has_premises` and `source_as_of` are best used as exact matches; a star on `geoq` or `source_as_of` is read as a plain character, so it returns `[]`. All filters ignore case. `location[latitude]` and `location[longitude]` accept a range with two dots, lower number first, such as `location[latitude]=51.4..51.6`. A hyphen range such as `location[latitude]=51-52` returns HTTP 400. Filtering on a blank (`null`) value is not possible: a parameter with no value is ignored.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `establishment_type` | Required | Yes | No | Kind of service, 6 values on every row (none is blank): GP Surgeries (648), Dental Practices (574, including Goodwin Partnership DBS Ltd, whose source type was blank and was taken from services=Dental), Clinics (236), Hospitals (193), Medical agencies (with no physical premises) (12), Hospices (11). Every call must include `establishment_type`, and a call without it returns HTTP 400. Partial wildcards allowed, so `establishment_type=*` returns all 1,674 rows, and `*Hospital*` matches Hospitals. Case ignored. |
| `name` | Optional | Yes | No | Service name as Healthcare Inspectorate Wales prints it, after two cleanups: a bracketed doctor name such as (Dr V Jayaraman) was removed, and a trailing de-registered date in brackets was moved to the `deregistered` field. Never blank. 1,601 distinct values; some names repeat (for example The Health Centre appears 11 times). Partial wildcards allowed, such as `*Dental*` or `*Cardiff*`. |
| `services` | Optional | Yes | No | Services provided as the directory prints them, such as GP services, Dental, or Laser - non-surgical (e.g. hair or tattoo removal). Present on 1,508 rows; blank (`null`) on 166. Partial wildcards allowed, such as `*Laser*` or `*Mental Health*`. |
| `sector` | Optional | Yes | No | `NHS` (831), `Private/Independent` (518), or `NHS, Private/Independent` (305). Blank (`null`) on 20 rows. Partial wildcards allowed, such as `*Private*` (823). |
| `address[street]` | Optional | Yes | No | First address line, such as Unit 2 Liberty Stadium Landore or Abbey Road. Present on 1,652 rows; blank (`null`) on 22, including Weight Loss Wales. Some agencies use a street or a PO Box. Partial wildcards allowed. |
| `address[city]` | Optional | Yes | No | Second address line, usually the town, such as Cardiff, Swansea or Gwynedd. Present on 1,633 rows; the other 41 are `null` (some agencies and a few services have only a street and postcode). Partial wildcards allowed, such as `*Swansea*`. |
| `address[postalcode]` | Optional | Yes | No | Postcode as text, spaced as published after cleaning (SA12FA becomes SA1 2FA). Present on every row. Two published typographical errors were kept (LL5 1TF, CF25 5LJ) and were geocoded as LL15 1TF and CF35 5LJ. Partial wildcards allowed, such as `CF*` or `LL55*`. |
| `local_authority` | Optional | Yes | No | Welsh principal area from postcodes.io, such as Cardiff or Gwynedd. Present on 1,656 rows. Blank (`null`) on 18 rows whose postcode is a terminated postcode and did not return an administrative district. Partial wildcards allowed, such as `*Gwynedd*`. |
| `phone` | Optional | Yes | No | Telephone number as text, after a leading apostrophe (an Excel artefact on 1,311 rows) and values of N/A were removed. Present on 1,310 rows; the others are `null`. No personal mobiles were identified. Partial wildcards allowed, such as `029*`. |
| `last_inspection` | Optional | Yes | No | Date of the last inspection as text, YYYY-MM-DD, from 2016-12-02 to 2026-11-19, present on 985 rows (`null` on the rest). Converted from the directory DD/MM/YYYY. Partial wildcards allowed, such as `2026*`. |
| `deregistered` | Optional | Yes | No | Date the service is marked de-registered in the name, as text YYYY-MM-DD, present on 36 rows (`null` otherwise). The note was removed from the name, including one Welsh note (wedi'i ddat-gofrestru). Partial wildcards allowed, such as `deregistered=*` for every de-registered row. |
| `has_premises` | Optional | Yes | No | `no` on the 12 Medical agencies (with no physical premises); `yes` on the other 1,662. The 12 still have a registered address or PO Box, but the directory says they have no physical premises. Use `has_premises=no` or `yes`. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees of the postcode (or a terminated postcode, or an outcode centroid), present on all 1,674 rows. Range filter with two dots, lower number first, such as `51.4..51.6`. A hyphen range returns 400. |
| `location[longitude]` | Optional | No | Yes | Longitude of the same point, all west of Greenwich (negative). Range filter with two dots, such as `-4..-3`. |
| `geoq` | Optional | No | No | How the point was matched, exact value: `postcode` (1,644, including 2 typo fixes used only for the match), `terminated_postcode` (18) or `outcode` (12). Never blank. Exact match, such as `geoq=postcode`. |
| `source_as_of` | Optional | No | No | Date this extract was taken from the live HIW directory, `2026-11-21`, the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: Healthcare Inspectorate Wales service directory under the Open Government Licence v3.0, with the postcodes.io and ONS or OS credit for map points. The HIW logo is not used. Returned in output only. |

## Response fields

`location.latitude` and `location.longitude` are numbers. `__id` is an integer. All other values are strings. Blank cells are returned as `null`. Street, city and postcode are nested under `address`; latitude and longitude are nested under `location`. The directory has no official service ID, and names and addresses can repeat, so the API returns a generated `__id` that is unique on every row and is the stable record key (it will change if the table is reloaded).

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID returned by the API, present on every row. This is the stable record key. |
| `name` | string | Service name, after bracketed doctor names and de-registered dates were removed. |
| `establishment_type` | string | `GP Surgeries`, `Dental Practices`, `Clinics`, `Hospitals`, `Medical agencies (with no physical premises)` or `Hospices`. |
| `services` | string | Services as the directory prints them, or `null`. |
| `sector` | string | `NHS`, `Private/Independent`, `NHS, Private/Independent`, or `null`. |
| `address` | object | `street`, `city` and `postalcode`. Street or city may be `null`. |
| `local_authority` | string | Welsh principal area from postcodes.io, or `null`. |
| `phone` | string | Telephone number, or `null`. |
| `last_inspection` | string | Last inspection date, YYYY-MM-DD, or `null`. |
| `deregistered` | string | De-registered date, YYYY-MM-DD, or `null`. |
| `has_premises` | string | `yes` or `no`. |
| `location` | object | `latitude` and `longitude` of the postcode, terminated postcode or outcode point. Never null. |
| `geoq` | string | `postcode`, `terminated_postcode` or `outcode`. |
| `source` | string | Credit line for the HIW directory and the postcodes.io or ONS credit for map points. |
| `source_as_of` | string | Date this extract was taken, `2026-11-21`. |

## Data notes

* The records are the 1,674 services in the Healthcare Inspectorate Wales service directory CSV (https://www.hiw.org.uk/service-directory.csv?_format=csv&page=), fetched 21 November 2026.
* Twelve medical agencies with no physical premises are flagged with `has_premises=no`. They still have a registered address or PO Box.
* Bracketed doctor names such as (Dr V Jayaraman) were removed from 6 service names. Practice names that themselves start with Dr were kept as the registered titles. 36 de-registered dates were moved from the name into the `deregistered` field, including one Welsh note (wedi'i ddat-gofrestru).
* Leading apostrophes were stripped from 1,311 telephone numbers, and N/A phones were left blank.
* Two published postcodes look typographical (LL5 1TF at Ruthin, CF25 5LJ at Bocam Park) and were geocoded as LL15 1TF and CF35 5LJ. The published postcode was not changed.
* Map points come from postcodes.io (ONS Postcode Directory), not from OpenStreetMap. Every row has a point: 1,644 current postcodes, 18 terminated postcodes and 12 outcode centroids. `local_authority` is blank on those 18 terminated postcodes.
* Some names repeat, and 3 pairs of rows are identical in every published field. Use `__id` to tell rows apart.
* This is a snapshot dated 21 November 2026, the `source_as_of` value. Check the HIW website before relying on a record.

## Examples

2 Hope View, a GP surgery whose source name included a bracketed doctor name that was removed. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_healthcare_services' --data-urlencode 'establishment_type=GP Surgeries' --data-urlencode 'name=2 Hope View'
```

```json
[
  {
    "__id": 4,
    "name": "2 Hope View",
    "establishment_type": "GP Surgeries",
    "services": "GP services",
    "sector": "NHS",
    "address": {
      "street": "Ffordd Top y Rhos",
      "city": "Flintshire",
      "postalcode": "CH7 4NE"
    },
    "local_authority": "Flintshire",
    "phone": "01352 770212",
    "last_inspection": null,
    "deregistered": null,
    "has_premises": "yes",
    "location": {
      "latitude": 53.11909,
      "longitude": -3.120071
    },
    "geoq": "postcode",
    "source": "Healthcare Inspectorate Wales, service directory, https://www.hiw.org.uk/ (CSV export). Contains public sector information licensed under the Open Government Licence v3.0. The HIW logo is not used. Map points from postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right.",
    "source_as_of": "2026-11-21"
  }
]
```

Weight Loss Wales, one of the 12 medical agencies with no physical premises. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_healthcare_services' --data-urlencode 'establishment_type=Medical agencies (with no physical premises)' --data-urlencode 'name=Weight Loss Wales'
```

```json
[
  {
    "__id": 1421,
    "name": "Weight Loss Wales",
    "establishment_type": "Medical agencies (with no physical premises)",
    "services": null,
    "sector": "Private/Independent",
    "address": {
      "street": null,
      "city": "Radyr, Cardiff",
      "postalcode": "CF15 8DJ"
    },
    "local_authority": "Cardiff",
    "phone": null,
    "last_inspection": "2024-07-25",
    "deregistered": null,
    "has_premises": "no",
    "location": {
      "latitude": 51.5192,
      "longitude": -3.262624
    },
    "geoq": "postcode",
    "source": "Healthcare Inspectorate Wales, service directory, https://www.hiw.org.uk/ (CSV export). Contains public sector information licensed under the Open Government Licence v3.0. The HIW logo is not used. Map points from postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right.",
    "source_as_of": "2026-11-21"
  }
]
```

St David's Hospice in Llandudno, the one service whose last inspection date is 19 November 2026. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_wales_healthcare_services' --data-urlencode 'establishment_type=*' --data-urlencode 'last_inspection=2026-11-19'
```

```json
[
  {
    "__id": 1671,
    "name": "St David's Hospice",
    "establishment_type": "Hospices",
    "services": null,
    "sector": "Private/Independent",
    "address": {
      "street": "Abbey Road",
      "city": "Llandudno",
      "postalcode": "LL30 3EN"
    },
    "local_authority": "Conwy",
    "phone": "01492 879058",
    "last_inspection": "2026-11-19",
    "deregistered": null,
    "has_premises": "yes",
    "location": {
      "latitude": 53.316538,
      "longitude": -3.773808
    },
    "geoq": "postcode",
    "source": "Healthcare Inspectorate Wales, service directory, https://www.hiw.org.uk/ (CSV export). Contains public sector information licensed under the Open Government Licence v3.0. The HIW logo is not used. Map points from postcodes.io, which uses the ONS Postcode Directory. Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right.",
    "source_as_of": "2026-11-21"
  }
]
```

## Data source

Contains public sector information licensed under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Source: Healthcare Inspectorate Wales, service directory, extract of 21 November 2026. The [HIW Crown copyright notice](https://www.hiw.org.uk/ccrown-copyright-2026) says most material on the site (not including logos) may be used under the Open Government Licence, and that the source must be identified and the copyright status acknowledged. Map points from postcodes.io, which uses the ONS Postcode Directory: Contains OS data Crown copyright and database right; Contains Royal Mail data Crown copyright and database right; Contains National Statistics data Crown copyright and database right. This data was changed from the source as described in the notes above. The HIW logo is not used, and Healthcare Inspectorate Wales does not endorse this API.
