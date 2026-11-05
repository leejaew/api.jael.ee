# US Transplant Programs

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1125](https://img.shields.io/badge/records-1125-lightgrey) ![updated 2026-10-23](https://img.shields.io/badge/updated-2026--10--23-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

The 1,125 US organ transplant center outcome rows from HRSA (254 centers), one row per center by organ by donor type (Deceased or Living), with graft and patient survival at 30 days and 1 year, plus the share of waitlist patients who received a deceased donor transplant within 30 days, 1 year, 2 years and 3 years. Filter by organ (required, `organ=*` lists all), then by state, center name, donor type and the rate fields.

The organs are Kidney (470), Liver (224), Heart (151), Kidney-Pancreas (124), Pancreas (82) and Lung (74). Living donor rows cover Kidney and Liver only (305 rows). Nine centers HRSA marks Inactive still appear, with `inactive=Yes` (18 rows). The source file has no address or coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_transplant_programs` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per center by organ by donor type |

## Query parameters

Required: 1 field (`organ`). Optional: 15 fields. Disallowed: 4 fields.

`organ` must be in every call. Use `organ=*` to list all 1,125 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `organ` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

The eight rate fields (`graft_survival_30d`, `graft_survival_1yr`, `patient_survival_30d`, `patient_survival_1yr`, `waitlist_transplant_30d`, `waitlist_transplant_1yr`, `waitlist_transplant_2yr` and `waitlist_transplant_3yr`) are numbers and take a range, such as `graft_survival_1yr=95..100`. Every other field is text, including dates and state codes.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `organ` | Required | Yes | No | Organ transplanted: `Kidney` (470), `Liver` (224), `Heart` (151), `Kidney-Pancreas` (124), `Pancreas` (82) or `Lung` (74). Wildcards work, so `organ=*` lists all 1,125 rows. |
| `center_name` | Optional | Yes | No | Transplant center name as HRSA lists it, such as `Children's of Alabama` or `University of Alabama Hospital`. Wildcards work, so `center_name=*Children*` finds 165 rows. |
| `state` | Optional | Yes | No | Full state or territory name where the center sits, such as `California`, `Texas` or `District of Columbia`. Wildcards work, such as `state=New*`. |
| `state_code` | Optional | No | No | Two letter postal code for the state, such as `CA`, `TX` or `DC`. |
| `inactive` | Optional | No | No | `Yes` for the 9 centers HRSA marks Inactive (18 rows); `No` for active centers. Inactive centers still have outcome data in the date window. |
| `donor_type` | Optional | No | No | `Deceased` (820 rows) or `Living` (305 rows; Kidney and Liver only). |
| `graft_survival_30d` | Optional | No | Yes | Percent of transplants with a functioning graft at 30 days, as a number such as `97.5`. Use a range such as `graft_survival_30d=95..100`. Blank where HRSA wrote Suppressed. |
| `graft_survival_1yr` | Optional | No | Yes | Percent of transplants with a functioning graft at 1 year, as a number. Use a range such as `graft_survival_1yr=90..100`. Blank where HRSA wrote Suppressed. Outcomes window is 2021-01-01 to 2025-03-31. |
| `patient_survival_30d` | Optional | No | Yes | Percent of patients alive at 30 days, as a number. Use a range such as `patient_survival_30d=95..100`. Blank where HRSA wrote Suppressed. |
| `patient_survival_1yr` | Optional | No | Yes | Percent of patients alive at 1 year, as a number. Use a range such as `patient_survival_1yr=90..100`. Blank where HRSA wrote Suppressed. |
| `waitlist_transplant_30d` | Optional | No | Yes | Percent of waitlist patients who received a deceased donor transplant within 30 days, as a number. Use a range such as `waitlist_transplant_30d=10..50`. Filled only on Deceased donor rows. Blank on Living rows and where HRSA had no figure. Waitlist window is 2018-01-01 to 2023-03-31. |
| `waitlist_transplant_1yr` | Optional | No | Yes | Percent of waitlist patients who received a deceased donor transplant within 1 year, as a number. Use a range such as `waitlist_transplant_1yr=20..100`. Same blank rules as `waitlist_transplant_30d`. |
| `waitlist_transplant_2yr` | Optional | No | Yes | Percent of waitlist patients who received a deceased donor transplant within 2 years, as a number. Use a range such as `waitlist_transplant_2yr=30..100`. |
| `waitlist_transplant_3yr` | Optional | No | Yes | Percent of waitlist patients who received a deceased donor transplant within 3 years, as a number. Use a range such as `waitlist_transplant_3yr=40..100`. |
| `outcomes_start` | Optional | Yes | No | Start of the graft and patient survival window as `YYYY-MM-DD` (`2021-01-01` on every row). Use a wildcard such as `outcomes_start=2021*`. String dates cannot use a range. |
| `outcomes_end` | Optional | Yes | No | End of the graft and patient survival window as `YYYY-MM-DD` (`2025-03-31` on every row). Use a wildcard such as `outcomes_end=2025*`. |
| `waitlist_start` | Disallowed | No | No | Start of the waitlist transplant rate window as `YYYY-MM-DD` (`2018-01-01` when rates are filled). Blank when waitlist rates are blank. Not filterable. |
| `waitlist_end` | Disallowed | No | No | End of the waitlist transplant rate window as `YYYY-MM-DD` (`2023-03-31` when rates are filled). Blank when waitlist rates are blank. Not filterable. |
| `source` | Disallowed | No | No | Data source: HRSA Organ Donation and Transplantation Public Use File (`https://data.hrsa.gov/topics/health-systems/organ-donation`), a US government work in the public domain. Not filterable. |
| `updated` | Disallowed | No | No | Date of the HRSA file refresh, `YYYY-MM-DD` (`2026-10-23`). Not filterable. |

* Parameters are combined with AND. Text matches ignore case, so `organ=heart` works the same as `organ=Heart`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `organ` returns HTTP 400 with `Your request is missing the following required parameters: organ. You did not supply any.`
* A filter on a disallowed field returns HTTP 400 with `Request attempts to filter on disallowed parameters: source`.
* A range written with a hyphen, such as `95-100`, returns HTTP 400 with `Value '95-100' is not a double`. Use two dots.

## Response fields

| Field | Type | Description |
|---|---|---|
| `center_name` | string | Transplant center name as HRSA lists it. |
| `state` | string | Full state or territory name. |
| `state_code` | string | Two letter postal code. |
| `inactive` | string | `Yes` or `No`. |
| `organ` | string | Organ transplanted. |
| `donor_type` | string | `Deceased` or `Living`. |
| `graft_survival_30d` | number | Percent with a functioning graft at 30 days. Null where HRSA wrote Suppressed. |
| `graft_survival_1yr` | number | Percent with a functioning graft at 1 year. Null where HRSA wrote Suppressed. |
| `patient_survival_30d` | number | Percent of patients alive at 30 days. Null where HRSA wrote Suppressed. |
| `patient_survival_1yr` | number | Percent of patients alive at 1 year. Null where HRSA wrote Suppressed. |
| `waitlist_transplant_30d` | number | Percent of waitlist patients transplanted within 30 days (deceased donor). Null on Living rows and where HRSA had no figure. |
| `waitlist_transplant_1yr` | number | Percent of waitlist patients transplanted within 1 year (deceased donor). Null on Living rows and where HRSA had no figure. |
| `waitlist_transplant_2yr` | number | Percent of waitlist patients transplanted within 2 years (deceased donor). Null on Living rows and where HRSA had no figure. |
| `waitlist_transplant_3yr` | number | Percent of waitlist patients transplanted within 3 years (deceased donor). Null on Living rows and where HRSA had no figure. |
| `outcomes_start` | string | Start of the graft and patient survival window, `YYYY-MM-DD`. |
| `outcomes_end` | string | End of the graft and patient survival window, `YYYY-MM-DD`. |
| `waitlist_start` | string | Start of the waitlist transplant rate window, `YYYY-MM-DD`. Null when waitlist rates are null. |
| `waitlist_end` | string | End of the waitlist transplant rate window, `YYYY-MM-DD`. Null when waitlist rates are null. |
| `source` | string | Data source name and link. |
| `updated` | string | Date of the HRSA file refresh, `YYYY-MM-DD`. |

Notes:

* No `__id` field. Each row is unique by `center_name`, `state_code`, `organ` and `donor_type`. The source has no separate center code.
* HRSA hides a survival rate when it is based on fewer than 11 patients, so those cells are null.
* Waitlist rates are filled only on Deceased donor rows. They are null on all Living rows and on some Deceased rows with no matching waitlist figure.
* The source file has no city, address, coordinates or center code.

## Examples

Heart programs in Texas. This call returns 14 rows; the first is shown:

```bash
curl -sG "https://api.jael.ee/JLEE/us_transplant_programs" --data-urlencode "organ=Heart" --data-urlencode "state_code=TX"
```

```json
[
  {
    "center_name": "Children's Medical Center of Dallas",
    "state": "Texas",
    "state_code": "TX",
    "inactive": "No",
    "organ": "Heart",
    "donor_type": "Deceased",
    "graft_survival_30d": 97.25,
    "graft_survival_1yr": 90.48,
    "patient_survival_30d": 97.25,
    "patient_survival_1yr": 92.31,
    "waitlist_transplant_30d": 31.4,
    "waitlist_transplant_1yr": 70.5,
    "waitlist_transplant_2yr": 74.4,
    "waitlist_transplant_3yr": 78.2,
    "outcomes_start": "2021-01-01",
    "outcomes_end": "2025-03-31",
    "waitlist_start": "2018-01-01",
    "waitlist_end": "2023-03-31",
    "source": "HRSA Organ Donation and Transplantation Public Use File (https://data.hrsa.gov/topics/health-systems/organ-donation)",
    "updated": "2026-10-23"
  }
]
```

Range on graft survival. Kidney programs with 1 year graft survival from 95 to 100. This call returns 245 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_transplant_programs" --data-urlencode "organ=Kidney" --data-urlencode "graft_survival_1yr=95..100" | jq -r '.[] | [.center_name, .state_code, .donor_type, .graft_survival_1yr] | @tsv' | head
```

## Data source

The rows come from the HRSA [Organ Donation and Transplantation Public Use File](https://data.hrsa.gov/topics/health-systems/organ-donation), refreshed October 23, 2026 and fetched on November 6, 2026. Outcomes cover January 1, 2021 to March 31, 2025. Waitlist transplant rates cover January 1, 2018 to March 31, 2023.

Credit lines:

* Source: HRSA. Data from the Health Resources and Services Administration (HRSA) Organ Donation and Transplantation Public Use File (https://data.hrsa.gov/topics/health-systems/organ-donation), refreshed October 23, 2026, fetched November 6, 2026. Outcomes cover January 1, 2021 to March 31, 2025; waitlist transplant rates cover January 1, 2018 to March 31, 2023. HRSA Organ Transplant Programs Data Usage Terms & Conditions say Usage limitations: None. HRSA does not endorse this API or the people who run it. Changes: kept the OTC Outcome Measures sheet (1,125 rows), joined deceased donor waitlist transplant rates from the Waitlist OTC Transplant Rate sheet by center and organ, split center name and state from the OTC_Name_State field, marked the 9 Inactive centers, stored percentages as plain numbers with Suppressed left blank, and added source and updated; no city, address, coordinates or center code (none in this file, and name matching to other HRSA/CMS lists was not reliable enough). US federal government works are in the public domain (17 U.S.C. 105).

[Back to the API list](../../README.md)
