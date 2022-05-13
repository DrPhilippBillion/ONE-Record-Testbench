# Basic Data Creation

This file serves to provide a basic data set for using ONE Record.

## Comments and Assumptions

Comment: Here we chose to go a top-down approach, creating first company, then companyBranch, then location, then addresse, then linking them by patching; a more efficient way would be to create bottom up (address first, company last), as this would not require any patch, and links can be included already at the time of creation.

Another way of doing it: embedding all objects in a single call, then making calls to find out the links, is disputed if possible and covered by the standard. This would then only require a single POST



## Open Issues

Creation of linked objects via embedding LOs


## Roles and Addresses

Shipper's server address: 1r.portal.com/apple (Shipper is hosted on a ONE Record portal)

Forwarder's server address: 1r.dbschenker.com (Forwarder is self-hosted)

Carrier's address: 1r.lufthansa-cargo.com (Carrier is self-hosted)

GHA's address: 1r.swissport.com (GHA is self-hosted)

## Companies

### Apple

Request

```http
POST /apple HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:Company",
  "cargo:company#companyName": "Apple Inc.",
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

#### Apple Headquarter

Request

```http
POST /apple/headquarter HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:CompanyBranch",
  "cargo:companyBranch#company": "1r.portal.com/apple",
  "cargo:companyBranch#branchName": "Apple Headquarter"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/headquarter
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/companyBranch
```
*Link company and branch*

Request

```http
PATCH /apple/ HTTP/1.1
Host: 1r.platform.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for companyBranch in company",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/company#branch"
      "o":{
        "value":"https://1r.portal.com/apple/headquarter",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Apple Headquarter Location

Request

```http
POST /apple/headquarter/ HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:location"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/headquarter/location
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

*Link branch and location*

Request

```http
PATCH /apple/headquarter HTTP/1.1
Host: 1r.platform.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for location in companyBranch",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/companyBranch#location"
      "o":{
        "value":"https://1r.portal.com/apple/headquarter/location",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Apple Headquarter Location Address

Request

```http
POST /apple/headquarter/location HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:address",
   "cargo:address#street": "One Apple Park Way",
   "cargo:address#postalCode": "95014",
   "cargo:address#cityName": "Cupertino",
   "cargo:address#regionCode": "CA",
   "cargo:address#country": "USA"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/headquarter/location/address
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/address
```

*Link location and address*

Request

```http
PATCH /apple/headquarter/location HTTP/1.1
Host: 1r.platform.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for street in location",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/companyBranch/location#address"
      "o":{
        "value":"https://1r.portal.com/apple/headquarter/location/address",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

### Schenker

Request

```http
POST / HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:Company",
  "cargo:company#companyName": "Schenker Deutschland AG",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

#### Schenker Headquarter

Request

```http
POST /headquarter HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:CompanyBranch",
  "cargo:companyBranch#company": "1r.dbschenker.com",
  "cargo:companyBranch#branchName": "DB Schenker Headquarter"
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/headquarter
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/companyBranch
```
*Link company and branch*

Request

```http
PATCH / HTTP/1.1
Host: 1r.dbschenker.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for companyBranch in company",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/company#branch"
      "o":{
        "value":"https://1r.dbschenker.com/headquarter",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Schenker Headquarter Location

Request

```http
POST /headquarter/ HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:location"
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/apple/headquarter/location
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

*Link branch and location*

Request

```http
PATCH /headquarter HTTP/1.1
Host: 1r.dbschenker.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for location in companyBranch",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/companyBranch#location"
      "o":{
        "value":"https://1r.dbschenker.com/headquarter/location",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Schenker Headquarter Location Address

Request

```http
POST /headquarter/location HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:address",
   "cargo:address#street": "Lyoner Strasse 15",
   "cargo:address#postalCode": "60528",
   "cargo:address#cityName": "Frankfurt/Main",
   "cargo:address#country": "Germany"
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/headquarter/location/address
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/address
```

*Link location and address*

Request

```http
PATCH /headquarter/location HTTP/1.1
Host: 1r.dbschenker.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for street in location",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/companyBranch/location#address"
      "o":{
        "value":"https://1r.dbschenker.com/headquarter/location/address",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```


### Lufthansa Cargo

Request

```http
POST / HTTP/1.1
Host: 1r.lufthansa-cargo.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:Company",
  "cargo:company#companyName": "Lufthansa Cargo AG",
}
```

Response

```http
201 Created
Location: https://1r.lufthansa-cargo.com/
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

#### Lufthansa Cargo Headquarter

Request

```http
POST /headquarter HTTP/1.1
Host: 1r.lufthansa-cargo.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:CompanyBranch",
  "cargo:companyBranch#company": "1r.lufthansa-cargo.com",
  "cargo:companyBranch#branchName": "Lufthansa Cargo Headquarter"
}
```

Response

```http
201 Created
Location: https://1r.lufthansa-cargo.com/headquarter
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/companyBranch
```
*Link company and branch*

Request

```http
PATCH / HTTP/1.1
Host: 1r.lufthansa-cargo.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for companyBranch in company",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/company#branch"
      "o":{
        "value":"https://1r.lufthansa-cargo.com/headquarter",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Lufthansa Cargo Headquarter Location

Request

```http
POST /headquarter/ HTTP/1.1
Host: 1r.lufthansa-cargo.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:location"
}
```

Response

```http
201 Created
Location: https://1r.lufthansa-cargo.com/apple/headquarter/location
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

*Link branch and location*

Request

```http
PATCH /headquarter HTTP/1.1
Host: 1r.lufthansa-cargo.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for location in companyBranch",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/companyBranch#location"
      "o":{
        "value":"https://1r.lufthansa-cargo.com/headquarter/location",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Lufthansa Cargo Headquarter Location Address

Request

```http
POST /headquarter/location HTTP/1.1
Host: 1r.lufthansa-cargo.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:address",
   "cargo:address#street": "Frankfurt Airport, Gate 21, Building 322",
   "cargo:address#postalCode": "60546",
   "cargo:address#cityName": "Frankfurt am Main",
   "cargo:address#country": "Germany"
}
```

Response

```http
201 Created
Location: https://1r.lufthansa-cargo.com/headquarter/location/address
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/address
```

*Link location and address*

Request

```http
PATCH /headquarter/location HTTP/1.1
Host: 1r.lufthansa-cargo.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for street in location",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/companyBranch/location#address"
      "o":{
        "value":"https://1r.lufthansa-cargo.com/headquarter/location/address",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

## General Locations

### SFO Airport at Schenker

Request

```http
POST /locations/airports HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:location",
  "cargo:location#locationName": "San Francisco Airport",
  "cargo:location#locationType": "International Airport",
  "cargo:location#locationCode": "SFO",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/locations/airports/SFO
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/location
```

### LHR Airport at Schenker

Request

```http
POST /locations/airports HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:location",
  "cargo:location#locationName": "London Heathrow Airport",
  "cargo:location#locationType": "International Airport",
  "cargo:location#locationCode": "LHR",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/locations/airports/SFO
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/location
```


## Shipment Record

This creates a complete example Shipment record with records of the shipper, the forwarder, and the carrier.

### Shipper's objects

#### iPhone 11 Product

Request

```http
POST /shipper-name/product HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:Product",
  "cargo:Product#productDescription": "iPhone 11",
  "cargo:Product#productIdentifier": "iP11",
  "cargo:Product#hsCode": "8517.12",
  "cargo:Product#hsType": "EU Harmonized System Code",
  "cargo:Product#hsCommodityName": "ELECTRICAL MACHINERY",
  "cargo:Product#hsCommodityDescription": "Telephone sets"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/product/iphone11_rev1233
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/product
```

#### Item 1: iPhone 11 with IMEI4223111

Request

```http
POST /apple/item HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:item",
  "cargo:Item#otherIdentifier": "IMEI4223111",
  "cargo:Item#batchNumber": "2022/08-55-n5",
  "cargo:Item#productionCountry": "China",
  "cargo:Item#product": "https://1r.portal.com/apple/products/iphone11_rev1233"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/item/IMEI4223111
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```

*Backlink Item in Product*

Request

```http
PATCH /apple/product HTTP/1.1
Host: 1r.platform.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"2",
  "description":"Backlink for item in product",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/product#isInItem"
      "o":{
        "value":"https://1r.portal.com/apple/items/IMEI4223111",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Item 2: iPhone 11 with IMEI4223112

Request

```http
POST /apple/item HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:item",
  "cargo:Item#otherIdentifier": "IMEI4223112",
  "cargo:Item#batchNumber": "2022/08-55-n5",
  "cargo:Item#productionCountry": "China",
  "cargo:Item#product": "https://1r.portal.com/apple/products/iphone11_rev1233"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/items/IMEI4223112
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```

*Backlink Item in product*

Request

```http
PATCH /apple/product HTTP/1.1
Host: 1r.platform.com
Authorization: (Bearer Token)
Content-Type: application/ld+json
{
  "revision":"3",
  "description":"Backlink for item in product",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/product#isInItem"
      "o":{
        "value":"https://1r.portal.com/apple/items/IMEI4223112",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```

Response

```http
204 The Update has been successful
```

#### Piece 8f5xxd by Apple with iPhones

Request

```http
POST /apple/piece HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:piece",
  "cargo:Piece#uPID": "8f5xxd",
  "cargo:Piece#grossWeight": {
      "cargo:value": "85",
      "cargo:unit": "kg"
  },
  "cargo:containedItems": "https://1r.portal.com/apple/items/IMEI4223112",
  "cargo:containedItems": "https://1r.portal.com/apple/items/IMEI4223111",
  "cargo:shipper": "https://1r.portal.com/apple",
  "cargo:goodsDescription": "320 iPhones",
  "cargo:declaredValueForCustoms": {
      "cargo:value": "6895",
      "cargo:unit": "USD"
}
```

Response

```http
201 Created
Location: https://1r.portal.com/apple/piece/8f5xxd
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/piece
```

*Backlink piece in item**

***Issue: no backlink in item to piece defined***

## Forwarders objects

Here we go from bottom to top to avoid patching backlinks in


### TransportMeansOperator: Drivers License of Driver for Schenker's truck

*possible problem here: Person doesn´t exist yet, still path can be used or not? If not, backlinking neccessary*

```http
POST /persons/KurtMueller/license HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:externalReference",
  "cargo:externalReference#documentName": "Führerschein",
  "cargo:externalReference#documentId": "5523488",
  "cargo:externalReference#documenttype": "Drivers License",
  "cargo:externalReference#documentdate": "2020-10-26T00:00:00+01:00",
  }
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/persons/KurtMueller/DriversLicense
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/externalReference
```

### TransportMeansOperator: Truckdriver details

```http
POST /persons/ HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:person",
  "cargo:person#fistName": "Kurt",
  "cargo:person#lastName": "Müller",
  "cargo:person#employeeID": "85523d5",
  "cargo:person#externalReference": "85523d5",
  }
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/persons/KurtMueller
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/person
```

### TransportMeans: Truck for the first leg

```http
POST /trucks/ HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:transportMeans",
  "cargo:transportMeans#transportCompany": "1r.dbschenker.com",
  "cargo:transportMeans#vehicleType": "Truck"
  "cargo:transportMeans#vehicleRegistration": "DUB 99 GL"
  }
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/trucks/DUB99GL
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/transportMeans
```
### transportMovement AppleHQ to SFO

```http
POST /transportMovement/ HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:TransportMovement",
  "cargo:transportMovement#movementType": "actual",
  "cargo:transportMovement#departureLocation": "https://1r.portal.com/apple/headquarter/location",
  "cargo:transportMovement#arrivalLocation": "https://1r.dbschenker.com/locations/airports/SFO",
    "cargo:transportMovement#TransportMeans": "https://1r.dbschenker.com/trucks/DUB99GL",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/transportMovement/AppleHQ-SFO
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/transportMovement
```

### HandlingInstructions 

```http
POST /HandlingInstructions/ HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:handlingInstructions",
  "cargo:handlingInstructions#serviceType": "SSR",
  "cargo:handlingInstructions#serviceDescription": "Always store below 0 degrees celsius",

}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/handlingInstructions/frozen
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/handlingInstructions
```

```http
POST /HandlingInstructions/ HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:handlingInstructions",
  "cargo:handlingInstructions#serviceType": "SPH",
  "cargo:handlingInstructions#serviceTypeCode": "HEA",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/handlingInstructions/HEA
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/handlingInstructions
```

### Piece 2255468 by Schenker including Apple's pieces

Request

```http
POST /piece HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:piece",
  "cargo:Piece#uPID": "2255468",
  "cargo:piece#containedPieces": "https://1r.portal.com/apple/piece/8f5xxd",
  "cargo:piece#shipper": "https://1r.dbschenker.com",
  "cargo:piece#handlingInstructions": "https://1r.dbschenker.com/handlingInstructions/frozen",
  "cargo:piece#handlingInstructions": "https://1r.dbschenker.com/handlingInstructions/HEA",  
  "cargo:piece#transportMovement": "https://1r.dbschenker.com/transportMovement/AppleHQ-SFO
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/piece/2255468
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/piece
```

### Shipment INT98221 by Schenker including all pieces

Request

```http
POST /shipments HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:shipment",
  "cargo:shipment#containedPieces": "https://1r.dbschenker.com/piece/2255468",
  "cargo:shipment#totalGrossWeight": {
      "cargo:value": "1300",
      "cargo:unit": "kg"
  }
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/shipment/2255468
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/shipment
```

### BookingSegment SFO-LHR by Schenker for MAWB booking at Carrier

Request

```http
POST /bookingSegment HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:BookingSegment",
  "cargo:BookingSegment#departureLocation": "https://1r.dbschenker.com/locations/airports/SFO",
  "cargo:BookingSegment#arrivalLocation": "https://1r.dbschenker.com/locations/airports/LHR"
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/bookingSegment/SFO-LHR
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/shipment
```

### Parties for MAWB Booking

Request

```http
POST / HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:party",
  "cargo:party#partyDetails": "https://1r.dbschenker.com/",
  "cargo:party#partyRole": "Freight Forwarer",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/dbSchenkerAsForwarder
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/party
```

Request

```http
POST /parties/ContractualShippers HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:party",
  "cargo:party#partyDetails": "https://1r.portal.com/apple/",
  "cargo:party#partyRole": "Shipper",
}
```

Response

```http
201 Created
Location: https://1r.dbschenker.com/ContractualShippers/Apple
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/party
```

### BookingOptionRequest SFO-LHR by Schenker for air leg

Request

```http
POST /bookingOptionRequest HTTP/1.1
Host: 1r.dbschenker.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:BookingOptionRequest",
  "cargo:BookingOptionRequest#bookingSegment": "https://1r.dbschenker.com/bookingSegment/SFO-LHR",
  "cargo:BookingOptionRequest#requestType": "booking",
  "cargo:BookingOptionRequest#shipmentSecurityStatus": "SPX"  
  "cargo:BookingOptionRequest#parties":
  {
    "https://1r.dbschenker.com/ContractualShippers/Apple",
    "https://1r.dbschenker.com/dbSchenkerAsForwarder"
  }

```

Response

```http
201 Created
Location: https://1r.dbschenker.com/bookingOptionRequest/22334
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/bookingOptionRequest
```
