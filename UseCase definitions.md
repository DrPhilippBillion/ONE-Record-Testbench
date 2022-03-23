# UseCase Overview

![UseCases](/EmbeddedIllustrations/UseCases.png)

## UseCase 1: Shipper creates piece
### Step 1: Shipper creates piece with grossWeight
### Step 2: Shipper publishes piece towards Forwarder
## UseCase 2: Forwarder subscribes on Shipper´s piece
## UseCase 3: Forwarder creates and links shipment 
### Step 1: Forwarder creates shipment with totalGrossWeight
### Step 2: Forwarder links piece in shipment
### Step 3: Forwarder sets CR to Shipper/piece to set backlink for shipment
## UseCase 4: Forwarder performs access delegation of piece to GHA
## UseCase 5: Carrier creates and publishes transport movement
### Step 1: Carrier creates transport movement with Origin and Destination
### Step 2: Carrier publishes transport movement towards GHA and Forwarder
## UseCase 6: GHA creates ULD with uPID and uldTypeCode
## UseCase 7: GHA links ULD with Carrier/transportMovement 
### Step 1: GHA creates link to transportMovement in ULD
### Step 2: GHA sets CR to Carrier/transportMovement to include piece link
### Step 3: Carrier confirms CR to transportMovement to include piece link
## UseCase 8: GHA switches assignment piece to shipment
### Step 1: Forwarder creates "alternative" shipment with totalGrossweight of 14 kg
### Step 2: GHA sets CR to Shipper/piece for changing shipment
### Step 3: GHA sets CR to Forwarder/shipment for containedPieces
## UseCase 9: GHA sets CR to Shipper/piece to correct weight to 45 kg
## UseCase 10: At DEP: Carrier triggers Memento incl. Piece, ULD, Shipment and flight
