# Taiwan Travel Agencies

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 4,251](https://img.shields.io/badge/records-4%2C251-lightgrey) ![updated 2026-11-13](https://img.shields.io/badge/updated-2026--11--13-orange) ![license OGDL 1.0](https://img.shields.io/badge/license-OGDL%201.0-green)

4,251 licensed travel agency offices in Taiwan, 3,503 headquarters and 748 branches, from the Tourism Administration register of 13 November 2026. Each row has the license number, tax ID, Chinese and English names, city and district, address, landline phone, license class (General, Class A or Class B), operating status, Travel Quality Assurance membership, performance bond and guarantee deposit. No field is required, so you can filter by city, class, office type, name, date or a performance bond range.

The largest cities are Taipei City (1,486), Taichung City (568), Kaohsiung City (544), Taoyuan City (275), Tainan City (252) and New Taipei City (252). License classes are Class A (3,336), General (587) and Class B (328). 4,142 offices are Travel Quality Assurance Association members. One office is suspended: 上順旅行社台南分公司. Owner and manager names, and personal mobile numbers, are left out.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_travel_agencies` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per office. All matches are returned. |

## Query parameters

Required: none. Optional: 26 fields. Disallowed: 1 field.

A call with no filters returns all 4,251 records (about 3.5 MB). Partial filters accept `*`, such as `name_zh=*雄獅*` or `established_date=2026*`. `performance_bond` and `guarantee_deposit` accept a range with two dots, such as `performance_bond=0..0`. Text filters ignore case. `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `registration_number` | Optional | Yes | No | Six-digit license number, kept as text so leading zeros remain, such as `000300` or `000804`. Unique on every row. Headquarters often end in 00. Partial wildcards allowed, such as `0008*`. |
| `tax_id` | Optional | Yes | No | Company uniform invoice number (統一編號), kept as text. Unique on 4,250 rows; 航典國際旅行社 and its Taichung branch share `00142519`. One value has 7 digits; the rest have 8. Never blank. Partial wildcards allowed. |
| `category` | Optional | No | No | License class in English: `General` (587), `Class A` (3,336) or `Class B` (328), from the official 綜合, 甲種 and 乙種 labels. Exact match. |
| `category_zh` | Optional | No | No | License class in Chinese as published: `甲種` (3,336), `綜合` (587) or `乙種` (328). Exact match. |
| `name_zh` | Optional | Yes | No | Agency office name in Chinese, such as `首都旅行社` or `雄獅旅行社台北分公司`. Unique on every row. Never blank. Partial wildcards allowed, such as `*雄獅*` (42). |
| `name_en` | Optional | Yes | No | English name as the register published it, on 3,324 rows. Blank on 927 rows where the source had no real English name (a single space, or in one case a Chinese street address). Spellings are kept as published, including typos. Partial wildcards allowed, such as `*Travel*`. |
| `formal_name` | Optional | Yes | No | Formal company name in Chinese, such as `首都旅行社股份有限公司` or `首都旅行社股份有限公司台南分公司`. Unique on every row. Never blank. Partial wildcards allowed. |
| `city` | Optional | Yes | No | City or county in English, from Chunghwa Post spellings, 22 values. The largest are Taipei City (1,486), Taichung City (568), Kaohsiung City (544), Taoyuan City (275), Tainan City (252) and New Taipei City (252). `city=*Taipei*` matches both Taipei City and New Taipei City (1,738). Partial wildcards allowed. |
| `city_zh` | Optional | Yes | No | City or county in Chinese, using the official 臺 form, such as `臺北市` (1,486) or `高雄市` (544). Partial wildcards allowed. |
| `district` | Optional | Yes | No | District, township or county-administered city in English, from Chunghwa Post spellings, such as Zhongshan Dist., Luodong Township or Magong City. A few names use a typographic apostrophe, such as Da’an Dist. (102, in Taipei City and Taichung City) and Su’ao Township. Partial wildcards allowed. |
| `district_zh` | Optional | Yes | No | District, township or county-administered city in Chinese, such as `中山區` or `大安區` (102). Partial wildcards allowed. |
| `postcode` | Optional | Yes | No | Postcode as text, 3 digits on 3,058 rows and 5 digits on 1,134 rows, such as `10446` or `709*`. Blank on 59 rows (9 were blank in the source; 50 other values were area codes or invalid strings and were not kept). Partial wildcards allowed. |
| `address` | Optional | Yes | No | Full street address in Chinese, such as `臺北市中山區民權東路2段55號2樓`. Built from the city, district and street in the register, without the commas the XML used as separators. Never blank. Partial wildcards allowed, such as `*中山北路*`. |
| `phone` | Optional | Yes | No | Company landline, usually with the area code, such as `02 25651515` or `06*`. Taiwan mobile numbers (starting 09) were dropped; 180 offices listed only a mobile and are blank, and 53 had no number, so 233 rows have no phone. Two rows that mixed a landline and a mobile keep only the landline. Partial wildcards allowed. |
| `established_date` | Optional | Yes | No | Date the office was established, as text YYYY-MM-DD, from 1950-04-25 to 2026-10-21. Filter by an exact date or a prefix, such as `2026*` (105) or `1970*` (8). Range does not work on this field. |
| `operating_status` | Optional | No | No | `active` on 4,250 rows, or `suspended` when the stop date is later than the reopen date. Only 上順旅行社台南分公司 (`081306`) is suspended. Exact match. |
| `stop_date` | Optional | Yes | No | Date the office last stopped operating, as text YYYY-MM-DD, on 42 rows. Blank on the rest. Filter by an exact date or a prefix, such as `2024*`. Range does not work on this field. |
| `reopen_date` | Optional | Yes | No | Date the office last reopened, as text YYYY-MM-DD, on 127 rows. Blank on the rest. Stored as text. Range does not work on this field. |
| `quality_assurance` | Optional | No | No | `Yes` if the office is a Travel Quality Assurance Association member (4,142), `No` if the register marks it as not a member (41). Blank on 68 rows where the source had no flag. Exact match. |
| `performance_bond` | Optional | No | Yes | Performance guarantee insurance amount in TWD, from 0 to 1,900,000,000. 931 offices have 0, often branches covered under the head office. Range uses two dots, such as `0..0` or `10000000..9999999999` (3,032). A hyphen returns HTTP 400. |
| `guarantee_deposit` | Optional | No | Yes | Current guarantee deposit total in TWD, from 0 to 12,400,000. 16 offices have 0. Range uses two dots, such as `0..0`. A hyphen returns HTTP 400. |
| `performance_bond_start` | Optional | Yes | No | Start date of the performance guarantee insurance, as text YYYY-MM-DD, on 3,320 rows (2025 or 2026). Blank on 931 rows, matching a 0 bond amount. Filter by an exact date or a prefix. Range does not work on this field. |
| `performance_bond_end` | Optional | Yes | No | End date of the performance guarantee insurance, as text YYYY-MM-DD, on 3,320 rows (2026 or 2027). Blank on 931 rows. Filter by an exact date or a prefix. Range does not work on this field. |
| `awards` | Optional | Yes | No | Current sustainability or environmental marks, on 78 offices (all marked valid in the source): 環保旅行業, 綠色旅行標章 and GSTC, with the class in parentheses, such as `環保旅行業 (銀級環保旅行業)`. Some offices have more than one mark, joined with a semicolon. Blank on the rest. Partial wildcards allowed, such as `*環保旅行業*` (73). |
| `office_type` | Optional | No | No | `HQ` (3,503) or `Branch` (748). A row is Branch when 分公司 appears in the Chinese or formal name, matching the official branch count. Exact match. |
| `parent_registration_number` | Optional | Yes | No | License number of the head office, on 744 branch rows, such as `000800` for 首都旅行社台南分公司. Blank on all headquarters and on 4 Taiwan offices of foreign firms that have no parent in this file. Kept as text. Partial wildcards allowed. |
| `source` | Disallowed | No | No | Tourism Administration, MOTC, Basic travel industry information (data.gov.tw dataset 72074), generated 2026-11-13, in all 4,251 rows. Not filterable. |

## Response fields

`__id`, `performance_bond` and `guarantee_deposit` are integers. All other values are strings. Blank fields come back as `null`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `registration_number` | string | Six-digit license number, as text, so leading zeros remain. |
| `tax_id` | string | Company uniform invoice number, as text. |
| `category` | string | License class in English: `General`, `Class A` or `Class B`. |
| `category_zh` | string | License class in Chinese: `綜合`, `甲種` or `乙種`. |
| `name_zh` | string | Agency office name in Chinese. |
| `name_en` | string | English name as the register published it. Null on 927 rows. |
| `formal_name` | string | Formal company name in Chinese. |
| `city` | string | City or county in English, from Chunghwa Post spellings. |
| `city_zh` | string | City or county in Chinese, using the official 臺 form. |
| `district` | string | District, township or county-administered city in English. |
| `district_zh` | string | District, township or county-administered city in Chinese. |
| `postcode` | string | Postcode as text, 3 or 5 digits. Null on 59 rows. |
| `address` | string | Full street address in Chinese. |
| `phone` | string | Company landline, usually with the area code. Null on 233 rows. |
| `established_date` | string | Date the office was established, YYYY-MM-DD. |
| `operating_status` | string | `active` or `suspended`. |
| `stop_date` | string | Date the office last stopped operating, YYYY-MM-DD. Null on most rows. |
| `reopen_date` | string | Date the office last reopened, YYYY-MM-DD. Null on most rows. |
| `quality_assurance` | string | `Yes` or `No`. Null on 68 rows. |
| `performance_bond` | integer | Performance guarantee insurance amount in TWD. |
| `guarantee_deposit` | integer | Current guarantee deposit total in TWD. |
| `performance_bond_start` | string | Start date of the performance guarantee insurance, YYYY-MM-DD. Null when the bond amount is 0. |
| `performance_bond_end` | string | End date of the performance guarantee insurance, YYYY-MM-DD. Null when the bond amount is 0. |
| `awards` | string | Current sustainability or environmental marks. Null on most rows. |
| `office_type` | string | `HQ` or `Branch`. |
| `parent_registration_number` | string | License number of the head office, as text. Null on headquarters and on 4 Taiwan offices of foreign firms. |
| `source` | string | Tourism Administration, MOTC, Basic travel industry information (data.gov.tw dataset 72074), generated 2026-11-13. |

## Data notes

* Owner and manager names, and personal mobile numbers, are left out. 180 offices listed only a mobile and have a blank phone.
* The official office-count table of 31 October 2026 listed 4,304 offices (3,556 headquarters and 748 branches). The register snapshot of 13 November 2026 has 4,251 offices: the 748 branches still match, and there are 53 fewer headquarters. City totals can differ by a few offices from that table.
* Four Taiwan offices of foreign firms are marked Branch and have no parent registration number in this file.
* Dates in the source were already Gregorian calendar dates; only the format was changed to YYYY-MM-DD. One office is marked suspended (上順旅行社台南分公司): its stop date is 2001-11-15 and its reopen date is the day before.
* English city and district names follow Chunghwa Post spellings. 員林市 and 頭份市, which were upgraded from townships to county-administered cities after the post office list was issued, are written Yuanlin City and Toufen City. A few names use a typographic apostrophe, such as Da’an Dist. and Su’ao Township. English agency names are kept only when the register itself supplied one (3,324 rows); none were invented.
* 50 postcode values that were area codes or other invalid strings were left blank; 3-digit and 5-digit postcodes are kept as published. There are no coordinates.
* The listing on [data.gov.tw](https://data.gov.tw/dataset/72074) is updated daily.

## Examples

Offices whose Chinese name contains 雄獅 (Lion Travel). This call returns 42 records: the head office in Taipei and 41 branches. First record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_travel_agencies' --data-urlencode 'name_zh=*雄獅*'
```

```json
[
  {
    "__id": 751,
    "registration_number": "201600",
    "tax_id": "04655091",
    "category": "General",
    "category_zh": "綜合",
    "name_zh": "雄獅旅行社",
    "name_en": "Lion Travel Service Co., Ltd.",
    "formal_name": "雄獅旅行社股份有限公司",
    "city": "Taipei City",
    "city_zh": "臺北市",
    "district": "Neihu Dist.",
    "district_zh": "內湖區",
    "postcode": "11469",
    "address": "臺北市內湖區石潭路151號9樓",
    "phone": "02 87939000",
    "established_date": "1977-07-01",
    "operating_status": "active",
    "stop_date": null,
    "reopen_date": null,
    "quality_assurance": "Yes",
    "performance_bond": 244000000,
    "guarantee_deposit": 2230000,
    "performance_bond_start": "2026-07-01",
    "performance_bond_end": "2027-01-01",
    "awards": "環保旅行業 (銀級環保旅行業); GSTC (GSTC-Certified)",
    "office_type": "HQ",
    "parent_registration_number": null,
    "source": "Tourism Administration, MOTC, Basic travel industry information (data.gov.tw dataset 72074), generated 2026-11-13"
  }
]
```

Offices with a performance bond of 0. This call returns 931 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_travel_agencies' --data-urlencode 'performance_bond=0..0'
```

## Data source

Source: Tourism Administration, Ministry of Transportation and Communications (MOTC), Basic travel industry information (旅行業基本資料), [data.gov.tw dataset 72074](https://data.gov.tw/dataset/72074). Official XML: <https://travelagency.tad.gov.tw/Forms/XML/TravelAgent.aspx?key=DF8T1NkUg>. Data generated 13 November 2026. Used under the [Open Government Data License, version 1.0](https://data.gov.tw/license). This license lets anyone use, copy, change and share the data for free, for any purpose, including commercial use, as long as the source is credited. English city and district names follow Chunghwa Post spellings, also under the Open Government Data License, version 1.0.

Credit: Tourism Administration, Ministry of Transportation and Communications, Basic travel industry information (data.gov.tw dataset 72074), generated 13 November 2026. Used under the Open Government Data License, version 1.0. This use does not imply endorsement by the Tourism Administration or MOTC. No agency logo is used.

[Back to the API list](../../README.md)
