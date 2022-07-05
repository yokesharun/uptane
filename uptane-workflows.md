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

#### Primary ECU Intractions

Intraction between Director Repo and Primary ECU on Refresh or on Check for new updates

```mermaid
sequenceDiagram
    Director_Repo->>Primary_ECU: Downloads timestamp json
    Primary_ECU-->>Primary_ECU: Checks expiry date(max 1 day)
    Director_Repo->>Primary_ECU: Downloads snapshot json
    Primary_ECU-->>Primary_ECU: Verifies timestamp's sha256, length, version matches with snapshot file
    Director_Repo->>Primary_ECU: Downloads targets json
    Primary_ECU-->>Primary_ECU: Verifies version matches with targets file
    Primary_ECU-->>Local_storage: Checks for any new updates with specific to ECU - compares with old local data
```
If no update it will discard, or if there is any new update it will fetch from image repo

```mermaid
sequenceDiagram
    Image_Repo->>Primary_ECU: Downloads timestamp json
    Primary_ECU-->>Primary_ECU: Checks expiry date(max 1 day)
    Image_Repo->>Primary_ECU: Downloads snapshot json
    Primary_ECU-->>Primary_ECU: Verifies timestamp's sha256, length, version matches with snapshot file
    Image_Repo->>Primary_ECU: Downloads targets json
    Primary_ECU-->>Primary_ECU: Verifies version matches with targets file
    Primary_ECU-->>Primary_ECU: Filter images by comparing images repo target file with director repo target file
    Image_Repo->>Primary_ECU: If everything is good, it will download the image file
    Primary_ECU-->>Primary_ECU: Verify image file data with target data - sha256, sha512, length
    Primary_ECU-->>Local_storage: Stores the date in local storage
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
