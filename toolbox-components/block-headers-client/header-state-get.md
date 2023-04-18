# Header State (GET)

| Method | Description                                            |
| ------ | ------------------------------------------------------ |
| GET    | Returns the header with the given hash and their state |

#### Request URI

```bash
/v1/chain/header/state/{hash}
```

#### Parameters Used

| ID   | TYPE   | Description     |
| ---- | ------ | --------------- |
| hash | String | The Header hash |

**Sample Response**

```
{
    "header": {
        "hash": "00000000dfd5d65c9d8561b4b8f60a63018fe3933ecb131fb37f905f87da951a",
        "version": 1,
        "prevBlockHash": "00000000a1496d802a4a4074590ec34074b76a8ea6b81c1c9ad4192d3c2ea226",
        "merkleRoot": "10f072e631081ad6bcddeabb90bc34d787fe7d7116fe0298ff26c50c5e21bfea",
        "creationTimestamp": 1233046715,
        "difficultyTarget": 486604799,
        "nonce": 2999858432,
        "transactionCount": 0,
        "work": 4295032833
    },
    "state": "STALE",
    "chainWork": 65537,
    "height": 2000,
    "confirmations": 4000
}
```

**Note:** The transactionCount is 0 since this client listens for header hashes only and does not capture the number of transactions in each block. This attribute will be removed in a later version of this application.

#### Parameters Returned

| ID    | Type | Values                          |
| ----- | ---- | ------------------------------- |
| state | ENUM | LONGEST\_CHAIN, ORPHAN OR STALE |
