# Deploy a Request Node

## Host a Request Node

Request Nodes are HTTP servers used to allow any client to communicate with the Request Protocol, these servers abstract the complexity of IPFS and Ethereum for the users. The users can easily create a request or execute an action on a request by sending messages to the Node via HTTP.

More details on the request nodes can be found on [github](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-node).

### Deploying your own node

Follow the steps on the [request-node package readme](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-node#deployment).

### Deploying a node for local development environment

You may want to deploy your own node in a development environment. This is how:

* `git clone https://github.com/RequestNetwork/requestNetwork.git`
* Console 1: run IPFS
  * Install and setup an IPFS node
  * `ipfs daemon`
* Console 2: run ganache
  * `cd packages/ethereum-storage`
  * `yarn run ganache`
* Console 3: deploy the contracts and run the node
  * `cd packages/ethereum-storage`
  * `yarn run deploy`
  * `cd ../request-node`
  * `yarn run start`

### Signing transactions

Transactions are signed through Signature Providers. Today, the providers available for use are:

* [Web3 Signature Provider](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/web3-signature), compatible with metamask
* [Ethereum Private Key Signature Provider](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/epk-signature), using directly the private keys

