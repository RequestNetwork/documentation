# Protocol

## Ecosystem Environment

A mindmap was created to gather high level information about the protocol and the use cases, feel free to have a look: [https://mm.tt/991002501?t=R1iofDilV0](https://mm.tt/991002501?t=R1iofDilV0)

## Smart Contract Architecture

There are 3 types of smart contracts within the Request Network:

1. **Core**. The core contracts store the base data of the request. It is as flexible as possible to allow any flow implementation.

   This modular architecture will allow us to upgrade more easily to the next versions.

2. **Currency contracts**. The rules are enforced within the whitelisted currency contracts which rule the currencies \(see Actions and States\). They specify which actions can be done at which moment and detect payments.
3. **Extensions**. Extensions specify business rules that can be developed on top of Core and Currency contracts. For example, an escrow that keeps the money until conditions specified by the rules have been fulfilled.

## Actions, States and Properties of a request

A request has these Properties:

* **Payee**, identity of the main payee
* **SubPayees**, identities of other payees \(used in the case of multiple payees\)
* **Payer**, identity of the payer
* **ExpectedAmount**, the amount to pay per payees
* **Balance**, the amount payed at a given moment per payees
* **State**, see below

A request can be in one of these States:

* **Created**
* **Accepted**
* **Cancelled**

A request supports these Actions:

* **Create** a request
* **Sign** a request. It creates a request offline to send to a payer, but doesn't broadcast it
* **Pay** a request, partially or entirely
* **Refund** a request, partially or entirely
* **Add an additional**. For example, to add tips
* **Add a subtract**. For example, refunds and discounts
* **Accept** a request. It is useful to recognize an invoice which have a later due date. It is not mandatory to accept a request to pay it
* **Cancel** a request

All of these Actions can be done by specific parties at specific States. ![Request diagram](https://i.imgur.com/WBUmhp6.png)

When the payer cancels a request, it is assimilated to a declined invoice. We will use this notion for the reputation system. In addition to the 3 states stored in the blockchain, applications can derive more information about the flow from the properies of a request. A request with a balance equal to the expected amount is considered complete and a Request with a partial payment can be considered in progress.

## ECDSA Request: how to sign a request and let the user broadcast it \(for online payments, Points of Sale...\)

When dealing with B2B invoices, you especially need to broadcast your transaction on the blockchain so that your client can detect it automatically. But when you are at a Point of Sale or online, the vendor doesn’t need to broadcast the invoice on the blockchain. If he would do so, he would have to wait for blockchain confirmation and it would add a gas cost.

The solution to bypass the invoice broadcasting is to work on an ECDSA optimization where the seller encrypts his transaction, provides it to the client who accepts the invoice, pays it and broadcasts it in a single transaction. Most blockchain systems \(including Ethereum and Bitcoin\) use the Elliptic Curve Digital Signature Algorithm \(ECDSA\) for cryptographically signing transactions. We use it to prove that the sender of the transaction had access to a private key and that the transaction has not been changed since it was signed.

## Financial flow: how to handle data to manage invoices, salaries, etc. - _Available soon_

Request can handle every flow of money from a point A to a point B.

Each type of financial flow implies different metadata are stored in the request. For example:

* Standard request 
* Invoice \(can vary depending on the country\)
* Salaries \(can vary depending on the country\)
* Lending

The metadata formats will be developed over time and the [Request default app interface](https://app.request.network/) will display most of them.

More information can be read [on this blog update](https://blog.request.network/request-network-project-update-december-8th-2017-financial-flows-management-request-colossus-ef62fed295c0)

The Request Network Foundation Team will release a first format for invoices metadata during Q1 2018.

## Fees

* A REQ fee is burnt when a request is created
* The amount is 0.1% of the amount of the request and limited to a maximum of ~1.50 USD
* The fee is paid by the broadcaster of the request
* The fee is paid even if the request is declined. It acts as an anti-phishing and anti-spam mechanism
* The fee is paid in ETH
* We use a decentralized exchange to convert and burn the fee

A mechanism to specify several recipients will be implemented inside the request which will allow developers to add a fee when someone creates a request through their application.

## Cross Currency - _Available soon_

Documentation for onchain cross currency will be released when implemented.

With the multiplication of currencies and tokens, people will often be asked to pay in a currency they do not own. To pay a request without owning tokens of the requested currency, there are 2 solutions:

* Convert and pay \(0x project\): the wallet or the bank storing the user's funds could convert the tokens and pay. This is a good use case for 0x as we can interact with offchain systems and select orders to fill.
* Pay with any token and let the request manage \(Kyber Network project\). The user will be able to simply pay the request with the equivalent amount of its own tokens. The request contract will forward the funds to the Kyber Network contract and exchange them transparently to finance the request. This is possible thanks to the Kyber Network decentralized exchange which facilitates instant conversion of crypto-assets with guaranteed liquidity.

Both options are interesting depending on the context and users, developers and wallets will be empowered to choose the solution filling their need the best.

## Identity

### Separation of ID and wallet

In the blockchain world, an address can take on different meanings. The distinction between ID and wallet address is becoming clearer and clearer, notably when looking at [EIP725](https://github.com/ethereum/EIPs/issues/725) and [EIP735](https://github.com/ethereum/EIPs/issues/735).

In Request, a payer can receive requests sent to his ID and is able to pay with another address.

### Register - _Available soon_

The register allows the payer to verify who is asking the money and allows accounting and auditing systems to be automated.

The network is registration agnostic. Anyone can create a registration system on top of the Request protocol and the applications can choose which one to refer to.

The Request Network Foundation Team will work on the support of one registration system.

At the moment, 3 main systems are developing their standards : Civic, uPort and the EIP725. As a reminder of the experience provided by these services, Civic was used by 0x during their token sale and you can experience uPort on the Olympia release of the Gnosis project.

We prefer using EIP725 as it aims to be decentralized and non-profit.

[EIP725](https://github.com/ethereum/EIPs/issues/725) ensures anonymity by allowing one person to create a claim to verify the identity of the user \([EIP735](https://github.com/ethereum/EIPs/issues/735)\).

## Multi-recipient management flows

Request Network is not bound to the current limitations of the financial system and aims to tackle the reality of payments best. When you pay somewhere today, your money is split between different payees. We usually count at least 3 payees: the seller, the payment provider, and the government \(for taxes\). In many cases, this results in having some parties incur a debt with another one at the moment of payment \(company debt to tax office for example\).

Request allows to split the funds right at the moment of payment. Instead of having a unique payee, Request manages an array of payees.

To manage an array of payees, the smart contracts also manages an array of balances and expected amounts per payee. The main payee can do all the actions while the other ones are just observers. Except when it comes to the refund: any payee can refund their own part of the invoice from their ID address or their wallet address.

## Reputation - _Available soon_

Documentation for reputation will be released when implemented.

Reputation is used to help users against phishing and save providers from bad payers.

The network is reputation agnostic. Anyone can create a reputation system on top of the protocol and the applications can choose which one to refer to.

The Request Network Foundation Team is going to support one and make it evolve in the future.

As in every reputation system, it is hard to detect the bad actors which are rating themselves with fake accounts. However circles can be detected, more reputation can be given by those who already have a good reputation and bad reputation can act retroactively to the previous ratings.

