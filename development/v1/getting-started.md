# Getting started

## Monorepo

The [repository](https://github.com/RequestNetwork/requestNetwork) of the protocol is a monorepo, using [lerna](https://github.com/lerna/lerna). Familiarize yourself with lerna's main commands \(bootstrap, add, publish\).

## Packages

The monorepo holds these packages:

* requestNetworkSmartContracts: the smart contracts of the protocol
* requestNetworkArtifacts: the artifacts of the smart contracts. Public npm package
* requestNetwork.js: the javascript wrapper for the smart contracts. Public npm package

The links are:

* requestNetwork.js depends on requestNetworkArtifacts
* The artifacts are created using the script exportArtifacts.js of requestNetworkSmartContracts and copied to requestNetworkArtifacts

## How to...

### Report an problem

* Create an issue [https://github.com/RequestNetwork/requestNetwork/issues](https://github.com/RequestNetwork/requestNetwork/issues)
* Join the [requesthub on slack](https://join.slack.com/t/requesthub/shared_invite/enQtMjkwNDQwMzUwMjI3LWNlYTlmODViMmE3MzY0MWFiMTUzYmNiMWEyZmNiNWZhMjM3MTEzN2JkZTMxN2FhN2NmODFkNmU5MDBmOTUwMjA)

## OK, but how to...

### Run the smart contract tests

1. npm install -g lerna
2. lerna bootstrap
3. npm install -g ganache-cli
4. \(from the smart contract directory\) npm run ganache
5. \(from the smart contract directory\) npm run test

### Run the javascript library tests

1. npm install -g lerna
2. lerna bootstrap
3. npm install -g ganache-cli
4. \(from the js lib directory\) npm run ganache \(keep open\)
5. \(from the js lib directory\) ipfs daemon \(keep open\)
6. \(from the js lib directory\) npm run testdeploy
7. \(from the js lib directory\) npm run test

## Build

We use Travis-CI to build the projects of the monorepo: [https://travis-ci.org/RequestNetwork/requestNetwork](https://travis-ci.org/RequestNetwork/requestNetwork)

## Readings

Good readings for anyone interested in developing on the protocol:

* [Whitepaper](https://request.network/assets/pdf/request_whitepaper.pdf)
* [Yellow paper](https://request.network/assets/pdf/request_yellowpaper_smart_audits.pdf)
* [BD Mindmap](https://www.mindmeister.com/1015399217?t=K66qE27OV5)
* [Ecosystem Mindmap](https://www.mindmeister.com/991002501?t=R1iofDilV0)
* [Why use Request?](https://blog.request.network/why-use-request-b28c3e788261)
* [FAQ blog post](https://blog.request.network/colossuss-frequently-asked-questions-faq-c086231b88fa)
* [Types de Request & Financial flows management — Request types](https://blog.request.network/request-network-project-update-december-8th-2017-financial-flows-management-request-colossus-ef62fed295c0)
* [The invoicing and reputation system](https://blog.request.network/the-invoicing-reputation-system-by-request-network-977831469cdc)



