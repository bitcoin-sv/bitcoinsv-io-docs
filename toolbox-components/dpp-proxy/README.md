---
cover: ../../.gitbook/assets/Dark Banner_DPPRv1.0.png
coverY: 0
---

# DPP Proxy

The sole purpose of DPP Proxy is to allow desktop wallets to receive incoming payments without needing to expose a web host to the internet from their local machine.

The Proxy sits on the open internet ready to receive incoming payment requests on users' behalf. When such a request comes in, the message is routed to the appropriate client wallet.&#x20;

### Mode: \["hybrid", "http", "socket"]

Usually DPP Proxy is set up in `hybrid` mode. This means the incoming requests are https, but the client connection is done over secure web socket.&#x20;

It is possible to configure DPP Proxy as entirely `socket` or `http`, which means the transports on both sides of the proxy would be either socket or http endpoints.

Regardless of mode, the messages conform to the [DPP](broken-reference) standard.&#x20;

You can see a public example of the DPP Proxy running on testnet with [swagger documentation](https://infra.bitcoinsv.io/dpp/swagger/index.html).

### Docker Image

{% embed url="https://hub.docker.com/r/bitcoinsv/dpp-proxy" %}
dpp-proxy official docker image
{% endembed %}

### Source Code

{% embed url="https://github.com/bitcoin-sv/dpp-proxy" %}
