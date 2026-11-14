# Hong Kong Declaration Centres

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 24](https://img.shields.io/badge/records-24-lightgrey) ![updated 2026-01-27](https://img.shields.io/badge/updated-2026--01--27-orange) ![license DATA.GOV.HK T&Cs v1.2](https://img.shields.io/badge/license-DATA.GOV.HK%20T%26Cs%20v1.2-green)

24 Home Affairs Department centres, covering all 18 Hong Kong districts, where a member of the public can make a statutory declaration, as listed on DATA.GOV.HK on 27 January 2026. Each record has the walk-in time slots or an appointment-only flag, English, Traditional Chinese and Simplified Chinese names and addresses, the official enquiry centre phone and a map point. No field is required, so you can filter by district, region, name, appointment need or a latitude and longitude box.

The New Territories have 15 centres, Kowloon 5 and Hong Kong Island 4. Islands has 4 centres; Kwai Tsing, Sai Kung and Sha Tin have 2 each; the other 14 districts have 1. Three Islands offices take declarations by appointment only. 15 centres include a 6:45 pm slot.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_declaration_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per centre. All matches are returned. |

## Query parameters

Required: none. Optional: 18 fields. Disallowed: 2 fields.

A call with no filters returns all 24 records. Partial filters accept `*`, such as `name=*Tung Chung*`, `name=*Sub-office*` or `service_time_slots=*6:45 pm*`. `lat` and `lng` accept a range with two dots, such as `lat=22.30..22.35&lng=114.15..114.25`. `district`, `district_zh`, `district_zh_hans`, `region`, `appointment_only` and `geo_match` are exact match. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Official English name as the Home Affairs Department lists it, such as Central & Western Home Affairs Enquiry Centre or Tsing Yi Sub-office. Never blank. All 24 names are unique. Partial wildcards allowed, such as `*Tung Chung*` (1) or `*Sub-office*` (2). |
| `name_zh` | Optional | Yes | No | Official Traditional Chinese name, such as 中西區民政諮詢中心. Never blank. Partial wildcards allowed, such as `*東涌*` (1). |
| `name_zh_hans` | Optional | Yes | No | Official Simplified Chinese name, such as 中西区民政谘询中心. Never blank. Partial wildcards allowed, such as `*东涌*` (1). |
| `district` | Optional | No | No | Official English district, one of the 18 districts. Islands has 4 centres; Kwai Tsing, Sai Kung and Sha Tin have 2 each; the other 14 districts have 1. Exact match, such as `district=Islands` (4) or `district=Sha Tin` (2). |
| `district_zh` | Optional | No | No | Official Traditional Chinese district, such as 離島 (4) or 沙田 (2). Exact match. |
| `district_zh_hans` | Optional | No | No | Official Simplified Chinese district, such as 离岛 (4) or 沙田 (2). Exact match. |
| `region` | Optional | No | No | Our grouping from the official 18 districts: Hong Kong Island (4: Central & Western, Eastern, Southern, Wan Chai), Kowloon (5) or New Territories (15, including all four Islands centres). The Islands District Office Main Office is in Harbour Building, Central, but its official district is Islands, so region is New Territories. Exact match. |
| `address` | Optional | Yes | No | English address as HAD lists it. Never blank. The official list still says The Grande Building for the Kwun Tong centre and Tung Chung Post Office Building for Tung Chung; the official Address Lookup Service names those sites Eastcore and Tung Chung Crescent. MOSTown in Ma On Shan is 新港城 (ALS English name Sunshine City). The Wan Chai address keeps the typographic apostrophe in O’Brien as published. Partial wildcards allowed, such as `*Harbour Building*` (2, Central and Western plus the Islands main office). |
| `address_zh` | Optional | Yes | No | Traditional Chinese address as HAD lists it. Never blank. Partial wildcards allowed, such as `*海港政府大樓*` (2). |
| `address_zh_hans` | Optional | Yes | No | Simplified Chinese address as HAD lists it. Never blank. Partial wildcards allowed, such as `*海港政府大楼*` (2). |
| `phone` | Optional | Yes | No | Official 8 digit enquiry centre landline, written as four digits, a space, then four digits, such as 2189 2819. Never blank. All 24 phones are unique. These are office lines, not personal mobiles. Partial wildcards allowed, such as `2399*` (1). |
| `service_time_slots` | Optional | Yes | No | Walk-in time slots as HAD lists them, with original line breaks joined by `; `. Never blank. 15 centres include a 6:45 pm slot. Three Islands offices (Main Office, Cheung Chau and Mui Wo) read "By appointment only". Tung Chung has Wednesday and Friday slots only; Tsing Yi is Monday, Tuesday, Thursday and Friday; Ma On Shan is Monday, Friday and Wednesday afternoon; the Sai Kung liaison office lists only "Tue & Thu" with no clock times. Partial wildcards allowed, such as `*6:45 pm*` (15) or `*appointment*` (3). |
| `service_time_slots_zh` | Optional | Yes | No | Traditional Chinese service time slots, with line breaks joined by `; `. The three appointment-only offices read 請先預約. Never blank. Partial wildcards allowed, such as `*請先預約*` (3). |
| `service_time_slots_zh_hans` | Optional | Yes | No | Simplified Chinese service time slots, with line breaks joined by `; `. The three appointment-only offices read 请先预约. Never blank. Partial wildcards allowed, such as `*请先预约*` (3). |
| `appointment_only` | Optional | No | No | Yes on the 3 Islands offices that take declarations by appointment only; No on the other 21, which publish walk-in slots. Exact match, Yes or No. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84) from the official Address Lookup Service, 5 decimals, from 22.20488 (Cheung Chau) to 22.49470 (North District, Fanling). Never blank. Use a range, for example `lat=22.30..22.35&lng=114.15..114.25` for five Kowloon centres. A hyphen range returns HTTP 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees (WGS 84) from the official Address Lookup Service, 5 decimals, from 113.93934 (Tung Chung) to 114.27233 (Sai Kung liaison office). Never blank. Use a range with `lat` to search a box. A hyphen range returns HTTP 400. |
| `geo_match` | Optional | No | No | How the map point was picked from ALS: `exact` on all 24 rows, meaning the ALS building name matches the building in the source, or the street name and number both match. Exact match, such as `geo_match=exact`. |
| `source` | Disallowed | No | No | Home Affairs Department, Centres administering declarations and service time slots, DATA.GOV.HK, in all 24 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the official CSVs, YYYY-MM-DD (`2026-01-27`). Text. Not filterable. |

## Response fields

`lat` and `lng` are numbers. All other values are strings. There is no `__id`; `name` is unique and is the stable key.

| Field | Type | Description |
|---|---|---|
| `name` | string | Official English name, unique on every row. |
| `name_zh` | string | Official Traditional Chinese name. |
| `name_zh_hans` | string | Official Simplified Chinese name. |
| `district` | string | Official English district, one of the 18 districts. |
| `district_zh` | string | Official Traditional Chinese district. |
| `district_zh_hans` | string | Official Simplified Chinese district. |
| `region` | string | Our grouping: `Hong Kong Island`, `Kowloon` or `New Territories`. |
| `address` | string | English address as HAD lists it. |
| `address_zh` | string | Traditional Chinese address as HAD lists it. |
| `address_zh_hans` | string | Simplified Chinese address as HAD lists it. |
| `phone` | string | Official 8 digit enquiry centre landline, unique on every row. |
| `service_time_slots` | string | Walk-in time slots, or `By appointment only` on 3 Islands offices. |
| `service_time_slots_zh` | string | Traditional Chinese service time slots. |
| `service_time_slots_zh_hans` | string | Simplified Chinese service time slots. |
| `appointment_only` | string | `Yes` or `No`. |
| `lat` | number | Latitude in decimal degrees (WGS 84). |
| `lng` | number | Longitude in decimal degrees (WGS 84). |
| `geo_match` | string | How the map point was picked from ALS. `exact` on every row. |
| `source` | string | Home Affairs Department, Centres administering declarations and service time slots, DATA.GOV.HK. |
| `source_as_of` | string | Date of the official CSVs, `2026-01-27`. |

## Data notes

* Hours and appointment rules can change, so call ahead before you go.
* The Islands District Office Main Office is in Harbour Building, Central, on the 20th floor. Its official district is still Islands, so `region` is New Territories. The Central and Western centre is on the ground floor of the same building.
* Three buildings have a newer name in the Address Lookup Service than in the HAD list: The Grande Building (Kwun Tong) is Eastcore, Tung Chung Post Office Building is Tung Chung Crescent, and MOSTown (Ma On Shan) is Sunshine City. The site is the same.
* Map points come from the official Address Lookup Service (Lands Department / Digital Policy Office). ALS says coverage is not exhaustive and that accuracy and timeliness are not guaranteed. Every centre in this edition has an `exact` point.
* Line breaks in the official time slot cells were joined with `; ` so each centre stays one row. The slot text itself is unchanged.
* This is a snapshot from 27 January 2026. This API is not legal advice.

## Examples

Home Affairs Department sub-offices. This call returns 2 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_declaration_centres' --data-urlencode 'name=*Sub-office*'
```

```json
[
  {
    "name": "Tsing Yi Sub-office",
    "name_zh": "青衣分處",
    "name_zh_hans": "青衣分处",
    "district": "Kwai Tsing",
    "district_zh": "葵青",
    "district_zh_hans": "葵青",
    "region": "New Territories",
    "address": "Room 108, 1/F, Cheung Fat Estate Community Centre, 6 Tam Kon Shan Road, Tsing Yi, N.T.",
    "address_zh": "新界青衣担杆山路6號長發邨社區中心1樓108室",
    "address_zh_hans": "新界青衣担杆山路6号长发邨社区中心1楼108室",
    "phone": "2497 0047",
    "service_time_slots": "Mon, Tue, Thu & Fri:; 11:30 am; 4:00 pm",
    "service_time_slots_zh": "星期一、二、四、五：; 上午11時30分; 下午4時",
    "service_time_slots_zh_hans": "星期一、二、四、五：; 上午11时30分; 下午4时",
    "appointment_only": "No",
    "lat": 22.36312,
    "lng": 114.10306,
    "geo_match": "exact",
    "source": "Home Affairs Department, Centres administering declarations and service time slots, DATA.GOV.HK",
    "source_as_of": "2026-01-27"
  },
  {
    "name": "Ma On Shan Sub-office",
    "name_zh": "馬鞍山分處",
    "name_zh_hans": "马鞍山分处",
    "district": "Sha Tin",
    "district_zh": "沙田",
    "district_zh_hans": "沙田",
    "region": "New Territories",
    "address": "Unit 1, G/F, MOSTown, No.18 On Luk Street, Ma On Shan, N.T.",
    "address_zh": "新界馬鞍山鞍祿街18號新港城第4期地下第1號舖",
    "address_zh_hans": "新界马鞍山鞍禄街18号新港城第4期地下第1号铺",
    "phone": "2633 9964",
    "service_time_slots": "Mon & Fri:; 10:00 am; 11:00 am; 12:00 noon; Wed:; 3:00 pm; 4:00 pm; 5:00 pm",
    "service_time_slots_zh": "星期一 、 五：; 上午： 10時; 11時; 中午： 12時; 星期三：; 下午： 3時; 4時; 5時",
    "service_time_slots_zh_hans": "星期一 、 五：; 上午： 10时; 11时; 中午： 12时; 星期三：; 下午： 3时; 4时; 5时",
    "appointment_only": "No",
    "lat": 22.42461,
    "lng": 114.23245,
    "geo_match": "exact",
    "source": "Home Affairs Department, Centres administering declarations and service time slots, DATA.GOV.HK",
    "source_as_of": "2026-01-27"
  }
]
```

Centres that take declarations by appointment only. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_declaration_centres' --data-urlencode 'appointment_only=Yes'
```

The Tung Chung centre, by its Traditional Chinese name. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_declaration_centres' --data-urlencode 'name_zh=*東涌*'
```

Five Kowloon centres in a latitude and longitude box. This call returns 5 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_declaration_centres' --data-urlencode 'lat=22.30..22.35' --data-urlencode 'lng=114.15..114.25'
```

## Data source

Source: Hong Kong Special Administrative Region Government, Home Affairs Department, [Centres administering declarations and service time slots](https://data.gov.hk/en-data/dataset/hk-had-json1-centres-administering-declarations-and-service-time-slots), DATA.GOV.HK. Map points come from the official [Address Lookup Service](https://data.gov.hk/en-data/dataset/hk-dpo-als_01-als) of the Lands Department and the Digital Policy Office, also on DATA.GOV.HK.

The [DATA.GOV.HK Terms and Conditions](https://data.gov.hk/en/terms-and-conditions), version 1.2 (26 May 2025), allow commercial and noncommercial reuse free of charge, with clear attribution to the Government, the Home Affairs Department and DATA.GOV.HK, and an acknowledgement of their intellectual property rights in the data. The data is provided as is. No government logos are used.

This API joins the official English, Traditional Chinese and Simplified Chinese files on the enquiry centre phone, collapses line breaks in the time slot text to `; `, and adds a region tag and ALS map points. Names, addresses, phones and time slots stay as the Home Affairs Department lists them. For the latest list visit the [dataset page on DATA.GOV.HK](https://data.gov.hk/en-data/dataset/hk-had-json1-centres-administering-declarations-and-service-time-slots).

Use of this data does not mean the Government, the Home Affairs Department or DATA.GOV.HK endorses this API or its maker.

[Back to the API list](../../README.md)
