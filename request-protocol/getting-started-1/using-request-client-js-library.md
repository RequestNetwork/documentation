# Using Request client js library

You can use the Request Client library to connect to an existing node, for example one you have deployed yourself. You can view an interactive example of using the request client below. 

The request client ships both as a commonjs and a UMD module. This means you can use it in node application and in web pages.

Below is an example of using the request-client.js as a commonjs module. 

{% embed url="https://codepen.io/admreq/pen/joBeqe" %}

## Using the request-client.js library

 `@requestnetwork/request-client.js` is a typescript library part of the [Request protocol](https://github.com/RequestNetwork/requestNetwork). This package allows you to interact with the Request blockchain through [Request nodes](https://github.com/RequestNetwork/requestNetwork-private/blob/development/packages/request-node). This client side library uses Request nodes as servers, connected in HTTP. See the Request node documentation for more details on their API. It ships both as a commonjs and a UMD module. This means you can use it in node application and in web pages.

Use `@requestnetwork/request-client.js` to connect to a node and create requests.

```bash
npm install @requestnetwork/request-client.js
```

For more information you can check the [request-client.js documentation](https://v2-docs-js-lib.request.network/index.html) or our [github](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-client.js) repo.

You can also [follow this example](https://github.com/RequestNetwork/requestNetwork/blob/development/packages/integration-test/test/node-client.test.ts).

## Configure the Node to be used

Connect the library to a node or mock a node for development environments [like explained here](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-client.js#configure-which-request-node-to-use). 

```javascript
const requestNetwork = new RequestNetwork({
  nodeConnectionConfig: { baseURL: 'http://super-request-node.com' },
});
```

 It can be further configured with option from [Axios](https://github.com/axios/axios#request-config).

Alternatively, you can use the request-client in development without a node by using the `useMockStorage` option. When the option `useMockStorage` is `true`, the library will use a mock storage in memory instead of a Request node. It is meant to simplify local development and should never be used in production. Nothing will be persisted on the Ethereum blockchain and IPFS, it will all stay in memory until your program stops.

```javascript
const requestNetwork = new RequestNetwork({ useMockStorage: true });
```

That's it! You can now use the Request client. 

## The request-client.js in practice 

Let's create a simple application using the request-client.js. We'll create a simple invoice flow between two parties which will include creating, accepting, fetching and updating the expected amount of the invoice. 

Firstly, we need to define some parameters for the invoice such as the currency, expected amounts, the payee

```javascript
const requestParameters = {
  currency: RequestLogicTypes.CURRENCY.BTC, 
  expectedAmount: '100000000000',
  payee: {
    type: IdentityTypes.TYPE.ETHEREUM_ADDRESS,
    value: '0x627306090abab3a6e1400e9345bc60c78a8bef57',
  },
  payer: {
    type: IdentityTypes.TYPE.ETHEREUM_ADDRESS,
    value: '0x740fc87Bd3f41d07d23A01DEc90623eBC5fed9D6',
  },
};
```

We will also need to define a payment network which will define which payment address will receive the BTC payment \(see [payment detection](../payment-detection/)\)

```javascript
const paymentNetwork = {
   id: RequestNetwork.Types.PAYMENT_NETWORK_ID.TESTNET_BITCOIN_ADDRESS_BASED,
   parameters: {
      paymentAddress: 'mgPKDuVmuS9oeE2D9VPiCQriyU14wxWS1v',
   }
};
```

We now have everything we need to create an invoice, we can create an invoice like so:

```javascript
const invoice = await requestNetwork.createRequest({
  requestParameters,
  signer: requestParameters.payee,
  paymentNetwork
});
```

You can now view information on this request

#### Get invoice information from its request ID

```javascript
const invoiceFromRequestID = await requestNetwork.fromRequestId(invoice);

const requestData = invoiceFromRequestID.getData(); 

console.log(requestData);

/* { 
  requestId,
  currency,
  expectedAmount,
  payee,
  payer,
  timestamp,
  extensions,
  version,
  events,
  state,
  creator,
  meta,
  balance,
  contentData,
} */
```

We can then use this object to check various fields of the request like `expectedAmount`, `balance`, `payer`, `payee`, and metadata attached to the request. 

#### Accepting / cancelling an invoice information

After the invoice has been created, the customer can accept the invoice. First you must create a signature as per [https://github.com/RequestNetwork/requestNetwork/blob/development/packages/types/src/signature-types.ts\#L2](https://github.com/RequestNetwork/requestNetwork/blob/development/packages/types/src/signature-types.ts#L2) - after this you can accept, decline or change the expected amount of the invoice

```javascript
//Accept
await request.accept(signatureInfo);

//Cancel
await request.cancel(signatureInfo);

//Increase the expected amount
await request.decreaseExpectedAmountRequest(amount, signatureInfo);

//Decrease the expected amount
await request.increaseExpectedAmountRequest(amount, signatureInfo);
```

Don't forget, we can then call `fromRequestId` to get the updated invoice information. 

### Signing transactions

Transactions are signed through Signature Providers. Today, the providers available for use are:

* [Web3 Signature Provider](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/web3-signature), compatible with metamask
* [Ethereum Private Key Signature Provider](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/epk-signature), using directly the private keys
