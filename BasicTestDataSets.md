# Basic Data Creation

This file serves to provide a basic data set for using ONE Record.

## Roles and Addresses

Shipper's server address: 1r.portal.com/apple (Shipper is hosted on a ONE Record portal)

Forwarder's server address: 1r.schenker.com (Forwarder is self-hosted)

Carrier's address: 1r.lufthansa-cargo.com (Carrier is self-hosted)

GHA's address: 1r.swissport.com (GHA is self-hosted)


## Shipment Record

This creates a complete example Shipment record with records of the shipper, the forwarder, and the carrier.

### Creation of Shipper's objects

#### Product

*POST-Request*

```http
POST /shipper-name/products HTTP/1.1
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
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/products/iphone11_rev1233
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/product
```

#### Item

*POST-Request*

```http
POST /apple/items HTTP/1.1
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
  "cargo:Item#product": "https://1r.portal.com/apple/products/iphone11_rev1233",
  }
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/items/IMEI4223111
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
  "cargo:Item#isInItems": "https://1r.portal.com/apple/items/IMEI4223111",
  }
}
```

*Expected Response*

```http
201 Created
Location: https://1r.portal.com/apple/products/iphone11_rev1233
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/item
```
