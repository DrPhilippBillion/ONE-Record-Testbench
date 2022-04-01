- [UseCase Overview](#usecase-overview)
  * [UseCase 1: Shipper creates piece](#usecase-1--shipper-creates-piece)
    + [Step 1: Shipper creates piece with grossWeight](#step-1--shipper-creates-piece-with-grossweight)
    + [Step 2: Shipper publishes piece towards Forwarder](#step-2--shipper-publishes-piece-towards-forwarder)
  * [UseCase 2: Forwarder subscribes on Shipper´s piece](#usecase-2--forwarder-subscribes-on-shipper-s-piece)
  * [UseCase 3: Forwarder creates and links shipment](#usecase-3--forwarder-creates-and-links-shipment)
    + [Step 1: Forwarder creates shipment with totalGrossWeight](#step-1--forwarder-creates-shipment-with-totalgrossweight)
    + [Step 2: Forwarder links piece in shipment](#step-2--forwarder-links-piece-in-shipment)
    + [Step 3: Forwarder sets CR to Shipper/piece to set backlink for shipment](#step-3--forwarder-sets-cr-to-shipper-piece-to-set-backlink-for-shipment)
  * [UseCase 4: Forwarder performs access delegation of piece to GHA](#usecase-4--forwarder-performs-access-delegation-of-piece-to-gha)
  * [UseCase 5: Carrier creates and publishes transport movement](#usecase-5--carrier-creates-and-publishes-transport-movement)
    + [Step 1: Carrier creates transport movement with Origin and Destination](#step-1--carrier-creates-transport-movement-with-origin-and-destination)
    + [Step 2: Carrier publishes transport movement towards GHA and Forwarder](#step-2--carrier-publishes-transport-movement-towards-gha-and-forwarder)
  * [UseCase 6: GHA creates ULD with uPID and uldTypeCode](#usecase-6--gha-creates-uld-with-upid-and-uldtypecode)
  * [UseCase 7: GHA links ULD with Carrier/transportMovement](#usecase-7--gha-links-uld-with-carrier-transportmovement)
    + [Step 1: GHA creates link to transportMovement in ULD](#step-1--gha-creates-link-to-transportmovement-in-uld)
    + [Step 2: GHA sets CR to Carrier/transportMovement to include piece link](#step-2--gha-sets-cr-to-carrier-transportmovement-to-include-piece-link)
    + [Step 3: Carrier confirms CR to transportMovement to include piece link](#step-3--carrier-confirms-cr-to-transportmovement-to-include-piece-link)
  * [UseCase 8: GHA switches assignment piece to shipment](#usecase-8--gha-switches-assignment-piece-to-shipment)
    + [Step 1: Forwarder creates "alternative" shipment with totalGrossweight of 14 kg](#step-1--forwarder-creates--alternative--shipment-with-totalgrossweight-of-14-kg)
    + [Step 2: GHA sets CR to Shipper/piece for changing shipment](#step-2--gha-sets-cr-to-shipper-piece-for-changing-shipment)
    + [Step 3: GHA sets CR to Forwarder/shipment for containedPieces](#step-3--gha-sets-cr-to-forwarder-shipment-for-containedpieces)
  * [UseCase 9: GHA sets CR to Shipper/piece to correct weight to 45 kg](#usecase-9--gha-sets-cr-to-shipper-piece-to-correct-weight-to-45-kg)
  * [UseCase 10: At DEP: Carrier triggers Memento incl. Piece, ULD, Shipment and flight](#usecase-10--at-dep--carrier-triggers-memento-incl-piece--uld--shipment-and-flight)
  * [UseCases >=11: Check Audit Trail, Multi-Link environment, Security, Authentication, etc.](#usecases---11--check-audit-trail--multi-link-environment--security--authentication--etc)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>
# UseCase Overview

This document is at early stage, please correct and comment extensively!

(for a UseCase overview, please refer to the image at the end of the document)

## UseCase 1: Shipper creates piece
### Step 1: Shipper creates piece with grossWeight
**Input**
```http
POST /shipper/piece/ HTTP/1.1
Host: 1r.platform.com 
Authorization: (Bearer Token)
Accept: application/ld+json
Content-Type: application/ld+json

{
**full JSON incl. header to be added**
  "grossWeight":
    {
    "value": 85,
    "unit": "kg"
    }
}
```
**Expected Output**
```http
HTTP/1.1 201 Created
Date: ...
Location: http://1r.platform.com/shipper/piece/8dc
```
### Step 2: Shipper publishes piece towards Forwarder
**Input**
```http
POST /forwarder/**???**
Host: 1r.platform.com 
Authorization: (Bearer Token)
Accept: application/ld+json
Content-Type: application/ld+json

{
  "@context": {
      "@vocab": "http://onerecord.iata.org/"
  },
  "@type": "Notification",
  "eventType": "OBJECT_CREATED",
  "topic": "http://onerecord.iata.org/Piece",
  "logisticsObjectRef": "/shipper/piece/8dc"
}
```
**Expected Output**
```http
204 Notification received successfully
```
**Comment**

Is is unclear, where the post-request should go. It´s looking like the target system is the external system, but I wonder if it should be a post request then....

**Issues**


## UseCase 2: Forwarder subscribes on Shipper´s piece
***tbd***
## UseCase 3: Forwarder creates and links shipment 
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
## UseCase 4: Forwarder performs access delegation of piece to GHA
***tbd***
## UseCase 5: Carrier creates and publishes transport movement
### Step 1: Carrier creates transport movement with Origin and Destination
***tbd***
### Step 2: Carrier publishes transport movement towards GHA and Forwarder
***tbd***
## UseCase 6: GHA creates ULD with uPID and uldTypeCode
***tbd***
## UseCase 7: GHA links ULD with Carrier/transportMovement 
### Step 1: GHA creates link to transportMovement in ULD
***tbd***
### Step 2: GHA sets CR to Carrier/transportMovement to include piece link
***tbd***
### Step 3: Carrier confirms CR to transportMovement to include piece link
***tbd***
## UseCase 8: GHA switches assignment piece to shipment
### Step 1: Forwarder creates "alternative" shipment with totalGrossweight of 14 kg
***tbd***
### Step 2: GHA sets CR to Shipper/piece for changing shipment
***tbd***
### Step 3: GHA sets CR to Forwarder/shipment for containedPieces
***tbd***
## UseCase 9: GHA sets CR to Shipper/piece to correct weight to 45 kg
***tbd***
## UseCase 10: At DEP: Carrier triggers Memento incl. Piece, ULD, Shipment and flight
***tbd***
## UseCases >=11: Check Audit Trail, Multi-Link environment, Security, Authentication, etc.
***tbd***


![UseCases](/EmbeddedIllustrations/UseCases.png)
        
