# NZ Pet Export Vets

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 53](https://img.shields.io/badge/records-53-lightgrey) ![updated 2026-11-12](https://img.shields.io/badge/updated-2026--11--12-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

This work is based on/includes MPI data which is licensed by Ministry for Primary Industries (MPI) for re-use under the Creative Commons Attribution 4.0 International licence. Source: Ministry for Primary Industries, [Veterinary practices listed for certification of dogs and cats to Australia](https://www.mpi.govt.nz/resources-and-forms/registers-and-lists/veterinary-practices-listed-for-certification-of-dogs-and-cats-to-australia). Map points: contains data sourced from the LINZ Data Service licensed for reuse under CC BY 4.0, see [LINZ Data Service](https://data.linz.govt.nz/). Links: [Creative Commons Attribution 4.0 licence](https://creativecommons.org/licenses/by/4.0/), [MPI copyright page](https://www.mpi.govt.nz/about-this-site/mpi-copyright).

The 53 veterinary practices that the New Zealand Ministry for Primary Industries (MPI) lists for certifying dogs and cats for export to Australia, from the list last reviewed on 12 November 2026: Auckland and Northland 16, Central and southern North Island 29 and South Island 8. Each record has the MPI listing number, practice name, address, suburb, city and phone, and 44 have a map point from LINZ NZ Addresses with a match quality in `geoq`. `city` is required on every call, and `city=*` returns all 53 practices.

Not affiliated with or endorsed by MPI or LINZ. MPI's list page says that listed veterinary practices are not employed by MPI, exercise their own clinical judgements and professional practices, and have sole discretion to decide the tests and checks needed for export certification. This API does not say which tests or checks an export needs, and it is a copy of the list as reviewed on 12 November 2026. The list changes, so check the MPI page and your practice before you rely on a record.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_pet_export_vets` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per practice. All matches are returned. |

## Query parameters

Required: 1 field (`city`). Optional: 10 fields. Disallowed: 1 field.

`city` is required. A call without it returns HTTP 400. Use `city=*` to return all 53 records (about 40 KB). Partial filters accept `*`, such as `city=Ta*`, `practice_name=*Vet*`, `address=*Road*` or `area=Central*`. `listing_number`, `phone`, `geoq` and `source_as_of` are exact matches, and a star there is read as a plain character, so it returns `[]`. All filters ignore case. `lat` and `lng` accept a range with two dots, lower number first, such as `lat=-37..-36` (latitudes are negative in New Zealand, so the lower number is the more negative one). A hyphen range such as `lat=36-37` returns HTTP 400.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `city` | Required | Yes | No | City or town as MPI prints it. 24 values, the largest are Auckland (14), Christchurch (5), Hamilton (4) and Wellington (4). It includes the old spelling Wanganui. Never blank. Every call must include city, and a call without it returns HTTP 400. Partial wildcards allowed, so city=* returns all 53 rows, and city=Ta* or *ton also work. Case ignored. |
| `area` | Optional | Yes | No | Region group, taken from the MPI table heading: Auckland and Northland (16), Central and southern North Island (29) or South Island (8). Never blank. Partial wildcards allowed, such as Central* or *North Island. Case ignored. |
| `practice_name` | Optional | Yes | No | Name of the veterinary practice as MPI prints it, such as Albany Veterinary Hospital or Vetcare Tauranga. One name (listing 1177, Shirley Vet Clinic) ends with the service note canine semen only. Never blank. Partial wildcards allowed, such as *Vet* or Care*. |
| `listing_number` | Optional | No | No | MPI veterinary practice listing number as text, a 4 digit number such as 1125, unique on every row and never blank. It identifies the practice on the MPI list. Exact match, such as listing_number=1125. |
| `address` | Optional | Yes | No | Street address as MPI prints it, such as 416 Albany Highway or 1/240 Ormistone Rd. Spelling and abbreviations follow the source (Rd, St), and a few have a suburb or the word Mobile Vet in the address. Never blank. Partial wildcards allowed, such as *Road* or *Great South*. |
| `suburb` | Optional | Yes | No | Suburb or locality as MPI prints it, such as Albany or Kilbirnie. Blank (null) on 17 rows where MPI lists none. One mobile practice has a service area instead: North, Central, and West Auckland. Partial wildcards allowed, such as Taka*. |
| `phone` | Optional | No | No | Practice telephone number as published, text such as 09 415 8909 or 07 8702696. The spacing is not standardized. 4 mobile practices list a mobile number. Never blank. Exact match. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84, 6 decimals), negative in New Zealand, from the LINZ NZ Addresses point for the practice address. Present on 44 rows, blank (null) on 9, see geoq. A number. Range filter with two dots, lower number first, such as -37..-36. A hyphen range returns 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same point, from 168.39 to 176.88. Present on 44 rows, blank (null) on 9. A number. Range filter with two dots, such as 174..175. |
| `geoq` | Optional | No | No | How the point was set. address_point (40): one LINZ address matches the street number, road and locality exactly. address_point_building (2): the address has several units, so the point is the mean of the unit points in the building. address_point_road_spelling (1): the road spelling differs slightly from LINZ (Buckland Beach Rd and Bucklands Beach Road). address_point_locality_differs (1): the street number and road match but LINZ puts the address in a different suburb than MPI (152A Great South Road, Drury and Takanini). no_street_number (3): MPI gives no street number. no_match (6): no LINZ address matched. Never blank. Exact match, such as geoq=address_point. |
| `source_as_of` | Optional | No | No | Date MPI last reviewed the list, 2026-11-12, the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: This work is based on/includes MPI data which is licensed by Ministry for Primary Industries (MPI) for re-use under the Creative Commons Attribution 4.0 International licence, with the credit for the LINZ map points. Returned in output only. |

## Response fields

`lat` and `lng` are numbers. All other values are strings. Blank cells are returned as `null`. There is no `__id`: `listing_number` is the key.

| Field | Type | Description |
|---|---|---|
| `area` | string | Table heading on the MPI page: Auckland and Northland, Central and southern North Island or South Island. Never `null`. |
| `practice_name` | string | Practice name as MPI prints it. |
| `listing_number` | string | MPI veterinary practice listing number. Unique on every record. |
| `address` | string | Street address as MPI prints it. |
| `suburb` | string | Suburb or locality, or `null` on 17 records. |
| `city` | string | City or town as MPI prints it. Never `null`. |
| `phone` | string | Practice telephone number as published. |
| `lat` | number | Latitude of the LINZ address point, or `null` on 9 records. |
| `lng` | number | Longitude of the LINZ address point, or `null` on 9 records. |
| `geoq` | string | Map point quality: `address_point`, `address_point_building`, `address_point_road_spelling`, `address_point_locality_differs`, `no_street_number` or `no_match`. |
| `source` | string | Attribution line for the MPI list and the LINZ map points. |
| `source_as_of` | string | Date MPI last reviewed the list, `2026-11-12`. |

## Data notes

* The records are the practices on the MPI list page, with the table heading as `area`. Nothing was dropped, and the list has no personal names. Four practices (1265, 1264, 1268 and 1256) list a mobile number as the practice phone, shown as published. Spelling, spacing and abbreviations follow MPI, such as Rd, Wanganui and `021124 4365`.
* Changes since the Wayback Machine copy of the page from 22 January 2026 (54 practices): 1 practice was added (1270 VetSouth Invercargill), 2 were removed (1146 Koru Vets, 1262 K9HQ Ltd), 1256 TCI Veterinary Services moved to 105 Lockwood Road, Kairanga, and 1155 Avonhead Vets gained its suburb.
* The map points come from LINZ NZ Addresses. A point is the address point for the street number and road, matched to the MPI suburb or city. `geoq` says how: `address_point` (40), `address_point_building` (2, the mean of the unit points in one building), `address_point_road_spelling` (1, the road spelling differs slightly from LINZ), `address_point_locality_differs` (1, LINZ puts the address in a different suburb, 152A Great South Road, which MPI lists in Takanini and LINZ in Drury), `no_street_number` (3, MPI gives no street number, so `lat` and `lng` are `null`) and `no_match` (6, no LINZ address matched, so `lat` and `lng` are `null`). The points are address points, not the practice entrance, and are not authoritative.
* `suburb` is `null` on 17 records where MPI lists none. One mobile practice has a service area (North, Central, and West Auckland) in place of a suburb.
* The data is a snapshot of the list as last reviewed on 12 November 2026, the `source_as_of` value. It is not refreshed automatically.

## Examples

Auckland practices with a latitude between -37 and -36.8. This call returns 7 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=Auckland' --data-urlencode 'lat=-37..-36.8'
```

```json
{
  "area": "Auckland and Northland",
  "practice_name": "Auckland Pet Hospital",
  "listing_number": "1103",
  "address": "46-48 Pollen Street",
  "suburb": "Grey Lynn",
  "city": "Auckland",
  "phone": "09 360 0961",
  "lat": -36.858443,
  "lng": 174.747879,
  "geoq": "address_point",
  "source": "This work is based on/includes MPI data which is licensed by Ministry for Primary Industries (MPI) for re-use under the Creative Commons Attribution 4.0 International licence. Source: Ministry for Primary Industries, Veterinary practices listed for certification of dogs and cats to Australia. Map points: contains data sourced from the LINZ Data Service licensed for reuse under CC BY 4.0.",
  "source_as_of": "2026-11-12"
}
```

Practices in the South Island table. This call returns 8 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=*' --data-urlencode 'area=South Island'
```

Practices with Care in the name. This call returns 6 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=*' --data-urlencode 'practice_name=*Care*'
```

Practices in Palmerston North. This call returns 2 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=Palmerston North'
```

The practice with MPI listing number 1270, added to the list since January 2026. This call returns 1 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=*' --data-urlencode 'listing_number=1270'
```

Practices with no map point because no LINZ address matched. This call returns 6 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=*' --data-urlencode 'geoq=no_match'
```

Practices at latitude -46 to -43, the lower South Island. This call returns 6 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/nz_pet_export_vets' --data-urlencode 'city=*' --data-urlencode 'lat=-46..-43'
```

## Data source

This work is based on/includes MPI data which is licensed by Ministry for Primary Industries (MPI) for re-use under the Creative Commons Attribution 4.0 International licence. Source: Ministry for Primary Industries, [Veterinary practices listed for certification of dogs and cats to Australia](https://www.mpi.govt.nz/resources-and-forms/registers-and-lists/veterinary-practices-listed-for-certification-of-dogs-and-cats-to-australia), last reviewed 12 November 2026. The [MPI copyright page](https://www.mpi.govt.nz/about-this-site/mpi-copyright) says that, except where otherwise stated, material on the MPI website is Crown copyright and licensed under the Creative Commons Attribution 4.0 New Zealand licence. The attribution is given in writing and no MPI emblem or logo is used.

Map points: contains data sourced from the LINZ Data Service licensed for reuse under CC BY 4.0. Source: Toitu Te Whenua Land Information New Zealand, NZ Addresses.

Not affiliated with or endorsed by MPI or LINZ. The data is as MPI publishes it, with the changes in the notes above, and it can contain errors.

[Back to the API list](../../README.md)
