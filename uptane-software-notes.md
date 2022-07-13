## Random points:

- Root, timestamp, snapshot, targets each one of them are having one private and public key of key type ed25519
- To create the keys and repository TUF is having some repository tools
- Each module will store the hash keys of other files and the length
- Primary ECU will download the target file by first updating the latest copy of the metadata. such as `timestamp.json`, `snapshot.json`, `target.json`, `root.json` verify that their length and hashes are valid and then it will be saved locally to complete the update process.
- Everytime when we update the `target.json` with new image, the version will be changed. since the `snapshot.json` and `timestamp.json` are the dependent files, then those values of versions and hashes also will be changed
- `Target.json` will have 2 hashes sha256, sha512 of the same image file along with the length. Once the images is fully downloaded in the device it will verify the hashes and the version. If the verification fails it will rollback the process and discard the updation.
- `Timestamp.json` will have sha256 hash, length and version of `snapshot.json` file
- `Snapshot.json` will have sha256 hashes, lengths and versions of `target.json` and `root.json` files
- Before the partial verification by secondary ECU, the downloaded image will be maintained in the unverified_targets folder in secondary ECU storage and also primary ECU will keep the downloaded images in target folder
- Both Primary and Secondary ECU's Local storages will maintain 2 folders current and past. By default current folder will have `root.json` while ECU's are being created.
- During the update of primary and secondary ECU's, it will download the OTA Director repo and image repo metadata files such as `timestamp.json`, `snapshot.json`, `target.json` and during verification failure time it will download the `root.json` file.
- It will compare the files with current folder and if there is any version, hashes, targets changed in the data. It will move the current folder files to past folder and save the downloaded files in the current folder.
- Until unless if the file is not changed it will be in the current folder like `root.json`.
- This above process will happen in both primary and secondary ECU's local storage.
- Targets will be downloaded in the targets folder of both ECU's
- Primary ECU will verify the metedata from the OTA's director repo and OTA image repo
- Secondary ECU will verify the metadata from the unverified folder files and unverified target files will be stored in unverified target folder of secondary ECU. once it got verifies it will be moved out of the folder.
- While creating primary and secondary local storage - root.json will be added in current folder from OT director and image repo.
- snapshot, target, timestamp keyids will be stored along with key threshold in `root.json`. threshold will be stored because we can have multiple signatures for verification


Pinned json in ECU's storage
```
{
   "delegations":[
      {
         "paths":[
            "*"
         ],
         "repositories":[
            "director",
            "imagerepo"
         ]
      }
   ],
   "repositories":{
      "director":{
         "mirrors":[
            "http://localhost:30401/democar"
         ]
      },
      "imagerepo":{
         "mirrors":[
            "http://localhost:30301"
         ]
      }
   }
} 
```
