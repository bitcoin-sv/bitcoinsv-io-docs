# Header (GET)

| Method | Description                            |
| ------ | -------------------------------------- |
| GET    | Returns the header with the given hash |

#### Request URI

```bash
/api/v1/chain/header/{hash}
```

The content type: `application/octet-stream` can be provided in the request header to return the header's raw bytes.

**Sample Response**

```
{
    "hash": "00000000000000000d0e0e8a507c059619c0cd84030406dcfcbe9a527c7d2333",
    "version": 536870912,
    "prevBlockHash": "00000000000000000502f254bb5502f8a7f2d49aaecc6441bc3271de00033ece",
    "merkleRoot": "59aa66a7c8b5b4e3f899fd624686129e35f2526936ad0af2b6017d17a345c41f",
    "creationTimestamp": 1644239852,
    "difficultyTarget": 403716217,
    "nonce": 1140181913,
    "transactionCount": 0,
    "work": 291133962747482974399
}
```

**Note:** The transactionCount is 0 since this client listens for header hashes only and does not capture the number of transactions in each block. This attribute will be removed in a later version of this application.
