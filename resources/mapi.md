---
description: Broadcasting Transactions via Block Producing Miners
cover: ../.gitbook/assets/Dark Banner_Documentationv1.0.png
coverY: 0
---

# Mapi

{% embed url="https://www.youtube.com/watch?v=RbB7wNsERck" %}

When you want to get your transactions settled, you need to send it to the nodes on the network who will mine it into the Bitcoin blockchain. You can connect through the Bitcoin P2P (peer to peer) network the way nodes usually connect to each other, however it would be nice to have an easy and reliable direct interface to the nodes. Luckily, we have that; it's called Mapi - pronounced "mAp-ee".

Through Mapi, miners can offer additional security services to users such as instant double-spend notifications to notify them if there has been a double-spend attempt on the network. Also, if you supply a callback URL to Mapi, the miner will send you the Merkle Proof(s) when your submitted transaction gets included in a block on the blockchain (by any miner - not necessarily just that specific miner). That way you can have guarantees that your transaction(s) has been settled on the blockchain. By linking up with the [Block Headers Client](../toolbox-components/block-headers-client/), you can check that your transaction has been included in the longest chain with a minimal amount of data (block headers along with the Merkle Proof of the transaction). In terms of the callback URL, if you don't have a server set up to be always online waiting to receive the callbacks from Mapi, you can use [Peer Channels](../toolbox-components/peer-channels/) which will catch the callback for you.

Along with the chain of block headers, users can also download the [coinbase transaction](https://wiki.bitcoinsv.io/index.php/Coinbase) along with it's Merkle Proof (proving inclusion). In their coinbase transactions, miners can put additional information they would like to advertise, such as their Mapi endpoint for example. Building on that, miners can include a public key in the coinbase transaction, called their [Miner ID](https://github.com/bitcoin-sv-specs/brfc-minerid). This Miner ID can be used as the basis of an objectively derived PKI (Public Key Infrastructure) system. Miners can use that public/private keypair (or keys derived from that keypair) to sign data that is backed by the reputation they've built using the proof of work expended to mine blocks.
