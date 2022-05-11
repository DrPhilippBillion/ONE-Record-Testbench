# Basic Data Creation

This file serves to provide a basic data set for using ONE Record.

## Comments and Assumptions

Comment: Here we chose to go a top-down approach, creating first company, then companyBranch, then location, then addresse, then linking them by patching; a more efficient way would be to create bottom up (address first, company last), as this would not require any patch, and links can be included already at the time of creation. 

Another way of doing it: embedding all objects in a single call, then making calls to find out the links, is disputed if possible and covered by the standard. This would then only require a single POST 



## Open Issues

Creation of linked objects via embedding LOs


## Roles and Addresses

Shipper's server address: 1r.portal.com/apple (Shipper is hosted on a ONE Record portal)

Forwarder's server address: 1r.schenker.com (Forwarder is self-hosted)

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
    "cargo": "https://onerecord.iata.org/",
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
    "cargo": "https://onerecord.iata.org/",
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
    "cargo": "https://onerecord.iata.org/",
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
    "cargo": "https://onerecord.iata.org/",
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

Response

```http
204 The Update has been successful
```


  "cargo:location#address": "https://1r.portal.com/apple/headquarter/location"

## Shipment Record

This creates a complete example Shipment record with records of the shipper, the forwarder, and the carrier.

### Creation of Shipper's objects

#### iPhone 11 Product

Request

```http
POST /shipper-name/product HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/",
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
    "cargo": "https://onerecord.iata.org/",
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

Backlink Item in Product

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
    "cargo": "https://onerecord.iata.org/",
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
    "cargo": "https://onerecord.iata.org/",
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

*POST: Backlink in item**

***Issue: no backlink in item to piece defined***
