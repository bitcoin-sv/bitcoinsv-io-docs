---
description: Understanding LiteClient Toolbox
---

# LCT Deep Dive

## What is LiteClient Toolbox (LCT)?

{% embed url="https://www.youtube.com/watch?v=J_js3JhTafc" %}

LiteClient Toolbox (LCT) provides reference implementations of the components which make up the [Simple Payment Verification (SPV)](simplified-payment-verification.md) infrastructure as outlined in the [Bitcoin Whitepaper](https://craigwright.net/bitcoin-white-paper.pdf), along with a few extensions and additional features. LCT allows users and service providers to interact with Bitcoin in a scalable way. All components are designed to be very modular so that different people can use different components of the toolbox depending on their needs, as well as easily replace or upgrade separate components.

## Motivation

Currently, the Bitcoin SV Blockchain node requires extensive resources in terms of storage capability and its operational costs keep increasing with time. With large block sizes, businesses face the challenges of running, synchronizing and maintaining a full node for the BSV Blockchain. It requires them to not only review their system capabilities but also come up with extra solutions to ensure resilience and availability of BSV based services.&#x20;

A core part of the [Bitcoin design](https://craigwright.net/bitcoin-white-paper.pdf) is that it supports both nodes and users while separating their roles and capabilities. Satoshi Nakamoto [designed a system for Bitcoin that supported both nodes and users](https://bitcointalk.org/index.php?topic=532.msg6269#msg6269) playing different roles. He envisioned nodes as "_big server farms_" and users as "_client nodes_ \[or LiteClients] _that only do transactions and don't generate_ \[blocks]". As a result of the Bitcoin system being subject to artificially imposed limits in the first decade of its existence, this distinction was not very clear and did not make its way into the infrastructure which remained very fragile and unscalable for far too long.

## Who should run a LiteClient?

The LiteClient infrastructure aims to fill that gap where enterprises who do not mine can move away from hosting a full node to a focused SPV infrastructure which allows them to just store and run Bitcoin SV blockchain based services, according to their needs. Such LiteClients are key for scaling bitcoin because they allow you to build applications using the Bitcoin network without hosting expensive node infrastructure.

Typical businesses that can adopt the LiteClient infrastructure are:&#x20;

1. Wallet providers
2. Exchanges who use payment capabilities&#x20;
3. Applications using BSV as a payment method&#x20;
4. Individuals who are interested in Peer to Peer capabilities and want to run their payment infrastructure (for BSV payments)
5. SME's who are interested in subsets of BSV Blockchain transactions (payment, data) who can also leverage this for transaction verification on blockchain.&#x20;

The above list is a suggestion, but in reality, any business interacting with Bitcoin SV Blockchain will be able to replace the work done via a full node with the LiteClient infrastructure.

### LiteClient attributes

A LiteClient is not required to maintain a full copy of every single transaction on the blockchain:

* State and data maintained locally relate only to the specific application
* Communications are done peer to peer (sender to receiver), including transaction exchange
* The Bitcoin node network handles transaction settlement
* Correctness is assured through externally sourced proofs

| Requirements                          | Delivered by                      |
| ------------------------------------- | --------------------------------- |
| Addressing and Service Location       | Paymail                           |
| Feature Negotiation                   | Paymail                           |
| Chain Verification                    | Block Headers Client              |
| Direct Node Interface                 | mAPI + Miner ID                   |
| Double Spend/Settlement Notifications | Miner ID + mAPI + Peer Channels   |
| Ancestry Validation                   | Block Header Clients + Wallet     |
| Direct Communications                 | DPP                               |
| Communication Relaying                | Peer Channels                     |
| Secure, Confidential Communications   | DPP + Peer Channels + HTTPS + WSS |

As described in the table above, there are various features and attributes offered with LCT that enable secure, instant, extensible, and scalable payments. The components and the toolbox are built to support future enhancements and features.

## Toolbox Components

<figure><img src="../.gitbook/assets/LiteClient Toolbox Componentsv1.0.png" alt=""><figcaption><p>LiteClient Toolbox Components</p></figcaption></figure>

As shown in the above illustration, there are various components that make up the setup required for any application that would like to send and receive Bitcoin payments, and any Bitcoin token or asset. The LiteClient structure lays the foundation to be extended for various use cases such as payment/state channels, multi-party payments, token/NFT transfers, data certification verification.
