# Malaysia Public Hospitals

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 166](https://img.shields.io/badge/records-166-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

Every public hospital in Malaysia, 166 hospitals from the Ministry of Health (MoH) registry of 31 December 2025: 151 run by the Ministry of Health, 10 university teaching hospitals and 5 armed forces hospitals. Each row has the facility code, name, type, agency, state, district, address, postcode and map coordinates. Bed, ICU and ventilator counts and use rates from 3 September 2024 are filled for 108 hospitals.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/my_public_hospitals` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 21 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 166 hospitals. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | One of 16 states and federal territories, such as `Sabah` (26), `Selangor` (17) or `Kuala Lumpur` (8). Wildcards work, so `state=*` lists all 166 hospitals. |
| `facility_code` | Optional | No | No | The MoH facility code, such as `11-01030012`. Unique per hospital. |
| `hospital_name` | Optional | Yes | No | The hospital name in capitals as the registry lists it, such as `*KAJANG*` or `HOSPITAL SULTAN*`. |
| `hospital_type` | Optional | Yes | No | The hospital type in English, 7 values: `Non specialist hospital` (79), `Major specialist hospital` (29), `Minor specialist hospital` (19), `State hospital` (14), `Special medical institution` (10), `University teaching hospital` (10), `Military hospital` (5). |
| `hospital_type_ms` | Optional | Yes | No | The hospital type in Malay as the registry lists it, such as `HOSPITAL TANPA PAKAR` or `HOSPITAL NEGERI`. |
| `agency` | Optional | Yes | No | The agency that runs the hospital: `Ministry of Health` (151), `Ministry of Higher Education` (10) or `Malaysian Armed Forces` (5). |
| `agency_ms` | Optional | No | No | The agency code in Malay: `KKM` (151), `KPT` (10) or `ATM` (5). |
| `district` | Optional | Yes | No | The district, such as `Kluang` or `Kota*`. 129 distinct values. |
| `address` | Optional | Yes | No | The street address in capitals, such as `*JALAN*`. |
| `postcode` | Optional | Yes | No | The 5 digit postcode as text with leading zeros, such as `50586` or `50*`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS84), from 1.17676 to 6.90774. Use a range, for example `latitude=3.0..3.3`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS84), from 99.796411 to 118.604336. Use a range, for example `longitude=101.5..101.8`. |
| `geocode_quality` | Optional | Yes | No | How the map point was checked: `source (checked inside its state)` for 164 rows, or `geocoded*` for 2 points replaced with OpenStreetMap. |
| `beds_non_icu` | Optional | No | Yes | Non ICU beds, 0 to 1672, as of `2024-09-03`. Blank where no figure. Use a range, for example `beds_non_icu=500..2000`. |
| `beds_icu` | Optional | No | Yes | ICU beds, 0 to 87, as of `2024-09-03`. Blank where no figure. Use a range, for example `beds_icu=20..100`. |
| `beds_total` | Optional | No | Yes | Total beds (non ICU plus ICU), 0 to 1706, as of `2024-09-03`. Blank where no figure. Use a range, for example `beds_total=1000..2000`. |
| `ventilators` | Optional | No | Yes | Ventilators, 0 to 158, as of `2024-09-03`. Blank where no figure. Use a range, for example `ventilators=50..200`. |
| `non_icu_use_rate_pct` | Optional | No | Yes | Non ICU bed use rate in percent on `2024-09-03`, 0 to 110.8 (can pass 100 as published). Blank where no beds. Use a range. |
| `icu_use_rate_pct` | Optional | No | Yes | ICU bed use rate in percent on `2024-09-03`, 0 to 162.5 (can pass 100 as published). Blank where no ICU beds. Use a range. |
| `ventilator_use_rate_pct` | Optional | No | Yes | Ventilator use rate in percent on `2024-09-03`, 0 to 82.2. Blank where no ventilators. Use a range. |
| `bed_data_status` | Optional | Yes | No | `reported` (108 with bed figures), `listed, no figures` (40) or `not in bed file` (18). |
| `capacity_as_of` | Optional | No | No | Date of the bed figures, `2024-09-03`, or blank where there are no figures. |
| `notes` | Disallowed | No | No | Our notes on fixed or blanked values. Blank for most hospitals. |
| `source` | Disallowed | No | No | Source and credit for the row. |
| `source_as_of` | Disallowed | No | No | Date of the hospital list, `2025-12-31` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `facility_code` | string | MoH facility code. Unique per hospital. |
| `hospital_name` | string | Hospital name in capitals, as the registry lists it. |
| `hospital_type` | string | Hospital type in English. |
| `hospital_type_ms` | string | Hospital type in Malay, as the registry lists it. |
| `agency` | string | Agency that runs the hospital, in English. |
| `agency_ms` | string | Agency code in Malay: `KKM`, `KPT` or `ATM`. |
| `state` | string | State or federal territory. |
| `district` | string | District. |
| `address` | string | Street address in capitals, without the postcode. |
| `postcode` | string | 5 digit postcode, as text. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). |
| `longitude` | number | Longitude in decimal degrees (WGS 84). |
| `geocode_quality` | string | How the map point was checked. |
| `beds_non_icu` | integer | Non ICU beds on 2024-09-03. Null where there is no figure. |
| `beds_icu` | integer | ICU beds on 2024-09-03. Null where there is no figure. |
| `beds_total` | integer | Total beds on 2024-09-03. Null where there is no figure. |
| `ventilators` | integer | Ventilators on 2024-09-03. Null where there is no figure. |
| `non_icu_use_rate_pct` | number | Non ICU bed use rate in percent on 2024-09-03. Null where there are no beds. |
| `icu_use_rate_pct` | number | ICU bed use rate in percent on 2024-09-03. Null where there are no ICU beds. |
| `ventilator_use_rate_pct` | number | Ventilator use rate in percent on 2024-09-03. Null where there are no ventilators. |
| `bed_data_status` | string | `reported`, `listed, no figures` or `not in bed file`. |
| `capacity_as_of` | string | Date of the bed figures. Null where there are none. |
| `notes` | string | Our notes on fixed or blanked values. Null for most hospitals. |
| `source` | string | Source and credit for the row. |
| `source_as_of` | string | Date of the hospital list. |

Notes: the bed figures are a one day snapshot from 3 September 2024, about 16 months older than the hospital list, and some use rates pass 100% as published. 58 hospitals have no bed figures, including the university and armed forces hospitals and Hospital Seri Iskandar, which opened in December 2024 and replaced Hospital Changkat Melintang. Hospital Limbang's non ICU and total beds are blank because the source shows a negative count. Two map points in the source were wrong (Hospital Tuanku Fauziah and Hospital Seri Iskandar) and were replaced with OpenStreetMap points, as `geocode_quality` shows.

## Examples

Hospitals in Johor Bahru district. This call returns 4 rows: Hospital Pasir Gudang, Hospital Permai, Hospital Sultan Ismail Johor Bahru and Hospital Sultanah Aminah, Johor Bahru.

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_hospitals" --data-urlencode "state=Johor" --data-urlencode "district=Johor*"
```

Response excerpt (third of 4 rows):

```json
[
  {
    "__id": 9,
    "facility_code": "11-01020400",
    "hospital_name": "HOSPITAL SULTAN ISMAIL JOHOR BAHRU",
    "hospital_type": "Major specialist hospital",
    "hospital_type_ms": "HOSPITAL PAKAR MAJOR",
    "agency": "Ministry of Health",
    "agency_ms": "KKM",
    "state": "Johor",
    "district": "Johor Bahru",
    "address": "JALAN PERSIARAN MUTIARA EMAS UTAMA, TAMAN MOUNT AUSTIN",
    "postcode": "81100",
    "latitude": 1.54839,
    "longitude": 103.79097,
    "geocode_quality": "source (checked inside its state)",
    "beds_non_icu": 692,
    "beds_icu": 32,
    "beds_total": 724,
    "ventilators": 59,
    "non_icu_use_rate_pct": 0.0,
    "icu_use_rate_pct": 56.2,
    "ventilator_use_rate_pct": 32.2,
    "bed_data_status": "reported",
    "capacity_as_of": "2024-09-03",
    "notes": null,
    "source": "Ministry of Health Malaysia, https://github.com/MoH-Malaysia/data-resources-public",
    "source_as_of": "2025-12-31"
  }
]
```

A wildcard on the name across all states. This call returns 1 row, Hospital Tengku Permaisuri Norashikin (Hospital Kajang) in Selangor:

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_hospitals" --data-urlencode "state=*" --data-urlencode "hospital_name=*KAJANG*" | jq -r '.[].hospital_name'
```

A range on ICU beds. Hospitals in Kuala Lumpur with 20 to 100 ICU beds. This call returns 1 row, Hospital Kuala Lumpur (34 ICU beds, 1,706 beds in total):

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_hospitals" --data-urlencode "state=Kuala Lumpur" --data-urlencode "beds_icu=20..100" | jq -c '.[] | [.hospital_name, .beds_icu, .beds_total]'
```

A box on latitude and longitude in the Klang Valley. This call returns 7 rows, from Hospital Ampang to Hospital Kajang:

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_hospitals" --data-urlencode "state=Selangor" --data-urlencode "latitude=2.9..3.2" --data-urlencode "longitude=101.4..101.8" | jq -r '.[].hospital_name'
```

Only Ministry of Health hospitals. This call returns 151 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_hospitals" --data-urlencode "state=*" --data-urlencode "agency=Ministry of Health" | jq length
```

## Data source

The data comes from the Ministry of Health Malaysia (Kementerian Kesihatan Malaysia, KKM) repository [data-resources-public](https://github.com/MoH-Malaysia/data-resources-public): the facilities list `facilities_master.csv` (as of 31 December 2025) joined with the bed use file `bedutil_facility.csv` (3 September 2024). MoH data on [KKMNOW](https://data.moh.gov.my) and [data.gov.my](https://data.gov.my) is open under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/). We added English labels and corrected two coordinates with [OpenStreetMap](https://www.openstreetmap.org/copyright) points. The Ministry of Health Malaysia does not endorse this API.

Credit lines:

* Source: Ministry of Health Malaysia (KKM), data-resources-public, via KKMNOW. Licensed under CC BY 4.0.
* Two map points © OpenStreetMap contributors, available under the Open Database License (ODbL).

[Back to the API list](../../README.md)
