## Hawker

Title: Hawker API\
Base: https://api.jael.ee/datasets/hawker
Description: List of Food Centres in Singapore
Authentication: Not Required
Version: 1.0
Format: JSON, XML (For XML output, https://api.jael.ee/datasets/hawker.xml)


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

Hawker API via https://api.jael.ee/datasets/hawker contains information from List of Government Markets Hawker Centres accessed on October 15, 2017 from https://data.gov.sg/dataset/list-of-government-markets-hawker-centres which is made available under the terms of the Singapore Open Data Licence version 1.0 https://data.gov.sg/open-data-licence
