---
description: Basic reference to build from
---

# Wallet

The wallet provided in LCT is a basic reference implementation to build from as a starting place, not intended for production use without modification. Master key encryption and authorization header requirements for the API would be the recommended minimum improvements.

[ElectrumSV](https://electrumsv.io/) is the trusted desktop wallet which will implement SPV as of v1.4. If you're just looking for something to run rather than building SPV into your own stack, that's the way to go.

The wallet has a [JSON api](usage.md) to control operation, but no graphic user interface.

It connects to the [Block Headers Client](../block-headers-client/) to get any headers referenced in transactions it receives for verification. Once all checks validate, transactions are sent via [Mapi](../../resources/mapi.md) to all miners.

<figure><img src="../../.gitbook/assets/LiteClient Toolbox Componentsv1.0.png" alt=""><figcaption><p>The Wallet is the core component of the LiteClient</p></figcaption></figure>

You can see a public example of the wallet running on testnet with [swagger documentation](https://faucet.bitcoinsv.io/payd/swagger/index.html).

### Docker Image

{% embed url="https://hub.docker.com/r/libsv/payd" %}
payd official image
{% endembed %}

### Source Code

{% embed url="https://github.com/libsv/payd/" %}
