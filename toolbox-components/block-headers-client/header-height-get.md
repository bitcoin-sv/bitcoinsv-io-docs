# Header Height (GET)

| Method | Description                              |
| ------ | ---------------------------------------- |
| GET    | Returns the header with the given height |

#### Request URI

```bash
/api/v1/chain/header/byHeight?height=<start_height>&count=<count>
```

**Note:** The http request **MUST** allow either `application/octet-stream` or `application/json` response bodies, preferably by setting an explicit "Accept" http request header. Octet-stream will return the block headers as a stream of raw bytes.

#### Parameters Used

| ID     | TYPE    | Description                                               | Required/Optional |
| ------ | ------- | --------------------------------------------------------- | ----------------- |
| height | Integer | A start height must be entered or it will return an error | Required          |
| count  | Integer | The count of headers (Default =1).                        | Optional          |

**Sample Response**

```
[
    {
        "hash": "00000000a160d1befcf1f03769e6ceaaaefee43c11778d45984f3f22c7ab4913",
        "version": 1,
        "prevBlockHash": "00000000d4dd87be8e30d9ca2b9b5a5be61d3b5347d25eb05019d39657f07944",
        "merkleRoot": "177b2d11d42e44c16cc31a32a0b0d96d2527fac42cbc15bb137f69f945c79a4a",
        "creationTimestamp": 1248330740,
        "difficultyTarget": 486604799,
        "nonce": 3507266603,
        "transactionCount": 0,
        "work": 4295032833
    }
]
```

**Note:** The transactionCount is 0 since this client listens for header hashes only and does not capture the number of transactions in each block. This attribute will be removed in a later version of this application.
