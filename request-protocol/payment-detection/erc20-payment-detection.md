---
description: >-
  This section describe how the payments and the refunds detection are made
  using ERC20 as currency of a request.
---

# ERC20 \(DAI, REQ etc.. \)

The balance of an ERC20 Request can be computed automatically.  

First, One address for the payment and one for the refund must be created and used exclusively for **one and only one** request.

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



