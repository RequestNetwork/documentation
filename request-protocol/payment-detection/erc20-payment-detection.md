---
description: >-
  This section describe how the payments and the refunds detection are made
  using ERC20 as currency of a request.
---

# ERC20 \(DAI, REQ etc.. \)

Two different payment networks can be used to detect ERC20 requests: ERC20 proxy contract and ERC20 address based payment networks.

### Proxy contract

The balance of an ERC20 request can be computed automatically.  

The payment network "**pn-erc20-proxy-contract**" must be given when creating an ERC20 request. Payment and refund addresses can be reused for several requests.

```javascript
const requestParameters = {
  currency: 'REQ', 
  expectedAmount: '1000000000000000000',
  payee: {
    type: IdentityTypes.TYPE.ETHEREUM_ADDRESS,
    value: '0x627306090abab3a6e1400e9345bc60c78a8bef57',
  },
};
const paymentNetwork = {
   id: RequestNetwork.Types.PAYMENT_NETWORK_ID.ERC20_PROXY_CONTRACT,
   parameters: {
      paymentAddress: '0xf17f52151EbEF6C7334FAD080c5704D77216b732',
      
   }
};
const invoice = await requestNetwork.createRequest({
  requestParameters,
  signer: requestParameters.payee,
  paymentNetwork
});
```

Payments for Request using ERC20 proxy contract payment network are documented in an Ethereum smart contract: ERC20Proxy. This smart contract is available on Ethereum [mainnet](https://etherscan.io/address/0x5f821c20947ff9be22e823edc5b3c709b33121b3) and [Rinkeby testnet](https://rinkeby.etherscan.io/address/0x162edb802fae75b9ee4288345735008ba51a4ec9).

The user also has to provide a payment reference to link the payments to the request. The payment reference is the last 8 bytes of a salted hash of the requestId: `last8Bytes(hash(lowercase(requestId + salt + address)))`.

Any payments documented in the ERC20 proxy contract with the correct reference is considered as a payment for the request.

### Address based

The balance of an ERC20 request can be computed automatically.  

First, one address for the payment and one for the refund must be created and used exclusively for **one and only one** request.

Then, the payment network "**pn-erc20-address-based**" must be given when creating an ERC20 request:

```javascript
const requestParameters = {
  currency: 'REQ', 
  expectedAmount: '1000000000000000000',
  payee: {
    type: IdentityTypes.TYPE.ETHEREUM_ADDRESS,
    value: '0x627306090abab3a6e1400e9345bc60c78a8bef57',
  },
};
const paymentNetwork = {
   id: RequestNetwork.Types.PAYMENT_NETWORK_ID.ERC20_ADDRESS_BASED,
   parameters: {
      paymentAddress: '0xf17f52151EbEF6C7334FAD080c5704D77216b732',
      
   }
};
const invoice = await requestNetwork.createRequest({
  requestParameters,
  signer: requestParameters.payee,
  paymentNetwork
});
```

Any ERC20 transaction for the request currency reaching the payment address given \(here `REQ` and `0xf17f52151EbEF6C7334FAD080c5704D77216b732`\) is considered as a payment. 

In the same way, the payer can add a refund address. For example when accepting the request:

```typescript
await request.accept(payerIdentity, {
    refundAddress: '0xC5fdf4076b8F3A5357c5E395ab970B5B54098Fef',
});
```