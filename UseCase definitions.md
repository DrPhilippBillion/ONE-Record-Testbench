# UseCase Overview

This document is at early stage, please correct and comment extensively!

(for a UseCase overview, please refer to the image at the end of the document)

# Setting

This testbench 

# Assumptions

# Requirements


## UseCase 1: Shipper creates piece
### Step 1: Shipper creates piece with grossWeight
#### POST-Request
```http
POST /shipper/piece HTTP/1.1
Host: 1r.example.com
Content-Type: application/ld+json
Accept: application/ld+json
{
	"@context": {
		"cargo": "https://onerecord.iata.org/",
		"api": "https://onerecord.iata.org/api/"
	},
	"@type": "cargo:Piece",
	"cargo:Piece#grossWeight": {
		"@type": [
			"cargo:Value"],
		"cargo:Value#unit": "kg",
		"cargo:Value#value": 85
	}	
}
```

#### Expected Answer

```http
201 Created
Location: https://1r.example.com/shipper/Piece_8dc
Content-Type: application/ld+json
LO-type: https://onerecord.iata.org/Piece
```

#### Comments
- none -

### Step 2: Shipper publishes piece towards Forwarder
#### POST-Request
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
#### Expected Answer
```http
204 Notification received successfully
```
#### Comments

Is is unclear, where the post-request should go. It´s looking like the target system is the external system, but I wonder if it should be a post request then....

#### Issues


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
        
