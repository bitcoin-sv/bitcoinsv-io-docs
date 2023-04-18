# Tips (GET)

| Method | Description                                                       |
| ------ | ----------------------------------------------------------------- |
| GET    | Returns the latest headers for the tip of the chain and any forks |

#### Request URI

```bash
/api/v1/chain/tips
```

**Sample Response**

```
[
    {
        "header": {
            "hash": "000000000000000002f1849fee8bd71ccea23a6c817bb4ae35d5dc4726a0c443",
            "version": 551550976,
            "prevBlockHash": "000000000000000008c45de5166aa4ec722a848d2239df2118ebe25d4d4c8f20",
            "merkleRoot": "c815383b92848eae8ca5d9bc64aac9775a35f83534d02613cc025ccfe9797ea5",
            "creationTimestamp": 1644246992,
            "difficultyTarget": 403709977,
            "nonce": 1034164745,
            "transactionCount": 0,
            "work": 292853008887592053244
        },
        "state": "LONGEST_CHAIN",
        "chainWork": 373377589654552669814938670,
        "height": 725658,
        "confirmations": 1
    }
]
```

**Note:** The transactionCount is 0 since this client listens for header hashes only and does not capture the number of transactions in each block. This attribute will be removed in a later version of this application.
