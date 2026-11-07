# Malaysia Public Dental Clinics

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access Public](https://img.shields.io/badge/access-Public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,682](https://img.shields.io/badge/records-1%2C682-lightgrey) ![updated 2025-12-31](https://img.shields.io/badge/updated-2025--12--31-orange)

1,682 public dental clinics run by the Ministry of Health Malaysia (MoH), from its registry snapshot of December 31, 2025. Each row has the facility code, clinic name, clinic type, state, district, address, postcode and map coordinates. Choose a required state or federal territory, then filter by clinic type, district, name, address or a coordinate range. All phone values are blank.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/my_public_dental_clinics` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 6 fields. Disallowed: 5 fields.

`state` must be in every call, as an exact match in capitals. Wildcards do not work on `state`. A call with no `state` returns HTTP 400. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `state` | Required | No | No | Exact state or federal territory in capitals, 16 values: `JOHOR` (165), `KEDAH` (128), `KELANTAN` (148), `MELAKA` (65), `NEGERI SEMBILAN` (82), `PAHANG` (145), `PERAK` (158), `PERLIS` (44), `PULAU PINANG` (96), `SABAH` (142), `SARAWAK` (204), `SELANGOR` (121), `TERENGGANU` (88), `WILAYAH PERSEKUTUAN KUALA LUMPUR` (82), `WILAYAH PERSEKUTUAN LABUAN` (8), `WILAYAH PERSEKUTUAN PUTRAJAYA` (6). |
| `clinic_type` | Optional | No | No | Exact clinic type in Malay, 11 values: `KLINIK PERGIGIAN DI KLINIK KESIHATAN` (598), `KLINIK PERGIGIAN DI HOSPITAL` (81), `KLINIK PERGIGIAN SEKOLAH RENDAH` (663), `KLINIK PERGIGIAN SEKOLAH MENENGAH` (221), `KLINIK PERGIGIAN (STANDALONE)` (71), `KLINIK PERGIGIAN DI KLINIK KESIHATAN IBU DAN ANAK (KKIA)` (10), `KLINIK PERGIGIAN DI UTC` (22), `KLINIK PERGIGIAN SEKOLAH (PUSAT PERGIGIAN SEKOLAH)` (8), `KLINIK PERGIGIAN DI INSTITUSI LAIN` (3), `KLINIK PERGIGIAN DI RTC` (4), `KLINIK PERGIGIAN DI KLINIK KOMUNITI` (1). |
| `district` | Optional | No | No | Exact district in capitals, such as `PERLIS`. |
| `clinic_name` | Optional | Yes | No | Clinic name in capitals as the registry lists it. Wildcards work, such as `*BESERI*`. |
| `address` | Optional | Yes | No | Address in capitals as the registry lists it. Wildcards work, such as `*JALAN*`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84). Use a range with two dots, such as `1.4..1.6`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84). Use a range with two dots, such as `103.5..104.0`. |
| `facility_code` | Disallowed | No | No | MoH facility code, such as `21-09010004D`. Unique per clinic. |
| `postcode` | Disallowed | No | No | 5 digit postcode as text, with leading zeros kept. |
| `phone` | Disallowed | No | No | Phone number as the registry lists it. Blank for all 1,682 clinics. |
| `source` | Disallowed | No | No | Link to the source registry file. |
| `updated` | Disallowed | No | No | Registry snapshot date, `2025-12-31` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `facility_code` | string | MoH facility code. Unique per clinic. |
| `clinic_name` | string | Clinic name in capitals, as the registry lists it. |
| `clinic_type` | string | Clinic type in Malay, as the registry lists it. |
| `state` | string | State or federal territory in capitals. |
| `district` | string | District in capitals. |
| `address` | string | Address in capitals as the registry lists it. |
| `postcode` | string | 5 digit postcode, as text. |
| `phone` | string | Phone number. Null for all 1,682 clinics. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). |
| `longitude` | number | Longitude in decimal degrees (WGS 84). |
| `source` | string | Link to the source registry file. |
| `updated` | string | Registry snapshot date. |

## Examples

A partial name match in Perlis. This call returns 3 rows:

```bash
curl -sG -A 'Mozilla/5.0' 'https://api.jael.ee/JLEE/my_public_dental_clinics' --data-urlencode 'state=PERLIS' --data-urlencode 'clinic_name=*BESERI*'
```

Response excerpt (first of 3 rows):

```json
[
  {
    "facility_code": "21-09010004D",
    "clinic_name": "KLINIK PERGIGIAN DI KLINIK KESIHATAN BESERI",
    "clinic_type": "KLINIK PERGIGIAN DI KLINIK KESIHATAN",
    "state": "PERLIS",
    "district": "PERLIS",
    "address": "PEKAN BESERI, 02400 BESERI, PERLIS",
    "postcode": "02400",
    "phone": null,
    "latitude": 6.514836,
    "longitude": 100.2305,
    "source": "https://github.com/MoH-Malaysia/data-resources-public/blob/main/facilities_master.csv",
    "updated": "2025-12-31"
  }
]
```

A box on latitude and longitude around Johor Bahru. This call returns 32 rows:

```bash
curl -sG -A 'Mozilla/5.0' 'https://api.jael.ee/JLEE/my_public_dental_clinics' --data-urlencode 'state=JOHOR' --data-urlencode 'latitude=1.4..1.6' --data-urlencode 'longitude=103.5..104.0' | jq length
```

## Data notes

* The data is a snapshot of the registry on December 31, 2025. It includes clinics marked open in that snapshot, which does not show whether a clinic is open today.
* Only MoH dental clinics are included. Clinic types keep the Malay names from the registry.
* All 1,682 clinics have blank phone values in the source. No mobile numbers needed to be removed.
* Every row has map coordinates. Checks used Malaysia bounds and broad state areas, not exact state borders or building checks.
* Extra spaces were removed.

## License

The data comes from the Ministry of Health Malaysia (Kementerian Kesihatan Malaysia, KKM) file `facilities_master.csv` in its [data-resources-public](https://github.com/MoH-Malaysia/data-resources-public) repository.

It is used under the [Malaysian Government Open Data Terms of Use 1.0](https://www.malaysia.gov.my/en/my-initiative/data-terbuka-kerajaan/policy-strategy-and-governance/terma-penggunaan-data-terbuka-kerajaan). The terms let you copy, adapt, share and use the data, including for business, as long as you credit the source and link to the terms. Personal data and official logos are not covered, and the data comes with no warranty.

We changed the data: we kept only open MoH dental clinics, chose and renamed fields, cleaned spaces and added the snapshot date. The Ministry of Health Malaysia does not endorse this API.

Credit line:

* Source: Ministry of Health Malaysia (KKM), data-resources-public. Data and information are subject to the Malaysian Government Open Data Terms of Use 1.0.

[Back to the API list](../../README.md)
