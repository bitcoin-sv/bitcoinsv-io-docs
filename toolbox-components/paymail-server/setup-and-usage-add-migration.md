---
description: Pay into your LiteClient from a Paymail wallet.
cover: ../../.gitbook/assets/Dark Banner_PayMail Serverv1.0.png
coverY: 0
---

# Setup & Usage (add migration)

### DNS Records Required

​In order to work with existing infrastructure, a DNS service record (SRV) is required. The record details the domain where the capabilities document is hosted. An address record (A) is required for the actual service, along with DNSSEC and an SSL certificate for best results.&#x20;

See [Paymail Standard](https://tsc.bitcoinassociation.net/standards/paymail/) for more details.&#x20;

**SRV Record**

```
_bsvalias._tcp.yourdomain.tld. 3600 10 10 443 yourdomain.tld.
```

**Note:** Sometimes registrars will make you separate out these values, in which case they are as follows:

| Label    | Value        |
| -------- | ------------ |
| service  | bsvalias     |
| protocol | tcp          |
| TTL      | 3600         |
| Priority | 10           |
| Weight   | 10           |
| Host     | example.com. |

### ​Configuration

Once you have the DNS records all pointing to the right hosts, you can simply add this domain name to the environment of the paymail-server:

{% code title="docker-compose.yaml" %}
```yaml
...
paymail:
   environment:
       PAYMAIL_ROOT: https://yourdomain.tld
       DOMAIN_TLD: yourdomain.tld
       # replace with domain pointed at your paymail service
       
       PAYD_HOST: payd
       PAYD_PORT: ":8443"
       # These are the default values which usually do not need to be specified.
       # They must point to the local wallet on the same docker network.
...
```
{% endcode %}

### Startup

In order to associate a Paymail alias with a particular user with the Wallet, you must register an alias using the domain name you intend to use to top the wallet up.

<details>

<summary>POST /api/alias</summary>

{% code title="Request" %}
```json
{
	"paymail": "username@your-domain.tld",
	"name": "Your Name",
	"email": "name@email.com"
}
```
{% endcode %}

{% code title="Response 202" %}
```json
{
	"user_id": 2,
	"paymail": "username@your-domain.tld"
}
```
{% endcode %}

</details>

Thereafter you can send payments to username@your-domain.tld in order to fund the Wallet.

### Usage

Whenever a Paymail client request is sent to this server it will create an invoice using the Wallet JSON api, and then call the the DPP endpoints of the wallet, translating between DPP and Paymail message standards before passing those details to the Paymail client making the request.

Starting from an existing Paymail wallet - simple make a payment to epic@yourdomain.tld and the funds will be in your wallet within seconds.

Use the `/api/v1/balance` route of your wallet to check the deposit was successful.
