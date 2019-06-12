  
# Bellugg Agent API Specification

### Table of Contents
* [Addresses](#addresses)
  * [Search addresses](#search-addresses)
* [Coverage areas](#coverage-areas)
  * [List source coverage areas](#list-source-coverage-areas)
  * [List destination coverage areas](#list-destination-coverage-areas)
* [Operation times](#operation-times)
  * [List available operation times](#list-available-operation-times)
* [Prices](#prices)
  * [Calculate price](#calculate-price)
* [Bookings](#bookings)
  * [Create booking](#create-booking)
  * [Get booking](#get-booking)
* [Error response](#error-response)
* [Object reference](#object-reference)

---

## Addresses

### Search addresses
##### Endpoint: `/addresses`
##### Method: GET
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Request query string
Attribute | Value | Description
--- | --- | ---
type | enum('source', 'destination') | Indicates whether search target is source or destination
query | string | Search query (by name)
sourceAddressId | integer or null | Indicates source address associates with target destination (required when type = 'destination')
language | string or null | Search language bias

#### Response body
Attribute | Value
--- | ---
payload | [SearchAddressPayload](#searchaddresspayload)\[\]

##### SearchAddressPayload
Attribute | Value | Description
--- | --- | ---
id | integer
query | string | Search query
placeId | string | Google's placeId
language | string
name | string
description | string
manuallyInput | boolean | Indicates whether this address info was input manually or programmatically
lat | float | Latitude
lng | float | Longitude
geometry | [Geometry](https://tools.ietf.org/html/rfc7946#section-3)
lastFetchFromGoogle | string (ISO8601) | Indicates last refresh placeId date
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)
isCovered | boolean | Indicates whether this address is support or not

---

## Coverage areas

### List source coverage areas
##### Endpoint: `/sourceCoverageAreas`
##### Method: GET
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Response body
Attribute | Value
--- | ---
payload | [CoverageAreaPayload](#coverageareapayload)\[\]

##### CoverageAreaPayload
Attribute | Value
--- | ---
id | integer
coverageAreaTypeId | integer
geometry | [Geometry](https://tools.ietf.org/html/rfc7946#section-3)
name | string
symbol | string or null
location | string
active | boolean
limousineSupport | boolean
placeId | string or null
legacyAirportId | integer or null
legacyBranchId | integer or null
ipPrinter | string or null
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

---

### List destination coverage areas
##### Endpoint: `/destinationCoverageAreas`
##### Method: GET
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Request query string
Attribute | Value | Description
--- | --- | ---
sourceLat | integer or null | Indicates source address latitude
sourceLng | integer or null | Indicates source address longitude

#### Response body
Attribute | Value
--- | ---
payload | [CoverageAreaPayload](#coverageareapayload)\[\]

##### CoverageAreaPayload
Attribute | Value
--- | ---
id | integer
coverageAreaTypeId | integer
geometry | [Geometry](https://tools.ietf.org/html/rfc7946#section-3)
name | string
symbol | string or null
location | string
active | boolean
limousineSupport | boolean
placeId | string or null
legacyAirportId | integer or null
legacyBranchId | integer or null
ipPrinter | string or null
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

---

## Operation times

### List available operation times
##### Endpoint: `/operationTimes`
##### Method: GET
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Request query string
Attribute | Value | Description
--- | --- | ---
sourceAddressId | integer | Indicates source address
destinationAddressId | integer | Indicates destintion address

#### Response body
Attribute | Value
--- | ---
payload | [OperationTimesPayload](#operation-time)\[\]

---

## Prices

### Calculate price
##### Endpoint: `/price/calculate`
##### Method: POST
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Request body
Attribute | Value
--- | ---
sourceAddressId | integer
destinationAddressId | integer
currency | string (ISO4217)
dropOffDateTime | string (ISO8601)
pickUpDateTime | string (ISO8601)
luggage | [Luggage](#luggage)\[\]

#### Response body
Attribute | Value
--- | ---
payload | [PricePayload](#pricepayload)

##### PricePayload
Attribute | Value
--- | ---
price | float
currency | string (ISO4217)
luggagePrices| [LuggagePrice](#luggageprice)\[\]

---

## Bookings

### Create booking  
##### Endpoint: `/bookings`
##### Method: POST
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Request body
Attribute | Value
--- | ---
name | string
email | string
social | string
passport | string or null
phoneNumber | string or null
sourceAddressId | integer
destinationAddressId | integer
dropOffDateTime | date (ISO8601)
pickUpDateTime | date (ISO8601)
luggage | [Luggage](#luggage)\[\]
agentRefId | string or null
currency | string (ISO4217)

#### Response body
Attribute | Value
--- | ---
payload | [BookingPayload](#booking)

### Get booking
##### Endpoint: `/bookings`
##### Method: GET
#### Request header
Attribute | Value | Description
--- | --- | ---
X-Api-Key | string | API key issued to agent

#### Request parameter
Attribute | Value
--- | ---
id | string

#### Response body
Attribute | Value
--- | ---
payload | [BookingPayload](#booking)

---

## Error response
##### Endpoint: All
##### Body
Attributes | Value
--- | ---
statusCode | integer
error | string
message | string, Array<T>, ObjectLiteral

---

## Object reference
##### Address
Attribute | Value | Description
--- | --- | ---
id | integer
query | string | Search query
placeId | string | Google's placeId
language | string
name | string
description | string
manuallyInput | boolean | Indicates whether this address info was input manually or programmatically
lat | float | Latitude
lng | float | Longitude
geometry | [Geometry](https://tools.ietf.org/html/rfc7946#section-3)
lastFetchFromGoogle | string (ISO8601) | Indicates last refresh placeId date
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

##### Operation time
Attribute | Value | Description
--- | --- | ---
id | integer
description | string or null
dropOffFrom | string | Indicates drop off start time
dropOffTo | string | Indicate drop off end time
dropOffTimezone | string | Indicate drop off [time zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
pickUpFrom | string or null | Indicates pickup start time (required if dynamicPickup = null)
dynamicPickUp | integer or null | Indicates pickup start time adjusted dynamically to choosen drop off time
pickUpTo | string | Indicates pickup end time
pickUpTimezone | string | Indicates pickup [time zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
isSameDayBooking | boolean | Applies when booking date and drop off date are the same
isSameDayDelivering | boolean | Applies when delivering date and pickup date are the same
priority | integer | Indicates priority, the greater the number the higher priority
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

##### Luggage
Attribute | Value | Description
--- | --- | ---
id | integer | Indicates product's type id
quantity | integer

##### LuggagePrice
Attribute | Value 
--- | ---
id | integer
quantity | integer
price | [Price](#price)
totalPrice | string
currency | string (ISO4217)
productPriceTier | [ProductPriceTier](#productpricetier)

##### Price
Attribute | Value
--- | ---
id | integer
priceTierId | integer
price | float
currency | string (ISO4217)
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

##### ProductPriceTier
Attribute | Value
--- | ---
id | integer
productId | integer
priceTierId | integer
startDate | string (ISO8601)
endDate | string (ISO8601)
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

##### Booking
Attribute | Value
--- | ---
id | string
status | enum('requested', 'approved' 'rejected')
legacyOrderTypeId | integer or null
agentId | integer
agentRefId | string or null
legacyId | string or null
thirdPartyId | string or null
customerName | string
customerEmail | string
customerSocial | string
customerPassport | string or null
customerPhoneNumber | string or null
sourceAddressId | integer
sourceAddressName | string
sourceAddressDescription | string
sourceAddressCoordinate | string
destinationAddressId | integer
destinationAddressName | string
destinationAddressDescription | string
destinationAddressCoordinate | string
dropOffDateTime | string (ISO8601)
pickUpDateTime | string (ISO8601)
luggage | string
price | float
currency | string (ISO4217)
extras | string or null
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)
bookingLines | [BookingLine](#bookingline)\[\]

##### BookingLine
Attribute | Value
--- | ---
id | integer
bookingId | integer
productId | integer
productPriceTierId | integer
price | float
quantity | integer
totalPrice | float
currency | string (ISO4217)
active | boolean

