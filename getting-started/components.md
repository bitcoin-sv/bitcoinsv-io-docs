---
description: Which components are actually required?
cover: ../.gitbook/assets/Dark Banner_Setupv2.png
coverY: 0
---

# 🧱 Components

## Wallet & Headers Only

If you're running things from a network which is exposed to the internet with firewall rules under your control, only the Wallet and Block Headers Client are required.

| Component                                                           | What it does                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Wallet](broken-reference)                                          | <ul><li>Stores users, invoices, transactions, keys, and proofs</li><li>Validates transactions against block headers</li><li>Broadcasts transactions to miners</li><li>Connects to other wallets directly for payments</li><li>Exposes an API to interface with</li></ul> |
| [Block Headers Client](../toolbox-components/block-headers-client/) | <ul><li>Stores block headers</li><li>Connects to the node network p2p</li><li>Exposes an API for the Wallet</li></ul>                                                                                                                                                    |

## With DPP Proxy

For routing payment requests to a wallet which is not exposed to the internet.

| Component                                     | What it does                                                                                                                                                                    |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [DPP Proxy](../toolbox-components/dpp-proxy/) | <ul><li>Stores relationship between paymentIDs and socket connections</li><li>Exposes DPP API endpoints</li><li>Routes payment messages between corresponding parties</li></ul> |

#### Example Use Case

Bob is running the Wallet locally on a laptop, and he is at an airport. Alice, who wants to send Bob a payment directly, cannot reach his Wallet through the airport wifi. In this case, Bob's Wallet can be configured to connect to a DPP Proxy service which is exposed to the internet. Alice's request will then relay through the proxy to Bob over a secure web socket connection.

<figure><img src="../.gitbook/assets/DPP proxy.drawio (1).png" alt=""><figcaption><p>DPP Proxy used when direct communication is not possible</p></figcaption></figure>

## With Peer Channels

For allowing Wallets to go offline while they waiting for their transactions to be included in a block. A secure inbox for Merkle proofs which miners write to, and payment counterparties read from.

| Component                                             | What it does                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Peer Channels](../toolbox-components/peer-channels/) | <ul><li>Manages access to secure persistent message channels with read &#x26; write tokens.</li><li>Exposes endpoint to write Merkle proof when registered transaction is mined.</li><li>Allows Wallets to subscribe to notifications via secure web socket</li><li>Allows Wallets to read Merkle proofs from channel as needed</li><li>Stores Merkle proofs while counterparty wallets are offline</li></ul> |

#### Example Use Case

Bob broadcasts a transaction he received from Alice using a Mapi service hosted by a well known block producing miner. He shares a write token, and sets the callbackURL to:

`https://peerchannel.alice.com/api/v1/proofs/{txid}`

Bob subscribes to Peer Channel notifications, and sends a URL and read token to Alice so she can do the same.

Eventually a block is mined, at which point the miner writes the Merkle proof to the Peer Channel via the callbackURL. It captures the proof and notifies Alice and Bob. Each reads the channel whenever they can.

## With Paymail Server

Solely for the purpose of migrating funds from external wallets. The Paymail standard is common to most web wallets, so this server will act as a bridge from those wallets to the world of SPV.

| Component                                               | What it does                                                                                                                                                                                                                                    |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Paymail Server](../toolbox-components/paymail-server/) | <ul><li>Stores relationship between an alias and a Wallet user</li><li>Exposes a web API for the minimum required paymail capabilities to be compatible with industry wallets</li><li>Translates between Paymail &#x26; DPP protocols</li></ul> |

#### Example Use Case

Alice is setting up her SPV Wallet for the first time. She doesn't have a way to pay an invoice because there are no DPP wallets in the market yet. She sets up the Paymail Server which allows her to send a payment to alice@paymailwallet.com from her regular web wallet.\
Now she has funds in her SPV Wallet which she can pay invoices with using DPP.
