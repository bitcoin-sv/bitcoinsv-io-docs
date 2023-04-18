---
description: Understanding SPV
cover: ../.gitbook/assets/Dark Banner_Additional Resourcesv2.png
coverY: 0
---

# Simplified Payment Verification

Simplified Payment Verification (SPV) is described in the [Bitcoin Whitepaper](https://craigwright.net/bitcoin-white-paper.pdf) as a method for constructing a payment verification process which does not involve running a full node software which miners typically run. SPV enables clients like wallets to verify that a transaction has been included in a block and therefore a payment has been made and accepted by the network.

This is possible because the Merkle tree structure stores the transactions in each block. The structure of a Merkle tree means that we only need to know the Merkle root/top hash and a small branch of the tree to verify if a transaction is part of the tree, i.e. whether it has been included in a Bitcoin block. This is done by taking the nodes that are in the path that connects the Merkle root with one of the bottom transactions and bundling them together to create a proof.

Running a node requires downloading the entire blockchain, but a Bitcoin user can use Merkle proofs and only needs to know the header of each block which includes the Merkle root, in order to verify any transaction in that block. So users only have to store 80 bytes per block, instead of the entire block required for nodes.

The steps for performing SPV Check are as follows:

1. Receive the raw transaction and the Merkle proof
2.  For each of the UTXO being spent, follow the steps below:

    a. Get the previous transaction ID and its block hash

    b. Query the block header with the given block hash

    c. Extract the Merkle root from the block header

    d. Follow the process for constructing the Merkle root from the previous transaction ID of the said UTXO

    e. Validate the constructed Merkle root value to be the same as the Merkle root obtained from the block header. If both values match, the UTXO is valid and can be spent.
3. If all of the input UTXOs are valid, consider the transaction as valid

### **Earlier SPV implementations**

Unfortunately, there have been many [misconceptions](https://blog.lopp.net/could-spv-support-a-billion-bitcoin-users-sizing-up-a-scaling-claim/) around SPV over the years and the ability to scale Bitcoin the way it was originally designed. Earlier implementations of "SPV nodes" or LiteClients heavily relied on bloom filters. They would connect to nodes and basically ask to forward them transactions that they're interested in using bloom filters as a method to obscure the transactions they're interested in for privacy reasons. However, take a step back and ask yourself why do we even need bloom filters in the first place? The answer is pretty much that we do **not**. The entire communication process involved in the earlier implementations is far from ideal but that does not mean that the entire system is flawed or unscalable.

Instead of the sender broadcasting the payment transaction to the Bitcoin network and then forcing the receiver to run a fully indexing node, that downloads every single transaction on the network, or query a node to get specific transactions (through a bloom filter); why doesn't the sender connect directly to the receiver and send them the payment directly? This is the essence of SPV and how Bitcoin can scale indefinitely without affecting the users of the network who want to send and receive payments. Users of the Bitcoin network don't need to care much more than about the transactions they're interested in. Users don't have to download every single transaction ever on the network if they only want to receive one simple payment transaction, "[\[t\]hat would be like every Usenet user runs their own NNTP server.](https://bitcointalk.org/index.php?topic=532.msg6269#msg6269)"

### Check analogy

To give a simple analogy, consider payments using checks. If I wanted to pay you using a bank check, would I post the check to the bank then have you run to the bank and ask around searching for the check? Especially when there are millions of checks being sent around every minute? Doesn't make sense does it?

Instead, I would hand you the check directly. Then it is _your_ responsibility to go and deposit the check and the bank and make sure it clears. It is not my responsibility to cash your check for you and I would be more than happy if you fail to cash it yourself because the money will stay in my account.

Linking back to Bitcoin, if I wanted to pay you, I would first have to find your device which will receive the payment (Paymail can be used to aid with that process). Once I have discovered the service endpoint, my payment device would need to speak the same language as yours (this is where the DPP comes in) in order for your device to dictate the exact terms and requirements of the payment and then for my device to craft that payment properly and send it back to you. Once you receive the payment, it is your job to validate that it fulfils your requirements and then get that settled on the Bitcoin network. Once the payment transaction has been settled, you can easily verify that the transaction has been included in the blockchain (the longest valid chain) with a low amount of data regardless of how big block sizes get - you only need the Merkle Proof of the transaction as well as the chain of block headers (which is only [4.2MB per year](https://bitcoinsv.io/bitcoin.pdf)).

**Note**: See [Bitcoin Wiki - SPV](https://wiki.bitcoinsv.io/index.php/Simplified\_Payment\_Verification) for more details.
