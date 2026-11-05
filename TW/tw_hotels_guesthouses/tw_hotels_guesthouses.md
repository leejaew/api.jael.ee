# Taiwan Hotels and Guesthouses

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 15703](https://img.shields.io/badge/records-15703-lightgrey) ![updated 2026-11-06](https://img.shields.io/badge/updated-2026--11--06-orange) ![license OGDL v1.0](https://img.shields.io/badge/license-OGDL%20v1.0-green)

Taiwan's official register of 15,703 hotels and guesthouses (B&Bs), with English and Chinese names, class, address, landline phone, star rating, room counts, listed prices and a map point. Filter by the required `city`, then by hotel class, name, town, zip, Taiwan Host flag, star rating, price or a latitude and longitude range.

Class counts: guesthouse 12,421, general hotel 3,166, international tourist hotel 71, general tourist hotel 45. Largest counties and cities: Yilan County (2,525), Hualien County (1,705), Taitung County (1,645), Pingtung County (1,430) and Penghu County (1,389). Taipei City has 618. Outlying islands: Penghu County, Kinmen County (530) and Lienchiang County (240). Taiwan Host flag `yes`: 2,157. Star rated (1 to 6): 146.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/tw_hotels_guesthouses` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`city`). Optional: 15 fields. Disallowed: 7 fields.

`city` must be in every call. Use `city=*` to list all 15,703 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `city` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

`lat`, `lng`, `hotel_stars`, `total_rooms`, `lowest_price`, `ceiling_price` and `total_capacity` are numbers and take a range with two dots, such as `hotel_stars=4..5` or `lat=25.0..25.1`. A hyphen range returns HTTP 400. Every other field is text, including zip codes, phones, ids and dates.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `city` | Required | Yes | No | County or city in English, such as `Yilan County` (2,525), `Hualien County` (1,705), `Taipei City` (618) or `Penghu County` (1,389). Wildcards work, so `city=*` lists all 15,703 rows and `city=*County` gives 11,950. Outlying islands: Penghu County, Kinmen County (530) and Lienchiang County (240). |
| `id` | Optional | No | No | Exact Tourism Administration hotel id, such as `Hotel_A15010000H_000171` (Hotel Royal-Nikko Taipei). Text. |
| `license_number` | Optional | Yes | No | Local license number as the register lists it (Chinese text), such as `*民宿*` or `交觀業字*`. Wildcards work. |
| `name` | Optional | Yes | No | English lodging name, such as `*Nikko*` (4) or `*Homestay*`. Wildcards work. |
| `name_zh` | Optional | Yes | No | Chinese lodging name, such as `*老爺*` (18) or `民宿`. Wildcards work. Blank on 47 rows that appear only in the English dump. |
| `hotel_class` | Optional | No | No | Lodging class label: `international tourist hotel` (71), `general tourist hotel` (45), `general hotel` (3,166) or `guesthouse` (12,421). Guesthouse covers B&Bs (民宿). |
| `town` | Optional | Yes | No | Township or district in English, such as `Wujie Township` or `Zhongshan District`. Wildcards work, for example `town=Wujie*` (504 in Yilan County). |
| `zip` | Optional | Yes | No | 3 digit postal code as the register lists it, such as `104` or `545*`. Text. Wildcards work. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS 84). Use a range to search a box, for example `lat=25.0..25.1`. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees (WGS 84). Use a range, for example `lng=121.5..121.6`. Outlying islands sit near `lng=118..120`. |
| `hotel_stars` | Optional | No | Yes | Star rating when the register gives one (1 to 6). Blank on 15,557 unrated rows (most guesthouses). Use a range such as `hotel_stars=4..5` (74). |
| `taiwan_host` | Optional | No | No | Exact filter from the source `TaiwanHost` field: `yes` (2,157) or `no` (13,546). Tourism Data Standard V2.1 defines this as whether the lodging is a Taiwan Host (好客民宿) B&B (0 no, 1 yes), separate from star ratings in `hotel_stars`. This dump also sets `yes` on every star-rated hotel (146) and on some other hotel-class rows (guesthouse 2,011, general hotel 101, international tourist hotel 35, general tourist hotel 10). |
| `total_rooms` | Optional | No | Yes | Number of rooms as the register lists it. Use a range, for example `total_rooms=50..200`. |
| `lowest_price` | Optional | No | Yes | Lowest listed room price in TWD. Use a range, for example `lowest_price=1000..3000` (4,773). |
| `ceiling_price` | Optional | No | Yes | Highest listed room price in TWD. Use a range, for example `ceiling_price=5000..20000`. |
| `total_capacity` | Optional | No | Yes | Total guest capacity as the register lists it. Use a range if needed. |
| `address` | Disallowed | No | No | English street address from the English dump. Not filterable. |
| `address_zh` | Disallowed | No | No | Chinese street address from the Chinese dump. Blank on 47 English-only rows. Not filterable. |
| `phone` | Disallowed | No | No | Landline phone as the register lists it, such as `(02)25423266`. Text. Multiple landlines on one row are joined with `; `. Taiwan mobile numbers (`09xxxxxxxx`, including `+886-9` / `886 9` forms) are left out; blank on 8,209 rows where the source listed only a mobile. Add +886 and drop the leading 0 to call from abroad. Not filterable. |
| `website` | Disallowed | No | No | Lodging website URL when the register gives one. Blank on 12,062 rows. Not filterable. |
| `services` | Disallowed | No | No | Semicolon separated service tags cleaned from the register (Wi-Fi, parking lot, pet friendly hotel, Bike friendly accommodation and so on). Blank on 6,410 rows. Not filterable. |
| `source` | Disallowed | No | No | Data source credit for the row. Not filterable. |
| `updated` | Disallowed | No | No | Date of the source dump used for this build, `YYYY-MM-DD` (`2026-11-06`). Not filterable. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `city` returns HTTP 400 with `Your request is missing the following required parameters: city. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.
* A range written with a hyphen, such as `4-5`, returns HTTP 400. Use two dots (`4..5`).
* Chinese values must be URL encoded, which `curl --data-urlencode` does for you.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `id` | string | Tourism Administration hotel id. |
| `license_number` | string | Local license number (Chinese text). |
| `name` | string | English lodging name. |
| `name_zh` | string | Chinese lodging name. Null on 47 English-only rows. |
| `hotel_class` | string | Class label (international tourist hotel, general tourist hotel, general hotel or guesthouse). |
| `city` | string | County or city in English. |
| `town` | string | Township or district in English. |
| `zip` | string | Postal code. |
| `address` | string | English street address. |
| `address_zh` | string | Chinese street address. Null on 47 English-only rows. |
| `phone` | string | Landline phone. Null when the source listed only a mobile. |
| `website` | string | Website URL. Null on 12,062 rows. |
| `lat` | number | Latitude (WGS 84). |
| `lng` | number | Longitude (WGS 84). |
| `hotel_stars` | number | Star rating 1 to 6. Null when unrated. |
| `taiwan_host` | string | `yes` or `no` from the source `TaiwanHost` field. |
| `total_rooms` | number | Number of rooms. |
| `lowest_price` | number | Lowest listed room price in TWD. |
| `ceiling_price` | number | Highest listed room price in TWD. |
| `total_capacity` | number | Total guest capacity. |
| `services` | string | Semicolon separated service tags. Null on 6,410 rows. |
| `source` | string | Credit line for the row. |
| `updated` | string | Source dump date, `YYYY-MM-DD`. |

Notes:

* `__id` is included. `id` (HotelID) is also unique.
* Taiwan mobile numbers were removed from `phone`. Landlines stay. When the source listed only a mobile, `phone` is null.

## Examples

Star rated hotels in Taipei City. This call returns 19 rows; one with an https website is shown:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hotels_guesthouses" --data-urlencode "city=Taipei City" --data-urlencode "hotel_stars=4..5"
```

```json
[
  {
    "__id": 377,
    "id": "Hotel_A15010000H_000895",
    "license_number": "臺北市旅館523號",
    "name": "Courtyard by Marriott Taipei",
    "name_zh": "台北六福萬怡酒店",
    "hotel_class": "general hotel",
    "city": "Taipei City",
    "town": "Nangang District",
    "zip": "115",
    "address": "7F.、7F.-1,、8F.、 9F. 、 9F-1、 30F., No.359, Sec. 7, Zhongxiao E. Rd., Nangang Dist., Taipei City 115,",
    "address_zh": "忠孝東路七段359號7-30樓",
    "phone": "(02)21716565",
    "website": "https://www.courtyardtaipei.com.tw/",
    "lat": 25.052397,
    "lng": 121.606904,
    "hotel_stars": 5.0,
    "taiwan_host": "yes",
    "total_rooms": 465.0,
    "lowest_price": 12000.0,
    "ceiling_price": 100000.0,
    "total_capacity": 715.0,
    "services": "restaurant; meeting room; cafe'; bar; banquet; gym; Internet; Wi-Fi; National travel card; parking lot; senoir room; Assessible room; laundry service; valuables depository; pick-up service; foreign currency exchange",
    "source": "Tourism Administration, MOTC (data.gov.tw 73280 / 7780)",
    "updated": "2026-11-06"
  }
]
```

Guesthouses only. This call returns 12,421 rows:

```bash
curl -s "https://api.jael.ee/JLEE/tw_hotels_guesthouses?city=*&hotel_class=guesthouse" | jq length
```

Taiwan Host flag. This call returns 2,157 rows:

```bash
curl -s "https://api.jael.ee/JLEE/tw_hotels_guesthouses?city=*&taiwan_host=yes" | jq length
```

Name wildcard. This call returns 4 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hotels_guesthouses" --data-urlencode "city=*" --data-urlencode "name=*Nikko*" | jq -r '.[] | [.name, .city, .hotel_class] | @tsv'
```

Map box over central Taipei. This call returns 568 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hotels_guesthouses" --data-urlencode "city=*" --data-urlencode "lat=25.0..25.1" --data-urlencode "lng=121.5..121.6" | jq length
```

Yilan County, Wujie Township. This call returns 504 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/tw_hotels_guesthouses" --data-urlencode "city=Yilan County" --data-urlencode "town=Wujie*" | jq length
```

## Data source

The lodgings come from the Tourism Administration, Ministry of Transportation and Communications (MOTC), through the Hotel and guesthouse registers on data.gov.tw: [English dump dataset 73280](https://data.gov.tw/en/datasets/73280) and [Chinese dump dataset 7780](https://data.gov.tw/dataset/7780), both updated 6 November 2026 and fetched on 6 November 2026. Licensed under the [Open Government Data License, version 1.0 (OGDL v1.0)](https://data.gov.tw/license), which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Credit lines:

* Source: Tourism Administration, Ministry of Transportation and Communications (MOTC). Data from the Hotel and guesthouse registers on data.gov.tw: English dump dataset 73280 (https://data.gov.tw/en/datasets/73280) and Chinese dump dataset 7780 (https://data.gov.tw/dataset/7780), both updated 6 November 2026, fetched 6 November 2026. Licensed under the Open Government Data License, version 1.0 (https://data.gov.tw/license). Changes: English and Chinese rows joined on HotelID; HotelClasses codes mapped to English labels (1 international tourist hotel, 2 general tourist hotel, 3 general hotel, 4 guesthouse); service tags cleaned; Taiwan mobile numbers removed from phone; empty and unused source fields dropped. No logos. Not endorsed by the Tourism Administration or MOTC.

[Back to the API list](../../README.md)
