# Create requests with a Request node

## Using the request-client.js library

Use `@requestnetwork/request-client.js` to connect to a node and create requests.

More information is available on [github](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-client.js).

You can also [follow this example](https://github.com/RequestNetwork/requestNetwork/blob/development/packages/integration-test/test/node-client.test.ts).

## Using an already deployed node

If you have access to an already deployed node, connect the library to it [like explained here](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-client.js#configure-which-request-node-to-use).

## Deploying your own node

You may want to deploy your own node, for example in a development environment. This is how:

* `git clone https://github.com/RequestNetwork/requestNetwork.git`
* Console 1
  * Install and setup an IPFS node
  * `ipfs daemon`
* Console 2
  * `cd packages/ethereum-storage`
  * `yarn run ganache`
* Console 3
  * `cd packages/ethereum-storage`
  * `yarn run deploy`
  * `cd ../request-node`
  * `yarn run start`

## Signing transactions

Transactions are signed through Signature Providers. Today, the providers available for use are:

* [Web3 Signature Provider](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/web3-signature), compatible with metamask
* [Ethereum Private Key Signature Provider](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/epk-signature), using directly the private keys



