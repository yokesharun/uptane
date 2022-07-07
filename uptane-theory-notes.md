## Uptane - secure the OTA

### ECU Maintaining 2 repos
- Supplier Repo (Offline)
    - Offline Key - Provides signed meta data for all vehicle ECU's
- Director Repo (Online)
    - Online Key - Provides signed meta data for vehicle / ECU's categories - act as mapping for supplier repo to pick the correct update for the   vehicle/ECU.

### Attack Preventions
- The ECU will check the updates provided by the Director Repo should match with the supplier Repo.

### Design Principles
- Seperation of duties - sign different type of keys for each meta data
- Threshold Signatures - minimum threshold no. of keys is required to sign the meta data
- Explicit and Implicit revocation of keys - 
    - Explicit - keys can be publish explicity to replace old one with new one
    - Implicit - same like cookie we can set some expire on that key, expiry details will be listed on meta data
- Minimising the risk by using Offline Keys

### Types of ECU's - categorised based on performance
- Primary ECU - which downloads, verifies and distributes (meta data + Images) to secondary ECUs
- Secondary ECU - Verifies meta data + Image distributed by primary ECU before updating

### Types of Verification
- Full Verification 
    - compares meta data and keys between Director Repo and Supplier Repo (mostly performed by primary ECU)
- Partial Verification 
    - checks meta data only in director repo (checks only one signature of one meta data)

### Verifying meta data on ECUs
Bootloader Perform the Full/Partial Verification during reboot before updating the image.

### Setup need to use Uptane

1. OEM setup and maintain supplier repo and director repo
2. Supplier should either sign images for supplier repo or ask OEM to sign them instead
3. ECU's will do Partial or Full Verification

### Things to know beforehand
1. For Full Verification ECU's should use additional storage to perform rollback on emergency - should be used safety critical ECUs that should not be hacked.
2. ECU's having constain with speed or memory can perform partical verification.
3. If the ECU's cannot perform atleast partial verification then dont do over the air updates

### Reference: 
- https://uptane.github.io/
- https://youtu.be/nDghHNxRGHA
- https://github.com/uptane/obsolete-reference-implementation

