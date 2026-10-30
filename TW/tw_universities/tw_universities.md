# Taiwan Universities

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 139](https://img.shields.io/badge/records-139-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license OGDL v1.0 and ODbL](https://img.shields.io/badge/license-OGDL%20v1.0%20%2B%20ODbL-green)

All 139 universities, colleges and junior colleges in Taiwan for the 2026 to 2027 school year (115 學年度), from the Ministry of Education (MOE) directory of higher education schools (大專校院名錄), in Traditional Chinese and English. Each row has the MOE school code, the official Chinese and English names, public or private, the school system (general, technical or teacher training), the kind of school, city or county, district, postal code, main campus address, phone, website and a map point. Filter by the required `city_en`, then by name, type, level, district or a coordinate range.

Rows by kind: university 121, junior college 12 and college 6. Rows by who runs them: private 92 and public 47. Rows by system: technical 73, general 61 and teacher training 5. The schools are in 21 cities and counties.

> **Use of this data.** School facts come from the MOE directory and map points from OpenStreetMap. Each row is the main campus only. This is a directory, not a ranking, and it does not cover courses, fees or admissions. Not affiliated with or endorsed by MOE or any agency. Check with the school.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_universities` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`city_en`). Optional: 20 fields. Disallowed: 5 fields.

`city_en` must be in every call. Use `city_en=*` to list all 139 schools. Disallowed means the field is returned in the output but cannot be used as a filter.

Only `lat` and `long` take ranges. Codes, postal codes and phone numbers are text, so filter them with a wildcard, such as `1*`. Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city_en` | Required | Yes | No | City or county, with the official postal spelling: `Taipei City` (24), `New Taipei City` (20), `Taichung City` (17), `Kaohsiung City` (14), `Tainan City` (13), `Taoyuan City` (12), `Hsinchu City` (5), `Pingtung County` (5) and 13 more. Wildcards work, so `city_en=*` lists all 139, `city_en=*County` lists the 30 in counties and `city_en=*City` the 109 in cities. |
| `school_code` | Optional | Yes | No | 4 digit MOE school code, kept as text so leading zeros stay, such as `0001` (1). Codes starting with `0` are national schools (46), `1` private (92) and `3` city run (1). |
| `name_zh` | Optional | Yes | No | School name in Traditional Chinese as written by MOE, such as `*科技大學*` (52). |
| `name_en` | Optional | Yes | No | Official English name from the MOE school directory, such as `*Science and Technology*` (21). Filled for every row. |
| `public_private` | Optional | Yes | No | Who runs the school: `private` (92) or `public` (47). |
| `public_private_zh` | Optional | Yes | No | The same in Chinese: `私立` (92) or `公立` (47). |
| `type` | Optional | Yes | No | MOE school system: `technical` (73), `general` (61) or `teacher training` (5). Technical means the technical and vocational system. |
| `type_zh` | Optional | Yes | No | School system in Chinese: `技職` (73), `一般` (61) or `師範` (5). |
| `level` | Optional | Yes | No | Kind of school, from its Chinese name: `university` (121), `junior college` (12) or `college` (6). |
| `level_zh` | Optional | Yes | No | Kind of school in Chinese: `大學` (121), `專科學校` (12) or `學院` (6). |
| `city_zh` | Optional | Yes | No | City or county in Chinese, such as `臺北市` (24). |
| `district_zh` | Optional | Yes | No | District or township in Chinese, such as `大安區` (5). |
| `district_en` | Optional | Yes | No | District or township in English, from Chunghwa Post spellings, such as `Da’an Dist.` (5) or `Jiaoxi Township` (1). See the notes on the apostrophe. |
| `postal_code` | Optional | Yes | No | 6 digit postal code, such as `116011` (1) or `106*` (5). |
| `address_zh` | Optional | Yes | No | Main campus street address in Chinese, as listed by MOE, such as `*大學路*` (10). |
| `address_en` | Optional | Yes | No | Address in English, built from Chunghwa Post road and district spellings (two from the school's own site), such as `*Daxue Rd.*` (6). Blank for 3 schools whose address uses a local place name with no postal spelling. |
| `phone` | Optional | Yes | No | Main phone number in international form, such as `+886 2 29393091` (1). |
| `phone_zh_format` | Optional | Yes | No | Phone number as MOE writes it, such as `(02)29393091` (1). `#` marks an extension (2 rows) and `~` a range of lines (3 rows). |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 22.485859 to 25.253669. Example: `lat=25..25.2` (39). |
| `long` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 118.322235 to 121.775731. Example: `long=121..122` (67). |
| `geocode_precision` | Optional | Yes | No | How the map point was found in OpenStreetMap: `campus` (135) or `street` (4). See the notes. |
| `name_en_source` | Disallowed | No | No | Web page where the English name was found. |
| `website` | Disallowed | No | No | School website. |
| `geocode_source` | Disallowed | No | No | OpenStreetMap link for the map point. |
| `source` | Disallowed | No | No | Source dataset of the row. |
| `source_as_of` | Disallowed | No | No | Date the sources were read, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `school_code` | string | 4 digit MOE school code. |
| `name_zh` | string | School name in Traditional Chinese. |
| `name_en` | string | Official English name. |
| `name_en_source` | string | Web page where the English name was found. |
| `public_private` | string | `public` or `private`. |
| `public_private_zh` | string | `公立` or `私立`. |
| `type` | string | `general`, `technical` or `teacher training`. |
| `type_zh` | string | `一般`, `技職` or `師範`. |
| `level` | string | `university`, `college` or `junior college`. |
| `level_zh` | string | `大學`, `學院` or `專科學校`. |
| `city_zh` | string | City or county in Chinese. |
| `city_en` | string | City or county in English. |
| `district_zh` | string | District or township in Chinese. |
| `district_en` | string | District or township in English. |
| `postal_code` | string | 6 digit postal code. |
| `address_zh` | string | Main campus address in Chinese. |
| `address_en` | string | Main campus address in English. Null for 3 rows. |
| `phone` | string | Main phone number in international form. |
| `phone_zh_format` | string | Phone number as MOE writes it. |
| `website` | string | School website. |
| `lat` | number | Latitude (WGS 84). |
| `long` | number | Longitude (WGS 84). |
| `geocode_precision` | string | `campus` or `street`. |
| `geocode_source` | string | OpenStreetMap link for the map point. |
| `source` | string | Source dataset of the row. |
| `source_as_of` | string | Date the sources were read, `YYYY-MM-DD`. |

## Notes

* Each row is the main campus as MOE lists it. Many schools have other campuses that are not listed.
* `school_code` starts with `0` for national schools (46), `1` for private schools (92) and `3` for the one city run school, University of Taipei. So `public` (47) is the 46 national schools plus University of Taipei.
* `level` comes from the end of the Chinese name: 大學 is a university, 學院 a college and 專科學校 a junior college.
* English names come from the MOE school and principal directory for 137 schools and from the school's own website for 2 (`name_en_source` shows which page).
* `district_en` uses the Chunghwa Post spelling, which has a curly apostrophe in names such as `Da’an Dist.`. A straight apostrophe (`Da'an Dist.`) returns 0 rows, so use a wildcard such as `Da*an*` if you cannot type the curly one.
* The 3 rows with a blank `address_en` are junior colleges in Miaoli, Taoyuan and Chiayi whose Chinese address uses a local place name with no postal spelling.
* `geocode_precision` tells you how far to trust the map point:
  * `campus` (135): the school itself as mapped in OpenStreetMap.
  * `street` (4): a point on the school's street, which can be up to a few kilometers off on a long road.
* `website` uses https for every school but 1, whose site could not be reached over https.
* The asterisk is the wildcard and cannot be escaped, so a filter value cannot match a literal `*`.

## Examples

One school by its code, shown as a full row. The leading zeros stay. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_universities" --data-urlencode "city_en=Taipei City" --data-urlencode "school_code=0001"
```

```json
[
  {
    "__id": 1,
    "school_code": "0001",
    "name_zh": "國立政治大學",
    "name_en": "National Chengchi University",
    "name_en_source": "https://udb.moe.edu.tw/ulist/Resource",
    "public_private": "public",
    "public_private_zh": "公立",
    "type": "general",
    "type_zh": "一般",
    "level": "university",
    "level_zh": "大學",
    "city_zh": "臺北市",
    "city_en": "Taipei City",
    "district_zh": "文山區",
    "district_en": "Wenshan Dist.",
    "postal_code": "116011",
    "address_zh": "臺北市文山區指南路二段64號",
    "address_en": "No. 64, Sec. 2, Zhinan Rd., Wenshan Dist., Taipei City 116011",
    "phone": "+886 2 29393091",
    "phone_zh_format": "(02)29393091",
    "website": "https://www.nccu.edu.tw/",
    "lat": 24.980209,
    "long": 121.574959,
    "geocode_precision": "campus",
    "geocode_source": "https://www.openstreetmap.org/relation/2946719",
    "source": "https://data.gov.tw/dataset/6091",
    "source_as_of": "2026-10-30"
  }
]
```

A wildcard on the required field with a partial Chinese name. Universities of science and technology (科技大學) in Taiwan's counties. This call returns 12 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_universities" --data-urlencode "city_en=*County" --data-urlencode "name_zh=*科技大學*" | jq -r '.[] | [.city_en, .name_zh, .name_en, .public_private] | @tsv'
```

```text
Yunlin County	國立雲林科技大學	National Yunlin University of Science & Technology	public
Pingtung County	國立屏東科技大學	National Pingtung University of Science and Technology	public
Yunlin County	國立虎尾科技大學	National Formosa University	public
Penghu County	國立澎湖科技大學	National Penghu University of Science and Technology	public
Hsinchu County	明新科技大學	Minghsin University of Science and Technology	private
Changhua County	建國科技大學	Chienkuo Technology University	private
Pingtung County	大仁科技大學	Tajen University	private
Nantou County	南開科技大學	Nan Kai University of Technology	private
Miaoli County	育達科技大學	Yu Da University of Science and Technology	private
Pingtung County	美和科技大學	Meiho University	private
Chiayi County	吳鳳科技大學	WuFeng University	private
Hsinchu County	敏實科技大學	Minth University of Science and Technology	private
```

A latitude and longitude range. Public schools in a box over central Taipei. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_universities" --data-urlencode "city_en=Taipei City" --data-urlencode "lat=25..25.05" --data-urlencode "long=121.5..121.55" --data-urlencode "public_private=public" | jq -r '.[] | [.name_zh, .name_en, .district_en, .lat, .long] | @tsv'
```

```text
國立臺灣大學	National Taiwan University	Da’an Dist.	25.016817	121.538457
國立臺灣師範大學	National Taiwan Normal University	Da’an Dist.	25.025758	121.526617
國立臺灣科技大學	National Taiwan University of Science and Technology	Da’an Dist.	25.013411	121.54162
國立臺北科技大學	National Taipei University of Technology	Da’an Dist.	25.043064	121.534688
國立臺北教育大學	National Taipei University of Education	Da’an Dist.	25.023446	121.545253
國立臺北商業大學	National Taipei University of Business	Zhongzheng Dist.	25.042352	121.525439
臺北市立大學	University of Taipei	Zhongzheng Dist.	25.03545	121.51327
```

## Data source

The list of schools, their codes, Chinese names, systems, addresses, phones and websites come from the MOE directory of higher education schools (大專校院名錄) for the 115 school year, data.gov.tw dataset 6091, updated 2026-08-03. English names and postal codes come from the MOE school and principal directory (大專校院一覽表) for the 114 school year. English addresses and districts are built from the Chunghwa Post romanization files. Map points come from OpenStreetMap. The English labels for type and level, the English addresses and the map points are set by us, and the data can go out of date. Check with the school.

Credit lines:

* Source: Ministry of Education (MOE), Taiwan, 大專校院名錄 (115 school year, data.gov.tw dataset 6091, updated 2026-08-03) and 大專校院一覽表 school and principal directory (114 school year) for English names and postal codes. Open Government Data License, version 1.0 (https://data.gov.tw/license).
* English addresses built from the Chunghwa Post romanization files.
* Coordinates © OpenStreetMap contributors, under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not affiliated with or endorsed by MOE or any agency.

[Back to the API list](../../README.md)
