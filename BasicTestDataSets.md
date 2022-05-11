# Basic Data Creation

This file serves to provide a basic data set for using ONE Record.

## Open Issues

Creation of linked objects via embedding LOs


## Roles and Addresses

Shipper's server address: 1r.portal.com/apple (Shipper is hosted on a ONE Record portal)

Forwarder's server address: 1r.schenker.com (Forwarder is self-hosted)

Carrier's address: 1r.lufthansa-cargo.com (Carrier is self-hosted)

GHA's address: 1r.swissport.com (GHA is self-hosted)

## Companies

### Apple

*POST*

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
*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```
#### Apple Headquarter
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
*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/headquarter
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/companyBranch
```
*Link company and branch*

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
  "cargo:company#branch": "1r.portal.com/apple/headquarter"
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```
#### Apple Headquarter Location 

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
*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/headquarter/location
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

*Link branch and location*

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
  "cargo:companyBranch#location": "https://1r.portal.com/apple/headquarter/location"
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/headquarter/location
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/company
```

#### Apple Headquarter Location Address

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
*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/headquarter/location/address
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/address
```


*Link location and address*

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
  "@type": "cargo:location",
  "cargo:location#address": "https://1r.portal.com/apple/headquarter/location"
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/headquarter/location/address
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/location
```

## Shipment Record

This creates a complete example Shipment record with records of the shipper, the forwarder, and the carrier.

### Creation of Shipper's objects

#### iPhone 11 Product

*POST-Request*

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

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/product/iphone11_rev1233
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/product
```

#### Item 1: iPhone 11 with IMEI4223111

*POST-Request*

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

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/item/IMEI4223111
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```

*POST: Backlink in product**

```http
POST /apple/prodcut HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:product",
  "cargo:Item#isInItems": "https://1r.portal.com/apple/items/IMEI4223111"
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/products/iphone11_rev1233
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```


#### Item 2: iPhone 11 with IMEI4223112

*POST-Request*

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

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/items/IMEI4223112
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```

*POST: Backlink in product**

```http
POST /apple/prodcut HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:product",
  "cargo:Item#isInItems": "https://1r.portal.com/apple/items/IMEI4223112",
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/products/iphone11_rev1233
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```

#### Piece 8f5xxd by Apple with iPhones

*POST-Request*

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

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/piece/8f5xxd
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/piece
```

*POST: Backlink in item**

***Issue: no backlink in item to piece defined***
