# Process

The diagram below illustrates the high level communication flow between the sender and receiver  of a direct payment over Bitcoin. This payment process works independently of the blockchain size. Bitcoin blocks can continue to grow with no limits without affecting the functionality of direct payments from one peer to another on the Bitcoin network.

<figure><img src="../.gitbook/assets/Direct Payment Protocol (DPP)v1.0.png" alt=""><figcaption><p>Direct Payment Protocol (DPP)</p></figcaption></figure>

The DPP Process consists of the following steps:

1. The sender sends the Purchase Order to the receiver.
2. The receiver provides the corresponding Invoice to the sender.
3. The sender prepares a payment for the provided Invoice.
4. The sender sends the Payment (in BSV) directly to the receiver.
5. The receiver validates the payment and once satisfied, broadcasts the payment to the network of nodes.
6. Miners add the payment to the next block and send confirmation back to the receiver.
7. The receiver provides a payment acknowledgement to the sender.
8. The node sends the Merkle proof to the sender.

For details on **Step 4: Payment Validation** shown in the above diagram, see [Simplified Payment Verification (SPV)](../resources/simplified-payment-verification.md).
