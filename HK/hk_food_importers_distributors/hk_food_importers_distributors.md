# Hong Kong Food Importers and Distributors

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 12,203](https://img.shields.io/badge/records-12%2C203-lightgrey) ![updated 2026-11-15](https://img.shields.io/badge/updated-2026--11--15-orange) ![license DATA.GOV.HK T&Cs v1.2](https://img.shields.io/badge/license-DATA.GOV.HK%20T%26Cs%20v1.2-green)

12,203 food importers and distributors on the Centre for Food Safety register of registered food importers and food distributors, as of 15 November 2026. Each record has the English and Chinese business name and address, a contact phone, importer and distributor flags, and the official food category and class codes the trader is registered for. No field is required, so you can filter by registration number, name, address, phone, importer or distributor status, or a category or class code.

11,701 traders are registered as importers and 9,462 as distributors. 8,960 are both, 2,741 are importers only and 502 are distributors only. The register covers all 16 official food categories and all 65 classes.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/hk_food_importers_distributors` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per trader. All matches are returned. |

## Query parameters

Required: none. Optional: 12 fields. Disallowed: 2 fields.

A call with no filters returns all 12,203 records. Partial filters accept `*`, such as `name_en=*Seafood*`, `trader_reg_no=TR-11-*` or `category_codes=*|C10|*`. To match one category or class exactly, wrap the code in pipes, for example `*|C1|*` for C1, which does not also match C10 through C16. `is_importer` and `is_distributor` are exact match. Text filters ignore case. `source` and `source_as_of` cannot be filtered. No field accepts a range.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `trader_reg_no` | Optional | Yes | No | Official trader registration number in the form TR-YY-NNNNNN, such as TR-11-001017 (Honortry Enterprise Limited) or TR-11-000041 (Grand Fortune Industrial Limited). Unique and never blank. All 12,203 values match this pattern. The middle two digits are a year code from 11 to 26 in this snapshot (TR-24 has the most rows, 1936; TR-11 has 797). Partial wildcards allowed, such as `TR-11-*` (797) or `TR-11-001017` (1). |
| `name_en` | Optional | Yes | No | English business name as the register lists it, such as Honortry Enterprise Limited or Grand Fortune Industrial Limited. Blank on 896 rows: 881 where the published name was clearly just a person's name and was removed, and 15 where the register itself had no English name. Partial wildcards allowed, such as `*Limited*` (10300) or `*Seafood*` (165). Matching is case insensitive, so `name_en=*limited*` returns the same 10300 rows. |
| `name_zh` | Optional | Yes | No | Chinese business name as the register lists it, such as 福宏實業有限公司 (Grand Fortune Industrial Limited). Blank on 3001 rows, including the 881 personal-name rows. Partial wildcards allowed, such as `*有限公司*` (8601). |
| `address_en` | Optional | Yes | No | English business address as the register lists it, such as Flat F, 12/F, City Industrial Complex 116-122 Kwok Shui Road, Kwai Chung, N.T. Blank on 881 rows (already blank in the source). Partial wildcards allowed, such as `*Kwai Chung*` (1070) or `*Kowloon*` (1940). |
| `address_zh` | Optional | Yes | No | Chinese business address as the register lists it. Blank on 6699 rows. Two rows have no English address and no Chinese address (TR-21-000956 and TR-21-000957); both are among the personal-name rows. Partial wildcards allowed. |
| `phone` | Optional | Yes | No | Contact phone as the register lists it, kept as text. Most values are an 8 digit Hong Kong number (8301 rows). Some rows list more than one number, or use spaces, slashes, hyphens or a leading plus. Blank on 881 rows, the same rows whose personal names were removed. Partial wildcards allowed, such as `2*` (4699). |
| `is_importer` | Optional | No | No | Yes if the trader is registered as a food importer for at least one class (11701), otherwise No (502). Exact match. |
| `is_distributor` | Optional | No | No | Yes if the trader is registered as a food distributor for at least one class (9462), otherwise No (2741). Exact match. 8960 traders are both, 2741 are importer only, and 502 are distributor only. |
| `category_codes` | Optional | Yes | No | Pipe-joined list of the 16 official main food category codes the trader is registered under, wrapped with a leading and trailing pipe so a wildcard can match one code exactly. Example: `|C1|C2|C4|`. Use `*|C1|*` (4697) for category C1 (cereal and grain products, other than bakery products and snack food). Do not use `*C1*`, which would also match C10 through C16. Never blank. Counts by code: C1 4697, C2 5836, C3 2294, C4 4718, C5 4990, C6 3155, C7 3658, C8 2476, C9 3459, C10 6251, C11 4649, C12 4443, C13 4100, C14 2738, C15 2463, C16 1618. Category wording follows Food Safety Ordinance Schedule 2 / FEHB 245: C1 cereal and grain products (other than bakery products and snack food); C2 fruit and vegetables (other than snack food, juices and Chinese herbs); C3 sashimi, sushi and ready-to-eat raw oysters; C4 aquatic products (other than snack food, sashimi and ready-to-eat raw oyster); C5 meat and meat products (other than snack food and sashimi); C6 eggs and egg products; C7 milk and dairy products (other than infant/follow-up/growing-up formula); C8 frozen confections; C9 fat and oil; C10 beverages (other than milk and dairy products); C11 sugars and sweets; C12 dim sum, Chinese pastry, mixed dishes, desserts, bakery products and snack food (other than candy, chocolate and chewing gum); C13 salts, condiments and sauces, herbs and spices; C14 Chinese herbs and their products; C15 infant/follow-up/growing-up formula and baby food; C16 miscellaneous (food that cannot be placed in categories 1-15). |
| `class_codes` | Optional | Yes | No | Pipe-joined list of the official food classification codes for the trader, wrapped the same way as `category_codes`, such as `|C1B|C2C|C4B|`. There are 65 classes, lettered under each category (C1A cereals, rice, wheat; C2C vegetables, including mushrooms, fungi and seaweed; C4A wild-caught coral reef fish, live and unprocessed, and so on). Use `*|C2C|*` (3841) or `*|C4A|*` (2298). Never blank. All 65 classes appear in this snapshot. Do not use a short wildcard such as `*C4*`, which would also match C4A and C14 classes; wrap the exact code in pipes. |
| `importer_class_codes` | Optional | Yes | No | Pipe-joined classification codes for which the trader is registered as an importer, wrapped the same way. Blank on 502 rows (distributor only). 883 traders have importer and distributor flags that differ from class to class, so this list can differ from `distributor_class_codes` even when both `is_importer` and `is_distributor` are Yes. Partial wildcards allowed, such as `*|C2C|*`. |
| `distributor_class_codes` | Optional | Yes | No | Pipe-joined classification codes for which the trader is registered as a distributor, wrapped the same way. Blank on 2741 rows (importer only). Partial wildcards allowed, such as `*|C4A|*`. |
| `source` | Disallowed | No | No | Centre for Food Safety, Food and Environmental Hygiene Department, Register of Registered Food Importers and Food Distributors, in all 12203 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the register snapshot, YYYY-MM-DD (`2026-11-15`). Text. Not filterable. |

## Response fields

`__id` is an integer. All other values are strings, or `null` when the register has no value, or when a personal name and phone were removed. `trader_reg_no` is the official public key.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `trader_reg_no` | string | Official trader registration number, unique on every row. |
| `name_en` | string | English business name, or `null` on 896 rows. |
| `name_zh` | string | Chinese business name, or `null` on 3,001 rows. |
| `address_en` | string | English business address, or `null` on 881 rows. |
| `address_zh` | string | Chinese business address, or `null` on 6,699 rows. |
| `phone` | string | Contact phone as the register lists it, or `null` on 881 rows. |
| `is_importer` | string | `Yes` or `No`. |
| `is_distributor` | string | `Yes` or `No`. |
| `category_codes` | string | Pipe-joined official food category codes, with a leading and trailing pipe. |
| `class_codes` | string | Pipe-joined official food classification codes, with a leading and trailing pipe. |
| `importer_class_codes` | string | Pipe-joined importer classification codes, or `null` on 502 distributor-only rows. |
| `distributor_class_codes` | string | Pipe-joined distributor classification codes, or `null` on 2,741 importer-only rows. |
| `source` | string | Centre for Food Safety, Food and Environmental Hygiene Department, Register of Registered Food Importers and Food Distributors. |
| `source_as_of` | string | Date of the snapshot, `2026-11-15`. |

## Data notes

* Personal names were removed from 881 rows. Those rows still appear under their trader registration number, with `name_en`, `name_zh` and `phone` blank. Two of them also have no address in the source (TR-21-000956 and TR-21-000957).
* Category and class codes are stored as a pipe-joined string with a leading and trailing pipe, such as `|C1|C2|C4|` or `|C4A|C4B|`. Use `*|C1|*` to match one code exactly. A short wildcard such as `*C1*` would also match C10 through C16.
* 883 traders have importer and distributor marks that differ from class to class, so `importer_class_codes` and `distributor_class_codes` can differ even when both flags are Yes.
* There are no map points. The register has no coordinates.
* This is a snapshot from 15 November 2026. The live register is updated every day.

## Examples

Seafood traders, by English name. This call returns 165 records. The first two:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_food_importers_distributors' --data-urlencode 'name_en=*Seafood*'
```

```json
[
  {
    "__id": 142,
    "trader_reg_no": "TR-11-000441",
    "name_en": "Yeung Hop Lee Seafood Company Limited",
    "name_zh": "楊合利海鮮有限公司",
    "address_en": "Dist 27, 6 Lo She Chow, NT.",
    "address_zh": null,
    "phone": "2450 8660",
    "is_importer": "No",
    "is_distributor": "Yes",
    "category_codes": "|C4|",
    "class_codes": "|C4A|",
    "importer_class_codes": null,
    "distributor_class_codes": "|C4A|",
    "source": "Centre for Food Safety, Food and Environmental Hygiene Department, Register of Registered Food Importers and Food Distributors",
    "source_as_of": "2026-11-15"
  },
  {
    "__id": 258,
    "trader_reg_no": "TR-11-000742",
    "name_en": "Yau Cheong Hoo Seafood Company Limited",
    "name_zh": "佑昌號海產有限公司",
    "address_en": "No A21, Western Wholesale Food Market, Western Reclamation, HK",
    "address_zh": null,
    "phone": "2547 8283",
    "is_importer": "No",
    "is_distributor": "Yes",
    "category_codes": "|C4|",
    "class_codes": "|C4B|C4C|C4D|",
    "importer_class_codes": null,
    "distributor_class_codes": "|C4B|C4C|C4D|",
    "source": "Centre for Food Safety, Food and Environmental Hygiene Department, Register of Registered Food Importers and Food Distributors",
    "source_as_of": "2026-11-15"
  }
]
```

One trader by official registration number. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_food_importers_distributors' --data-urlencode 'trader_reg_no=TR-11-001017'
```

Traders registered under category C16 (miscellaneous). This call returns 1,618 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_food_importers_distributors' --data-urlencode 'category_codes=*|C16|*'
```

Distributors that are not also importers. This call returns 502 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/hk_food_importers_distributors' --data-urlencode 'is_distributor=Yes' --data-urlencode 'is_importer=No'
```

## Data source

Source: Hong Kong Special Administrative Region Government, Centre for Food Safety, Food and Environmental Hygiene Department, [Register of Registered Food Importers and Food Distributors](https://data.gov.hk/en-data/dataset/hk-fehd-fehdftp-fehdftp), DATA.GOV.HK. The current register file is also published at [www.ftp.cfs.gov.hk](https://www.ftp.cfs.gov.hk/enquiry/registered_trader_data.json).

The [DATA.GOV.HK Terms and Conditions](https://data.gov.hk/en/terms-and-conditions), version 1.2 (26 May 2025), allow commercial and noncommercial reuse free of charge, with clear attribution to the Government, the Centre for Food Safety, the Food and Environmental Hygiene Department and DATA.GOV.HK, and an acknowledgement of their intellectual property rights in the data. The data is provided as is. No government logos are used.

This API stores the official category and class codes as pipe-joined strings with a leading and trailing pipe, derives importer and distributor flags from the marks on each class, and blanks personal names and phones on 881 rows. Business names, addresses and registration numbers stay as the Centre for Food Safety lists them. There are no map points, because the register has no coordinates. For the latest list visit the [dataset page on DATA.GOV.HK](https://data.gov.hk/en-data/dataset/hk-fehd-fehdftp-fehdftp) or the [current register file](https://www.ftp.cfs.gov.hk/enquiry/registered_trader_data.json).

Use of this data does not mean the Government, the Centre for Food Safety, the Food and Environmental Hygiene Department or DATA.GOV.HK endorses this API or its maker.

[Back to the API list](../../README.md)
