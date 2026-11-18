# Taiwan Customs Brokers

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1456](https://img.shields.io/badge/records-1456-lightgrey) ![updated 2026-10-13](https://img.shields.io/badge/updated-2026--10--13-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

Customs Administration, Ministry of Finance, 2026, Customs Broker Register (data.gov.tw 6704). Made available under the Open Government Data License, version 1.0, https://data.gov.tw/license. Source: [Customs Administration, Ministry of Finance, Customs Broker Register](https://data.gov.tw/dataset/6704), updated 13 October 2026. District center points and English names: Chunghwa Post and the Ministry of the Interior, same license.

1,456 customs broker registrations in Taiwan, for 1,446 distinct brokers, from the Customs Administration register of 13 October 2026. Each record is a broker at one of four customs offices (Keelung 507, Taipei 502, Kaohsiung 305, Taichung 142), with the Chinese name, box number, address and phone as published, the English office, city and district names, and the center point of the district on 1,348 records. No field is required, so you can filter by customs office, city, district, name or address, or by a latitude and longitude box.

This is not an official Customs Administration or Taiwan government service, and it is not affiliated with the Customs Administration. It is a copy of the register as of 13 October 2026, and the register changes. Check the Customs Administration before you rely on a broker. The register has Chinese names only, and the English fields give the customs office, city and district, not the broker name.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_customs_brokers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per broker registration at a customs office. All matches are returned. |

## Query parameters

Required: none. Optional: 15 fields. Disallowed: 1 field.

A call with no filters returns all 1,456 records (about 1.3 MB). Partial filters accept `*`, such as `name_zh=*通運*`, `address_zh=*忠三路*`, `box_no=0*` or `district_en=*Zhongshan*`. `customs_office_zh`, `customs_office_en`, `city_zh`, `city_en`, `district_src` and `geoq` are exact matches, and a star there is read as a plain character, so it returns `[]`. `lat` and `lng` accept a range with two dots, lower number first, such as `lat=25..25.2`. A hyphen range such as `lat=25-26` returns HTTP 400.

Chinese filter values must be URL encoded. With curl, use `--data-urlencode`, as in the examples below. Address and city values in the register use both 臺 and 台, while `city_zh`, `district_zh` and `customs_office_zh` use 臺 (臺北市, 臺中關), so `address_zh` wildcards may need both spellings.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `customs_office_zh` | Optional | No | No | Customs office that holds the broker registration, in Chinese: 基隆關 (Keelung, 507), 臺北關 (Taipei, 502), 高雄關 (Kaohsiung, 305) or 臺中關 (Taichung, 142), as published (臺 is written with the traditional 臺). Never blank. Exact match, such as customs_office_zh=基隆關. |
| `customs_office_en` | Optional | No | No | English name of the customs office, added here: Keelung, Taipei, Taichung or Kaohsiung. The source has no English names, so this is a plain translation of the four office names. Never blank. Exact match, such as customs_office_en=Keelung, case ignored. |
| `box_no` | Optional | Yes | No | Customs broker box number (報關箱號) as the register prints it, a short code such as 1, 301, 0F2 or E50. It is text, so leading zeros and letters are kept. The same number is used by different customs offices, so it is not unique on its own: only the pair customs_office_zh and box_no is unique. Never blank. Partial wildcards allowed, such as 0*. |
| `name_zh` | Optional | Yes | No | Broker business name in Chinese as published, such as 南部報關有限公司 or 億興報關行. 1,446 distinct names on 1,456 rows, because 9 brokers are registered at two or three customs offices. No English names exist in the source. Never blank. Partial wildcards allowed, such as *報關行 or *通運*. Chinese text must be URL encoded. |
| `address_zh` | Optional | Yes | No | Business address in Chinese as published, such as 高雄市苓雅區新光路24巷1號6樓之6. Some entries use 台 and others 臺, and 383 are written without the district. Present on 1,455 rows, blank (`null`) on 1. Partial wildcards allowed, such as *忠三路*. |
| `phone` | Optional | Yes | No | Contact telephone as published, a text value such as 2216062~5, (07)5363233 or 2912967、2015161. It can hold several numbers, a range with ~, an extension after #, and sometimes no area code. These are business office numbers. Never blank. Partial wildcards allowed, such as (02)*. |
| `city_zh` | Optional | No | No | City or county parsed from the start of the address, in Chinese with 臺 (臺北市, 基隆市, 高雄市), 13 values. A few abbreviated or old names are corrected (高市 to 高雄市, 桃園縣 to 桃園市, 台北縣 to 新北市). Present on 1,455 rows, blank (`null`) on 1. Exact match. |
| `city_en` | Optional | No | No | English name of the city or county, such as Taipei City, Keelung City, Taoyuan City or Hualien County, from the Chunghwa Post county and township table on data.gov.tw. Present on all rows with an address. Exact match, such as city_en=Keelung City. |
| `district_zh` | Optional | Yes | No | District, township or county-level city in Chinese without the city part, such as 苓雅區 or 蘇澳鎮. Present on 1,348 rows, blank (`null`) on 108 where the address names no district and none could be matched, see district_src. Partial wildcards allowed, such as *中*. |
| `district_en` | Optional | Yes | No | English name of the district, such as Zhongshan District, Ren'ai District or Su'ao Township, from the Chunghwa Post county and township table on data.gov.tw (Hanyu Pinyin), with the Ministry of the Interior 2026 spellings Danshui and Sanzhong where the two lists differ. Present on 1,348 rows, blank (`null`) on 108. Partial wildcards allowed, such as *Zhongshan* or *Township. |
| `district_src` | Optional | No | No | How the district was found: address (1072 rows, written in the address), same_address (166, the same street and house number appears in another row that names the district) or same_road (110, Keelung City only: every other row on that street names the same district, at least two of them). Blank (`null`) on 108 rows with no district. Exact match, such as district_src=address. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84, 6 decimals) of the center point of the district, not of the address, from the Chunghwa Post district center table on data.gov.tw. Present on 1,348 rows, blank (`null`) on 108 with no district. Many rows share one point. Values run from 22.55 to 26.23. A number. Range filter with two dots, min..max, such as 25..25.2. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees of the same district center point. Present on the same 1,348 rows. Values run from 118.23 to 121.83, all east of Greenwich. A number. Range filter with two dots, such as 121.4..121.8. A hyphen range returns 400. |
| `geoq` | Optional | No | No | Quality of the point: district_center (1287 rows, the center lies inside the named district) or district_center_offset (61, the center point falls in a neighboring district or off the land for an irregular district: Keelung Zhongzheng and four Kinmen townships). Blank (`null`) on 108 rows with no point. Exact match, such as geoq=district_center. |
| `source_as_of` | Optional | Yes | No | Date the register was last updated by the Customs Administration, 2026-10-13, the same on every row. Text, YYYY-MM-DD. Partial wildcards allowed, such as 2026*. No range on dates. |
| `source` | Disallowed | No | No | Not filterable. Attribution line, the same on every row: Customs Administration, Ministry of Finance, 2026, Customs Broker Register (data.gov.tw 6704), made available under the Open Government Data License, version 1.0, with the credit for district points and English names. Returned in output only. |

## Response fields

`lat` and `lng` are numbers. `__id` is an integer. All other values are strings. Blank cells are returned as `null`.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Record ID returned by the API, present on every row. It is generated, and it can change when the data is refreshed, so do not store it or treat it as a stable key. The pair `customs_office_zh` and `box_no` identifies a registration. |
| `customs_office_zh` | string | Customs office in Chinese: `基隆關`, `臺北關`, `高雄關` or `臺中關`. |
| `customs_office_en` | string | `Keelung`, `Taipei`, `Kaohsiung` or `Taichung`. |
| `box_no` | string | Broker box number, such as `1`, `301` or `0F2`. |
| `name_zh` | string | Broker business name in Chinese. |
| `address_zh` | string | Business address in Chinese, or `null` on 1 row. |
| `phone` | string | Business phone as published, possibly several numbers. |
| `city_zh` | string | City or county in Chinese, or `null` on 1 row. |
| `city_en` | string | City or county in English, or `null` on 1 row. |
| `district_zh` | string | District in Chinese, or `null` on 108 rows. |
| `district_en` | string | District in English, or `null` on 108 rows. |
| `district_src` | string | `address`, `same_address` or `same_road`, or `null` on 108 rows. |
| `lat` | number | Latitude of the district center, or `null` on 108 rows. |
| `lng` | number | Longitude of the district center, or `null` on 108 rows. |
| `geoq` | string | `district_center` or `district_center_offset`, or `null` on 108 rows. |
| `source` | string | Attribution line for the register, the point and the English names. |
| `source_as_of` | string | Date the register was last updated, `2026-10-13`. |

## Examples

Brokers at the Kaohsiung customs office with 通運 (forwarding) in the name. This call returns 8 records, and the first one is shown:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode 'customs_office_en=Kaohsiung' --data-urlencode 'name_zh=*通運*'
```

```json
{
  "__id": 1463,
  "customs_office_zh": "高雄關",
  "customs_office_en": "Kaohsiung",
  "box_no": "12",
  "name_zh": "中和行報關通運股份有限公司",
  "address_zh": "高雄市苓雅區海邊路46號7樓之6",
  "phone": "3320302~5",
  "city_zh": "高雄市",
  "city_en": "Kaohsiung City",
  "district_zh": "苓雅區",
  "district_en": "Lingya District",
  "district_src": "address",
  "lat": 22.623594,
  "lng": 120.32091,
  "geoq": "district_center",
  "source": "Customs Administration, Ministry of Finance, 2026, Customs Broker Register (data.gov.tw 6704). Made available under the Open Government Data License, version 1.0, https://data.gov.tw/license District center points and English names: Chunghwa Post (data.gov.tw 25489 and 5949) and Ministry of the Interior, same license.",
  "source_as_of": "2026-10-13"
}
```

Brokers in Ren'ai District, Keelung. This call returns 324 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode "district_en=Ren'ai District"
```

Brokers in one Taoyuan district. This call returns 42 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode 'city_en=Taoyuan City' --data-urlencode 'district_en=Dayuan District'
```

Box number 301 is used by three customs offices, which shows that a box number alone is not a key. This call returns 3 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode 'box_no=301'
```

Brokers whose address has a street name containing 忠三路. This call returns 86 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode 'address_zh=*忠三路*'
```

Brokers whose district center falls in a map box over central Taipei. The numbers are in degrees, with the lower number first. This call returns 324 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode 'lat=25.0..25.1' --data-urlencode 'lng=121.5..121.6'
```

Records whose district center is flagged as falling outside the named district. This call returns 61 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/tw_customs_brokers' --data-urlencode 'geoq=district_center_offset'
```

## Data source

Customs Administration, Ministry of Finance, 2026, Customs Broker Register (data.gov.tw 6704). Made available under the Open Government Data License, version 1.0, https://data.gov.tw/license. Source: [Customs Administration, Ministry of Finance, Customs Broker Register](https://data.gov.tw/dataset/6704) (dataset A07080000D-000002, Chinese title 關務署轄區報關業者名冊), updated 13 October 2026. The Open Government Data License, version 1.0, lets anyone copy, share, adapt and build products or services from the data for any purpose, if the attribution above is kept ([license text](https://data.gov.tw/license)). The provider does not endorse this API.

The register is a CSV file with six columns: customs office, box number, name, address, phone and remarks. This API keeps the first five, and the remarks column is not included. It has 1,456 rows, none of them exact duplicates. The 1,446 distinct names are fewer than the rows because 9 brokers are registered at two or three customs offices, each with its own box number. The customs office is where the broker is registered, and brokers can be in other cities, so `city_en` includes cities such as Taoyuan, Hsinchu, Hualien and Kinmen.

English names: `customs_office_en` is a plain translation of the four office names. `city_en` and `district_en` come from the Chunghwa Post county and township table ([data.gov.tw 5949](https://data.gov.tw/dataset/5949)), with the Ministry of the Interior 2026 village boundary list used for the spellings Danshui District and Sanzhong District. The register has no English broker names, and none were added.

Districts: the city and district are read from the Chinese address. 1,072 records name the district in the address, including one that writes 苓雅 without 區. For 166 records the district comes from another record with the same street and house number (`same_address`), and for 110 Keelung records it comes from the same street in other records that all name one district (`same_road`). 108 records have no district, so they have no point either.

Map points: `lat` and `lng` are the center of the district, from the Chunghwa Post district center table ([data.gov.tw 25489](https://data.gov.tw/dataset/25489)), and not the position of the broker address. Many records share a point. Each used center was checked with the free district lookup of the National Land Surveying and Mapping Center. 67 of the 72 centers fall inside the named district. The other 5 (Keelung Zhongzheng, and Kinmen Lieyu, Jincheng, Jinning and Jinhu) fall in a neighboring district or off the land, and their 61 records are marked `district_center_offset`. No address-level geocoder was used, because the official Taiwan address services need an application.

The register lists business names, business addresses and business phone numbers.

[Back to the API list](../../README.md)
