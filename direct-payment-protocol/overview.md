# Overview

{% embed url="https://www.youtube.com/watch?v=QP1RljoA-ss" %}

DPP is a protocol used for communication between a receiver (usually either a beneficiary such as a merchant, a payment processor, or simply the recipient's wallet) and their customer (the sender of the funds), enabling a better customer experience, simpler wallet infrastructure, more advanced features such as multiple outputs, and better security against man-in-the-middle attacks on the payment process.

DPP is the _key_ ingredient in achieving [SPV ](../resources/simplified-payment-verification.md)and direct payments between two peers: a sender and a receiver. It basically dictates the communication language spoken when sending or receiving payments. Tools such as Paymail aid in the two peers discovering each other, or more accurately, the sender discovering the service endpoint of the receiver. However, once that connection/link is established, the two peers need to be able to speak the same language in order for the receiver to communicate the terms required in order to accept the payment on one end, and for the sender to craft the payment and send it to the receiver on the other end. This is the primary function of the DPP.

### Extendability

DPP is designed to be extremely extensible and integrable. It can be used for any type of payment mode, not just native Satoshis which is the simplest. DPP merely defines the communication protocol between the sender and the receiver of the payment. Whether the payment involves a different type of token such as Run, STAS, sensible token, does not make a difference. The DPP can easily be extended to support different payment modes.

Sub-protocols for the different tokens or payment modes define how each payment mode is to be treated and they are identified by their BRFID which is similar to Paymail capabilities.

In the DPP, the receiver is king. They determine what and how they will accept the payment by specifying everything in the PaymentTerms (previously PaymentRequest in BIP270) message including the payment mode(s) that they will accept.

### Features and Benefits

Along with the above features, DPP has the following benefits:

* Payments are made to human readable URLs such as `example.com/pay/Inv123` rather than a Base58 check encoded bitcoin address.
* Proof of payments can be stored
* Ability to communicate directly with vendor instead of directly with the bitcoin network
* Payments Acknowledgement
* Ability to obtain refund addresses
* Receivers are able to obtain Merkle proofs
