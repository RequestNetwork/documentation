# How the Request Protocol works

## What is Request

Request is a network of financial transaction. In other words, a database of invoices based on a distributed ledger.

## What you can do with it

You can create requests for payments. Picture a request as an invoice. You can create requests to invoice clients and track payments.This is the simplest use case but there are more things you can do with Request.

## How it works

### Transactions

A request is an object containing the information of the invoice \(such as payer, payee, payable amount\) that can be modified by actions under specific conditions \(such as accept, cancel, increase/reduce amount\)  


Each action is a transaction on the Request Network. A request can therefore be seen as a collection of transactions that put together, make up the state of the request.

### Signed and encrypted

Transaction are signed by their creator, to prove this person made the transaction. They are encrypted with the keys of each stakeholder for privacy.  


More information on the encryption will be made available later.

### Transactions saved on IPFS

Transactions are batched together in blocks. The blocks are saved on IPFS.

### IPFS hash on Ethereum

The IPFS hashes are saved on an Ethereum smart contract, for immutability and proof. This contract serves as base for the Request Network and is what makes it trustless.

### Payment detection

A request stores the information \(invoice\) and links it to the payment. For this, we need a way to identify a transaction before it happens

#### The actual financial transaction happens in the currency's payment network

Request does not transfer money, it detects payments. The money goes directly from the sender to the receiver. For transfer of cryptocurrencies, the transfers happen in the cryptocurrency’s blockchain.  


In the case of ETH, the transaction is identified with tx data. Or more complex solutions like a proxy contract.

In the case of BTC, the transaction is identified with a payment address

#### How do we know the balance of a request?

On the Request blockchain, the requests have a status: created, accepted, canceled. To know the balance, we check the target blockchain.  


With the javascript library, an app can get the details of a request and its payments. To make it easier and faster, that's the role of the portals.

### Request fees

In addition to the gas cost of sending a transaction on Ethereum, the Request Network charges a tiny fee for usage. The fee to pay for usage is relative to the size of the data. A minimum fee of 0.10 USD is needed per block of transactions, for up to 10kB \(approximate size of a request\) and 0.03 USD will be asked per additional 10kB. Pooling transactions together reduces the average cost.

### How to use the Request protocol

Interacting with IPFS and Ethereum through the libraries

One way to create requests is to:

1. Create a request creation transaction and sign it
2. Put it in a block on IPFS
3. Put the IPFS hash on Ethereum and pay the fees

This is the most decentralized way to use Request. Anybody can do it, and we provide javascript libraries to do it.

See [Create requests directly on IPFS](create-requests-directly-on-ipfs.md) for more details.

### Through Request Nodes

An easier but less decentralized way to do it is through the nodes. Nodes are HTTP servers to which you can send transactions and will put it for you on IPFS and Ethereum. You don’t need to handle the Ethereum gas cost and Request fees because the nodes pay them, but node charge its own fees to be sustainable. Nodes are also useful for batching transactions and saving on costs. Even when using nodes to send the transactions on the Request Network, the proof of creation of transaction is still present, as the transactions are signed by the creator.

See [Create requests with a Request node](create-requests-with-a-request-node.md) for more details.

### Portal

The last possibility for using Request is through Portals. Portals cache the request data and offer a high-level API with all the info, including the payment information. They are the most usable way to interact with the request blockchain, at the expense of decentralization.  


More information on Portals will be available later.  


