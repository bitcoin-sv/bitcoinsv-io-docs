---
description: Short cut to understanding DPP
cover: ../.gitbook/assets/Dark Banner_DPPv1.0.png
coverY: 0
---

# Quick Start

Payer requests the terms of the payment.

<details>

<summary>GET /api/v1/payment/{id}</summary>

Returns the Payment Terms message

{% code title="200 Response" %}
```json
{
    "network": "bitcoin-sv",
    "ancestryRequired": true,
    "destinations": {
        "outputs": [
            {
                "amount": 200000,
                "script": "76a914a446a65f70b3e4c104bb2221a8058e9281ece09e88ac",
                "description": "payment reference w6zwZzn"
            }
        ]
    },
    "creationTimestamp": "2022-03-16T10:09:47.56942977Z",
    "expirationTimestamp": "2022-03-17T10:09:47.56942977Z",
    "paymentUrl": "http://dpp:8445/api/v1/payment/w6zwZzn",
    "memo": "invoice w6zwZzn",
    "beneficiary": {
        "avatar": "https://thispersondoesnotexist.com/image",
        "name": "Epictetus",
        "email": "epic@nchain.com",
        "address": "1 Athens Avenue",
        "extendedData": {
            "dislikes": "Malfeasance",
            "likes": "Stoicism & placeholder data",
            "paymentReference": "w6zwZzn"
        }
    },
    "fees": {
        "data": {
            "miningFee": {
                "satoshis": 100,
                "bytes": 200
            },
            "relayFee": {
                "satoshis": 100,
                "bytes": 200
            }
        },
        "standard": {
            "miningFee": {
                "satoshis": 100,
                "bytes": 200
            },
            "relayFee": {
                "satoshis": 100,
                "bytes": 200
            }
        }
    }
}
```
{% endcode %}

</details>

Payer builds a transaction using the terms provided, then sends it to the recipient:

<details>

<summary>POST /api/v1/payment/{id}</summary>

Payer sends the transaction along with metadata.

{% code title="Request" %}
```json
{
    "modeId": "outputs",
    "originator": {
        "avatar": "https://thispersondoesnotexist.com/image",
        "name": "Epictetus",
        "email": "epic@nchain.com",
        "address": "1AthensAvenue",
        "paymentReference": "w6zwZzn",
        "extendedData": {
            "dislikes": "Malfeasance",
            "likes": "Stoicism\u0026placeholderdata",
            "paymentReference": "w6zwZzn"
        }
    },
    "rawtx": "01000000016ab6f76c774e44bf7b11360765183d6300972ff3e72df042095128e09256d346010000006b483045022100d24204e59085dd626b7e28bf962a1cfacb2c1fb593626b5fb44b9792f998539b0220717884542de3007f7f383f6f4b6879f1c58f572cc68357038f7c8e12574927394121030867e6b42629e4106313fb35f5a78553bae19f3b2e71e3a582af3cac847689c0ffffffff02d0070000000000001976a91470d94b9935ff44c3ff6f7e7aede2da09deee4d0388ac8240d705000000001976a914c8d8833a4dbf02f56fb9dc5095d8a767fc39632588ac00000000",
    "ancestry": "0101fd75010200000002781d445f49c2dcd04736832ad49226d4ca55f00ede8121dca99f43997650351e010000006b483045022100ad794772e5ec12fd771677da56aac3fd693d37420b61392210767e17ee2d546a02207da9e8b09156c6a4e808a4153695f9ff977817e4a2dc1c5fec50ce09b817d3d4412102294dd14d4bf8dc9d3f16a86deb2b2c9cfcaf90354740b960fbdf3121991c2a15feffffffc93fb7cfcab5b63b92a59cccfd9944bbe8db54d439c491136a9203017b773984010000006a473044022018d04c0e4f606684c0136472d90d6686d39d8946845054c1caa00bd5dd170d3e02200f5dfaecdd336b7918ab9344cd529b94660c1c5c9812486d19afc7dcd5cc401f412103608aeafb5530cb3ae318ff0fe0a96e53a1bb947fea11e6cbe7d96616d7b3ca7bfeffffff0200e1f505000000001976a9142f304dc35b27d6c59095891695b4b90afc78f43f88acc348d705000000001976a9143aa2e0c1c913bd2cb974c4c0e349c7e86545b40b88ac83000000026400016ab6f76c774e44bf7b11360765183d6300972ff3e72df042095128e09256d34689f7ae65ea7411357006fa091d97a24b3478ed6e055ca3fe25d8a157288ea0530100a1796e5eb481b047cbb8246814592ab41835c2d7c664245949c6d8431133d7d9"
}
```
{% endcode %}

Payment ACK message response on successful payment.

{% code title="201 Response" %}
```json

{

    "id": "w6zwZzn",
    "tx_id": "6c7795c8cf240306df16e33fa1958cd86f12cb452af2bad495e18b0d39567453",
    "memo": "",
    "peer_channel": {
        "host": "https://infra.bitcoinsv.io",
        "path": "peerchannels",
        "channel_id": "GSHdYivo7Rz1gpZrG8GO4FEJLG1qGbiBFRU1lpWAGXyTXsr3nAdETfM_1D4N5gvcTrverjUsdNH7FNa5ai93Cw",
        "token": "68BBko6NqokD3ZC35IfE1no62diXYqkXX0jbwUvloIkRNNUSjE4d1Jn2QuS7DrloOXgrHxHxqQr10sjUF2KE7w"
    }
}
```
{% endcode %}

</details>

