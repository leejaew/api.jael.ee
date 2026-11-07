# Malaysia Public Clinics

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access Public](https://img.shields.io/badge/access-Public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 2,915](https://img.shields.io/badge/records-2%2C915-lightgrey) ![updated 2025-12-31](https://img.shields.io/badge/updated-2025--12--31-orange)

2,915 public clinics run by the Ministry of Health Malaysia (MoH), from its registry of 31 December 2025. Each row has the facility code, clinic name, clinic type, state, district, address, postcode, landline phone and map coordinates. Choose a required state or federal territory, then filter by clinic type, district, name, address or a coordinate range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/my_public_clinics` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 6 fields. Disallowed: 5 fields.

`state` must be in every call, as an exact match in capitals. Wildcards do not work on `state`. A call with no `state` returns HTTP 400. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `state` | Required | No | No | Exact state or federal territory in capitals, 16 values: `PERLIS` (39), `KEDAH` (258), `PULAU PINANG` (100), `PERAK` (288), `SELANGOR` (210), `WILAYAH PERSEKUTUAN KUALA LUMPUR` (41), `WILAYAH PERSEKUTUAN PUTRAJAYA` (4), `NEGERI SEMBILAN` (151), `MELAKA` (90), `JOHOR` (377), `PAHANG` (336), `TERENGGANU` (173), `KELANTAN` (241), `SABAH` (325), `SARAWAK` (270), `WILAYAH PERSEKUTUAN LABUAN` (12). |
| `clinic_name` | Optional | Yes | No | Clinic name in capitals as the registry lists it. Wildcards work, such as `*BESERI*`. |
| `clinic_type` | Optional | No | No | Exact clinic type in Malay, 4 values: `KLINIK KESIHATAN` (1,112), `KLINIK DESA` (1,573), `KLINIK KOMUNITI` (160), `KLINIK KESIHATAN IBU DAN ANAK` (70). |
| `district` | Optional | No | No | Exact district in capitals, such as `KANGAR`. |
| `address` | Optional | Yes | No | Street address in capitals, without the postcode. Wildcards work, such as `*JALAN*`. One address is blank. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84). Use a range with two dots, such as `1.4..1.6`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84). Use a range with two dots, such as `103.5..104.0`. |
| `facility_code` | Disallowed | No | No | MoH facility code, such as `21-09010004`. Unique per clinic. |
| `postcode` | Disallowed | No | No | 5 digit postcode as text, with leading zeros kept. |
| `phone` | Disallowed | No | No | Landline numbers as the registry lists them. Blank for 2,451 clinics. |
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
| `address` | string | Street address in capitals, without the postcode. Null for one clinic. |
| `postcode` | string | 5 digit postcode, as text. |
| `phone` | string | Landline numbers. Null where there are none. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). Null for 2 clinics. |
| `longitude` | number | Longitude in decimal degrees (WGS 84). Null for 2 clinics. |
| `source` | string | Link to the source registry file. |
| `updated` | string | Registry snapshot date. |

## Examples

A partial name match in Perlis. This call returns 1 row, KK BESERI:

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_clinics" --data-urlencode "state=PERLIS" --data-urlencode "clinic_name=*BESERI*"
```

Response:

```json
[
  {
    "facility_code": "21-09010004",
    "clinic_name": "KK BESERI",
    "clinic_type": "KLINIK KESIHATAN",
    "state": "PERLIS",
    "district": "KANGAR",
    "address": "KM 10,JALAN KAKI BUKIT, BESERI",
    "postcode": "02400",
    "phone": "04-9381355",
    "latitude": 6.5190354,
    "longitude": 100.2298976,
    "source": "https://github.com/MoH-Malaysia/data-resources-public/blob/main/facilities_master.csv",
    "updated": "2025-12-31"
  }
]
```

A box on latitude and longitude around Johor Bahru. This call returns 42 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_clinics" --data-urlencode "state=JOHOR" --data-urlencode "latitude=1.4..1.6" --data-urlencode "longitude=103.5..104.0" | jq length
```

Rural clinics in Johor. This call returns 253 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/my_public_clinics" --data-urlencode "state=JOHOR" --data-urlencode "clinic_type=KLINIK DESA" | jq length
```

## Data notes

* The data is a snapshot of the registry on 31 December 2025. It does not show whether a clinic is open today.
* Only MoH clinics are included. Dental clinics are left out.
* Clinic types keep the Malay names from the registry. `KLINIK KESIHATAN` is a health clinic, `KLINIK DESA` a rural clinic, `KLINIK KOMUNITI` a community clinic and `KLINIK KESIHATAN IBU DAN ANAK` a mother and child health clinic.
* We removed all 18 Malaysian mobile numbers (starting with 01) from 17 rows to keep out personal data. Office landlines were kept. 2,451 clinics have no phone after this. Some landlines have odd lengths or short extensions and are kept as published.
* The map points for KD ROMPON (`21-12200282`) and KKIA KUDAT (`21-12090339`) fall far outside Malaysia, even with latitude and longitude swapped, so we left them blank. We did not guess new points.
* Email and urban or rural columns were blank on every row and were dropped. Extra spaces were removed.

## License

The data comes from the Ministry of Health Malaysia (Kementerian Kesihatan Malaysia, KKM) file `facilities_master.csv` in its [data-resources-public](https://github.com/MoH-Malaysia/data-resources-public) repository.

It is used under the [Malaysian Government Open Data Terms of Use 1.0](https://www.malaysia.gov.my/en/my-initiative/data-terbuka-kerajaan/policy-strategy-and-governance/terma-penggunaan-data-terbuka-kerajaan). The terms let you copy, adapt, share and use the data, including for business, as long as you credit the source and link to the terms. Personal data and official logos are not covered, and the data comes with no warranty.

We changed the data: we kept only MoH clinics, chose and renamed fields, cleaned spaces, removed mobile numbers, blanked 2 bad map points and added the snapshot date. The Ministry of Health Malaysia does not endorse this API.

Credit line:

* Source: Ministry of Health Malaysia (KKM), data-resources-public. Data and information are subject to the Malaysian Government Open Data Terms of Use 1.0.

[Back to the API list](../../README.md)
