---
description: Bringing funds into the wallet.
cover: ../../.gitbook/assets/Dark Banner_PayMail Serverv1.0.png
coverY: 0
---

# Paymail Server

Paymail is a service discovery mechanism which allows for casual payments to be sent to an alias in a familiar format: `user@company.com`. [More details in the TSC publication.](https://tsc.bitcoinassociation.net/standards/paymail/)

{% embed url="https://www.youtube.com/watch?v=N0JigW1p5VE" %}

This Paymail Server runs a basic implementation of the standard service with cut down capabilities specifically built to integrate with an SPV wallet. Its main purpose is to allow incoming funds from existing Paymail wallets.

### Docker Image

{% embed url="https://hub.docker.com/r/bitcoinsv/paymail-server" %}
Paymail Server official image
{% endembed %}

### Source Code

{% embed url="https://github.com/bitcoin-sv/paymail-server" %}
