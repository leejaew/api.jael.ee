# Taiwan Travel Medicine Clinics

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 33](https://img.shields.io/badge/records-33-lightgrey) ![updated 2026-10-21](https://img.shields.io/badge/updated-2026--10--21-orange) ![license OGDL 1.0](https://img.shields.io/badge/license-OGDL%201.0-green)

The 33 hospitals that Taiwan Centers for Disease Control (Taiwan CDC) contracts to run travel medicine clinics, in 22 cities and counties, in English and Traditional Chinese. These clinics give the vaccines, malaria pills and health advice you may need before a trip abroad, and they issue the international vaccination certificate (yellow card). Each row has the NHI institution code, names, city or county, postcode, address, clinic or department, phone and extension, website, the date the hospital joined the program, whether it gives the mpox vaccine, and a map point. Filter by the required `county_city`, then by name, clinic, mpox vaccine or a coordinate range.

Rows by city or county: Taipei City 5, Taichung City 3, New Taipei City, Taoyuan City, Nantou County, Kaohsiung City and Hualien County 2 each, and 1 each in the other 15. 8 hospitals give the mpox vaccine.

> **Fees.** All travel medicine services are self pay and not covered by National Health Insurance. Taiwan CDC sets the price of the vaccines and drugs it buys for these clinics, for example yellow fever vaccine NT$2,810 a dose, mpox vaccine (JYNNEOS) NT$6,986 a dose, Malarone NT$275 a tablet and a new international vaccination certificate NT$210. Other vaccines and the registration fee (about NT$250 to 700 a visit) vary by hospital, so check with the hospital before you go. Prices are from the [Taiwan CDC fee page](https://www.cdc.gov.tw/Category/ListContent/YipvR33b32K-dsMI2eIrIg?uaid=7xJffAe-293ImV5qeweoeQ), updated 2026-02-05.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_travel_medicine_clinics` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`county_city`). Optional: 15 fields. Disallowed: 5 fields.

`county_city` must be in every call. Use `county_city=*` to list all 33 hospitals. Disallowed means the field is returned in the output but cannot be used as a filter. Without `county_city` the API returns HTTP 400.

Dates are text in the form `YYYY-MM-DD`, so filter them with a prefix wildcard, such as `2014*`. Only `latitude` and `longitude` take ranges. Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `county_city` | Required | Yes | No | City or county in English, 22 values, such as `Taipei City` (5), `Taichung City` (3) or `Kaohsiung City` (2). Wildcards work, so `county_city=*` lists all 33, `county_city=*County` lists the 15 in counties and `county_city=*City` the 18 in cities. Hsinchu City and Hsinchu County, and Chiayi City and Chiayi County, are different places. |
| `institution_code` | Optional | No | No | 10 digit NHI medical institution code, kept as text so leading zeros stay, such as `0401180014` (1). |
| `name_en` | Optional | Yes | No | Hospital name in English as Taiwan CDC lists it, such as `*Tzu Chi*` (2), `*MOHW` (8) or `*National Taiwan University*` (3). |
| `name_zh` | Optional | Yes | No | Full hospital name in Traditional Chinese, such as `*慈濟*` (2) or `衛生福利部*` (8). |
| `name_zh_short` | Optional | Yes | No | Short Chinese name used on the Taiwan CDC clinic page, such as `臺大醫院` (1). |
| `county_city_zh` | Optional | No | No | City or county in Chinese, such as `臺北市` (5). |
| `postcode` | Optional | No | No | 6 digit postcode, as text, such as `100229` (1). |
| `address_en` | Optional | Yes | No | Street address in English, such as `*Taiwan Blvd.*` (2). Translated by us from the Chinese address in Hanyu Pinyin, so spellings may differ from street signs. |
| `address_zh` | Optional | Yes | No | Street address in Chinese as the source gives it, such as `*中山*`. |
| `department_en` | Optional | Yes | No | Clinic or department that runs travel medicine, in English: `Family Medicine` (14), `Travel Medicine Clinic` (7), `Infectious Diseases` (2), `International Travel Medicine Clinic` (2) and 8 more with 1 each. `*Travel*` matches 12. |
| `department_zh` | Optional | Yes | No | Clinic or department in Chinese, such as `家庭醫學科` (14) or `旅遊醫學門診` (7). |
| `phone` | Optional | No | No | Main phone number with the area code, as Taiwan CDC lists it, such as `(02)2312-3456` (1). To call from abroad, dial +886 and drop the leading 0. |
| `start_date` | Optional | Yes | No | Date the hospital joined the Taiwan CDC travel medicine program, from `2006-10-20` to `2026-03-02`, such as `2014*` (14). |
| `mpox_vaccine` | Optional | No | No | `yes` (8) if the hospital gives the self pay mpox vaccine, `no` (25) if not. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.5674103 to 26.1609852. Example: `latitude=24.9..25.2`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 118.4180251 to 121.7715737. Example: `longitude=118..120` (3, the outlying islands of Kinmen, Matsu and Penghu). |
| `phone_ext` | Disallowed | No | No | Extension for the travel clinic, from the Taiwan CDC clinic page, such as `5610, 5618`. Filled for 9 rows. |
| `website` | Disallowed | No | No | Hospital website as the source gives it. Most are http. Links were not tested. |
| `geocode_quality` | Disallowed | No | No | Where the map point comes from: `source` (32, the point given by Taiwan CDC) or `osm_hospital_match` (1). See the notes. |
| `source` | Disallowed | No | No | Data source and license credit for the row. |
| `updated` | Disallowed | No | No | Date Taiwan CDC last updated the dataset, `2026-10-21` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `institution_code` | string | 10 digit NHI medical institution code, as text. |
| `name_en` | string | Hospital name in English. |
| `name_zh` | string | Full hospital name in Traditional Chinese. |
| `name_zh_short` | string | Short Chinese name. |
| `county_city` | string | City or county in English. |
| `county_city_zh` | string | City or county in Chinese. |
| `postcode` | string | 6 digit postcode, as text. |
| `address_en` | string | Street address in English (our Hanyu Pinyin translation). |
| `address_zh` | string | Street address in Chinese. |
| `department_en` | string | Clinic or department in English. |
| `department_zh` | string | Clinic or department in Chinese. |
| `phone` | string | Main phone number with the area code. |
| `phone_ext` | string | Travel clinic extension. Null for 24 rows. |
| `website` | string | Hospital website. |
| `start_date` | string | Date the hospital joined the program, `YYYY-MM-DD`. |
| `mpox_vaccine` | string | `yes` or `no`. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | `source` or `osm_hospital_match`. |
| `source` | string | Data source and license credit. |
| `updated` | string | Date the source dataset was last updated. |

Notes:

* English names and county names: hospital names in English come from the Taiwan CDC English Travel Clinics page. That page puts Ton-Yen General Hospital in Hsinchu City and Chiayi Chang Gung Memorial Hospital in Chiayi City. The dataset and the addresses put them in Hsinchu County and Chiayi County, so `county_city` follows the dataset.
* English addresses: the source has no English addresses. `address_en` is our translation of `address_zh` in Hanyu Pinyin, the spelling used on most road signs. Some older spellings on signs and hospital sites differ, such as Jhongjheng for Zhongzheng.
* Departments: `department_zh` is the dataset value, and `department_en` is the English for it. On 3 rows (the Taoyuan airport clinic, Lienchiang County Hospital and the NTUH Yunlin Branch) the Taiwan CDC clinic page names the department a little differently. The API keeps the dataset value.
* Map points: 32 points are the coordinates given in the Taiwan CDC dataset. The point for Taichung Veterans General Hospital in the dataset was 12.6 km from the hospital, so it was replaced with the OpenStreetMap point for the hospital (`osm_hospital_match`).
* Hours: clinic hours are not in the API. Taiwan CDC publishes a timetable on its [travel medicine clinic page](https://www.cdc.gov.tw/Category/Page/ucmuQnzcJPue77qHt0IXeg). Taiwan CDC advises a visit 4 to 8 weeks before you travel.

## Examples

Hospitals in Taipei City that give the mpox vaccine, shown as a full row. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_travel_medicine_clinics" --data-urlencode "county_city=Taipei City" --data-urlencode "mpox_vaccine=yes"
```

```json
[
  {
    "__id": 3,
    "institution_code": "0401180014",
    "name_en": "National Taiwan University Hospital",
    "name_zh": "國立臺灣大學醫學院附設醫院",
    "name_zh_short": "臺大醫院",
    "county_city": "Taipei City",
    "county_city_zh": "臺北市",
    "postcode": "100229",
    "address_en": "No. 1, Changde St., Zhongzheng Dist., Taipei City",
    "address_zh": "臺北市中正區常德街1號",
    "department_en": "Family Medicine, Room 10 (Travel Medicine Special Clinic)",
    "department_zh": "家庭醫學科第10診（旅遊醫學特別門診）",
    "phone": "(02)2312-3456",
    "phone_ext": "267614",
    "website": "http://www.ntuh.gov.tw/ntuh_chinese.aspx",
    "start_date": "2008-01-01",
    "mpox_vaccine": "yes",
    "latitude": 25.0416163,
    "longitude": 121.5171944,
    "geocode_quality": "source",
    "source": "Taiwan CDC, Travel Medicine Contract Hospitals (data.gov.tw 42783), Open Government Data License v1.0",
    "updated": "2026-10-21"
  }
]
```

A wildcard on the required field. All 8 hospitals in Taiwan that give the mpox vaccine. This call returns 8 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_travel_medicine_clinics" --data-urlencode "county_city=*" --data-urlencode "mpox_vaccine=yes" | jq -r '.[] | [.county_city, .name_en, .phone] | @tsv'
```

```text
Taipei City	National Taiwan University Hospital	(02)2312-3456
New Taipei City	Far Eastern Memorial Hospital	(02)8966-7000
New Taipei City	Taipei Tzu Chi Hospital	(02)6628-9779
Taoyuan City	Tao Yuan General Hospital, MOHW	(03)369-9721
Taichung City	Taichung Hospital, MOHW	(04)2229-4411
Taichung City	Tungs' Taichung MetroHarbor Hospital, Wuchi Branch	(04)2658-1919
Tainan City	National Cheng Kung University Hospital	(06)235-3535
Kaohsiung City	Kaohsiung Municipal United Hospital	(07)555-2565
```

A longitude range. Clinics on the outlying islands of Kinmen, Matsu and Penghu. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_travel_medicine_clinics" --data-urlencode "county_city=*" --data-urlencode "longitude=118..120" | jq -r '.[] | [.county_city, .name_en, .department_en, .latitude, .longitude] | @tsv'
```

```text
Kinmen County	Kinmen Hospital, MOHW	Family Medicine	24.4406948	118.4180251
Lienchiang County	Lienchiang County Hospital	Health Checkup Clinic	26.1609852	119.9488321
Penghu County	Tri-Service General Hospital, Penghu Branch	Travel Medicine Clinic	23.5552607	119.5833391
```

A Chinese filter. Hospitals whose travel clinic is listed as 旅遊醫學門診. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_travel_medicine_clinics" --data-urlencode "county_city=*" --data-urlencode "department_zh=旅遊醫學門診" | jq -r '.[].name_en'
```

## Data source

The list of hospitals, codes, Chinese names, addresses, departments, phones, websites, start dates, mpox vaccine flags and map points comes from the Taiwan CDC open data set [Travel Medicine Contract Hospitals](https://data.gov.tw/dataset/42783) (旅遊醫學合約醫院), data.gov.tw dataset 42783, updated 2026-10-21 and downloaded 2026-11-03. English hospital and county names come from the [Taiwan CDC English Travel Clinics page](https://www.cdc.gov.tw/En/Category/ListContent/nbvOxj5T7SmkLuzFbc2Xfw?uaid=76cb7HXJPGkkmEkfSjd1oQ), and short Chinese names and phone extensions from the [Taiwan CDC travel medicine clinic page](https://www.cdc.gov.tw/Category/Page/ucmuQnzcJPue77qHt0IXeg), both updated 2026-03-04. English addresses were translated by us, and one map point comes from OpenStreetMap. The data can go out of date, so check with the hospital before you go.

Credit lines:

* Source: Taiwan Centers for Disease Control, Travel Medicine Contract Hospitals (data.gov.tw dataset 42783), 2026-10-21. Open Government Data License, version 1.0 (https://data.gov.tw/license). This license lets anyone use, copy, change and share the data for free, for any purpose, including commercial use, as long as the source is credited.
* One map point (Taichung Veterans General Hospital) © OpenStreetMap contributors, under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not affiliated with or endorsed by Taiwan CDC or any agency.

[Back to the API list](../../README.md)
