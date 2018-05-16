# Online Payments

## Introduction

[Request Network](https://request.network) is a decentralized way to accept cryptocurrencies, in the future the Request Network Foundation will also develop an even more straight forward way to start accepting them, feel free to [subscribe to the newsletter](https://network.us12.list-manage.com/subscribe/post?u=e17500a255904b2fa3482005c&id=62ea53d6f0) to get alerted.

_Disclaimer: Note that the Request Network is still in beta and the library is subject to evolve in the coming months. The Request Network is a protocol which is not owned or controlled by a company. This website won’t hold your cryptocurrencies. You are in charge of setting up a wallet on your own._

**Features:**

During the first beta phase, the Request Network will only allow ether payments. During the second beta phase, the Request Network will allow ERC20 payments. Support for Bitcoin and other cryptocurrencies will follow closely.

**Plugins:**

* Shopify: No plugin available yet
* WooCommerce: [https://wooreq.com/](https://wooreq.com/)

**Steps to integrate Request Network:**

If you already have a cryptocurrency account, there are only 2 steps:

1. Create a request for payment on your back-end
2. Add a payment button on your front-end and redirect the user to the gateway

It can be done in under 30 lines of code for a quick integration and can also be customized more if you wish to synchronize the orders with your own system.

A full example of calling Request is available on [this Github](https://github.com/RequestNetwork/Request-OP).

## Preconfiguration

### Create a wallet to store your currencies

_If you already have your wallets set up, you can skip this part and go_ [_here_](online-payments.md#create-an-account-to-store-your-identity)_._

#### Ether \(ETH\)

You need to prepare a wallet to receive the ethers.

An ether address looks like this: 0xdD76B55ee6dAfe0c7c978bff69206d476a5b9Ce7

You can create your own address on different websites including:

* [Coinbase](https://www.coinbase.com) \(easy set up, can be on mobile\)
* [MyCrypto](https://mycrypto.com/) \(advanced setup\)
* [Metamask](https://metamask.io/) \(in browser setup\)
* Hardware wallet like [Ledger](https://www.ledgerwallet.com/) or [Trezor](https://trezor.io/) \(Hardware setup\)

### Create an account to store your identity.

This account is a key that does not hold your money, the purpose is to be the proof that you are the one asking for a payment.

You need to generate a 12 words mnemonic seed, then store it somewhere safe. We recommend:

* [https://iancoleman.io/bip39/](https://iancoleman.io/bip39/)
* [Metamask](https://metamask.io/)
* [Ledger](https://www.ledgerwallet.com/) Wallet \(using a 12 words seed\)

If someone accesses your mnemonic seed, he won’t be able to access the money you received on the other account. However he will be able to generate Requests in your name. Consider these words as a very important password.

## Integrating Request Network

The developer view of the flow can be seen in the picture below:

![Developer view of the flow](https://i.imgur.com/7bGGF3Ag.png)

### 1. Create a request for payment on your back-end

#### a. Add dependencies and instantiate Request Network

> On your backend, import the libraries:

```javascript
import RequestNetwork from '@requestnetwork/request-network.js';

var HDWalletProvider = require ('truffle-hdwallet-provider');
```

* Install these two dependencies:

**request-network.js** `npm install @requestnetwork/request-network.js --save`

**truffle-hdwallet-provider** `npm install truffle-hdwallet-provider`

* Instantiate Request Network:

> Instantiating requestnetwork with HDWalletProvider on **Mainnet**:

```javascript
var mnemonic = 'twelve word mnemonic phrase of the wallet used for signing the requests';
var provider = new HDWalletProvider(mnemonic, 'https://mainnet.infura.io/');
var networkId = 1; // 1 is for Mainnet (4 for Rinkeby testnet, 99 for local rpc)

try {
  var requestnetwork = new RequestNetwork(provider, networkId);
} catch (err) {
  console.error(err);
}
```

request-network.js needs a Web3 provider to sign requests. Web3 library is a collection of modules which contain specific functionality for the ethereum ecosystem.

In this guide we use **truffle-hdwallet-provider** which instantiates a Web3 provider to sign transactions for addresses derived from a 12-word mnemonic phrase.

`new HDWalletProvider(mnemonic, provider_uri, index)` \(see example\)

| Parameter | Type | Description |
| --- | --- | --- |
| mnemonic | string | 12 word mnemonic which addresses are created from. |
| provider\_uri | number | URI of Ethereum client to send all Web3 requests. |
| index | number \(optional\) | If specified, will tell the provider to manage the address at the index specified. Defaults to the first address \(index 0\). |

By default, the HDWalletProvider will use the address of the first address that's generated from the mnemonic. If you pass in a specific index, it will use that address instead. Currently, the HDWalletProvider manages only one address at a time.

`new RequestNetwork(provider, network_id)` \(see example\)

| Parameter | Type | Description |
| --- | --- | --- |
| provider | object | Web3 Provider for the library to use. |
| networkId | number | network id you're using, **must match the network of the provider's Ethereum client**. |

#### b. Signing a request

> Example:

```javascript
var requestData = { reason: 'Order #030890 from Just Another Shop',
                    orderId: '030890' }

var signedRequest = await requestnetwork.requestEthereumService.signRequestAsPayee(
          ['0x8F0255e24B99825e9AD4bb7506678F18C630453F'],
          ['175000000000000000'],
          new Date().getTime() + 3600, // we put expiration after 1 hour here
          ['0xf9DF490146b29418a59F43dDb4Afc57Cd3fEf856'],
          JSON.stringify(requestData),
        );
```

> The above command returns JSON structured like this:

```javascript
{
  "currencyContract": "0xafa312973909c3a541665e11c883a24a8eb10b2c",
  "data": "QmSsrdAPhD5UkRVDKVpCwf63nrUxBaNbYmpZ2njbku9Mi5",
  "expectedAmounts": [
    "175000000000000000"
  ],
  "expirationDate": 1522207081496,
  "hash": "0x0071762fae039b38da6c0685172c2de8df98eed1cbfc7b709bbe23df51c16fb3",
  "payeesIdAddress": [
    "0x8F0255e24B99825e9AD4bb7506678F18C630453F"
  ],
  "payeesPaymentAddress": [
    "0xf9DF490146b29418a59F43dDb4Afc57Cd3fEf856"
  ],
  "signature": "0x4708d801416d8ee4604af4bf8b3a2328592ab450d6a48c0d66446e26b51066cb2d2c568d37c38ae1bc3b48f1582e24b9c8b63bac9c548d8754f5d50b33e5fb491c"
}
```

Now we need to ask the library to create a request for payment.

`async requestnetwork.requestEthereumService.signRequestAsPayee(payeesIdAddress, expectedAmounts, expirationDate, payeesPaymentAddress, metadata)` \(see example\)

| Parameter | Type | Description |
| --- | --- | --- |
| payeesIdAddress | string\[\] | ID address of the payee \([see step in Preconfiguration](online-payments.md#create-an-account-to-store-your-identity)\). Additional payees can be added in the array. Position 0 is the main payee \(the one who sign the request\). |
| expectedAmounts | number\[\] | Amount in Wei of the payment Request for each payee. \(1Eth = 1000000000000000000 Wei\) |
| expirationDate | number | Timestamp in seconds of the date after which the signed request can not be paid anymore. We recommend to set:  `new Date().getTime() + 3600` |
| payeesPaymentAddress | string\[\] \(optional\) | Address on which to receive the payment \([see step in Preconfiguration](online-payments.md#create-a-wallet-to-store-your-currencies)\) for each payee |
| metadata | String \(optional\) | Json string of the request's details \(data will be hosted on ipfs\). See below for more information |

#### c. Other currencies \(available soon\)

#### d. Store metadata

Request Network supports adding metadata to every request.

Privacy: The metadata are public as of today. You will be able to select the privacy during the second part of the beta phase.

Accounting: Accounting standardized data will be specified during the first beta phase.

We use the following format `metadata = { reason: String, orderId: String }`

### 2. Add a payment button on your front-end and redirect the user to the gateway

> Example of Html and css code for styling a “pay with ETH” button:

```markup
<button class="pay-with-button">
  <img src="https://payments.request.network/assets/request-logo.png" style="height: 30px;">
  <span>Pay with&nbsp;</span>
  <img src="https://payments.request.network/assets/eth-logo-white.png" style="height: 20px;">
  <span _ngcontent-c1="">&nbsp;ETH</span>
</button>

`
```

```css
pay-with-button { 
  height: 60px;
  min-width: 200px;
  display: flex;
  align-items: center;
  background-color: #103b56;
  font-size: 16px;
  font-weight: normal;
  color: white;
  border: none;
  border-radius: 2px;
  border-bottom: 3px solid #6CFDCC;
  padding: 0 16px;
}
```

#### a. Adding the button to your website

![pay-with button](https://i.imgur.com/p6gr2CB.png)

See example code for styling a "paywith ETH" button.

#### b.  Redirect to the gateway

In order to ease the process for integrating a pay-with-request button on your website, the request app provides a gateway for displaying the info of a signed request and allowing someone to pay it.

> Example for redirectiong to the gateway with right parameters:

```javascript
var qs = JSON.stringify({signedRequest: signedRequest, callbackUrl: myCallbackUrl, networkId: 1}));
var qsBase64 = btoa(qs);
var qsb64Encoded = encodeURIComponent(qsBase64);
document.location.href = 'https://app.request.network/#/pay-with-request/' + qsb64Encoded;
```

The gateway url is `https://app.request.network/#/pay-with-request/<qsBase64>`

where `qsBase64` is a base 64 encoded string of a JSON object containing the following **mandatory** parameters

| Parameter | Type | Description |
| --- | --- | --- |
| signedRequest | object | signed request object \(see paragraph above for format\) |
| callbackUrl | string | url for the gateway to redirect after request has been paid |
| networkId | string | id of the network on which you signed the request \([see step 1.](online-payments.md#1-create-a-request-for-payment-on-your-back-end)\) |

### Paying on the gateway

You can find an example of how a signed request looks like on the gateway on this [link](https://app.request.network/#/pay-with-request/eyJzaWduZWRSZXF1ZXN0Ijp7ImN1cnJlbmN5Q29udHJhY3QiOiIweGQ4OGFiOWIxNjkxMzQwZTA0YTViYmY3ODUyOWMxMWQ1OTJkMzVmNTciLCJkYXRhIjoiUW1Tc3JkQVBoRDVVa1JWREtWcEN3ZjYzbnJVeEJhTmJZbXBaMm5qYmt1OU1pNSIsImV4cGVjdGVkQW1vdW50cyI6WyIxNzUwMDAwMDAwMDAwMDAwMDAiXSwiZXhwaXJhdGlvbkRhdGUiOjE1MjQ3MTgxMzEzNjYsImhhc2giOiIweDQ1MmRlMGE2OWI3ZDBmM2I5N2ZlOTY5Mjg0NmZmOGM2NzkyODZhZDRmZGY0NGFkNjI3YWNkNWJmNzlmM2RjYTciLCJwYXllZXNJZEFkZHJlc3MiOlsiMHg4RjAyNTVlMjRCOTk4MjVlOUFENGJiNzUwNjY3OEYxOEM2MzA0NTNGIl0sInBheWVlc1BheW1lbnRBZGRyZXNzIjpbIjB4ZjlERjQ5MDE0NmIyOTQxOGE1OUY0M2REYjRBZmM1N0NkM2ZFZjg1NiJdLCJzaWduYXR1cmUiOiIweDE1ZWMwZjRlZjE3N2U1ZTk5YjRjYmUyOTI4NzY1MTY5ZDEwNzYxYzNlNzVjNzU0OTI3ODE3MDVhZTQ4MzlmZTg0NmVkYzAyMmExNjI1MzViOWQ1MDg4MmI2NTFmZGMyYTc1MTg3MzY0MWY2ZjA0NjQ5YjBjNGU4ODUzN2MwNjg1MWMifSwiY2FsbGJhY2tVcmwiOiJodHRwczovL3BheW1lbnRzLnJlcXVlc3QubmV0d29yay8jL2NvbmZpcm0vIiwibmV0d29ya0lkIjo0fQ)

On the gateway, the payer connects to his ether wallet and pays the request.

Once the payment transaction is broadcasted, the gateway redirect to the **callbackUrl** concatenated with the string of the transaction hash.

Example:

`myCallbackUrl/0xb8e90ba15bb09d1b5baba628fcc01d0302d8d47dc854e85e6deeb5c60f6d3f2b`

Otherwise, if the payer click on 'cancel & go back to merchant', the gateway will redirect to the **callbackUrl** concatenated with the signed request json object stringified.

Your signature ensure that only the information you signed can be broadcasted on ethereum's blockchain

## Additional info

### Verify your orders and/or Reconcile

After paying on the gateway the user is now redirected to your callback URL and you can display a custom message to confirm the success of the operation.

Note that as a blockchain payment is only confirmed after a certain time, you can’t be sure that the payment has been done at this moment.

You can view your payments at this URL, replacing the address with your own ID address \(created in paragraph [Preconfiguration](online-payments.md#preconfiguration)\) [https://app.request.network/\#/search/0x54B4B2d8E2ECcC99385B01bE13DFB473c7885286](https://app.request.network/#/search/0x54B4B2d8E2ECcC99385B01bE13DFB473c7885286)

### Reconcile the payment with your own system

> Example:

```javascript
 try {
      var txHash = 0xb8e90ba15bb09d1b5baba628fcc01d0302d8d47dc854e85e6deeb5c60f6d3f2b;
      var result = await requestnetwork.requestCoreService.getRequestByTransactionHash(txHash);
    } catch (err) {
      console.error(err);
    }
  }
```

> The above command returns JSON structured like this:

```javascript
{
  "transaction": {
    "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "blockNumber": null,
    "from": "0x1AA3fFefeaeC333f9BF76b705547D08c90cC1280",
    "gas": 287504,
    "gasPrice": "1000000000",
    "hash": "0x8083f504e6f515a5c98debcbd54fd1c98c86d9eff7098d1b701e8eacbe5856e8",
    "input": "0x9dbdd85c00000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000018000000000000000000000000000000000000000000000000000000000000001c00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000001626ac2d6390000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000008c8f0255e24b99825e9ad4bb7506678f18c630453f0000000000000000000000000000000000000000018f0255e24b99825e9ad4bb7506678f18c630453f000000000000000000000000000000000000000000000000026db992a3b180002e516d537372644150684435556b5256444b567043776636336e72557842614e62596d705a326e6a626b75394d693500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000f9df490146b29418a59f43ddb4afc57cd3fef8560000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000026db992a3b180000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004156d628e48ead956625124627ad2dafb4d51a9e913339038de8aeefd1b017fc921917a2c9d73e94d8836a757ac7722588063b907c14834f069627bf36a6ca0ef61b00000000000000000000000000000000000000000000000000000000000000",
    "nonce": 79,
    "to": "0xAFA312973909C3a541665e11C883a24a8eb10b2C",
    "transactionIndex": 0,
    "value": "175000000000000000",
    "v": "0x2c",
    "r": "0xbaf3e6351bd41581f3557a0baef9f63eaee184327b2f698a1b7697f70f9006fe",
    "s": "0x664b8f600a6ce7b0806780dfcc657e84dd281d2896289eaf11da9daf303e1640",
    "method": {
      "name": "broadcastSignedRequestAsPayer",
      "parameters": {
        "0": "0x8f0255e24b99825e9ad4bb7506678f18c630453f0000000000000000000000000000000000000000018f0255e24b99825e9ad4bb7506678f18c630453f000000000000000000000000000000000000000000000000026db992a3b180002e516d537372644150684435556b5256444b567043776636336e72557842614e62596d705a326e6a626b75394d6935",
        "1": [
          "0xf9DF490146b29418a59F43dDb4Afc57Cd3fEf856"
        ],
        "2": [
          "175000000000000000"
        ],
        "3": [],
        "4": "1522209576505",
        "5": "0x56d628e48ead956625124627ad2dafb4d51a9e913339038de8aeefd1b017fc921917a2c9d73e94d8836a757ac7722588063b907c14834f069627bf36a6ca0ef61b",
        "__length__": 6,
        "_requestData": "0x8f0255e24b99825e9ad4bb7506678f18c630453f0000000000000000000000000000000000000000018f0255e24b99825e9ad4bb7506678f18c630453f000000000000000000000000000000000000000000000000026db992a3b180002e516d537372644150684435556b5256444b567043776636336e72557842614e62596d705a326e6a626b75394d6935",
        "_payeesPaymentAddress": [
          "0xf9DF490146b29418a59F43dDb4Afc57Cd3fEf856"
        ],
        "_payeeAmounts": [
          "175000000000000000"
        ],
        "_additionals": [],
        "_expirationDate": "1522209576505",
        "_signature": "0x56d628e48ead956625124627ad2dafb4d51a9e913339038de8aeefd1b017fc921917a2c9d73e94d8836a757ac7722588063b907c14834f069627bf36a6ca0ef61b"
      }
    }
  }
}
```

#### 1. Query the hash received during the callback from your backend to know which order it is

If you need to reconcile the payment with your own system, the library provides a method for getting the transaction information from a txHash:

`async requestnetwork.requestCoreService.getRequestByTransactionHash(txHash)`

| Parameter | Type | Description |
| --- | --- | --- |
| txHash | string | hash of the ethereum transaction |

Transaction information may not appear instantly after user paid the request and got redirected to the callbackUrl. Also you will have to call the method getRequestByTransactionHash until you receive a response with the transaction data.

#### 2. Compare the signature to verify the order is as expected

Once you receive the **transaction** object, you need to check some parameters to ensure it actually corresponds to the broadcast of a request payment transaction.

* First one is verifying the name of the method called is "broadcastSignedRequestAsPayer":

`transaction.method.name == 'broadcastSignedRequestAsPayer'`

> Example for getting additionnal info of the transaction:

```javascript
data = requesnetwork.requestCoreService.parseBytesRequest(result.transaction.method.parameters._requestData);
```

> The above command returns JSON structured like this:

```javascript
{
  "creator": "0x8f0255e24b99825e9ad4bb7506678f18c630453f",
  "payer": "0x0000000000000000000000000000000000000000",
  "mainPayee": {
    "address": "0x8f0255e24b99825e9ad4bb7506678f18c630453f",
    "expectedAmount": "26db992a3b18000"
  },
  "subPayees": [],
  "data": "QmSsrdAPhD5UkRVDKVpCwf63nrUxBaNbYmpZ2njbku9Mi5"
}
```

* Second one is verifying that the amount paid matches the total amount of the request

For that you need to call an additionnal method:

`requestnetwork.requestCoreService.parseBytesRequest(requestData)`

| Parameter | Type | Description |
| --- | --- | --- |
| requestData | string | request data bytes \(variable found in transaction json object above `transaction.method.parameters._requestData`\) |

From that point if you stored the signed request information on a database, you just need to verify that transaction.method.parameters.\_signature matches a known signature

#### 3. Query the blockchain until the payment is complete

You need to query getRequestByTransactionHash until you have a request object with and verify the payment is correct.

### Extras

#### Converting currencies

A request has to be paid in the cryptocurrency it was created with for the moment.

In the future, the request will be able to convert the currency automatically.

If you would like to exchange cryptocurrency to fiat money, you will have to use one of the cryptocurrency exchange available in your country.

#### Disputes & Fraud

There is no chargeback on the blockchain. To protect the user, automated escrow systems will be created and adding to this documentation in S2 2018.

#### Recurring payments \(not available yet\)

#### Creating your own gateway

The Request Network Foundation is managing the app.request.network gateway. Anyone can create its own process or gateway to pay a request, by using the resources below.

Resources:

[https://github.com/RequestNetwork/Request/wiki](https://github.com/RequestNetwork/Request/wiki)

[https://github.com/RequestNetwork/Requestnetwork](https://github.com/RequestNetwork/Requestnetwork)

#### Next steps

A dashboard is under development and will simplify the merchant process.

