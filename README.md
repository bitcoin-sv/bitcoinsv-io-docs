---
description: Modular Components for Simplified Payment Verification
cover: .gitbook/assets/Dark Banner_Introductionv2.png
coverY: 0
---

# LiteClient Toolbox

## Motivation

The Bitcoin node software has been used by both miners and users of the network for over a decade. However, as we scale - the need to separate concerns has become apparent. The [Simplified Payment Verification (SPV)](resources/simplified-payment-verification.md) model outlined in the [Bitcoin Whitepaper](https://craigwright.net/bitcoin-white-paper.pdf) explains how we scale for users.

The LiteClient Toolbox is a collection of modular components which enable the construction of SPV services today. Security, speed, scale, and stability are all achieved at low cost by enabling each entity to independently validate their own subset of transactions within the blockchain.

## Key Takeaway

**You do not need to run the node software** to validate transactions; you only need the block headers, Merkle proofs, and the ability to check the state of UTXOs with miners.

This will **dramatically reduce running costs** because resource requirements will scale with _**your business' volume**,_ as opposed to the network volume as a whole.

## Direct Payment Protocol

Many of the LiteClient Toolbox components use the [Direct Payment Protocol (DPP)](broken-reference).

## Production LiteClient Wallet Release

You can use the tools presented here to build SPV into your own stack now, getting ahead of the competition. Otherwise, a LiteClient Wallet Release will be available to download in Q1 2023.
