# Payment detection

If a payment network has been given to the request, the payment detection can be done.

From the information provided in payment network, the library will feed the property `balance` of the request with:

* `balance`: the sum of the amount of all payments minus the sum of amount of all refunds
* `events`: all the payments and refunds events with the amount, timestamp etc...

The payment networks available are:

* Detection made programatically:
  * `Types.PAYMENT_NETWORK_ID.BITCOIN_ADDRESS_BASED` \('pn-bitcoin-address-based'\): handle Bitcoin payments associated to a BTC address to the request. Every transaction hitting this address will be consider as a payment. Eventually, the payer can provide a BTC address for the refunds. Note that **the addresses must be used for one and only one request** otherwise one transaction will be considered as a payment for more than one request. \(see [Bitcoin](bitcoin.md)\)
  * `Types.PAYMENT_NETWORK_ID.TESTNET_BITCOIN_ADDRESS_BASED` \('pn-testnet-bitcoin-address-based'\): Same as previous but for the bitcoin testnet. \(for test purpose\) \(see [Bitcoin](bitcoin.md)\)
  * `Types.PAYMENT_NETWORK_ID.ERC20_ADDRESS_BASED` \('pn-erc20-address-based'\): handle ERC20 payments to an unique Ethereum address associated with the request. Every transaction hitting this address will be consider as a payment. Eventually, the payer can provide an unique Ethereum address for the refunds. Note that **the addresses must be used for one and only one request** otherwise one transaction will be considered as a payment for more than one request. \(see [ERC20](erc20-payment-detection.md)\)
* Consensus between the request stakeholders:
  * `Types.PAYMENT_NETWORK_ID.DECLARATIVE` \('pn-any-declarative'\): Can be use with any currency. The payments and refunds are documented by the payer and the payee of the request. It does not ensure payment detection, only a consensus is made between the payer and the payee. \(see [declarative requests](declarative-requests.md)\)



