---
description: BHC
---

# Block Headers Client

The Block Headers Client (BHC) forms a layer on top of the Bitcoin Node Network and provides blockchain verification information for applications integrating with the Bitcoin Blockchain for either payments or data transactions. It is an essential component for enabling SPV verification, as defined in the [Bitcoin Whitepaper](https://craigwright.net/bitcoin-white-paper.pdf).

<figure><img src="../.gitbook/assets/Bitcoin Networkv1.0.png" alt=""><figcaption><p>Bitcoin Network</p></figcaption></figure>

BHCs usually track the longest chain and store the block header information for all the blocks present in that longest chain. A typical block header is about 80 bytes and a full blockchain block header chain currently takes up around 56 MB in size.

### Objectivity and Simplicity

Any device running a BHC can easily verify the correctness of the block header by simply hashing it and checking that the header hash is below the desired target. This is what is meant by _objectivity_: any new node joining the network can independently arrive at the same state or consensus as the rest of the network with no extra knowledge than the protocol definition. Validating proof of work is completely objective, it cannot be cheated.

The simplicity/objectivity of Bitcoin and PoW often gets overlooked or forgotten but it is one of the most important things about the system and its ability to scale securely and robustly (in comparison to other consensus algorithms such as PoS and pBFT for example). The beauty of Bitcoin is that it allows for LiteClients that can easily and securely have an objective view of any part of the entire system data in spite of the system growing indefinitely.&#x20;

Think about how the same thing could be achieved using something like Proof of Stake. There's no way to easily and objectively verify the blockchain state by just checking for the longest chain with the most proof of work behind it. Things get considerably more complex. You have to pretty much track the block stakers over the course of the blockchain in order to check that they've staked funds to be able to produce blocks and verify their (as well as the rest of the block staker's) signatures against their public keys, etc. etc. Complexity is the enemy of security. The simpler you can make the system, the more secure it will be, always.
