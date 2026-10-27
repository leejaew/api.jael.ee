# NZ DOC Visitor Centres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 13](https://img.shields.io/badge/records-13-lightgrey) ![licence CC BY 4.0](https://img.shields.io/badge/licence-CC%20BY%204.0-orange)

The 13 Department of Conservation (DOC) visitor centres in New Zealand, in 8 regions. Each row is one centre with its name, region, town, address, postcode, phone, email, opening hours, open season, wifi and booking service flags, and map coordinates. The Queenstown centre is not included because it closed on 27 October 2026. Other visitor centres, such as isite centres, are not part of this data set.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_visitor_centres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 0 fields. Optional: 11 fields. Disallowed: 6 fields.

No parameter is required, because the data set has only 13 rows. A call with no parameters returns all 13. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Centre name, for example `*Mount Cook*`, `*Fiordland*` or `*Haast*`. |
| `centre_type` | Optional | No | No | Always `DOC` in this data set. |
| `region` | Optional | Yes | No | New Zealand region, such as `Canterbury`, `West Coast`, `Southland`, `Taranaki`, `Otago`, `Tasman`, `Waikato` or `Manawatū-Whanganui`. 8 values. |
| `city` | Optional | Yes | No | Town or village, such as `Te Anau`, `Thames` or `Haast`. |
| `address` | Optional | Yes | No | Street address, for example `*State Highway*`. |
| `postcode` | Optional | Yes | No | New Zealand postcode as text, such as `9600` or `7*`. |
| `open_season` | Optional | No | No | `Year round` (12 centres) or `Seasonal` (1 centre). |
| `wifi` | Optional | No | No | `Yes` when the DOC page says the centre has free wifi (4 centres). Blank for 9 centres where the page does not say. |
| `booking_services` | Optional | No | No | `Yes` when the centre sells or arranges bookings such as huts, tours or passes (11 centres). Blank for 2 centres. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees, from -46.898158 to -37.12208. Use a range to search a box, for example `latitude=-45..-42`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees, from 167.719484 to 175.627559. Use a range to search a box, for example `longitude=168..172`. |
| `phone` | Disallowed | No | No | Centre phone number in international form. |
| `email` | Disallowed | No | No | General centre email address. |
| `hours` | Disallowed | No | No | Typical opening hours as short text. Hours change with the season. |
| `website` | Disallowed | No | No | DOC web page for the centre. |
| `source` | Disallowed | No | No | DOC pages and open data layer the row was built from. |
| `updated` | Disallowed | No | No | Date the row was last checked, such as `2026-10-27`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Centre name. |
| `centre_type` | string | Always `DOC`. |
| `region` | string | New Zealand region. |
| `city` | string | Town or village. |
| `address` | string | Street address. |
| `postcode` | string | New Zealand postcode. |
| `phone` | string | Phone number in international form. |
| `email` | string | General centre email address. |
| `hours` | string | Typical opening hours as short text. |
| `open_season` | string | `Year round` or `Seasonal`. |
| `wifi` | string, null | `Yes` when free wifi is listed. Null when not known. |
| `booking_services` | string, null | `Yes` when bookings are sold or arranged. Null when not known. |
| `latitude` | number | Latitude in decimal degrees, from DOC open data. |
| `longitude` | number | Longitude in decimal degrees, from DOC open data. |
| `website` | string | DOC web page for the centre. |
| `source` | string | DOC pages and open data layer the row was built from. |
| `updated` | string | Date the row was last checked. |

## Examples

Visitor centres in Southland.

```bash
curl -s "https://api.jael.ee/JLEE/nz_visitor_centres?region=Southland"
```

Response (2 rows, first row shown, `source` shortened):

```json
[
  {
    "__id": 18,
    "name": "Rakiura National Park Visitor Centre",
    "centre_type": "DOC",
    "region": "Southland",
    "city": "Oban",
    "address": "15 Main Road",
    "postcode": "9818",
    "phone": "+64 3 219 0009",
    "email": "stewartisland@doc.govt.nz",
    "hours": "Mon to Fri 8:30 am to 4 pm; Sat and holidays 9:15 am to 3:15 pm; closed Sun and Dec 25",
    "open_season": "Year round",
    "wifi": null,
    "booking_services": "Yes",
    "latitude": -46.898158,
    "longitude": 168.126206,
    "website": "https://www.doc.govt.nz/footer-links/contact-us/visitor-centres/rakiura-national-park-visitor-centre/",
    "source": "Department of Conservation Te Papa Atawhai, doc.govt.nz visitor centre page: https://www.doc.govt.nz/...; DOC visitor centres map layer (CC BY 4.0, Crown copyright); ...",
    "updated": "2026-10-27"
  }
]
```

Centres in the south of the South Island that sell bookings, using a range on latitude. This call returns 4 centres:

```bash
curl -s "https://api.jael.ee/JLEE/nz_visitor_centres?latitude=-46..-43&booking_services=Yes" | jq -r '.[].name'
```

```text
Tititea/Mount Aspiring National Park Visitor Centre
Te Rua-o-te-moko/Fiordland National Park Visitor Centre
Awarua/Haast Visitor Centre
Westland Tai Poutini National Park Visitor Centre
```

West Coast centres with free wifi. This call returns 2 centres:

```bash
curl -s "https://api.jael.ee/JLEE/nz_visitor_centres?region=West%20Coast&wifi=Yes" | jq -r '.[].name'
```

```text
Paparoa National Park Visitor Centre
Westland Tai Poutini National Park Visitor Centre
```

## Data source

Department of Conservation Te Papa Atawhai: each centre's page on [doc.govt.nz](https://www.doc.govt.nz) and the DOC visitor centres feature service on the DOC open data portal. Licence: Creative Commons Attribution 4.0 International (CC BY 4.0). Credit: Crown copyright, Department of Conservation Te Papa Atawhai. Text fields are written in plain words from the DOC pages. Hours change with the season, so check the DOC page before you travel.

[Back to the API list](../../README.md)
