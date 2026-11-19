# TW Public Childcare Centers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 530](https://img.shields.io/badge/records-530-lightgrey) ![updated 2025-12-31](https://img.shields.io/badge/updated-2025--12--31-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

Ministry of Health and Welfare, Social and Family Affairs Administration, 2025, public infant care centers and community childcare homes list (114.12.31), OGDL Taiwan 1.0, https://data.gov.tw/license. This Open Data is made available to the public under the [Open Government Data License, version 1.0](https://data.gov.tw/license). Source: [Ministry of Health and Welfare, childcare services list page](https://www.mohw.gov.tw/fp-88-230-1.html). District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.

The 530 public infant care centers and community childcare homes (公設民營托嬰中心及社區公共托育家園) on the Ministry of Health and Welfare list dated 31 December 2025, in 22 counties and cities. Each record has the Chinese name, address and phone, the English county and district names, the approved number of children (17,391 in all) and the center point of the district. `county_en` is required on every call, and `county_en=*` returns all 530 records.

Not affiliated with or endorsed by the Ministry of Health and Welfare (MOHW). It is a copy of the list dated 114.12.31, and the list changes, so check the MOHW page and call the center before you rely on a record. The list is for sites that take children aged 0 to 2.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_public_childcare_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per center or home. All matches are returned. |

## Query parameters

Required: 1 field (`county_en`). Optional: 14 fields. Disallowed: 1 field.

`county_en` is required. A call without it returns HTTP 400. Use `county_en=*` to return all 530 records (about 470 KB). Partial filters accept `*`, such as `county_en=*Hsinchu*`, `name=*社區公共托育家園*`, `address=*路*` or `phone=02-*`. `facility_kind`, `geoq`, `source_as_of` and `seq` are exact matches, and a star in a text field there is read as a plain character, so it returns `[]` (a star on the whole-number `seq` returns HTTP 400). All filters ignore case. `places`, `lat` and `lng` accept a range with two dots, lower number first, such as `places=12..20` or `lat=24..25`. A hyphen range such as `places=12-20` returns HTTP 400.

Chinese filter values must be URL encoded. With curl, use `--data-urlencode`, as in the examples below. The `county` field uses 臺 (臺北市), so `county=台北市` returns `[]`. Addresses and names use both 臺 and 台, so address wildcards may need both spellings.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `county_en` | Required | Yes | No | County or city in English, from the Chunghwa Post county and township table, 22 values on every row (none is blank): New Taipei City (126); Taipei City (93); Kaohsiung City (67); Taoyuan City (57); Taichung City (48); Yilan County (19); Tainan City (17); Pingtung County (11); Hsinchu County (9); Miaoli County (9); Changhua County (9); Chiayi County (9); Yunlin County (8); Penghu County (7); Kinmen County (7); Taitung County (6); Keelung City (6); Nantou County (5); Chiayi City (5); Lienchiang County (5); Hualien County (4); Hsinchu City (3). Every call must include county_en, and a call without it returns HTTP 400. Partial wildcards allowed, so county_en=* returns all 530 rows, and *Hsinchu* matches Hsinchu City and Hsinchu County, *City or *County also work. Case ignored. The Chinese county is in the county field. |
| `county` | Optional | Yes | No | County or city in Chinese as the list prints it, using 臺 (臺北市, 臺中市, 臺南市, 臺東縣), 22 values, never blank, such as 新北市 or 高雄市. Partial wildcards allowed, such as *北* or 臺*. 台 does not match 臺. Chinese filter values must be URL encoded. |
| `district` | Optional | Yes | No | District, township or county-level city in Chinese as the list prints it, such as 汐止區, 礁溪鄉 or 朴子市, 199 county and district pairs, never blank. Without the county part. Partial wildcards allowed, such as *中*. |
| `district_en` | Optional | Yes | No | English name of the district, such as Xizhi District, Jiaoxi Township or Zhubei City, from the Chunghwa Post county and township table (Hanyu Pinyin), with the Ministry of the Interior 2026 spellings Danshui District and Sanzhong District where the two lists differ. Never blank. Partial wildcards allowed, such as *Zhongshan* or *Township. |
| `name` | Optional | Yes | No | Name of the center or home as the list prints it, in Chinese. Public centers often carry the city government and the operator in the name, such as 新北市政府委託新北市兒童教保協會辦理汐止忠厚公共托育中心(托嬰中心), up to 48 characters. 3 names are cut off in the source (seq 115, 172, 393). Never blank. Partial wildcards allowed, such as *托嬰中心 or *礁溪*. |
| `address` | Optional | Yes | No | Address in Chinese as the list prints it, such as 新北市汐止區樟樹一路137巷26號2樓. It starts with the county and district. 39 repeat the county and district twice, such as 新北市新店區新北市新店區安康路2段120號2樓, and some use 台 where others use 臺. Never blank. Partial wildcards allowed, such as *路* or 新北市*. |
| `type` | Optional | Yes | No | Establishment type (設立別) from the list. All 530 rows say 公設民營, a publicly owned facility run by a private operator. Never blank. Partial wildcards allowed, such as 公設*. |
| `facility_kind` | Optional | No | No | Kind of site, worked out from the name here, not a column in the list: Infant care center (369 rows, names with 托嬰中心 or 公共托育中心) or Community childcare home (161, names with 托育家園). Never blank. Exact match, such as facility_kind=Community childcare home. |
| `phone` | Optional | Yes | No | Telephone number as the list prints it, text such as 02-26413300 or 0836-88089, with the area code and a hyphen. 7 rows have none (null): the list gave a 09 mobile number there, which is not published here. Partial wildcards allowed, such as 02-* or 0836*. |
| `places` | Optional | No | Yes | Approved number of children (核定收托人數), a whole number from 12 to 80, never blank. The total is 17,391. Range filter with two dots, lower number first, such as 12..20. A hyphen range returns 400. |
| `seq` | Optional | No | No | Row number (項次) in the list, a whole number from 1 to 530. Unique on every row in this snapshot, but it can change when MOHW reissues the list, so it is not a stable ID. Exact match, such as seq=1. A wildcard on this whole-number field returns HTTP 400. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84, 6 decimals) of the center point of the district, not of the address, from the Chunghwa Post district center table on data.gov.tw. Present on all 530 rows. Many rows share one point. Values run from 21.99 to 26.37. A number. Range filter with two dots, min..max, such as 25..25.2. A hyphen range returns 400. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same district center point, from 118.23 to 121.85, all east of Greenwich. Present on all 530 rows. A number. Range filter with two dots, such as 121.4..121.8. |
| `geoq` | Optional | No | No | Quality of the point: district_center (514 rows, the National Land Surveying and Mapping Center lookup places the center inside the named district, or inside the city for the districts of Hsinchu City and Chiayi City), district_center_offset (8, the center falls in a neighboring district or off the land: Keelung Zhongzheng and islands in Kinmen and Lienchiang) or district_center_unchecked (8, the lookup found no land parcel at the center, in 6 mainland districts). Never blank. Exact match, such as geoq=district_center. |
| `source_as_of` | Optional | No | No | Date of the list, 2025-12-31 (114.12.31 in the title of the PDF), the same on every row. Text, YYYY-MM-DD. Exact match. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: Ministry of Health and Welfare, Social and Family Affairs Administration, 2025, public infant care centers and community childcare homes list (114.12.31), OGDL Taiwan 1.0, https://data.gov.tw/license, with the credit for district points and English names. Returned in output only. |

## Response fields

`seq`, `places`, `lat` and `lng` are numbers (`seq` and `places` are whole numbers). All other values are strings. Blank cells are returned as `null`. There is no `__id`: `seq` is the row key in this snapshot.

| Field | Type | Description |
|---|---|---|
| `seq` | integer | Row number (項次) in the MOHW list, 1 to 530. Unique in this snapshot. |
| `county` | string | County or city in Chinese, using 臺. |
| `county_en` | string | County or city in English. |
| `district` | string | District, township or county-level city in Chinese. |
| `district_en` | string | District in English. |
| `name` | string | Name of the center or home in Chinese, as listed. 3 are cut off in the list. |
| `type` | string | Establishment type, 公設民營 on every record. |
| `facility_kind` | string | `Infant care center` or `Community childcare home`, worked out from the name. |
| `address` | string | Address in Chinese, as listed. |
| `phone` | string | Telephone number as listed, or `null` on 7 records. |
| `places` | integer | Approved number of children (核定收托人數). |
| `lat` | number | Latitude of the district center point. |
| `lng` | number | Longitude of the district center point. |
| `geoq` | string | `district_center`, `district_center_offset` or `district_center_unchecked`. |
| `source` | string | Credit line for the MOHW list and the district data. |
| `source_as_of` | string | Date of the list, `2025-12-31`. |

## Data notes

* The records are the 530 numbered rows of the PDF name list on the MOHW childcare services page (updated 7 October 2026, which links one PDF dated 114.12.31). Nothing was dropped. The county is printed only on the first row of each group in the PDF and was filled down. All 22 counties and cities have at least 3 sites.
* The PDF has flaws that are kept as published: 3 names are cut off (seq 115, 172, 393), 39 addresses repeat the county and district twice, and some use 台 where others use 臺. The address of seq 268 was missing from the PDF table and was restored from the text layer of the same PDF. Seq 411 and 427 have the same address, phone and places under two names (宜蘭縣公共托嬰中心-礁溪中心 and 公共托嬰中心-礁溪中心), so the places total of 17,391 may count that site twice.
* 7 phone numbers that the list gives as 09 mobile numbers are left out (`phone` is `null`). All other phones are as listed.
* `facility_kind` is not in the list. Names with 托育家園 give Community childcare home (161), the others Infant care center (369). `type` is 公設民營 (a public facility run by a private operator) on every row.
* The English county and district names come from the Chunghwa Post county and township table (data.gov.tw 5949), with the Ministry of the Interior 2026 spellings Danshui District and Sanzhong District.
* `lat` and `lng` are the center of the district from the Chunghwa Post district center table (data.gov.tw 25489), not the position of the site, so many sites share a point. No address level geocoder was used, because the official Taiwan address services need a key. `geoq` says how far to trust the center: `district_center` (514, the National Land Surveying and Mapping Center lookup places the center inside the district, or inside the city for Hsinchu City and Chiayi City), `district_center_offset` (8, the center falls in a neighboring district or off the land, in Keelung Zhongzheng and on islands in Kinmen and Lienchiang) and `district_center_unchecked` (8, the lookup found no land parcel at the center, in 6 mainland districts).
* The data is a snapshot of the list dated 114.12.31 (2025-12-31), the `source_as_of` value.

## Examples

Community childcare homes in Taipei City, found by the name. This call returns 64 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=Taipei City' --data-urlencode 'name=*社區公共托育家園*'
```

```json
{
  "seq": 142,
  "county": "臺北市",
  "county_en": "Taipei City",
  "district": "萬華區",
  "district_en": "Wanhua District",
  "name": "臺北市政府社會局委託中華民國蒙特梭利教師協會經營管理臺北市雙園社區公共托育家園",
  "type": "公設民營",
  "facility_kind": "Community childcare home",
  "address": "臺北市萬華區和德里興義街2號",
  "phone": "02-23052362",
  "places": 12,
  "lat": 25.02859,
  "lng": 121.497986,
  "geoq": "district_center",
  "source": "Ministry of Health and Welfare, Social and Family Affairs Administration, 2025, public infant care centers and community childcare homes list (114.12.31), OGDL Taiwan 1.0, https://data.gov.tw/license District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.",
  "source_as_of": "2025-12-31"
}
```

All community childcare homes. This call returns 161 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*' --data-urlencode 'facility_kind=Community childcare home'
```

Hsinchu City and Hsinchu County together. This call returns 12 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*Hsinchu*'
```

Sites approved for 60 to 80 children. This call returns 67 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*' --data-urlencode 'places=60..80'
```

Sites in a township (鄉 or 鎮), by the English district name. This call returns 75 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*' --data-urlencode 'district_en=*Township'
```

Sites with a Lienchiang County (Matsu) area code. This call returns 5 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*' --data-urlencode 'phone=0836-*'
```

Sites in districts with a center point between latitude 22 and 23, the far south. This call returns 88 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*' --data-urlencode 'lat=22..23'
```

Sites whose district center point falls off the district land. This call returns 8 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=*' --data-urlencode 'geoq=district_center_offset'
```

New Taipei City sites approved for exactly 75 children, filtering on both the English and Chinese county. This call returns 18 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_public_childcare_centers' --data-urlencode 'county_en=New Taipei City' --data-urlencode 'county=新北市' --data-urlencode 'places=75..75'
```

## Data source

Ministry of Health and Welfare, Social and Family Affairs Administration, 2025, public infant care centers and community childcare homes list (114.12.31), OGDL Taiwan 1.0, https://data.gov.tw/license. This Open Data is made available to the public under the [Open Government Data License, version 1.0](https://data.gov.tw/license). Source: Ministry of Health and Welfare, [childcare services](https://www.mohw.gov.tw/fp-88-230-1.html), PDF list of public infant care centers and community childcare homes dated 114.12.31. The [MOHW open data statement](https://www.mohw.gov.tw/cp-185-9466-2.html) says that data and materials on the MOHW website that are protected by copyright are provided under the Open Government Data License, version 1.0, and that attribution shall be provided in further use.

District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.

Not affiliated with or endorsed by MOHW. The data is as the list publishes it, with the changes in the notes above, and it can contain errors.

[Back to the API list](../../README.md)
