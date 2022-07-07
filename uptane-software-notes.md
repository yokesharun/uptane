## Random points:

- Root, timestamp, snapshot, targets each one of them are having one private and public key of key type ed25519
- To create the keys and repository TUF is having some repository tools
- Each module will store the hash keys of other files and the length
- Primary ECU will download the target file by first updating the latest copy of the metadata. such as `timestamp.json`, `snapshot.json`, `target.json`, root.json verify that their length and hashes are valid and then it will be saved locally to complete the update process.
- Everytime when we update the `target.json` with new image, the version will be changed. since the `snapshot.json` and `timestamp.json` are the dependent files, then those values of versions and hashes also will be changed
- `Target.json` will have 2 hashes sha256, sha512 of the same image file along with the length. Once the images is fully downloaded in the device it will verify the hashes and the version. If the verification fails it will rollback the process and discard the updation.
- `Timestamp.json` will have sha256 hash, length and version of `snapshot.json` file
- `Snapshot.json` will have sha256 hashes, lengths and versions of `target.json` and `root.json` files
- Before the partial verification by secondary ECU, the downloaded image will be maintained in the unverified_targets folder in secondary ECU storage and also primary ECU will keep the downloaded images in target folder
