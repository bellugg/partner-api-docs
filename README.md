
# Bellugg Agent API Specification

### Table of Contents
* [Addresses](#addresses)
  * [Search addresses](#search-addresses)
* [Coverage areas](#coverage-areas)
  * [List coverage areas](#list-coverage-areas)
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
Authorization | string | API key issued to agent

#### Request query string
Attribute | Value
--- | ---
type | enum('source', 'destination')
query | string
sourceAddressId | integer or null (required integer if type='destination')
language | string or null

#### Response body
Attribute | Value
--- | ---
payload | [SearchAddressPayload](#searchaddresspayload)\[\]

##### SearchAddressPayload
Attribute | Value
--- | ---
id | integer
query | string
placeId | string
language | string
name | string
description | string
manuallyInput | boolean
lat | float
lng | float
geometry | [Geometry](https://tools.ietf.org/html/rfc7946#section-3)
lastFetchFromGoogle | string (ISO8601)
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)
isCovered | boolean

---

## Coverage areas

### List coverage areas
##### Endpoint: `/coverageAreas`
##### Method: GET
#### Request header
Attribute | Value | Description
--- | --- | ---
Authorization | string | API key issued to agent

#### Request query string
Attribute | Value
--- | ---
sourceCoverageAreaId | integer or null

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

## Prices

### Calculate price
##### Endpoint: `/price/calculate`
##### Method: POST
#### Request header
Attribute | Value | Description
--- | --- | ---
Authorization | string | API key issued to agent

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
Authorization | string | API key issued to agent

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
Authorization | string | API key issued to agent

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
Attribute | Value
--- | ---
id | integer
query | string
placeId | string
language | string
name | string
description | string
manuallyInput | boolean
lat | float
lng | float
geometry | [Geometry](https://tools.ietf.org/html/rfc7946#section-3)
lastFetchFromGoogle | string (ISO8601)
insertedAt | string (ISO8601)
updatedAt | string (ISO8601)

##### Luggage
Attribute | Value
--- | ---
id | integer
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
