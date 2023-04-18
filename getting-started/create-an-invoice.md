---
description: Receive funds from the testnet faucet
cover: ../.gitbook/assets/Dark Banner_Setupv2.png
coverY: 0
---

# 📄 Create an invoice

### Setup Your Wallet in Testnet Mode

We're going to use testnet so that no real funds need to be used. The easiest way to do this is to clone the payd repo and run a make command which sets the wallet up with BA hosted DPP Proxy to make this as simple as possible. Run this bash script in your terminal to get set up:

```bash
git clone https://github.com/libsv/payd.git
cd payd
make run-compose-faucet
```

Once that's all up and running you can test out a basic endpoint to check things are operational in a different terminal window:

```bash
curl --request GET \
  --url http://localhost:8443/api/v1/balance
```

The response should be:

```json
{ "satoshis":0 }
```

You can then create an invoice for 1,000 testnet satoshis.

```bash
curl --request POST \
  --url http://localhost:8443/api/v1/invoices \
  --header 'Content-Type: application/json' \
  --data '{
	"satoshis": 1000
}'
```

You will get a response like:

<pre class="language-json"><code class="lang-json">{
<strong>  "id": "3gJA9rq",
</strong>  "reference": null,
  "description": null,
  "satoshis": 1000,
  "expiresAt": "2022-08-23T14:18:05.6993126Z",
  "paymentReceivedAt": null,
  "refundTo": null,
  "refundedAt": null,
  "state": "pending",
  "createdAt": "2022-08-22T14:18:05.6993126Z",
  "updatedAt": "2022-08-22T14:18:05Z",
  "deletedAt": null
}
</code></pre>

The only important part at this stage is the `id`. This will form the part of the URL for requesting payment from the faucet, so you should copy it and paste it into a URL of this format:

```
https://infra.bitcoinsv.io/dpp/api/v1/payment/{id}
```

### Receive Funds

Go to [DPP testnet faucet](https://faucet.bitcoinsv.io/) and paste the above payment url with your invoiceId into the payToUrl text input, then hit that pay to url button.

<figure><img src="../.gitbook/assets/Faucet BitcoinSV.png" alt=""><figcaption><p>Should look something like this</p></figcaption></figure>

Your balance should have updated by 1000 sats, which you can check by hitting the balance endpoint in browser: [http://localhost:8443/api/v1/balance](http://localhost:8443/api/v1/balance) ; or using the curl command:

```shell
curl --request GET \
  --url http://localhost:8443/api/v1/balance
```

Requests are limited to 100,000 satoshis max, which should be plenty to test out your applications and learn more about the Lite Client Toolbox modules.&#x20;
