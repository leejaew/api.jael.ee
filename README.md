# api.jael.ee

Free public JSON APIs for open and curated data sets: transport, places, sport, food and baseball. Every API is a plain `GET` request. No sign up and no API key.

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![APIs 62](https://img.shields.io/badge/APIs-62-lightgrey)

**Total APIs: 62** across Australia, Hong Kong, Singapore, Malaysia, United Arab Emirates, New Zealand, Taiwan, the United Kingdom and United States (counts update with every new API)

**Base URL:** `https://api.jael.ee/JLEE/`

An API URL is the base URL plus the API name, for example `https://api.jael.ee/JLEE/python_world_series`.

## Contents

* [Quick start](#quick-start)
* [All APIs](#all-apis)
* [How filters work](#how-filters-work)
* [Errors](#errors)

## Quick start

Get every World Series the Yankees won against a team with Giants in its name:

```bash
curl -s "https://api.jael.ee/JLEE/python_world_series?winner=*Yankees*&loser=*Giants*"
```

The response is a JSON array:

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

Filter by a number range, here Triple-A ballparks that seat 10,000 to 16,600 fans:

```bash
curl -s "https://api.jael.ee/JLEE/us_milb_ballparks?level=Triple-A&capacity=10000..16600"
```

## All APIs

| API | Description | Region | Records |
|---|---|---|---:|
| [`sg_active_ageing_centres`](SG/sg_active_ageing_centres/sg_active_ageing_centres.md) | Active Ageing Centres in Singapore with operator, address, hours and phone. | Singapore | 93 |
| [`sg_axs_stations`](SG/sg_axs_stations/sg_axs_stations.md) | Partial sample of AXS payment machines in Singapore mapped in OpenStreetMap. | Singapore | 27 |
| [`sg_cd_public_shelters`](SG/sg_cd_public_shelters/sg_cd_public_shelters.md) | Civil Defence public shelters in Singapore, with host site, host type, address and coordinates. | Singapore | 589 |
| [`sg_cnr_amenities`](SG/sg_cnr_amenities/sg_cnr_amenities.md) | Amenities in the Central Catchment Nature Reserve in Singapore, such as huts, toilets and carparks, with type code and coordinates. | Singapore | 449 |
| [`sg_community_clubs`](SG/sg_community_clubs/sg_community_clubs.md) | Community clubs and PAssion WaVe outlets run by the People's Association in Singapore, with address, postal code and coordinates. | Singapore | 128 |
| [`sg_community_use_sites`](SG/sg_community_use_sites/sg_community_use_sites.md) | State land sites in Singapore open to community use, with agency, site area, contact and center point. | Singapore | 240 |
| [`sg_dementia_friendly_gtp`](SG/sg_dementia_friendly_gtp/sg_dementia_friendly_gtp.md) | Places in Singapore that are set up to help people with dementia, such as MRT stations, care homes and community clubs. | Singapore | 831 |
| [`sg_family_service_centres`](SG/sg_family_service_centres/sg_family_service_centres.md) | Family Service Centres in Singapore that offer counselling and support to families. | Singapore | 55 |
| [`sg_golf_courses`](SG/sg_golf_courses/sg_golf_courses.md) | The 16 golf courses on the Singapore Government list with club type, holes, address and coordinates. | Singapore | 16 |
| [`sg_hawker_centres`](SG/sg_hawker_centres/sg_hawker_centres.md) | Government hawker centres and markets in Singapore, with stall counts and popular dishes. | Singapore | 107 |
| [`sg_heritage_trees`](SG/sg_heritage_trees/sg_heritage_trees.md) | The 255 trees on the NParks Heritage Trees register in Singapore, with names, location text and coordinates. | Singapore | 255 |
| [`sg_historic_sites`](SG/sg_historic_sites/sg_historic_sites.md) | The 100 historic sites of Singapore marked by the National Heritage Board, with history, address and coordinates. | Singapore | 100 |
| [`sg_hotels`](SG/sg_hotels/sg_hotels.md) | Licensed hotels and hostels in Singapore with room counts, phone, address and coordinates. | Singapore | 468 |
| [`sg_korean_restaurants`](SG/sg_korean_restaurants/sg_korean_restaurants.md) | Korean restaurants in Singapore with menu highlights, atmosphere and contact details. | Singapore | 42 |
| [`sg_lta_mrt_station_exit_geojson_api`](SG/sg_lta_mrt_station_exit_geojson_api/sg_lta_mrt_station_exit_geojson_api.md) | Exit points of Singapore MRT stations as GeoJSON style features, with coordinates. | Singapore | 597 |
| [`sg_national_monuments`](SG/sg_national_monuments/sg_national_monuments.md) | The 77 national monuments of Singapore, with short history, address, photo link and coordinates. | Singapore | 77 |
| [`sg_nature_ways`](SG/sg_nature_ways/sg_nature_ways.md) | The 56 Nature Ways in Singapore, tree lined roadside routes, with length and midpoint. | Singapore | 56 |
| [`sg_nparks_park_facilities`](SG/sg_nparks_park_facilities/sg_nparks_park_facilities.md) | Facility points in Singapore parks from NParks, such as carparks, toilets, playgrounds and BBQ pits, with coordinates. | Singapore | 14754 |
| [`sg_nparks_parks`](SG/sg_nparks_parks/sg_nparks_parks.md) | The 462 NParks parks in Singapore as map points, with park name and coordinates. | Singapore | 462 |
| [`sg_nparks_tracks`](SG/sg_nparks_tracks/sg_nparks_tracks.md) | Paths in Singapore parks and park connectors from NParks, with path type, width, allowed users, length and midpoint. | Singapore | 15425 |
| [`sg_park_connector_segments`](SG/sg_park_connector_segments/sg_park_connector_segments.md) | Line segments of the NParks Park Connector Network in Singapore, with park, route loop, length and midpoint. | Singapore | 878 |
| [`sg_rc_rn_centres`](SG/sg_rc_rn_centres/sg_rc_rn_centres.md) | Residents Committee and Residents Network centres in Singapore, with tag, address and coordinates. | Singapore | 714 |
| [`sg_retail_pharmacies`](SG/sg_retail_pharmacies/sg_retail_pharmacies.md) | Licensed retail pharmacies in Singapore with brand, address and coordinates. | Singapore | 249 |
| [`sg_social_service_agencies`](SG/sg_social_service_agencies/sg_social_service_agencies.md) | Member agencies of the National Council of Social Service in Singapore, tagged by service type. | Singapore | 908 |
| [`sg_social_service_offices`](SG/sg_social_service_offices/sg_social_service_offices.md) | The 24 Social Service Offices in Singapore, with address parts, postal code and coordinates. | Singapore | 24 |
| [`sg_student_care_centres`](SG/sg_student_care_centres/sg_student_care_centres.md) | Registered student care centres in Singapore, with operator type, audit grade, monthly fee, enrolment and coordinates. | Singapore | 336 |
| [`sg_sportsg_facilities`](SG/sg_sportsg_facilities/sg_sportsg_facilities.md) | Sport facilities listed by Sport Singapore: sport centres, pools, gyms, stadiums and pickleball and padel courts. | Singapore | 87 |
| [`sg_vet_centres`](SG/sg_vet_centres/sg_vet_centres.md) | Veterinary clinics and animal hospitals in Singapore (partial list) with address, phone and hours. | Singapore | 40 |
| [`sg_my_international_schools`](SG/sg_my_international_schools/sg_my_international_schools.md) | International schools in Singapore, Kuala Lumpur, Johor Bahru and Penang, with curriculum and contact details. | Singapore and Malaysia | 126 |
| [`jb_korean_restaurants`](MY/jb_korean_restaurants/jb_korean_restaurants.md) | Korean restaurants in Johor Bahru and Iskandar Puteri with menu highlights and contact details. | Malaysia | 46 |
| [`jb_night_markets`](MY/jb_night_markets/jb_night_markets.md) | The 29 pasar malam night markets of Johor Bahru by weekday, area and stalls. | Malaysia | 29 |
| [`kl_korean_restaurants`](MY/kl_korean_restaurants/kl_korean_restaurants.md) | Korean restaurants in Kuala Lumpur with menu highlights, atmosphere and contact details. | Malaysia | 122 |
| [`kl_park_and_ride`](MY/kl_park_and_ride/kl_park_and_ride.md) | Park and ride car parks at Klang Valley rail stations with bays, operator and fees. | Malaysia | 77 |
| [`my_bus_terminals`](MY/my_bus_terminals/my_bus_terminals.md) | Bus terminals and public transport hubs in Malaysia with facilities and coordinates. | Malaysia | 228 |
| [`my_highway_rest_areas`](MY/my_highway_rest_areas/my_highway_rest_areas.md) | PLUS expressway rest areas, lay-bys and service areas with facilities and direction. | Malaysia | 82 |
| [`my_national_parks`](MY/my_national_parks/my_national_parks.md) | Malaysian national, state and marine parks with state, operator, year gazetted and area. | Malaysia | 65 |
| [`auh_public_parks`](UAE/auh_public_parks/auh_public_parks.md) | Public parks of the emirate of Abu Dhabi from the municipality parks layer, one centre point each, with classification, region, district, hectares and coordinates. | United Arab Emirates | 558 |
| [`auh_shopping_malls`](UAE/auh_shopping_malls/auh_shopping_malls.md) | Shopping malls and shopping centres in the emirate of Abu Dhabi, with region, Arabic name, address, store count, phone, website and coordinates. | United Arab Emirates | 43 |
| [`dxb_korean_restaurants`](UAE/dxb_korean_restaurants/dxb_korean_restaurants.md) | Korean restaurants, fast food places and cafes in Dubai, with cuisine, area, phone, website, opening hours and coordinates. | United Arab Emirates | 15 |
| [`dxb_metro_tram_stations`](UAE/dxb_metro_tram_stations/dxb_metro_tram_stations.md) | Stations on the Dubai Metro Red and Green lines and the Dubai Tram, with coordinates and exit descriptions. | United Arab Emirates | 66 |
| [`dxb_public_parks`](UAE/dxb_public_parks/dxb_public_parks.md) | Public parks, gardens, dog parks and nature reserves in Dubai from OpenStreetMap, with area, park type, fee, hours and coordinates. | United Arab Emirates | 183 |
| [`dxb_shopping_malls`](UAE/dxb_shopping_malls/dxb_shopping_malls.md) | Shopping malls, centres and souks in Dubai, with Arabic name, area, opening year, store count, leasable area, phone, website and coordinates. | United Arab Emirates | 106 |
| [`uae_international_schools`](UAE/uae_international_schools/uae_international_schools.md) | International private schools in Dubai, Abu Dhabi and Sharjah, with English and Arabic names, emirate, area, curriculum, grades, fees, inspection rating, contacts and coordinates. | United Arab Emirates | 489 |
| [`uae_museums`](UAE/uae_museums/uae_museums.md) | Museums across the UAE from OpenStreetMap and Wikidata, with emirate, city, theme, opening year, ticket price and coordinates. | United Arab Emirates | 82 |
| [`au_aged_care_homes`](AU/au_aged_care_homes/au_aged_care_homes.md) | Government funded residential aged care homes across Australia, with provider, organisation type, places, address, state, remoteness, 2024-25 funding and coordinates. | Australia | 2590 |
| [`au_tafe_campuses`](AU/au_tafe_campuses/au_tafe_campuses.md) | TAFE and public vocational college campuses across Australia, from 29 providers, with state, suburb, address, postcode, phone, campus type and coordinates. | Australia | 402 |
| [`hk_boundary_control_points`](HK/hk_boundary_control_points/hk_boundary_control_points.md) | Every Hong Kong immigration control point (land, rail, sea, air), with English and Chinese names, mode, hours, status, cargo and passenger flags, address and coordinates. | Hong Kong | 16 |
| [`hk_elderly_care_homes`](HK/hk_elderly_care_homes/hk_elderly_care_homes.md) | Licensed residential care homes for the elderly in Hong Kong, with English and Chinese names and addresses, district, care type, accreditation and coordinates. | Hong Kong | 857 |
| [`hk_licensed_hotels_guesthouses`](HK/hk_licensed_hotels_guesthouses/hk_licensed_hotels_guesthouses.md) | Licensed hotels and guesthouses in Hong Kong, with English and Chinese names and addresses, licence number, region, district, contacts and coordinates. | Hong Kong | 1807 |
| [`nz_visitor_centres`](NZ/nz_visitor_centres/nz_visitor_centres.md) | The 13 Department of Conservation visitor centres in New Zealand, with region, address, phone, hours, wifi and booking flags and coordinates. | New Zealand | 13 |
| [`tw_international_schools`](TW/tw_international_schools/tw_international_schools.md) | International schools for foreign residents in Taiwan, with English and Chinese names, city, district, address, national type, grades offered and coordinates. | Taiwan | 24 |
| [`tw_mandarin_learning_centers`](TW/tw_mandarin_learning_centers/tw_mandarin_learning_centers.md) | Mandarin teaching centers at 70 universities in Taiwan, with English and Chinese names and addresses, city, district, 2025 foreign student counts and coordinates. | Taiwan | 71 |
| [`tw_night_markets`](TW/tw_night_markets/tw_night_markets.md) | Night markets in Taiwan in English and Traditional Chinese, with city, district, address, open days, hours, stall count and coordinates. | Taiwan | 107 |
| [`uk_jobcentres`](UK/uk_jobcentres/uk_jobcentres.md) | Every active Jobcentre Plus office in Great Britain, with address, postcode, council area, DWP district and area, nation and coordinates. | United Kingdom | 633 |
| [`uk_motorway_service_areas`](UK/uk_motorway_service_areas/uk_motorway_service_areas.md) | Motorway service areas, rest areas and truck stops in the United Kingdom, with motorway, junction, operator, amenities and coordinates. | United Kingdom | 107 |
| [`uk_ofs_registered_providers`](UK/uk_ofs_registered_providers/uk_ofs_registered_providers.md) | Every higher education provider on the Office for Students Register in England, with names, UKPRN, registration category, degree awarding powers, university title, TEF status, fee limit, address, region and postcode coordinates. | United Kingdom | 425 |
| [`uk_youth_hostels`](UK/uk_youth_hostels/uk_youth_hostels.md) | Every YHA hostel in England and Wales and every Hostelling Scotland hostel, with operator, partner flag, nation, region, town, council area, address, postcode and coordinates. | United Kingdom | 141 |
| [`python_world_series`](python_world_series/python_world_series.md) | Winner, loser and final game score of the MLB World Series, 1903 to 2022. | United States | 118 |
| [`us_cbp_ports_of_entry`](US/us_cbp_ports_of_entry/us_cbp_ports_of_entry.md) | US Customs and Border Protection ports of entry, airports, seaports and land crossings, with port code, address, state, field office, port type and coordinates. | United States | 346 |
| [`us_milb_ballparks`](US/us_milb_ballparks/us_milb_ballparks.md) | Ballparks of 120 Minor League Baseball clubs from Single A to Triple A, with capacity and year opened. | United States | 120 |
| [`us_stadiums`](US/us_stadiums/us_stadiums.md) | Home stadiums of 154 clubs in the NFL, MLB, NBA, NHL and MLS, with address and coordinates. | United States | 154 |
| [`us_uscis_offices`](US/us_uscis_offices/us_uscis_offices.md) | USCIS field offices and application support centers in the United States and its territories. | United States | 220 |

Browse by region:

* **Singapore:** [`sg_active_ageing_centres`](SG/sg_active_ageing_centres/sg_active_ageing_centres.md), [`sg_axs_stations`](SG/sg_axs_stations/sg_axs_stations.md), [`sg_cd_public_shelters`](SG/sg_cd_public_shelters/sg_cd_public_shelters.md), [`sg_cnr_amenities`](SG/sg_cnr_amenities/sg_cnr_amenities.md), [`sg_community_clubs`](SG/sg_community_clubs/sg_community_clubs.md), [`sg_community_use_sites`](SG/sg_community_use_sites/sg_community_use_sites.md), [`sg_dementia_friendly_gtp`](SG/sg_dementia_friendly_gtp/sg_dementia_friendly_gtp.md), [`sg_family_service_centres`](SG/sg_family_service_centres/sg_family_service_centres.md), [`sg_golf_courses`](SG/sg_golf_courses/sg_golf_courses.md), [`sg_hawker_centres`](SG/sg_hawker_centres/sg_hawker_centres.md), [`sg_heritage_trees`](SG/sg_heritage_trees/sg_heritage_trees.md), [`sg_historic_sites`](SG/sg_historic_sites/sg_historic_sites.md), [`sg_hotels`](SG/sg_hotels/sg_hotels.md), [`sg_korean_restaurants`](SG/sg_korean_restaurants/sg_korean_restaurants.md), [`sg_lta_mrt_station_exit_geojson_api`](SG/sg_lta_mrt_station_exit_geojson_api/sg_lta_mrt_station_exit_geojson_api.md), [`sg_national_monuments`](SG/sg_national_monuments/sg_national_monuments.md), [`sg_nature_ways`](SG/sg_nature_ways/sg_nature_ways.md), [`sg_nparks_park_facilities`](SG/sg_nparks_park_facilities/sg_nparks_park_facilities.md), [`sg_nparks_parks`](SG/sg_nparks_parks/sg_nparks_parks.md), [`sg_nparks_tracks`](SG/sg_nparks_tracks/sg_nparks_tracks.md), [`sg_park_connector_segments`](SG/sg_park_connector_segments/sg_park_connector_segments.md), [`sg_rc_rn_centres`](SG/sg_rc_rn_centres/sg_rc_rn_centres.md), [`sg_retail_pharmacies`](SG/sg_retail_pharmacies/sg_retail_pharmacies.md), [`sg_social_service_agencies`](SG/sg_social_service_agencies/sg_social_service_agencies.md), [`sg_social_service_offices`](SG/sg_social_service_offices/sg_social_service_offices.md), [`sg_student_care_centres`](SG/sg_student_care_centres/sg_student_care_centres.md), [`sg_sportsg_facilities`](SG/sg_sportsg_facilities/sg_sportsg_facilities.md), [`sg_vet_centres`](SG/sg_vet_centres/sg_vet_centres.md)
* **Singapore and Malaysia:** [`sg_my_international_schools`](SG/sg_my_international_schools/sg_my_international_schools.md)
* **Malaysia:** [`jb_korean_restaurants`](MY/jb_korean_restaurants/jb_korean_restaurants.md), [`jb_night_markets`](MY/jb_night_markets/jb_night_markets.md), [`kl_korean_restaurants`](MY/kl_korean_restaurants/kl_korean_restaurants.md), [`kl_park_and_ride`](MY/kl_park_and_ride/kl_park_and_ride.md), [`my_bus_terminals`](MY/my_bus_terminals/my_bus_terminals.md), [`my_highway_rest_areas`](MY/my_highway_rest_areas/my_highway_rest_areas.md), [`my_national_parks`](MY/my_national_parks/my_national_parks.md)
* **United Arab Emirates:** [`auh_public_parks`](UAE/auh_public_parks/auh_public_parks.md), [`auh_shopping_malls`](UAE/auh_shopping_malls/auh_shopping_malls.md), [`dxb_korean_restaurants`](UAE/dxb_korean_restaurants/dxb_korean_restaurants.md), [`dxb_metro_tram_stations`](UAE/dxb_metro_tram_stations/dxb_metro_tram_stations.md), [`dxb_public_parks`](UAE/dxb_public_parks/dxb_public_parks.md), [`dxb_shopping_malls`](UAE/dxb_shopping_malls/dxb_shopping_malls.md), [`uae_international_schools`](UAE/uae_international_schools/uae_international_schools.md), [`uae_museums`](UAE/uae_museums/uae_museums.md)
* **Australia:** [`au_aged_care_homes`](AU/au_aged_care_homes/au_aged_care_homes.md), [`au_tafe_campuses`](AU/au_tafe_campuses/au_tafe_campuses.md)
* **Hong Kong:** [`hk_boundary_control_points`](HK/hk_boundary_control_points/hk_boundary_control_points.md), [`hk_elderly_care_homes`](HK/hk_elderly_care_homes/hk_elderly_care_homes.md), [`hk_licensed_hotels_guesthouses`](HK/hk_licensed_hotels_guesthouses/hk_licensed_hotels_guesthouses.md)
* **New Zealand:** [`nz_visitor_centres`](NZ/nz_visitor_centres/nz_visitor_centres.md)
* **Taiwan:** [`tw_international_schools`](TW/tw_international_schools/tw_international_schools.md), [`tw_mandarin_learning_centers`](TW/tw_mandarin_learning_centers/tw_mandarin_learning_centers.md), [`tw_night_markets`](TW/tw_night_markets/tw_night_markets.md)
* **United Kingdom:** [`uk_jobcentres`](UK/uk_jobcentres/uk_jobcentres.md), [`uk_motorway_service_areas`](UK/uk_motorway_service_areas/uk_motorway_service_areas.md), [`uk_ofs_registered_providers`](UK/uk_ofs_registered_providers/uk_ofs_registered_providers.md), [`uk_youth_hostels`](UK/uk_youth_hostels/uk_youth_hostels.md)
* **United States:** [`python_world_series`](python_world_series/python_world_series.md), [`us_cbp_ports_of_entry`](US/us_cbp_ports_of_entry/us_cbp_ports_of_entry.md), [`us_milb_ballparks`](US/us_milb_ballparks/us_milb_ballparks.md), [`us_stadiums`](US/us_stadiums/us_stadiums.md), [`us_uscis_offices`](US/us_uscis_offices/us_uscis_offices.md)

## How filters work

Each API page lists the query parameters it accepts. The rules below apply to all of them.

| Rule | Syntax | Example |
|---|---|---|
| Exact match | `field=value`. Ignores case. | `?tag=nfl` |
| Wildcard | `*` at the start, the end or both. Only on parameters marked Wildcard Yes. | `?winner=*Yankees*`, `?stadium=Soldier*`, `?stadium=*Field` |
| Range | `min..max`, both bounds included. Only on parameters marked Range Yes. Both bounds are required. | `?capacity=5000..8000` |
| Several filters | Joined with AND. | `?level=Triple-A&capacity=10000..16600` |
| Nested field | Bracket notation. | `?address[postalcode]=440053` |
| Spaces | Encode as `%20`. | `?address[city]=Mont%20Kiara` |

Notes:

* `__id`, where an API returns it, accepts ranges such as `__id=3..10`.
* A wildcard or range on a parameter that does not allow it returns an empty array, `[]`.
* Curl treats `[` and `]` as a glob pattern. Add `-g` to the command when a URL has brackets.
* Every API page lists all fields of the API. A parameter marked Disallowed is returned in the output but is rejected with a 400 error when you use it as a filter.
* Add `.xml` to the API name to get XML instead of JSON, for example `/JLEE/python_world_series.xml`. XML is not available for `us_stadiums` and the three Korean restaurant APIs, because some of their keys clash in XML.

## Errors

| Status | Meaning |
|---|---|
| `200` | Success. A search with no match returns `[]`. |
| `400` | Bad request. The JSON body has a `message`, for example `Request includes unknown parameters: foo`, `Request attempts to filter on disallowed parameters: address[state]` or `Your request is missing the following required parameters: tag`. |
| `404` | No API with that name. |

An error body looks like this:

```json
{
  "timestamp": "2026-10-26T17:34:29.850943814",
  "status": "BAD_REQUEST",
  "message": "Your request is missing the following required parameters: tag. You did not supply any."
}
```


