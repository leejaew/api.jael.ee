# Taiwan Post Offices

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1277](https://img.shields.io/badge/records-1277-lightgrey) ![updated 2026-06-01](https://img.shields.io/badge/updated-2026--06--01-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

All 1,277 Chunghwa Post business outlets across Taiwan (main post offices and branches), with English and Chinese names, address, mail and savings phones, weekday and weekend hours, extended service flags and a map point. Filter by the required `county_city`, then by district, name, Saturday or extended service, or a latitude and longitude range.

Most outlets: New Taipei City (163), Taipei City (152), Taichung City (114) and Kaohsiung City. County rows: 427. Open on Saturday: 273. Extended weekday hours: 318. Open on Sunday: 1 (National Palace Museum Post Office). Outlying islands: Kinmen County (7), Lienchiang County (6) and Penghu County (10).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_post_offices` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`county_city`). Optional: 21 fields. Disallowed: 8 fields.

`county_city` must be in every call. Use `county_city=*` to list all 1,277 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `county_city` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges. Office codes, postal codes and phones are text, so leading zeros and hyphens stay.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `county_city` | Required | Yes | No | County or city in English, such as `Taipei City` (152), `New Taipei City` (163) or `*County` (427). Wildcards work, so `county_city=*` lists all 1,277 rows. Hsinchu City and Hsinchu County, and Chiayi City and Chiayi County, are different places. Outlying islands: Kinmen County (7), Lienchiang County (6), Penghu County (10). |
| `office_code` | Optional | No | No | Exact computerized office code (電腦局號), such as `000100-6` (Taipei Beimen). Text, keeps the hyphen and check digit. |
| `postal_code` | Optional | No | No | Exact 6 digit postal office code (郵務局號), such as `100000`. Text. |
| `name_en` | Optional | Yes | No | English outlet name from the Chunghwa Post English locator, such as `*Beimen*` (2) or `*Post Office(Taipei Branch*`. Wildcards work. |
| `name_zh` | Optional | Yes | No | Chinese outlet name (局名), such as `*北門*` (2) or `臺北*`. Wildcards work. |
| `branch_name_zh` | Optional | Yes | No | Short Chinese branch label (電腦局名), such as `臺北901支`. Wildcards work. |
| `county_city_zh` | Optional | No | No | Exact county or city in Chinese, such as `臺北市` (152). |
| `district` | Optional | Yes | No | District or township in English, such as `Zhongzheng Dist.` (31) or `*Township`. Wildcards work. |
| `district_zh` | Optional | Yes | No | District or township in Chinese, such as `中正區`. Wildcards work. |
| `postcode` | Optional | Yes | No | Postcode as Chunghwa Post lists it (5 digits in this set), such as `10044` or `100*`. Text. Wildcards work. |
| `address_en` | Optional | Yes | No | English address from the Chunghwa Post English locator, such as `*Zhongxiao*`. Wildcards work. |
| `address_zh` | Optional | Yes | No | Chinese street address as the source gives it, such as `*忠孝*`. Wildcards work. |
| `phone_mail` | Optional | No | No | Mail counter phone with area code, such as `(02)2331-1374`. Text. Add +886 and drop the leading 0 to call from abroad. |
| `weekday_mail_hours` | Optional | Yes | No | Monday to Friday mail counter hours, such as `08:30-17:30` or `08:30*`. Wildcards work. Blank on 2 rows. |
| `weekday_service` | Optional | No | No | Exact: `yes` (1,277) if the outlet serves on weekdays. All rows are `yes` in this set. |
| `weekday_extended` | Optional | No | No | Exact: `yes` (318) if the outlet offers extended weekday hours past the normal close, `no` (959) if not. |
| `saturday_service` | Optional | No | No | Exact: `yes` (273) if open on Saturday, `no` (1,004) if not. |
| `sunday_service` | Optional | No | No | Exact: `yes` (1) if open on Sunday, `no` (1,276) if not. |
| `extended_mail_only` | Optional | No | No | Exact: `yes` (213) if extended weekday hours cover mail only, `no` (1,064) if not. |
| `extended_mail_and_savings` | Optional | No | No | Exact: `yes` (105) if extended weekday hours cover both mail and savings, `no` (1,172) if not. |
| `latitude` | Optional | No | Yes | Latitude of the outlet in decimal degrees (WGS 84), from 21.940146 to 26.36671. Use a range to search a box, for example `latitude=25.0..25.1`. |
| `longitude` | Optional | No | Yes | Longitude of the outlet in decimal degrees (WGS 84), from 118.247577 to 121.924054. Use a range, for example `longitude=121.5..121.6`. Outlying islands sit near `longitude=118..120`. |
| `phone_savings` | Disallowed | No | No | Savings and remittance counter phone with area code, such as `(02)2381-3135`. Text. |
| `fax` | Disallowed | No | No | Fax number with area code. Blank on 3 rows. |
| `weekday_savings_hours` | Disallowed | No | No | Monday to Friday savings counter hours, such as `08:30-17:00`. |
| `saturday_hours` | Disallowed | No | No | Saturday hours when `saturday_service` is `yes`, such as `09:00-12:00`. Blank when closed. |
| `sunday_hours` | Disallowed | No | No | Sunday hours when `sunday_service` is `yes`. Only the National Palace Museum Post Office (`09:00-17:00`) is open on Sunday. |
| `hours_note` | Disallowed | No | No | Free text note on hours or temporary changes, in Chinese as the source gives it. Blank on 1,234 rows. |
| `source` | Disallowed | No | No | Data source and credit for the row. |
| `updated` | Disallowed | No | No | Date the source dataset was last updated, `YYYY-MM-DD` (`2026-06-01`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `county_city` returns HTTP 400 with `Your request is missing the following required parameters: county_city. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.
* Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `office_code` | string | Computerized office code with check digit. |
| `postal_code` | string | 6 digit postal office code. |
| `name_en` | string | English outlet name. |
| `name_zh` | string | Chinese outlet name. |
| `branch_name_zh` | string | Short Chinese branch label. |
| `county_city` | string | County or city in English. |
| `county_city_zh` | string | County or city in Chinese. |
| `district` | string | District or township in English. |
| `district_zh` | string | District or township in Chinese. |
| `postcode` | string | Postcode. |
| `address_en` | string | Address in English. |
| `address_zh` | string | Address in Chinese. |
| `phone_mail` | string | Mail counter phone. |
| `phone_savings` | string | Savings counter phone. |
| `fax` | string | Fax number. Null on 3 rows. |
| `weekday_mail_hours` | string | Monday to Friday mail hours. |
| `weekday_savings_hours` | string | Monday to Friday savings hours. |
| `saturday_hours` | string | Saturday hours. Null when closed. |
| `sunday_hours` | string | Sunday hours. Null when closed. |
| `weekday_service` | string | `yes` or `no`. |
| `weekday_extended` | string | `yes` or `no`. |
| `saturday_service` | string | `yes` or `no`. |
| `sunday_service` | string | `yes` or `no`. |
| `extended_mail_only` | string | `yes` or `no`. |
| `extended_mail_and_savings` | string | `yes` or `no`. |
| `hours_note` | string | Free text note in Chinese. Null on most rows. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `source` | string | Credit line for the row. |
| `updated` | string | Source dataset date, `YYYY-MM-DD`. |

## Examples

Exact office code. Taipei Beimen. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_post_offices" --data-urlencode "county_city=*" --data-urlencode "office_code=000100-6"
```

```json
[
  {
    "__id": 1,
    "office_code": "000100-6",
    "postal_code": "100000",
    "name_en": "Taipei Beimen Post Office(Taipei Branch 901)",
    "name_zh": "臺北北門郵局",
    "branch_name_zh": "臺北901支",
    "county_city": "Taipei City",
    "county_city_zh": "臺北市",
    "district": "Zhongzheng Dist.",
    "district_zh": "中正區",
    "postcode": "10044",
    "address_en": "No. 120, Sec. 1, Zhongxiao W. Rd., Zhongzheng Dist., Taipei 100003",
    "address_zh": "忠孝西路一段120號1樓",
    "phone_mail": "(02)2331-1374",
    "phone_savings": "(02)2381-3135",
    "fax": "(02)2314-9258",
    "weekday_mail_hours": "08:30-21:00",
    "weekday_savings_hours": "08:30-21:00",
    "saturday_hours": "09:00-12:00",
    "sunday_hours": null,
    "weekday_service": "yes",
    "weekday_extended": "yes",
    "saturday_service": "yes",
    "sunday_service": "no",
    "extended_mail_only": "no",
    "extended_mail_and_savings": "yes",
    "hours_note": "1.週一至週五 集郵服務中心：08:30-17:30 2.延時營業時間：17:30-21:00 3.營業時間及延時營業時間受理項目請參閱服務性質說明",
    "latitude": 25.047246,
    "longitude": 121.512212,
    "source": "Chunghwa Post, Nationwide Business Outlets (data.gov.tw 5950), Open Government Data License v1.0; English names and addresses from Chunghwa Post English locator",
    "updated": "2026-06-01"
  }
]
```

Taipei City Zhongzheng district. This call returns 24 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_post_offices" --data-urlencode "county_city=Taipei City" --data-urlencode "district=Zhongzheng*" | jq length
```

Open on Saturday. This call returns 273 rows:

```bash
curl -s "https://api.jael.ee/JLEE/tw_post_offices?county_city=*&saturday_service=yes" | jq length
```

Open on Sunday. This call returns 1 row (National Palace Museum Post Office):

```bash
curl -s "https://api.jael.ee/JLEE/tw_post_offices?county_city=*&sunday_service=yes" | jq -r '.[] | [.name_en, .county_city, .saturday_hours, .sunday_hours] | @tsv'
```

Map box over central Taipei. This call returns 115 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_post_offices" --data-urlencode "county_city=*" --data-urlencode "latitude=25.0..25.1" --data-urlencode "longitude=121.5..121.6" | jq length
```

## Data source

The outlets come from Chunghwa Post Co., Ltd., [Nationwide Business Outlets](https://data.gov.tw/dataset/5950) on data.gov.tw, updated 1 June 2026, fetched on 5 November 2026. Licensed under the [Open Government Data License, version 1.0 (OGDL v1.0)](https://data.gov.tw/license), which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Changes: English outlet names and addresses were added from the Chunghwa Post English post office locator; county and district English names were added from the Chunghwa Post county English table. The data is not guaranteed to be complete or accurate. Check hours with the outlet before you visit.

Credit lines:

* Source: Chunghwa Post Co., Ltd., Nationwide Business Outlets (https://data.gov.tw/dataset/5950), updated 1 June 2026, fetched 5 November 2026. Licensed under the Open Government Data License, version 1.0 (https://data.gov.tw/license).
* English names and addresses from the Chunghwa Post English locator.
* No logos. Not endorsed by Chunghwa Post.

[Back to the API list](../../README.md)
