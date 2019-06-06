# Introduction

Request is a decentralized network built on top of Ethereum, which allows anyone, anywhere to request, validate and execute payment requests.

The strength of this open network relies on the ecosystem of builders who transform the potential of Request into innovation, in the hands of final users.

This documentation explains what Request does, how it works and how developers can build with it. Getting started with Request is incredibly simple, in just a few lines you can begin creating, finding and updating invoices using the blockchain. 

Contribution on this documentation is welcome!

## Overview

The development is organized like this:

* The protocol is specified [here](dev/technical-documentation.md). It consists of five layers: storage, data access, transaction, logic and advanced logic.
* The layers are implemented in Typescript in [this repository](https://github.com/RequestNetwork/requestNetwork). The protocol V1 is being deprecated, the V2 is currently in beta
* The [Request Node](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-node) relies on the two bottom layers.
* The [Request Client](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-client.js) relies on the three top layers.

