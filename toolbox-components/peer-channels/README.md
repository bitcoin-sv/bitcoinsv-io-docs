---
description: A secure inbox for Merkle proofs.
cover: ../../.gitbook/assets/Dark Banner_PeerChannelsv1.0.png
coverY: 0
---

# Peer Channels

{% embed url="https://www.youtube.com/watch?v=fGMENoOxN84" %}

Peer Channels is a secure persistent messaging medium, allowing broadcast communication in a one-to-many manner between entities. It allows [Mapi](../../resources/mapi.md) to broadcast a Merkle Proof to anyone involved with a payment.

### Peer Channels End to End Process

The following steps provide an end to end process of the Peer Channels usage:

1. The sender sends payment to the receiver.
2. Once the receiver deems the payment to be valid (for example, ancestors is verified), they create a new Peer Channel.
3. Once the channel has been created, the receiver creates three API Tokens for the new channel, as follows:

* Two read only tokens
* One write only token.

1. The receiver broadcasts the transaction to mapi, with a callback url pointing to the new Peer Channel, and an auth token as the write only API token.
2. The receiver uses one of the read-only tokens to subscribe to the new Peer Channel for notifications.
3. The receiver sends the other read-only token, along with the Peer Channel information (peerchannel host name, peerchannel ID) back to the sender.
4. The sender uses this information to also subscribe to the Peer Channel for notifications.

\-- -- -- TIME DELAY -- -- --

1. Once the block is mined, mAPI uses its callback URL and write-only API token to POST the Merkle proof for the block to the Peer Channel.
2. When the Peer Channel receives the proof, it notifies all subscribers of a new message (in this case, the sender and the receiver).
3. Both the sender and the receiver make a REST API call to the Peer Channel, with their write-only tokens, to receive the sent Merkle proof.
4. Both the sender and the receiver store the Merkle proof in their respective data stores.

#### Peer Channels Process Flow

<figure><img src="../../.gitbook/assets/Peer Channelsv1.0.png" alt=""><figcaption><p>Peer Channels</p></figcaption></figure>

### Docker Images

[Peer Channels Official Docker Image](https://hub.docker.com/r/bitcoinsv/spvchannels)

[Database for Peer Channels Official Docker Image](https://hub.docker.com/r/bitcoinsv/spvchannels-db)

### **Source Code**

{% embed url="https://github.com/bitcoin-sv/spvchannels-reference" %}
