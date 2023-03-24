## Hotels API

**Key Information**

| Field          | Specification                                                       |
|----------------|---------------------------------------------------------------------|
| title          | Hotels API                                                          |
| description    | List of Hotels (current list include hotels in Singapore)           |
| base           | https://api.jael.ee/JLEE/hotels                                     |
| format         | JSON, XML (For XML output, https://api.jael.ee/JLEE/hotels.xml)     |
| authentication | Not Required                                                        |
| version        | 1.0                                                                 |


**Input parameters**

| Parameter  | Type   | Required | Description                                         |
|------------|--------|----------|-----------------------------------------------------|
| name       | string | No (Allows partial) | 	Name of hotel                          |
| address[city] | string | No (Allows partial) | City or town                          |
| address[postalcode] | double | No (Allows ranges) | Postal code (e.g. 169663)        |
| country    | string | Yes (Allows partial) | Country name                            |


**Output returned by the API**

| Field              |   Type   | Description                                                              |
|--------------------|----------|------------------------------------------------------|
| name               |  string  | Name of hotel                                        |
| totalrooms         |  integer | Total number of rooms                                |
| address[street]    |  string  | Street address of hotel                              |
| address[city]      |  string  | City or town                                         |
| address[state]     |  string  | Province or state                                    |
| address[postalcode]|  double  | Postal code                                          |
| country            |  string  | Country name                                         |
| phone              |  string  | Hotel phone number                                   |
| location[latitude] |  double  | Latitude of hotel location                           |
| location[longitude]|  double  | Longitude of hotel location                          |
| social[instagram]  |  string  | Hotel instagram page url                             |
| social[facebook]   |  string  | Hotel facebook page url                              |

**Example API path**

Search for hotels in Singapore with "copthorne" included in name.
```
https://api.jael.ee/JLEE/hotels?country=singapore&name=*copthorne*
```

Hotels API via https://api.jael.ee/JLEE/hotels contains information from Location of Hotels by Hotels Licensing Board accessed on December 4, 2018 from https://data.gov.sg/dataset/hotels which is made available under the terms of the Singapore Open Data Licence version 1.0 https://data.gov.sg/open-data-licence

Hotels API icon was made by <a href="https://www.flaticon.com/authors/vectors-market" title="Vectors Market">Vectors Market</a> from <a href="https://www.flaticon.com/" 			    title="Flaticon">www.flaticon.com</a> is licensed by <a href="http://creativecommons.org/licenses/by/3.0/" title="Creative Commons BY 3.0" target="_blank">CC 3.0 BY</a></div>
