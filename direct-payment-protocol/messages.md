---
cover: ../.gitbook/assets/Dark Banner_DPPv1.0.png
coverY: 0
---

# Messages

DPP consists of three messages exchanged between the sender and the receiver:

* Payment Terms
* Payment
* Payment ACK (acknowledgement)

**Prerequisite:** An invoice should be created. The schema for the invoice is kept open as it is left to the receiver's discretion.

## Payment Terms

Payment terms (GET) provide the sender the required information for making the payment to the receiver (payment host).

**Request**

Submit the following payment GET request.

**Request URI**

```bash
/api/v1/payment/{payment_id}
```

**Status**

200 OK

**Response**

Returns the information for the sender to make the payment to the receiver.

**Response Body**

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

The returned payment information contains required details such as:

* url (payment url)
* a list of **outputs** (see below)
* expiry date
* other meta data that may be used by the receiver to identify the PaymentRequest.

The payment url specified in the PaymentRequest will remain valid at least until the PaymentRequest expirationTimestamp (or as long as possible if the PaymentRequest does not expire).

**Note:** This is irrespective of any state change in the underlying payment request; for example, cancellation of an order should not invalidate the paymentUrl, as it is important that the receiver's server can record mis-payments in order to refund the payment.

When a Bitcoin wallet application receives a payment request, it must authorize the payment by doing the following:

* Validate the receiver's identity and signature using the PKI system. The PKI system is assumed to occur in a wrapper layer (such as HTTPS) and if no PKI system is available, then the wallet can choose whether or not to display the payment request. The PKI system is usually HTTPS, and if the payment request is retrieved over HTTPS then the domain name should be displayed as the receiver.
* Verify that the sender's system unix time (UTC) is before PaymentRequest.expirationTimestamp. If it is not, the payment request must be rejected.
* Display the receiver's identity and check if the sender would like to submit a payment (for example, display the "Common Name" in the first X.509 certificate).

**Outputs**

Outputs are used in PaymentRequest messages to specify where a payment (or part of a payment) should be sent. They are also used in Payment messages to specify where a refund should be sent.

## Payment

A payment (POST) is based on a payment id (the identifier for a payment above) and is sent to the receiver when the sender authorises a payment:

1. Creates and signs a transaction that satisfies (pay in full) PaymentRequest.outputs. It may add additional outputs if needed.
2. Verifies that the sender's system unix time (UTC) is before PaymentDetails.expirationTimestamp. If it is not, the payment should be cancelled.
3. Posts a Payment message to the paymentUrl URL. The Payment message is serialized and sent as the body of the POST request.
4. May optionally broadcast the transaction themselves to the Bitcoin network, but they are not required to do so as the receiver is expected to broadcast the transaction. If the receiver did not broadcast the transaction, the client can recover their funds within two months.

#### Request

```json
{
    "modeId": "outputs",
    "memo": "invoicew6zwZzn",
    "beneficiary": {
        "avatar": "https://thispersondoesnotexist.com/image",
        "name": "Epictetus",
        "email": "epic@nchain.com",
        "address": "1AthensAvenue",
        "paymentReference": "",
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

**Status**

201 OK

**Sample Response**

```
{

    "id": "w6zwZzn",
    "tx_id": "6c7795c8cf240306df16e33fa1958cd86f12cb452af2bad495e18b0d39567453",
    "memo": "",
    "peer_channel": {
        "host": "peerchannels:25009",
        "path": "",
        "channel_id": "GSHdYivo7Rz1gpZrG8GO4FEJLG1qGbiBFRU1lpWAGXyTXsr3nAdETfM_1D4N5gvcTrverjUsdNH7FNa5ai93Cw",
        "token": "68BBko6NqokD3ZC35IfE1no62diXYqkXX0jbwUvloIkRNNUSjE4d1Jn2QuS7DrloOXgrHxHxqQr10sjUF2KE7w"
    }
}
```

Errors communicating with the paymentUrl server should be communicated to the user. When the receiver's server receives multiple identical Payment messages for an individual PaymentRequest, it must acknowledge each. The second and further PaymentACK messages sent from the receiver's server may vary by memo field to indicate the current state of the payment (for example, the number of confirmations seen on the network). This is required in order to ensure that in case of a transport level failure during transmission, recovery is made possible by the Bitcoin client re-sending the payment message.

PaymentDetails.paymentUrl should be made secure against man-in-the-middle attacks that might alter Payment.refundTo because the wallet and receiver are using HTTPS or some other secure transit layer.

Wallet software sending payment messages via HTTPS must set appropriate Content-Type and Accept headers, as specified in BIP 271:

```
    Content-Type: application/bitcoinsv-payment  
    Accept: application/bitcoinsv-paymentack
```

When the receiver's server receives the payment message, it must determine whether the transaction satisfies payment conditions. If and only if they do, should it broadcast the transaction to the Bitcoin Peer network.

Payment messages larger than 10 MB should be rejected by the receiver's server, to mitigate denial-of-service attacks.

## Payment ACK

Payment Acknowledgement (Payment ACK) is the final message in the payment protocol. It is sent from the receiver's server to the bitcoin wallet in response to a payment message.

**Note:** LCT will adopt the following payment method once it is approved: \
[https://tsc.bitcoinassociation.net/standards/direct-payment-protocol/](https://tsc.bitcoinassociation.net/standards/direct-payment-protocol/)

**Note:** This is a reference implementation and is provided as an open source code for application developers to either use it as is, or to customise it to suit their requirements.
