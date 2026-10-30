# Taiwan Emergency Hospitals

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 205](https://img.shields.io/badge/records-205-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license OGDL v1.0 and ODbL](https://img.shields.io/badge/license-OGDL%20v1.0%20%2B%20ODbL-green)

Taiwan's 205 designated emergency responsibility hospitals (急救責任醫院) from the Ministry of Health and Welfare (MOHW) list of 2026, in Traditional Chinese and English. Each row has the National Health Insurance (NHI) code, name, emergency care region, city or county, district, postal code, address, phone, overall level (severe, moderate or general), the rating in each of six chapters (emergency care, stroke, heart attack, trauma, high risk pregnancy and newborn care, and intensive care), the dates the rating holds and a map point. Filter by the required `city_en`, then by level, chapter, region, district, name or a coordinate range.

Rows by overall level: general 76, moderate 75 and severe 54. Severe is the highest. Rows by region: Taipei 51, Central 44, Kaohsiung and Pingtung 40, Southern 29, Northern 28 and Eastern 13.

> **Use of this data.** In an emergency in Taiwan, call 119. This list shows how MOHW rates each hospital's emergency care. It does not show wait times, beds or which hospital an ambulance will take you to. Not affiliated with or endorsed by MOHW or any agency.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_emergency_hospitals` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`city_en`). Optional: 26 fields. Disallowed: 8 fields.

`city_en` must be in every call. Use `city_en=*` to list all 205 hospitals. Disallowed means the field is returned in the output but cannot be used as a filter.

Dates are text in the form `YYYY-MM-DD`, so filter them with a prefix wildcard, such as `2029*`. Only `lat` and `long` take ranges. Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city_en` | Required | Yes | No | City or county, with the official postal spelling: `Taichung City` (23), `Kaohsiung City` (23), `Taipei City` (19), `New Taipei City` (19), `Pingtung County` (15), `Changhua County` (14), `Tainan City` (13), `Taoyuan City` (11) and 14 more. Wildcards work, so `city_en=*` lists all 205, `city_en=*County` lists the 83 in counties and `city_en=*City` the 122 in cities. |
| `hospital_code` | Optional | Yes | No | 10 digit NHI facility code, kept as text so leading zeros stay, such as `1111060015` (1). Some campuses share one code with their main hospital, so 15 rows carry a code that is on more than one row, such as `0101090517` (5 Taipei City Hospital branches). |
| `name_zh` | Optional | Yes | No | Full hospital name in Traditional Chinese, as written in the MOHW list, such as `*長庚*` (8). |
| `name_en` | Optional | Yes | No | English name, taken only from the hospital's own or its operator's website, such as `*Chang Gung*` (6). Blank for 84 hospitals with no English name found on an official site. |
| `region_zh` | Optional | Yes | No | Emergency care region in Chinese: `臺北區` (51), `中區` (44), `高屏區` (40), `南區` (29), `北區` (28) or `東區` (13). |
| `region_en` | Optional | Yes | No | Emergency care region in English: `Taipei` (51), `Central` (44), `Kaohsiung and Pingtung` (40), `Southern` (29), `Northern` (28) or `Eastern` (13). |
| `city_zh` | Optional | Yes | No | City or county in Chinese, such as `臺中市` (23). 22 are used. |
| `district_zh` | Optional | Yes | No | District or township in Chinese, such as `安樂區` (1). |
| `district_en` | Optional | Yes | No | District or township in English, from Chunghwa Post spellings, such as `Anle Dist.` or `Luodong Township` (2). |
| `postal_code` | Optional | Yes | No | 3 digit postal code of the district, such as `204` (1) or `1*` (19). |
| `address_zh` | Optional | Yes | No | Street address in Chinese, from the city health department list or the NHI register, such as `*中正路*` (10). Only the first address is kept when a hospital lists several. |
| `address_en` | Optional | Yes | No | Street address in English, built from Chunghwa Post road and district spellings, such as `*Zhongzheng Rd.*` (10). Blank for 4 rows whose address uses a local place name with no postal spelling. |
| `phone` | Optional | Yes | No | Main phone number in international form, such as `+886 2 24313131` (1). Blank for 5 campuses with no number of their own in the sources. In an emergency in Taiwan, call 119. |
| `level` | Optional | Yes | No | Overall emergency care level set by MOHW: `general` (76), `moderate` (75) or `severe` (54). Severe is the highest. |
| `level_zh` | Optional | Yes | No | Overall level as written in Chinese: `一般級` (76), `重度級` (54), `中度級*` (40) or `中度級` (35). See the notes on the asterisk. |
| `er` | Optional | Yes | No | Rating in the emergency care chapter: `moderate` (82), `severe` (64) or `general` (59). |
| `stroke` | Optional | Yes | No | Rating in the acute stroke chapter: `severe` (72), `general` (71) or `moderate` (62). |
| `cardiac` | Optional | Yes | No | Rating in the acute coronary syndrome (heart attack) chapter: `severe` (71), `general` (70) or `moderate` (64). |
| `trauma` | Optional | Yes | No | Rating in the major trauma chapter: `moderate` (76), `general` (72) or `severe` (57). |
| `pregnancy_newborn` | Optional | Yes | No | Rating in the high risk pregnancy and newborn care chapter: `general` (73), `severe` (57), `not rated` (40) or `moderate` (35). Not rated means the hospital did not apply for this chapter. |
| `icu` | Optional | Yes | No | Rating in the intensive care chapter: `moderate` (82), `severe` (62) or `general` (61). |
| `valid_from` | Optional | Yes | No | First day the rating holds, such as `2025*` (44). Blank for 57 general level hospitals listed with no dates. |
| `valid_until` | Optional | Yes | No | Last day the rating holds, such as `2029*` (52) or `2026*` (17). Blank for 57 general level hospitals listed with no dates. |
| `list_updated` | Optional | Yes | No | Date MOHW last updated this part of the list: `2026-02-12` (114), `2026-04-29` (51) or `2026-06-24` (40). |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 21.987786 to 26.16113. Example: `lat=25..25.2` (36). |
| `long` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 118.418062 to 121.848914. Example: `long=121..122` (79). |
| `geocode_precision` | Optional | Yes | No | How the map point was found in OpenStreetMap: `named place` (160) or `street` (45). See the notes. |
| `name_en_source` | Disallowed | No | No | Web page (https) where the English name was found. Blank when `name_en` is blank. |
| `joint_name_zh` | Disallowed | No | No | Name in Chinese of the children's hospital rated together with this hospital, as listed by MOHW. Filled for 5 rows. |
| `joint_hospital_code` | Disallowed | No | No | NHI facility code of that children's hospital. Filled for 5 rows. |
| `level_note` | Disallowed | No | No | Explains the asterisk on 40 moderate hospitals. Blank on all other rows. |
| `addon_capabilities_zh` | Disallowed | No | No | Extra chapter ratings above the overall level, in Chinese as in the list, such as `具備急性腦中風重度級能力`. Filled for 44 rows. |
| `addon_capabilities_en` | Disallowed | No | No | The same extra ratings in English, such as `Severe level in acute stroke`. Filled for 44 rows. |
| `source` | Disallowed | No | No | Sources of the row (https), with the OpenStreetMap link for the map point. |
| `source_as_of` | Disallowed | No | No | Date the sources were read, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `hospital_code` | string | 10 digit NHI facility code, as text. |
| `name_zh` | string | Hospital name in Traditional Chinese. |
| `name_en` | string | English name from an official website. Null for 84 rows. |
| `name_en_source` | string | Page where the English name was found. Null for 84 rows. |
| `joint_name_zh` | string | Children's hospital rated together with this one. Null for 200 rows. |
| `joint_hospital_code` | string | NHI code of that children's hospital. Null for 200 rows. |
| `region_zh` | string | Emergency care region in Chinese. |
| `region_en` | string | Emergency care region in English. |
| `city_zh` | string | City or county in Chinese. |
| `city_en` | string | City or county in English. |
| `district_zh` | string | District or township in Chinese. |
| `district_en` | string | District or township in English. |
| `postal_code` | string | 3 digit postal code, as text. |
| `address_zh` | string | Street address in Chinese. |
| `address_en` | string | Street address in English. Null for 4 rows. |
| `phone` | string | Main phone number in international form. Null for 5 rows. |
| `level` | string | Overall level: `severe`, `moderate` or `general`. |
| `level_zh` | string | Overall level in Chinese. |
| `level_note` | string | Note on the asterisk. Null for 165 rows. |
| `addon_capabilities_zh` | string | Extra chapter ratings in Chinese. Null for 161 rows. |
| `addon_capabilities_en` | string | Extra chapter ratings in English. Null for 161 rows. |
| `er` | string | Emergency care chapter rating. |
| `stroke` | string | Acute stroke chapter rating. |
| `cardiac` | string | Acute coronary syndrome chapter rating. |
| `trauma` | string | Major trauma chapter rating. |
| `pregnancy_newborn` | string | High risk pregnancy and newborn care chapter rating, or `not rated`. |
| `icu` | string | Intensive care chapter rating. |
| `valid_from` | string | First day the rating holds, `YYYY-MM-DD`. Null for 57 rows. |
| `valid_until` | string | Last day the rating holds, `YYYY-MM-DD`. Null for 57 rows. |
| `list_updated` | string | Date MOHW last updated this part of the list. |
| `lat` | number | Latitude (WGS 84). |
| `long` | number | Longitude (WGS 84). |
| `geocode_precision` | string | `named place` or `street`. |
| `source` | string | Sources of the row, with the OpenStreetMap link. |
| `source_as_of` | string | Date the sources were read. |

Notes:

* Chapter ratings: each chapter field is the higher of the overall level and any extra rating the list gives for that chapter (`addon_capabilities_en`). For example, a moderate hospital with "Severe level in acute stroke" has `stroke` set to `severe`.
* The asterisk: 40 moderate hospitals are listed as `中度級*` because they did not apply for a rating in the high risk pregnancy and newborn care chapter, so `pregnancy_newborn` is `not rated`. The API reads `*` as a wildcard even when it is sent as `%2A`, so `level_zh=中度級%2A` returns all 75 moderate hospitals, and `level_zh=中度級` returns only the 35 without the asterisk. To get just the 40, use `pregnancy_newborn=not rated`.
* Shared codes: 15 rows carry an NHI code that is also on another row, because some campuses share one code with their main hospital. The 5 Taipei City Hospital branches all use `0101090517`.
* Ratings that end soon: 17 hospitals have `valid_until` 2026-12-31. Their ratings will be refreshed when MOHW updates the list, expected in January 2027.
* `geocode_precision`: `named place` (160) is the hospital itself as mapped in OpenStreetMap. `street` (45) is a point on the hospital's street, which can be up to a few kilometers off on a long road.
* English names are used only when the hospital or its operator gives one on an official website. No names were translated by us.

## Examples

One hospital by NHI code, shown as a full row. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_emergency_hospitals" --data-urlencode "city_en=Keelung City" --data-urlencode "hospital_code=1111060015"
```

```json
[
  {
    "__id": 1,
    "hospital_code": "1111060015",
    "name_zh": "長庚醫療財團法人基隆長庚紀念醫院",
    "name_en": "Chang Gung Memorial Hospital, Keelung",
    "name_en_source": "https://www.cgmh.org.tw/eng/research/branch",
    "joint_name_zh": null,
    "joint_hospital_code": null,
    "region_zh": "臺北區",
    "region_en": "Taipei",
    "city_zh": "基隆市",
    "city_en": "Keelung City",
    "district_zh": "安樂區",
    "district_en": "Anle Dist.",
    "postal_code": "204",
    "address_zh": "基隆市安樂區麥金路222號",
    "address_en": "No. 222, Maijin Rd., Anle Dist., Keelung City 204",
    "phone": "+886 2 24313131",
    "level": "severe",
    "level_zh": "重度級",
    "level_note": null,
    "addon_capabilities_zh": null,
    "addon_capabilities_en": null,
    "er": "severe",
    "stroke": "severe",
    "cardiac": "severe",
    "trauma": "severe",
    "pregnancy_newborn": "severe",
    "icu": "severe",
    "valid_from": "2025-01-01",
    "valid_until": "2030-12-31",
    "list_updated": "2026-04-29",
    "lat": 25.120963,
    "long": 121.722153,
    "geocode_precision": "named place",
    "source": "MOHW emergency hospital list https://www.mohw.gov.tw/dl-97025-23d029a4-14c2-4ae9-a057-7a32d3885839.html; NHIA contracted institutions https://info.nhi.gov.tw/api/iode0000s01/Dataset?rId=A21030000I-D2100G-001; map point https://www.openstreetmap.org/relation/7765115",
    "source_as_of": "2026-10-30"
  }
]
```

A wildcard on the required field. Severe level hospitals in Taiwan's counties. This call returns 16 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_emergency_hospitals" --data-urlencode "city_en=*County" --data-urlencode "level=severe" | jq -r '.[] | [.city_en, .name_zh, .name_en, .phone] | @tsv'
```

```text
Yilan County	國立陽明交通大學附設醫院	National Yang Ming Chiao Tung University Hospital	+886 3 9325192
Yilan County	醫療財團法人羅許基金會羅東博愛醫院	Lotung Poh-Ai Hospital	+886 3 9543131
Hsinchu County	東元醫療社團法人東元綜合醫院	Ton-Yen General Hospital	+886 3 5527000
Miaoli County	為恭醫療財團法人為恭紀念醫院		+886 37 676811
Miaoli County	大千綜合醫院		+886 37 357125
Changhua County	秀傳醫療財團法人彰濱秀傳紀念醫院	Chang Bing Show Chwan Memorial Hospital	+886 4 7813888
Changhua County	秀傳醫療社團法人秀傳紀念醫院	Show Chwan Memorial Hospital	+886 4 7256166
Changhua County	彰化基督教醫療財團法人彰化基督教醫院	Changhua Christian Hospital	+886 4 7238595
Nantou County	衛生福利部南投醫院	Nantou Hospital, Ministry of Health and Welfare	+886 49 2231150
Yunlin County	國立臺灣大學醫學院附設醫院雲林分院		+886 5 5323911
Chiayi County	長庚醫療財團法人嘉義長庚紀念醫院	Chang Gung Memorial Hospital, Chiayi	+886 5 3621000
Chiayi County	佛教慈濟醫療財團法人大林慈濟醫院	Dalin Tzu Chi Hospital	+886 5 2648000
Pingtung County	屏基醫療財團法人屏東基督教醫院	Pingtung Christian Hospital	+886 8 7368686
Pingtung County	安泰醫療社團法人安泰醫院		+886 8 8329966
Hualien County	佛教慈濟醫療財團法人花蓮慈濟醫院	Hualien Tzu Chi Hospital	+886 3 8561825
Taitung County	台灣基督長老教會馬偕醫療財團法人台東馬偕紀念醫院	Taitung MacKay Memorial Hospital	+886 89 310150
```

A latitude and longitude range with a chapter filter. Hospitals rated severe for stroke in a box over central Taichung. This call returns 5 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_emergency_hospitals" --data-urlencode "city_en=Taichung City" --data-urlencode "lat=24.1..24.2" --data-urlencode "long=120.6..120.7" --data-urlencode "stroke=severe" | jq -r '.[] | [.name_zh, .name_en, .district_en, .level, .stroke, .lat, .long] | @tsv'
```

```text
澄清綜合醫院中港分院		Xitun Dist.	severe	severe	24.182962	120.617061
中山醫學大學附設醫院		South Dist.	severe	severe	24.121566	120.650135
臺中榮民總醫院	Taichung Veterans General Hospital	Xitun Dist.	severe	severe	24.184553	120.60476
中國醫藥大學附設醫院	China Medical University Hospital	North Dist.	severe	severe	24.154015	120.683349
林新醫療社團法人林新醫院	Lin Shin Hospital	Nantun Dist.	moderate	severe	24.149316	120.640231
```

## Data source

The list of hospitals, their levels, chapter ratings and dates come from the MOHW emergency responsibility hospital list (急救責任醫院分區名單) for 2026. NHI codes, addresses and phones come from the National Health Insurance Administration (NHIA) list of contracted institutions (read 2026-10-29) and the emergency hospital lists of the Taipei, New Taipei, Taichung and Changhua health departments. English addresses are built from the Chunghwa Post romanization files. English names come only from each hospital's official website. Map points come from OpenStreetMap. Chapter ratings, English addresses and map points are set by us, and the data can go out of date. In an emergency, call 119.

Credit lines:

* Source: Ministry of Health and Welfare (MOHW), Taiwan, 急救責任醫院分區名單 (2026); National Health Insurance Administration contracted institution list (2026-10-29); city health department emergency hospital lists (Taipei, New Taipei, Taichung, Changhua); Chunghwa Post romanization files. Open Government Data License, version 1.0 (https://data.gov.tw/license).
* English hospital names from each hospital's official website.
* Coordinates © OpenStreetMap contributors, under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* Not affiliated with or endorsed by MOHW or any agency. In an emergency, call 119.

[Back to the API list](../../README.md)
