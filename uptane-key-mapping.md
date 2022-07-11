we need to create public and private keys with ed25519

eg: public key
```
{
   "keyid_hash_algorithms":[
      "sha256",
      "sha512"
   ],
   "keyval":{
      "public":"fdba7eaa358fa5a8113a789f60c4a6ce29c4478d8d8eff3e27d1d77416696ab2"
   },
   "keytype":"ed25519"
}
```

eg: private key

```
7f8fcae94b95a04baa1f07aa374937f9@@@@100000@@@@48d5bd2502df5d921e714f0782f42e1df1ae08ffb4d0dbf5df1923557096b005@@@@a724429acda285bfb3e7f2b4db0b0928@@@@f6056beade9cd29b105c8f82186b24f5e9257a11420ddf2e4056a6ce7907ffb334dc130c6f12268f0229a5b34364d6375cd398de2b3aea93f6fd55355a725b8352caea89a625bf020e7a2a165aec5f95794f73265779056f9e3a227469cf89f484ff4067e4fa50241492810c89a5a9e83945af92a41a37096068f7fd01976ec209c3156bfc9308c5e95a1381859bff06a62df2ad2aabb9f417e2588941b9239d6a6d130572de94813cfd089eefbe73fe59e297f047dac67e8dfa437360bbc7ec953a507ca69fc48a21dadb6124643f6bf3e813d603a5609391cf5c1b5b30321a3024f3a8f97f325e96be102d0dceba28bf1f5a4c7614c0e0ffd2b6819758e0d02ad1df6adb16e23282dc502a
```

Each and very role will have the private and public pair

Each JSON file will have 2 segments `signatures` and `signed`

### ROOT.JSON

#### signatures

```
{
   "signatures":[
      {
         "keyid":"fdba7eaa358fa5a8113a789f60c4a6ce29c4478d8d8eff3e27d1d77416696ab2",
         "method":"ed25519",
         "sig":"dcbe4f76589cab2182dea476128ea7443a8116ede6ae044b80136ee3a0dbaa9bf0ed83f5d67b2e40fafbaac4ba9e513804f8cce4b842745ff0a58dc5f327b205"
      }
   ],
}
```

- Root Public key keyval ->> signatures `keyid`
- Root Private will generate signature ->> signatures `sig`

#### signed

- `_type` defines the actual role file. here is root
- `keys` - 4 types of keys for 4 roles along with hash algorithms, key type and key val -> pub of each roles
- signature->keyid === signed->keys->[] === signed->roles->root->keyid  ----->>> how this key is generated ---->>> A hexadecimal value in '23432df87ab..' format `SCHEMA.RegularExpression(r'[a-fA-F0-9]+')`
- threshold - no. of keyids pair are there for verification
- version - no of time the `root.json` file has changed
- `root.json` file change only if keys or signature is changed

```
{
   "signed":{
      "_type":"Root",
      "compression_algorithms":[
         "gz"
      ],
      "consistent_snapshot":false,
      "expires":"2023-07-07T12:19:10Z",
      "keys":{
         "630cf584f392430b2119a4395e39624e86f5e5c5374507a789be5cf35bf090d6":{
            "keyid_hash_algorithms":[
               "sha256",
               "sha512"
            ],
            "keytype":"ed25519",
            "keyval":{
               "public":"99ef8790687ca252c4677a80a34e401efb7e17ccdf9b0fcb5f1bc3260c432cb9"
            }
         },
         "da9c65c96c5c4072f6984f7aa81216d776aca6664d49cb4dfafbc7119320d9cc":{
            "keyid_hash_algorithms":[
               "sha256",
               "sha512"
            ],
            "keytype":"ed25519",
            "keyval":{
               "public":"d1ab5126fd6f0e30944910e81c0448044dfe9d5a39f478212b2afa913bb7ca7c"
            }
         },
         "f93cfcf33d335ff43654ec6047e0a18dd5595ee3de53136b94c9c756788a0f97":{
            "keyid_hash_algorithms":[
               "sha256",
               "sha512"
            ],
            "keytype":"ed25519",
            "keyval":{
               "public":"228342cc8b78a65b8840ef5691a693d8c368e053a7e8e8f85faf7c83eff1e1d2"
            }
         },
         "fdba7eaa358fa5a8113a789f60c4a6ce29c4478d8d8eff3e27d1d77416696ab2":{
            "keyid_hash_algorithms":[
               "sha256",
               "sha512"
            ],
            "keytype":"ed25519",
            "keyval":{
               "public":"f3b4c231520580eca92e17ae1581a708f606f72d43cc200af493afeec22a5e79"
            }
         }
      },
      "roles":{
         "root":{
            "keyids":[
               "fdba7eaa358fa5a8113a789f60c4a6ce29c4478d8d8eff3e27d1d77416696ab2"
            ],
            "threshold":1
         },
         "snapshot":{
            "keyids":[
               "f93cfcf33d335ff43654ec6047e0a18dd5595ee3de53136b94c9c756788a0f97"
            ],
            "threshold":1
         },
         "targets":{
            "keyids":[
               "630cf584f392430b2119a4395e39624e86f5e5c5374507a789be5cf35bf090d6"
            ],
            "threshold":1
         },
         "timestamp":{
            "keyids":[
               "da9c65c96c5c4072f6984f7aa81216d776aca6664d49cb4dfafbc7119320d9cc"
            ],
            "threshold":1
         }
      },
      "version":1
   }
}
```
