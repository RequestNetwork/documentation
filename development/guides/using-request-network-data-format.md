# Using Request Network Data Format

### Introduction

Because Request Network Protocol want to provide a full set of tools for the commerce, we provides also standard for the request data. 

On [Request Network github](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/requestNetworkDataFormat), you can find a set of data format as Json Schema library that will provide such standards. It also provides a Javascript library to play with.

### Example of use with the library

```javascript
const RequestNetworkDataFormat = require('requestnetwork-data-format');
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

const verySimpleData = {
  "meta": {
    "format": "rnf_invoice",
    "version": "0.0.1"
  },
  "creationDate": "2018-01-01T18:25:43.511Z",
  "invoiceNumber": "123456789",
  "note": "this is a very simple example of invoice",
  "invoiceItems": [{
      "name": "Candlestick",
      "reference": "cs666",
      "quantity": 2,
      "unitPrice": 0.01,
      "discount": 0.002,
      "taxPercent": 16.9,
      "deliveryDate": "2019-01-01T18:25:43.511Z"
    }]
}


let result = RequestNetworkDataFormat.validate(verySimpleDataformated);

if (result.valid) {
    const { request } = await requestNetwork.createRequest(
      RequestNetwork.Types.Role.Payee,
      RequestNetwork.Types.Currency.ETH,
      payeesInfo,
      payerInfo,
      {data:JSON.stringify(dataformated)}
    );
} else {
    // use the errors from result.errors
    console.error(result.errors)
}
```

### List of standards

You can find the list of standard provided [here](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/requestNetworkDataFormat).

