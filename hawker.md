List of Food Centres in Singapore

API base URL
* https://api.jael.ee/datasets/hawker

Authentication
* Not Required


**Input parameters**

| Parameter  | Type   | Required | Description                                            |
|------------|--------|----------|--------------------------------------------------------|
| name       | string | no       | Name of the food centre (e.g. Amoy Street Food Centre) |
| postalcode | double | no       | Postal code of the food centre (e.g. 069111)           |


**Output returned by the API**

| Field            | Type    | Description                                                              |
|------------------|---------|--------------------------------------------------------------------------|
| name             | string  | Name of Market or Hawker Centre                                          |
| type             | string  | MK: Market only, HC: Hawker Centre only, MHC: Markets cum Hawker Centres |
| owner            | string  | Can be owned by HDB(Housing and Development Board) or Government         |
| stalls           | integer | Number of stalls                                                         |
| cookedfoodstalls | integer | Number of cooked food stalls                                             |
| mktproducestalls | integer | Number of market produce stalls                                          |
| address          | string  | -                                                                        |
| postalcode       | double  | -                                                                        |
| city             | string  | -                                                                        |
| country          | string  | -                                                                        |
| lat              | double  | Latitude (GPS coordinates)                                               |
| lng              | double  | Longitude (GPS coordinates)                                              |

