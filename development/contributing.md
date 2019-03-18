# Contributing

## Monorepo

The[ repository](https://github.com/RequestNetwork/requestNetwork) of the protocol is a monorepo, using [yarn workspaces](https://yarnpkg.com/lang/en/docs/workspaces/) and [lerna](https://github.com/lerna/lerna). It holds every packages composing the implementation of the protocol. This implementation is done in typescript.

## Architecture

The Protocol is designed in layers:

![](https://lh5.googleusercontent.com/o9AlH3RaDP4xAzPVzmo6WJnSKm7NfykmoI_cL78sYLof5pH-Hfpm0M6bkREWcGLKY9mFxbs2UHyyoLaQEy4sTVRK9ml3tSF9nMzu4lhIyE-_a9WM8TMeZO5FSQLlTcte6MwLnpIj)

Items in light green are done in v2.0-alpha. Items in dark green are items under development.

* [Storage](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/ethereum-storage): Stores the data about requests on IPFS and Ethereum
* [Data Access](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/data-access): Indexes transactions and will eventually batch them
* [Transaction](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/transaction-manager): Creates transactions and will eventually handle the encryption
* [Request Logic](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-logic): Business logic: properties and actions of requests
* [Advanced Logic](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/advanced-logic): Hosts the extensions to the protocol
* [Content Data](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/data-format): Hosts the standards for the data of the Request Protocol
* ETH/[BTC](https://github.com/RequestNetwork/requestNetwork/blob/development/packages/advanced-logic/specs/payment-network-btc-address-based-0.1.0-DRAFT.md)/Fiat Payment: Detection of payments
* Smart Requests: Smart requests, in addition to documenting transactions, have control over them. It enables extensions such as continuous payments, cross-currency, escrow, ICO
* [Request Node](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-node): Web server that accepts transactions and saves them in IPFS and Ethereum. They aim at making easy to use the Request Storage.
* [Request Client js](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-client.js): A web and node.js library to connect to Request Nodes, meant to simplify the use of the protocol.
* Request Portal: Request Portals exist on top of Request Nodes and libraries. They provide an even easier usage of the protocol in exchange for a fee. It is not necessary to go through the portals to use the Request protocol

## How to...

### Contribute

Check the contribution guidelines: [https://github.com/RequestNetwork/requestNetwork/blob/development/CONTRIBUTING.md](https://github.com/RequestNetwork/requestNetwork/blob/development/CONTRIBUTING.md)

### Report a problem

* Create an issue[ https://github.com/RequestNetwork/requestNetwork/issues](https://github.com/RequestNetwork/requestNetwork/issues)
* Join the[ requesthub on slack](https://join.slack.com/t/requesthub/shared_invite/enQtMjkwNDQwMzUwMjI3LWNlYTlmODViMmE3MzY0MWFiMTUzYmNiMWEyZmNiNWZhMjM3MTEzN2JkZTMxN2FhN2NmODFkNmU5MDBmOTUwMjA)

### Build all packages

1. `git clone https://github.com/RequestNetwork/requestNetwork.git`
2. `yarn run build`

Sometimes it helps to run `yarn run build:tsc` to fix typescript build problems.

### Lint all packages

1. `git clone https://github.com/RequestNetwork/requestNetwork.git`
2. `yarn run lint`
3. `yarn run packageJsonLint`

### Run all the tests

1. `git clone https://github.com/RequestNetwork/requestNetwork.git`
2. Console 1
   1. Install and setup an IPFS node
   2. `ipfs daemon`
3. Console 2
   1. `cd packages/ethereum-storage`
   2. `yarn run ganache`
4. Console 3
   1. `cd packages/ethereum-storage`
   2. `yarn run deploy`
   3. `cd ../`
   4. `yarn run test`

## CircleCI Build

We use CircleCI to build the projects of the monorepo: [https://circleci.com/gh/RequestNetwork/requestNetwork](https://circleci.com/gh/RequestNetwork/requestNetwork)

## Readings

Good readings for anyone interested in developing on the protocol:

* [Whitepaper](https://request.network/assets/pdf/request_whitepaper.pdf)
* [Yellow paper](https://request.network/assets/pdf/request_yellowpaper_smart_audits.pdf)
* [BD Mindmap](https://www.mindmeister.com/1015399217?t=K66qE27OV5)
* [Ecosystem Mindmap](https://www.mindmeister.com/991002501?t=R1iofDilV0)
* [Why use Request?](https://blog.request.network/why-use-request-b28c3e788261)
* [FAQ blog post](https://blog.request.network/colossuss-frequently-asked-questions-faq-c086231b88fa)
* [Types of Request & Financial flows management — Request types](https://blog.request.network/request-network-project-update-december-8th-2017-financial-flows-management-request-colossus-ef62fed295c0)
* [The invoicing and reputation system](https://blog.request.network/the-invoicing-reputation-system-by-request-network-977831469cdc)

