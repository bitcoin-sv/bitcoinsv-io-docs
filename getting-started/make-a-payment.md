---
description: From your local wallet back to the faucet
cover: ../.gitbook/assets/Dark Banner_Setupv2.png
coverY: 0
---

# 💸 Make a payment

Once you're finished with the funds, please return them to the faucet for other developers to learn with. You can do this by creating an invoice on the faucet side remotely:

<pre><code>curl --request POST \
  --url https://faucet.bitcoinsv.io/payd/api/v1/invoices \
  --header 'Content-Type: application/json' \
  --data '{
<strong>	"satoshis": 883
</strong>}'
</code></pre>

and actioning the payment locally with the id in the response from the above request:&#x20;

<pre><code>curl --request POST \
  --url http://localhost:8443/api/v1/pay \
  --header 'Content-Type: application/json' \
  --data '{
<strong>	"payToURL": "https://faucet.bitcoinsv.io/payd/api/v1/payments/{id}"
</strong>}'
</code></pre>
