# Singapore Money Changers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 231](https://img.shields.io/badge/records-231-lightgrey) ![updated 2026-10-03](https://img.shields.io/badge/updated-2026--10--03-orange) ![license SODL v1.0](https://img.shields.io/badge/license-SODL%20v1.0-green)

All 231 money changers licensed by the Monetary Authority of Singapore (MAS), as published on data.gov.sg (dataset updated 3 October 2026), each with its UEN, business address, postal code and a map point. Filter by the required `name`, then by UEN, address, postal code, record date, or a latitude and longitude range.

Names that include EXCHANGE: 98. Names that include MONEY CHANGER: 39. Private limited companies (`*PTE*LTD*`): 49. Four licensees have no address or coordinates in the MAS data (Changi Travel Exchange, Marina Bay Sands, Resorts World at Sentosa and Fountain Money Changer); those fields are blank rather than guessed.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_money_changers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`name`). Optional: 7 fields. Disallowed: 2 fields.

`name` must be in every call. Use `name=*` to list all 231 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `name` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Only `latitude` and `longitude` take ranges. Postal codes and UENs are text, so leading zeros stay.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Required | Yes | No | Name of the licensed money changer as MAS lists it, in capitals, such as `*EXCHANGE*` (98), `*MONEY CHANGER*` (39), `*PTE*LTD*` (49, companies) or `*HOTEL*` (6). Wildcards work, so `name=*` lists all 231 rows. |
| `uen` | Optional | No | No | Exact Unique Entity Number (UEN) of the licensee, such as `197902968M` (TRAVELEX HOLDINGS (S) PTE LTD). 9 characters for businesses (172) or 10 for companies (59). Text. |
| `address_line1` | Optional | Yes | No | First address line (block, street and unit) as the licensee gave it, such as `*Collyer Quay*` (24) or `*Orchard R*` (26). Wildcards work and case is ignored. Blank on 4 rows. |
| `address_line2` | Optional | Yes | No | Second address line, usually the building or mall, such as `*Lucky Plaza*` (13) or `*The Arcade*` (17). Wildcards work. Blank on 92 rows; some rows put the building name in `address_line1` instead. |
| `postal_code` | Optional | Yes | No | 6 digit Singapore postal code, kept as text with leading zeros, such as `049317` (24, The Arcade at 11 Collyer Quay), `238863` (15, Lucky Plaza) or `04*` (27, Raffles Place area). Wildcards work. Blank on 4 rows. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 1.256573 to 1.448227, as published by MAS. Use a range, for example `latitude=1.27..1.29&longitude=103.84..103.86` for Raffles Place and Collyer Quay. Blank on 4 rows. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 103.705351 to 103.98517. Use a range with latitude to search a box. Blank on 4 rows. |
| `record_updated` | Optional | Yes | No | Date MAS last updated this licensee's record, `YYYY-MM-DD`, from 2020-04-06 to 2026-04-01, such as `2024-04-01` (121) or `2026*` (64). Wildcards work. |
| `source` | Disallowed | No | No | The data.gov.sg dataset the row comes from. |
| `updated` | Disallowed | No | No | Date the source dataset was last updated, `YYYY-MM-DD` (`2026-10-03`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `name` returns HTTP 400 with `Your request is missing the following required parameters: name. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.

## Response fields

| Field | Type | Description |
|---|---|---|
| `name` | string | Licensed money changer name. Unique key. |
| `uen` | string | Unique Entity Number. Unique key. |
| `address_line1` | string | First address line. Null on 4 rows. |
| `address_line2` | string | Second address line. Null on 92 rows. |
| `postal_code` | string | 6 digit postal code. Null on 4 rows. |
| `latitude` | number | Latitude (WGS 84). Null on 4 rows. |
| `longitude` | number | Longitude (WGS 84). Null on 4 rows. |
| `record_updated` | string | Date MAS last updated this record, `YYYY-MM-DD`. |
| `source` | string | Link to the data.gov.sg dataset. |
| `updated` | string | Source dataset date, `YYYY-MM-DD`. |

## Examples

Lucky Plaza money changers. This call returns 13 rows (one shown):

```bash
curl -sG "https://api.jael.ee/JLEE/sg_money_changers" --data-urlencode "name=*" --data-urlencode "address_line2=*Lucky Plaza*"
```

```json
[
  {
    "name": "$&C XCHANGE CENTRE",
    "uen": "53367237J",
    "address_line1": "304 orchard road",
    "address_line2": "01-51 lucky plaza",
    "postal_code": "238863",
    "latitude": 1.304486,
    "longitude": 103.833942,
    "record_updated": "2025-04-01",
    "source": "https://data.gov.sg/datasets/d_f2dc6f340fcf53007e18dad9df92d229/view",
    "updated": "2026-10-03"
  }
]
```

Exact UEN. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_money_changers" --data-urlencode "name=*" --data-urlencode "uen=197902968M" | jq -r '.[].name'
```

Postal code The Arcade. This call returns 24 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_money_changers" --data-urlencode "name=*" --data-urlencode "postal_code=049317" | jq length
```

Map box over Raffles Place. This call returns a box of downtown licensees:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_money_changers" --data-urlencode "name=*" --data-urlencode "latitude=1.27..1.29" --data-urlencode "longitude=103.84..103.86" | jq length
```

## Data source

The licensees come from the Monetary Authority of Singapore, [Money Changers](https://data.gov.sg/datasets/d_f2dc6f340fcf53007e18dad9df92d229/view) on data.gov.sg, dataset updated 3 October 2026, fetched on 5 November 2026. Made available under the [Singapore Open Data Licence version 1.0 (SODL v1.0)](https://data.gov.sg/open-data-licence), which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Changes: columns were renamed, postal codes were padded to 6 digits, record dates were changed to YYYY-MM-DD, extra spaces were removed, map points were rounded to 6 decimals, and the address and map point were left blank for 4 licensees whose source rows held placeholder coordinates instead of an address. Map points were checked against OneMap (Singapore Land Authority); 225 checked rows agree within 300 m. The data is not guaranteed to be complete or accurate.

Credit lines:

* Source: Monetary Authority of Singapore, Money Changers, on data.gov.sg (https://data.gov.sg/datasets/d_f2dc6f340fcf53007e18dad9df92d229/view), dataset updated 3 October 2026, fetched 5 November 2026. Contains information from Money Changers accessed on 5 November 2026 from data.gov.sg which is made available under the terms of the Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence).
* Not endorsed by MAS.

[Back to the API list](../../README.md)
