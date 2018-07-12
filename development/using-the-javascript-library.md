# Using the Javascript Library

## Introduction

Welcome to the requestNetwork.js documentation! requestNetwork.js is a Javascript library for interacting with the Request Network protocol. Using the library you can create new requests from your applications, pay them, consult them or update them from your own off-chain applications.

## Installing

### Using NPM

`npm install @requestnetwork/request-network.js --save`

### Using Yarn

`yarn add @requestnetwork/request-network.js`

\(We are currently working on retrieving the name of requestnetwork.js\)

## Example

```javascript
const RequestNetwork = require('@requestnetwork/request-network.js');
const Web3 = require('Web3');

const web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));
const requestNetwork = new RequestNetwork.default({
  provider: web3.currentProvider,
  ethNetworkId: 10000000000,
  useIpfsPublic: false,
});

const [payeeAddress, payerAddress] = await web3.eth.getAccounts();

// Create a request as payer
const payerInfo = {
  idAddress: payerAddress,
  refundAddress: payerAddress,
};

const payeesInfo = [{
  idAddress: payeeAddress,
  paymentAddress: payeeAddress,
  expectedAmount: web3.utils.toWei('1.5', 'ether'),
}];

const { request } = await requestNetwork.createRequest(
  RequestNetwork.Types.Role.Payee,
  RequestNetwork.Types.Currency.ETH,
  payeesInfo,
  payerInfo,
);

// Pay a request
await request.pay([web3.utils.toWei('1.5', 'ether')], [0], { from: payerAddress });

// The balance is the same amount as the the expected amount: the request is paid
const data = await request.getData();
console.log(data.payee.expectedAmount.toString());
console.log(data.payee.balance.toString());
```

## Importing and Initializing

Using import

```javascript
import RequestNetwork, { Types } from '@requestnetwork/request-network.js';
const requestNetwork = new RequestNetwork({ provider, ethNetworkId });
```

Using require

```javascript
const RequestNetwork = require('@requestnetwork/request-network.js');
const requestNetwork = new RequestNetwork.default({ provider, ethNetworkId });
const Types = RequestNetwork.Types;
```

The parameter for the constructor \(all optional\) are

* provider: Web3.js Provider instance an url as string
* ethNetworkId: the Ethereum network ID \(1: main, 2: morden, 3: ropsen, 4: rinkeby, 42: kovan, other: private\)
* bitoinNetworkId: the Bitcoin network ID
* useIpfsPublic: false to use a private ipfs network

## Features

### Create a Request

```javascript
const { request } = await requestNetwork.createRequest(
    Types.Role.Payee,
    Types.Currency.ETH,
    [{
        idAddress: '0xc157274276a4e59974cca11b590b9447b26a8051',
        paymentAddress: '0xc157274276a4e59974cca11b590b9447b26a8051',
        additional: 5,
        expectedAmount: 100,
    }],
    {
        idAddress: '0x014fcc05c76687456e569561ae9956c0ec0ec223',
        refundAddress: '0x014fcc05c76687456e569561ae9956c0ec0ec223',
    }
);
```

Use the currency parameter to choose the currency of the request. Note that in local and rinkeby, REQ is the only ERC20 available.

### Retrieve a Request from its ID

```javascript
const request = await requestNetwork.fromRequestId(requestId);
```

### Pay a Request

```javascript
await request.pay([web3.utils.toWei('1.5', 'ether')]);
```

For example for Requests in ETH, the amounts are in [wei](http://ethdocs.org/en/latest/ether.html). An amount of 100 will be 100 wei. An amount of 0.1 will be rounded down to 0 wei.

### Create and Pay a Request

Payers can create and pay a request at once:

```javascript
const { request } = await requestNetwork.createRequest(
    Types.Role.Payee,
    Types.Currency.ETH,
    [{
        idAddress: '0xc157274276a4e59974cca11b590b9447b26a8051',
        paymentAddress: '0xc157274276a4e59974cca11b590b9447b26a8051',
        additional: 5,
        expectedAmount: 100,
        amountToPayAtCreation: 100,
    }],
    {
        idAddress: '0x014fcc05c76687456e569561ae9956c0ec0ec223',
        refundAddress: '0x014fcc05c76687456e569561ae9956c0ec0ec223',
    }
);
```

Note the `amountToPayAtCreation` property in the array of payees.

### Get information about a request

Instances of the Request class hold minimal and constant values like the requestId and the currency:

```javascript
request.currency;
request.requestId;
```

Other data of a request \(like payer, payee, balance\) should be retrieved with getData\(\):

```javascript
await request.getData();
```

### Add additionals \(ex: tips\)

```javascript
await request.addAdditionals([web3.utils.toWei('.05', 'ether')]);
```

### Add subtractions \(ex: discounts\)

```javascript
await request.addSubtractions([web3.utils.toWei('.05', 'ether')]);
```

### Refund a Request

```javascript
await request.refund([web3.utils.toWei('1.5', 'ether')]);
```

### Accept a Request

```javascript
await request.accept();
```

### Cancel a Request

```javascript
await request.cancel();
```

### Signed Requests

#### Sign a request

```javascript
const signedRequest = await requestNetwork.createSignedRequest(
    Types.Role.Payee,
    Types.Currency.ETH,
    payeesInfo,
    Date.now() + 3600*1000
);
```

#### Retrieve from a serialized signed request

```javascript
import { SignedRequest } from '@requestnetwork/request-network.js';

const signedRequest = new SignedRequest(signedRequestData);
```

#### Broadcast a signed request

```javascript
const { request } = await requestNetwork.broadcastSignedRequest(signedRequest, payerInfo);
```

The payer is necessary to broadcast the signed request

#### Check a signed request

```javascript
signedRequest.isValid(payerInfo);
```

The payer is necessary to validate the signed request

### Broadcasted events

```javascript
await request.pay([web3.utils.toWei('1.5', 'ether')]).on('broadcasted', () => {});
```

## Accessing the internal services

For advanced use, it is possible to access the internal layers of the library:

```javascript
const requestNetwork = new RequestNetwork();
await requestNetwork.requestEthereumService.accept(requestId);
```

