# UseCase Overview

This document is at early stage, please correct and comment extensively!

(for a UseCase overview, please refer to the image at the end of the document)

# Setting

This testbench reflects a typical setting of Shipper, Forwarder and Carrier in the supply chain. But keep in mind, that it only roughly reflects the business process around these three stakeholders.

Shipper's server address: 1r.portal.com/shipper-name (Shipper is hosted on a ONE Record portal)

Forwarder's server address: 1r.forwarder-name.com (Forwarder is self-hosted)

Carrier's address: 1r.carrier-name.com (Carrier is self-hosted)

To simplify the setting, GHA and Carrier are combined in the Carrier role.

# Assumptions

# Requirements

- here: Auxiliary script for the required linked objects for the test runs

- TransportMovement by Carrier/GHA

- ULD

# UseCase definitions

## UseCase 1: Shipper creates and publishes piece
### Step 1: Shipper creates piece
#### Scope

Scope of this step is to evaluate the function to create data objects in ONE Record.

#### Todos

- Mandatory fields must be checked / added according to TTL
- Write axiliary script to provide required linked objects

#### Issues

#### Comments

#### POST-Request

```http
POST /shipper-name/piece HTTP/1.1
Host: 1r.portal.com
Content-Type: application/ld+json
Accept: application/ld+json
{
  "@context": {
    "cargo": "https://onerecord.iata.org/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "cargo:Piece",
  "cargo:Piece#grossWeight": {
    "@type": "cargo:Value",
    "cargo:Value#unit": "kg",
    "cargo:Value#value": 85
  }
}
```

#### Expected Response

```http
201 Created
Location: https://1r.portal.com/shipper-name/Piece_8dc
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/Piece
```


### Step 2: Shipper publishes piece towards Forwarder
#### Scope

Scope of this step is to evaluate the publish function in ONE Record.

### Step 2.1: Shipper proposes a subscription to Forwarder

#### Todos

#### Issues

#### Comments

#### POST-Request

```http
GET /forwarder-name?topic=https://onerecord.iata.org/cargo/Piece HTTP/1.1
Host: 1r.forwarder-name.com
Content-Type: application/ld+json
Accept: application/ld+json
```

#### Expected Response
```http
200 OK
Contept-Type: application/ld+json

{
  "@type": "api:Subscription",
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/",
    "publisher": "https://1r.portal.com/shipper-name",
    "subscriber": "https://1r.forwarder-name.com"
  },
  "api:Subscription#contentTypes": " application/ld+json ",
  "api:Subscription#cacheFor": "86400",
  "api:Subscription#callbackUrl": "subscriber/callback",
  "api:Subscription#errors": [],
  "api:Subscription#myCompanyIdentifier": "subscriber",
  "api:Subscription#secret": "C89583BA9B1FEEAB25F715A3BA2F3",
  "api:Subscription#sendLogisticsObjectBody": true,
  "api:Subscription#subscribeToStatusUpdates": true,
  "api:Subscription#subscribedTo": "publisher",
  "api:Subscription#topic": "cargo:Piece"
}
```

### Step 2.2 Shipper sends notification about the creation of the Piece to Forwarder's callback endpoint
#### Todos

#### Issues

#### Comments

- No authorization, verification etc. considered

#### POST-Request
```http
POST /callback
Host: 1r.forwarder-name.com 
Accept: application/ld+json
Content-Type: application/ld+json

{
  "@context": {
    "cargo": "https://onerecord.iata.org/cargo/",
    "api": "https://onerecord.iata.org/api/"
  },
  "@type": "api:Notification",
  "eventType": "OBJECT_CREATED",
  "topic": "cargo:Piece",
  "logisticsObjectRef": "https://1r.portal.com/shipper-name/Piece_8dc"
}
```
#### Expected Response
```http
204 Notification received successfully
Content-Type: application/ld+json
```


## UseCase 2: Forwarder subscribes on Shipper´s piece

#### Scope

#### Todos

#### Issues

- A specification of the usage of the API or endpoints is missing in the API specification?

#### Comments

#### POST-Request

```http
... 
```

#### Expected Response

```http
....
```


## UseCase 3: Forwarder creates and publishes shipment and links pieces

### Step 1: Forwarder creates shipment with totalGrossWeight

**Input**
```http
POST /forwarder/shipment HTTP/1.1
Host: 1r.platform.com 
Authorization: (Bearer Token)
Accept: application/ld+json
Content-Type: application/ld+json

{
  "totalGrossWeight":
    {
    "value": 12000,
    "unit": "kg"
    }
}
```
**Expected Output**
```http
HTTP/1.1 201 Created
Date: ...
Location: http://1r.platform.com/forwarder/shipment/a01
```

**Comment**

### Step 2: Forwarder links piece in shipment
**Input**
```http
POST /forwarder/shipment/a01 HTTP/1.1
Host: 1r.platform.com 
Authorization: (Bearer Token)
Accept: application/ld+json
Content-Type: application/ld+json

{
  "containedPiece": "1r.platform.com/shipper/piece/8dc"
}
```
**Expected Output**
```http
201 Logistics Object has been published to the Internet of Logistics
```
**Issues**

Is this not clear to me, how data fields are added after the creation of the LO. This is a suggestion to be evaluated, based on POST. This applies if the field wasn´t ever created. If it exists, PATCH might be used.

### Step 3: Forwarder sets CR to Shipper/piece to set backlink for shipment
**Input**
```http
PATCH /shipper/piece/8dc HTTP/1.1
Host: 1r.platform.com 
Authorization: (Bearer Token)
Content-Type: application/ld+json

{
  "revision":"1",
  "description":"Backlink for Piece in Shipment",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/Piece#shipment"
      "o":{
        "value":"1r.platform.com/forwarder/shipment/a01",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```



**Expected Output**
```http
204 The Update has been successful
```
### Step 4: Forwarder publishes shipment to Carrier / GHA
**Input**
```http
PATCH /shipper/piece/8dc HTTP/1.1
Host: 1r.platform.com 
Authorization: (Bearer Token)
Content-Type: application/ld+json

{
  "revision":"1",
  "description":"Backlink for Piece in Shipment",
  "operations":[
      "op":"add",
      "p":"https://onerecord.iata.org/Piece#shipment"
      "o":{
        "value":"1r.platform.com/forwarder/shipment/a01",
        "datatype":"https://www.w3.org/2001/XMLSchema#string"
        }
   ]
}
```



**Expected Output**
```http
204 The Update has been successful
## UseCase 4: Forwarder performs access delegation of piece to GHA

#### Scope

#### Todos

#### Issues

#### Comments

#### POST-Request

```http
POST 
```

#### Expected Response

## UseCase 4: GHA/Carrier links piece with ULD

## UseCase 5: GHA/Carrier links ULD with transportMovement 
### Step 1: GHA/Carrier creates link to transportMovement in ULD
***tbd***
### Step 2: GHA/Carrier sets CR to Carrier/transportMovement to include piece link
***tbd***
### Step 3: GHA/Carrier confirms CR to transportMovement to include piece link
***tbd***
## UseCase 6: GHA/Carrier perform CR to Shipper/piece to correct weight to 45 kg
***tbd***
Step 1: CR

Step 2: Check on notification by FF
## UseCase 7: Shipper executes CR

## UseCase 8: Forwarder get notified on change

## UseCase 9: Check audit trail of Piece by Carrier

## UseCase 10: At DEP: GHA/Carrier triggers Memento incl. Piece, ULD, Shipment and flight
***tbd***
## UseCases >=11: Multi-Link environment, Security, Authentication, etc.
***tbd***


![UseCases](/EmbeddedIllustrations/UseCases2.png)
        
