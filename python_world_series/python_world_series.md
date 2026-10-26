# World Series Results

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 118](https://img.shields.io/badge/records-118-lightgrey)

Results of the Major League Baseball World Series from 1903 to 2022. Each row is one series: the date it ended, the winning and losing team, and the runs each team scored in the final game. There are no rows for 1904 and 1994, when no series was played.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/python_world_series` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `winner` | Optional | Yes | No | Winning team, for example `*Yankees*`. |
| `loser` | Optional | Yes | No | Losing team, for example `*Giants*`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../README.md#how-filters-work) for the wildcard and range syntax.
* Any field that is not in this table is returned in the output but cannot be used as a filter. Sending one returns an error.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `id` | integer | Row number in date order, starting at 1. |
| `date` | string | Date of the final game, in ISO 8601 format. |
| `winner` | string | Team that won the series. |
| `loser` | string | Team that lost the series. |
| `winningruns` | integer | Runs scored by the winning team in the final game. |
| `losingruns` | integer | Runs scored by the losing team in the final game. |

## Example

Series the Yankees won against a team with Giants in its name.

```bash
curl -s "https://api.jael.ee/JLEE/python_world_series?winner=*Yankees*&loser=*Giants*"
```

Response, showing 2 of 5 rows:

```json
[
  {
    "id": 20,
    "date": "1923-10-15T00:00:00+00:00",
    "winner": "New York Yankees",
    "loser": "New York Giants",
    "winningruns": 6,
    "losingruns": 4
  },
  {
    "id": 33,
    "date": "1936-10-06T00:00:00+00:00",
    "winner": "New York Yankees",
    "loser": "New York Giants",
    "winningruns": 13,
    "losingruns": 5
  },
  ...
]
```

[Back to all APIs](../README.md)
