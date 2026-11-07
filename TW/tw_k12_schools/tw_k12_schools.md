# Taiwan K-12 Schools

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access Public](https://img.shields.io/badge/access-Public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 3,855](https://img.shields.io/badge/records-3%2C855-lightgrey) ![updated 2026-09-24](https://img.shields.io/badge/updated-2026--09--24-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

3,855 elementary, junior high and senior high schools in Taiwan for school year 115 (2026-27), from the Ministry of Education school lists. Each row has the school code, name, public or private status, county, district, address, phone and website in Traditional Chinese. Choose a required level, then filter by ownership, county, school code, or part of a name, district or address.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_k12_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`level`). Optional: 6 fields. Disallowed: 5 fields.

`level` must be in every call. A call with no `level` returns HTTP 400. Disallowed means the field is returned in the output but cannot be used as a filter. Filtering on a disallowed field returns HTTP 400. URL encode Chinese values, for example with `curl -G --data-urlencode`.

| Parameter | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `level` | Required | No | No | Exact school level: `elementary` (2,609), `junior_high` (740) or `senior_high` (506). |
| `ownership` | Optional | No | No | Exact value: `公立` (public) or `私立` (private). |
| `county` | Optional | No | No | Exact county or city in Traditional Chinese, 22 values, such as `臺北市`. Use `臺`, not `台`. |
| `school_code` | Optional | No | No | Exact 6 character MoE school code, such as `011601`. Leading zeros are kept and some codes hold a letter, such as `361B09`. One campus can share a code across levels, so pair it with `level`. |
| `name` | Optional | Yes | No | School name in Traditional Chinese as published. Wildcards work, such as `*大安*`. |
| `district` | Optional | Yes | No | Township, city or district taken from the address, such as `*大安區*`. Some names repeat across counties (for example `東區`), so pair it with `county`. |
| `address` | Optional | Yes | No | Street address in Traditional Chinese, without the postal code. Wildcards work, such as `*信義路*`. |
| `school_year` | Disallowed | No | No | ROC school year as text, `115` on every row. |
| `phone` | Disallowed | No | No | School phone as published, area code in brackets and extension after `#`. Country code +886. |
| `website` | Disallowed | No | No | School website. |
| `source` | Disallowed | No | No | data.gov.tw dataset page for the level. |
| `updated` | Disallowed | No | No | Date of the source file, as `YYYY-MM-DD` text. |

* Parameters are combined with AND. See [How filters work](../../README.md#how-filters-work) for the wildcard syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `level` | string | `elementary`, `junior_high` or `senior_high`. |
| `school_year` | string | ROC school year, `115`. |
| `school_code` | string | MoE school code, as text. |
| `name` | string | School name in Traditional Chinese. |
| `ownership` | string | `公立` (public) or `私立` (private). |
| `county` | string | County or city. |
| `district` | string | Township, city or district. |
| `address` | string | Street address, without the postal code. |
| `phone` | string | School phone as published. |
| `website` | string | School website. Null for 2 rows. |
| `source` | string | data.gov.tw dataset page. |
| `updated` | string | Date of the source file. |

## Examples

One school by its code. This call returns 1 row, 私立育才國小 in 新北市永和區:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_k12_schools" --data-urlencode "level=elementary" --data-urlencode "school_code=011601"
```

Response:

```json
[
  {
    "level": "elementary",
    "school_year": "115",
    "school_code": "011601",
    "name": "私立育才國小",
    "ownership": "私立",
    "county": "新北市",
    "district": "永和區",
    "address": "新北市永和區福和路125巷20號",
    "phone": "(02)29214630",
    "website": "https://www.ytes.ntpc.edu.tw",
    "source": "https://data.gov.tw/dataset/6087",
    "updated": "2026-08-28"
  }
]
```

Senior high schools in any 大安區. This call returns 9 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_k12_schools" --data-urlencode "level=senior_high" --data-urlencode "district=*大安區*" | jq -r '.[].name'
```

Private senior high schools. This call returns 193 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_k12_schools" --data-urlencode "level=senior_high" --data-urlencode "ownership=私立" | jq length
```

Elementary schools in Taipei City. This call returns 147 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_k12_schools" --data-urlencode "level=elementary" --data-urlencode "county=臺北市" | jq length
```

## Data notes

* All rows are for school year 115 (2026-27). The junior high source file holds many years, and we kept only year 115.
* Counts by level: elementary 2,609 (2,575 public, 34 private), junior high 740 (726 public, 14 private), senior high 506 (313 public, 193 private).
* A school that teaches more than one level, such as a 完全中學 or 國中(小), shows up once in each level list. The same campus can appear under more than one `level`.
* We removed the bracket codes from county names and the postal codes from addresses. `district` is taken from the address, right after the county. Every row has one.
* Names and places are in Traditional Chinese only, as published. Counties use `臺`, not `台`, so search for `臺北市`, not `台北市`.
* Websites: we tested every http link over https on 8 November 2026. 661 links worked and now use https. 146 rows stay on http because their sites do not work over https, and 2 rows have no website.
* No mobile numbers were found in the phone field. The senior high file's notes column was not kept.
* `updated` is the date of each source file: 2026-08-28 for elementary, 2026-08-04 for junior high and 2026-09-24 for senior high. It does not mean each school record changed that day.

## License

The data comes from the Ministry of Education (MOE), Taiwan, Department of Statistics school lists on data.gov.tw:

* Elementary schools: [dataset 6087](https://data.gov.tw/dataset/6087)
* Junior high schools: [dataset 6088](https://data.gov.tw/dataset/6088)
* Senior high schools: [dataset 6089](https://data.gov.tw/dataset/6089)

It is used under the [Open Government Data License, version 1.0](https://data.gov.tw/license) (OGDL v1.0). The license lets you copy, adapt, share and use the data, including for business, as long as you credit the Ministry of Education and link to the dataset. The data comes with no warranty.

We changed the data: we joined the three level lists, kept only school year 115, removed bracket codes and postal codes, added the district and level fields, and moved working websites to https. The Ministry of Education does not endorse this API.

Credit line:

* Source: Ministry of Education (MOE), Taiwan, school lists for school year 115 (data.gov.tw datasets 6087, 6088 and 6089). Open Government Data License, version 1.0 (https://data.gov.tw/license).

[Back to the API list](../../README.md)
