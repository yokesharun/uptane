## Uptane Workflow

#### Uptane Repositories

```mermaid
graph TD
    A[Uptane Repositories] --> B[The Image]
    A -->C[The Director]
```

#### Available uptane roles

```mermaid
graph TD
    A[Available Roles] --> B[Root]
    A -->C[Target]
    A -->D[Snapshot]
    A -->E[Timestamp]
```

#### Vehicle ECU's

```mermaid
graph TD
    A[Vehicle ECU's] --> B[Primary ECU]
    A -->C[Secondary ECU]
```

#### Verification of json files

```mermaid
graph TD
    A[root.json] --> C[snapshot.json]
    B[target.json] --> C[snapshot.json]
    C -->D[timestamp.json]
```

#### Primary ECU Intractions

### Intraction between Director Repo and Primary ECU on Refresh or on Check for new updates

```mermaid
sequenceDiagram
    OTA_Director_Repo->>Primary_ECU: Downloads timestamp json
    Primary_ECU-->>Primary_ECU: Checks expiry date(max 1 day)
    OTA_Director_Repo->>Primary_ECU: Downloads snapshot json
    Primary_ECU-->>Primary_ECU: Verifies timestamp's sha256, length, version matches with snapshot file
    Primary_ECU-->>Primary_ECU: Compares and verifies local root json with downloaded snapshot data 
    Primary_ECU-->>Primary_ECU: (if its not matches it will download root json and start the workflow from first)
    OTA_Director_Repo->>Primary_ECU: Downloads targets json
    Primary_ECU-->>Primary_ECU: Verifies version matches with targets file
    Primary_ECU-->>Local_storage: Checks for any new updates with specific to ECU - compares with old local data
```
### If no update it will discard, or if there is any new update it will fetch from image repo

```mermaid
sequenceDiagram
    OTA_Image_Repo->>Primary_ECU: Downloads timestamp json
    Primary_ECU-->>Primary_ECU: Checks expiry date(max 1 day)
    OTA_Image_Repo->>Primary_ECU: Downloads snapshot json
    Primary_ECU-->>Primary_ECU: Verifies timestamp's sha256, length, version matches with snapshot file
    OTA_Image_Repo->>Primary_ECU: Downloads targets json
    Primary_ECU-->>Primary_ECU: Verifies version matches with targets file
    Primary_ECU-->>Primary_ECU: Filter images by comparing images repo target file with director repo target file
    OTA_Image_Repo->>Primary_ECU: If everything is good, it will download the image file
    Primary_ECU-->>Primary_ECU: Verify image file data with target data - sha256, sha512, length
    Primary_ECU-->>Local_storage: Stores the metadata and software images in local storage
```

### Secondary ECU intraction with local storage and device:

```mermaid
sequenceDiagram
    Local_Storage->>Secondary_ECU: Downloads director repo timestamp.json
    Secondary_ECU-->>Secondary_ECU: does partial verification
    Local_Storage->>Secondary_ECU: Downloads director repo snapshot.json
    Secondary_ECU-->>Secondary_ECU: Verifies timestamp's sha256, length, version matches with snapshot file
    Local_Storage->>Secondary_ECU: Downloads director repo targets.json
    Secondary_ECU-->>Secondary_ECU: Verifies version matches with targets file
    Local_Storage-->>Secondary_ECU: Downloads image repo timestamp.json
    Secondary_ECU-->>Secondary_ECU: does partial verification
    Local_Storage->>Secondary_ECU: Downloads image repo snapshot.json
    Secondary_ECU-->>Secondary_ECU: Verifies timestamp's sha256, length, version matches with snapshot file
    Local_Storage->>Secondary_ECU: Downloads image repo targets.json
    Secondary_ECU-->>Secondary_ECU: Verifies version matches with targets file
    Secondary_ECU-->>Device: install the software to device
```

### workflow of fallback or failure senario:

```mermaid
sequenceDiagram
    OTA_Director_Repo->>Primary_ECU: Downloads director repo timestamp.json
    Primary_ECU-->>Primary_ECU: verification FAILED
    OTA_Director_Repo->>Primary_ECU: Downloads root.json to check in case keys have changed for other metadata roles
    Primary_ECU->>Local_Storage: updates the root.json in current folder and backup in past folder
    OTA_Director_Repo->>Primary_ECU: Again it downloads director repo timestamp.json
    Primary_ECU-->>Primary_ECU: does verification and it got FAILED AGAIN
    Primary_ECU-->>Primary_ECU: Primary ECU will discard with BadSignatureError FAILS TO UPDATE AND ROLLBACK
```

#### Overall workflow

```mermaid
graph TD
    G[OTA Portal] --> |Uploading new image|C
    C --> |Attach image to director repo target with VIN no, img path & ECU|A
    A[Director Repo] -->|Fetching new metadata on refresh for verification| B
    C[Image Repo] -->|Fetching new images if director repo target has| B[Primary ECU]
    B --> |Download metadata & images -after verification-| D[Temp local vehicle storage]
    D --> |fetch's new metadata on refresh and verify metadata again before installation| E[Secondary ECU]
    E --> |Attaching secondary to primary ECU|B
    E --> |Partial verification|D 
    E --> |Installing new image|F[Vehicle]
```
