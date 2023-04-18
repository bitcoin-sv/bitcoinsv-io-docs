---
description: Connecting to the p2p node network for headers only
cover: ../../.gitbook/assets/Dark Banner_HeaderSVv1.0.png
coverY: 0
---

# Block Headers Client

{% embed url="https://www.youtube.com/watch?v=be2R1I57pLs" %}

With LiteClient toolbox, we introduce a Java implementation of BHC called HeaderSV.

HeaderSV is a Java implementation of Block Header Client. The client will connect to the BitcoinSV network and manage whitelists and blacklists internally. The application ensures that it is always connected to at least the minimum number of peers configured before synchronizing any blocks.

A REST API is also provided by the application which will allow rapid lookups of chain and network state, as well as basic controls to control stored data.

The networks currently supported by the client are:

* Mainnet
* Testnet
* STNnet

Additional networks can be added with ease.

### Endpoints

The endpoints used by HeaderSV are listed below:

| Endpoint             | Method | Description                                                       |
| -------------------- | ------ | ----------------------------------------------------------------- |
| /header/{hash}       | GET    | Returns the header with the given hash                            |
| /header/byHeight     | GET    | Returns the header with the given height                          |
| /header/state/{hash} | GET    | Returns the header with the given hash and their state            |
| /peers               | GET    | Returns a list of peers that are connected to the application     |
| /peers/count         | GET    | Returns the count of nodes connected                              |
| /prune               | GET    | Prunes a specified fork                                           |
| /tips                | GET    | Returns the latest headers for the tip of the chain and any forks |

### Docker Image

{% embed url="https://hub.docker.com/r/bitcoinsv/block-headers-client" %}
HeaderSV official image
{% endembed %}

### Source Code

{% embed url="https://github.com/bitcoin-sv/block-headers-client" %}
