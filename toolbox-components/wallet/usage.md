---
description: Basic wallet api
---

# Usage

The wallet is controlled by a JSON api which provides the interface for operation. This should only be accessed from a local machine or a secure front end, since it's not protected by any credential requirements.

### Balance

<details>

<summary>Get wallet balance</summary>

GET /api/v1/balance

Sums all spendable satoshis under the wallet's control.

{% code title="Response 200 OK" %}
```json
{
    "balance": 1000
}
```
{% endcode %}

</details>

### Invoices

Creating an invoice is a prerequisite for accepting payment.&#x20;

<details>

<summary>Create a new invoice</summary>

POST /api/v1/invoices

{% code title="Request" %}
```json
{
  "description": "string",
  "expiresAt": "string",
  "reference": "string",
  "satoshis": 1000
}
```
{% endcode %}

{% code title="Response 201 OK" %}
```json
{
    "id": "13MzVgA",
    "reference": "ref",
    "description": "description",
    "satoshis": 1000,
    "expiresAt": "2022-01-26T12:11:16.267690974Z",
    "paymentReceivedAt": null,
    "refundTo": null,
    "refundedAt": null,
    "state": "pending",
    "createdAt": "2022-01-25T12:11:16.267690974Z",
    "updatedAt": "2022-01-25T12:11:16Z",
    "deletedAt": null
}
```
{% endcode %}

</details>

<details>

<summary>Get invoice by id</summary>

GET /api/v1/invoices/{id}

{% code title="200 OK" %}
```json
{
    "id": "{id}",
    "reference": "ref",
    "description": "description",
    "satoshis": 2000,
    "expiresAt": "2022-01-26T12:11:16.267690974Z",
    "paymentReceivedAt": null,
    "refundTo": null,
    "refundedAt": null,
    "state": "pending",
    "createdAt": "2022-01-25T12:11:16.267690974Z",
    "updatedAt": "2022-01-25T12:11:16Z",
    "deletedAt": null
}
```
{% endcode %}

</details>

<details>

<summary>Delete invoice by id</summary>

DELETE /api/v1/invoices/{id}

`Response 204 OK no content`

</details>

<details>

<summary>List all invoices</summary>

GET /api/v1/invoices

{% code title="Response 200 OK" %}
```json
[
  {
    "id": "13MzVgA",
    "reference": "ref",
    "description": "description",
    "satoshis": 2000,
    "expiresAt": "2022-01-26T12:11:16.267690974Z",
    "paymentReceivedAt": null,
    "refundTo": null,
    "refundedAt": null,
    "state": "pending",
    "createdAt": "2022-01-25T12:11:16.267690974Z",
    "updatedAt": "2022-01-25T12:11:16Z",
    "deletedAt": null
  },
  {
    "id": "2dH24s32",
    "reference": "ref",
    "description": "description",
    "satoshis": 2000,
    "expiresAt": "2022-01-26T13:11:16.267690974Z",
    "paymentReceivedAt": null,
    "refundTo": null,
    "refundedAt": null,
    "state": "pending",
    "createdAt": "2022-01-25T13:11:16.267690974Z",
    "updatedAt": "2022-01-25T13:11:16Z",
    "deletedAt": null
  }
]
```
{% endcode %}

</details>

### Sending Payments

<details>

<summary>Action a payment to another wallet via DPP</summary>

POST /api/v1/pay

{% code title="Request" %}
```json
{
    "payToURL": "https://infra.bitcoinsv.io/api/v1/payment/1o9GgvP"
}
```
{% endcode %}

{% code title="Response 200 OK" %}
```json
{
    "id": "1o9GgvP",
    "tx_id": "0d8102845954ed8802a42a5c90ee1e30006a450e2a91a1858f9f4a039e168006",
    "memo": "note of some sort",
    "peer_channel": {
        "host": "https://peer.bitcoinsv.io/",
        "path": "/peerchannels",
        "id": "k8xWqbMFSmWWtHofHpt1ny30YmTa_TpM00-Fyh6tUYFSqEGXHVNHzEGmlXOVGvGzY-6GuWg4UNpxj0pwipaEAA",
        "token": "4pjgAt3V55x5s1QHMHcopffyb78ZJyziJZ5vK6TvlJZDUQs9ptiMX7XWQNP2JIy3RzqZDGaQtK1v_uFJ02pXkg"
    }
}
```
{% endcode %}

</details>

### Users

<details>

<summary>Create new user</summary>

POST /api/v1/users

{% code title="Request" %}
```json
{
    "name": "Epictetus",
    "email": "epic@example.com",
    "avatar": "https://thispersondoesnotexist.com/image",
    "address": "1 Athens Avenue",
    "phoneNumber": "0800-call-me"
}
```
{% endcode %}

{% code title="Response 201 OK" %}
```json
{
    "id": 1,
    "name": "Epictetus",
    "email": "epic@example.com",
    "avatar": "https://thispersondoesnotexist.com/image",
    "address": "1 Athens Avenue",
    "phoneNumber": "0800-call-me",
    "extendedData": {
        "pki": "{publickey}",
    }
}
```
{% endcode %}

</details>

<details>

<summary>Get user profile data by id</summary>

GET /api/v1/users/{id}

<pre class="language-json" data-title="Response 200 OK"><code class="lang-json"><strong>{
</strong>    "id": 1,
    "name": "Epictetus",
    "email": "epic@example.com",
    "avatar": "https://thispersondoesnotexist.com/image",
    "address": "1 Athens Avenue",
    "phoneNumber": "0800-call-me",
    "extendedData": {
        "pki": "{publickey}",
    }
}
</code></pre>

</details>

### Receiving Payments

The wallet exposes [Direct Payment Protocol ](broken-reference)endpoints which can be exposed to allow receiving payments to invoices. You can read the specific message structure in the dedicated section.
