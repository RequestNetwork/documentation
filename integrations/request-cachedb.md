---
description: >-
  CacheDB is a useful tool developed by Request which allows dApp developers to
  find and filter Requests and associated IPFS data from the blockchain quickly.
---

# Request CacheDB

Previously, developers would have to pragmatically find the request directly on the blockchain and then have a second lookup for any associated IPFS data - the load times can have a negative impact on the user's experience of your apps.

With CacheDB we parse the blockchain in real-time and store the data in a fast and easy to query database. dApps can now query this database in for a much-improved user experience.

CacheDB also allows us to integrate more payment methods \(like mobile wallets, MEW etc\) as we can now accurately detect when these Requests have been paid.

There is also a rinkeby version available at: [https://rinkeby.requestnetworkapi.com/](https://rinkeby.requestnetworkapi.com/) - all the API endpoints below are the same.

{% api-method method="get" host="https://mainnet.requestnetworkapi.com" path="/requests" %}
{% api-method-summary %}
Get all Requests
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get a list of all requests 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="cbUUID" type="string" required=false %}
Filters requests by the cbUUID stored on IPFS - this is useful to detect payments with mobile wallets, MEW etc.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="string" required=false %}
Used for pagination. Value between 1 and 500 \(default 50\)
{% endapi-method-parameter %}

{% api-method-parameter name="endBlock" type="string" required=false %}
End block to select the requests from
{% endapi-method-parameter %}

{% api-method-parameter name="startBlock" type="number" required=false %}
Start block to select the requests from
{% endapi-method-parameter %}

{% api-method-parameter name="builderId" type="string" required=false %}
Filters requests by the builderId
{% endapi-method-parameter %}

{% api-method-parameter name="requestId" type="string" required=false %}
Retrieves a request by it's ID
{% endapi-method-parameter %}

{% api-method-parameter name="payer" type="string" required=false %}
Filters requests for a given payer's address
{% endapi-method-parameter %}

{% api-method-parameter name="page" type="number" required=false %}
The API will do its best to find a cake matching the provided recipe.
{% endapi-method-parameter %}

{% api-method-parameter name="payee" type="string" %}
Filters requests for a given payee's address
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{  
   [  
      "addresses":[  
         "0x8f222321b87df5d879cebd61e58d5a4cb01cf117",
         "0x3038045cd883abff0c6eea4b1954843c0fa5a735"
      ],
      "_id":"5bfb68e207f694a150ebbc43",
      "_requestId":"0xdb600fda54568a35b78565b5257125bebc51eb2700000000000000000000023e",
      "_transactionId":"0x3e858dd7f54d37879691caa6df1bab5103bbe89c7f963c3d14b06d66f3117360",
      "balance":"5079000000000000",
      "blockNumber":6606165,
      "builderId":"RequestDonations",
      "cbUUID":"undefined",
      "creator":"0x798367b35c3ecd8130c74d9687207e20808de2f6",
      "expectedAmount":"5079000000000000",
      "from":"0x8f222321b87df5d879cebd61e58d5a4cb01cf117",
      "gas":"258359",
      "gasPrice":"2600000000",
      "input":"0x9dbdd85c00000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000018000000000000000000000000000000000000000000000000000000000000001c0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000166c0b8a9f20000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000008c798367b35c3ecd8130c74d9687207e20808de2f6000000000000000000000000000000000000000001798367b35c3ecd8130c74d9687207e20808de2f600000000000000000000000000000000000000000000000000120b52d6d070002e516d503462767962676265446b527759566448535548626b706f76547064475162776e4d613352313379614e6e67000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000004cdc86fa95ec2704f0849825f1f8b077deed8d39000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000120b52d6d0700000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000041773b6ad97c19286b6abaeacba540de6e6ced85ad513b7928aff1a152f1acd05e2d271bae4ca49d27d11e61f57c8b93ab7a1fdff5a9b730ebcfeeafc837ed14ae1b00000000000000000000000000000000000000000000000000000000000000",
      "ipfsHash":"QmP4bvybgbeDkRwYVdHSUHbkpovTpdGQbwnMa3R13yaNng",
      "method":"broadcastSignedRequestAsPayer",
      "nonce":20,
      "payee":"0x798367b35c3ecd8130c74d9687207e20808de2f6",
      "payer":"0x8f222321b87df5d879cebd61e58d5a4cb01cf117",
      "reason":"Donation to https://www.coingecko.com",
      "state":"1",
      "subPayees":"",
      "timeStamp":"1540828253",
      "to":"0x3038045cd883abff0c6eea4b1954843c0fa5a735",
      "value":"5084079000000000",
      "id":"5bfb68e207f694a150ebbc43"
   ],
   "total":35,
   "limit":25,
   "page":2,
   "pages":2
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a cake matching this query.
{% endapi-method-response-example-description %}

```javascript
{
    "docs": [],
    "total": 0,
    "limit": 25,
    "page": 1,
    "pages": 1
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://mainnet.requestnetworkapi.com" path="/requests/:requestId" %}
{% api-method-summary %}
Get Request by ID
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to retrieve a single request based on it's ID 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{  
   "addresses":[  
      "0x025a9af6800444559cb9b8d75f9d7b43e0860408",
      "0x3038045cd883abff0c6eea4b1954843c0fa5a735"
   ],
   "_id":"5bfb6db407f694a150ede9e6",
   "_requestId":"0xdb600fda54568a35b78565b5257125bebc51eb2700000000000000000000023c",
   "_transactionId":"0xddb97ecd301214faebf953626c9b0c52eb9f2e5f9004b4adebeac9c69dc41d78",
   "balance":"50850000000000000",
   "blockNumber":6605437,
   "builderId":"RequestDonations",
   "cbUUID":"undefined",
   "creator":"0x798367b35c3ecd8130c74d9687207e20808de2f6",
   "expectedAmount":"50850000000000000",
   "from":"0x025a9af6800444559cb9b8d75f9d7b43e0860408",
   "gas":"258225",
   "gasPrice":"7100000000",
   "input":"0x9dbdd85c00000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000018000000000000000000000000000000000000000000000000000000000000001c0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000166c0237b3c0000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000008c798367b35c3ecd8130c74d9687207e20808de2f6000000000000000000000000000000000000000001798367b35c3ecd8130c74d9687207e20808de2f600000000000000000000000000000000000000000000000000b4a7ce3ad420002e516d55417a434a476d4c5573735a635a7a366f38445434376a4d504c764b67667148774551626e4b7a4b714d5550000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000004cdc86fa95ec2704f0849825f1f8b077deed8d39000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000b4a7ce3ad4200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000041308ee9000a65844300fa9b37a2a278c4ea1f48a3b5dac6761385e11c9f657f800503781538c86b2e793e0b26aeaf4bcbb081bd711e7a9f57ac3db9a4d8fa379d1c00000000000000000000000000000000000000000000000000000000000000",
   "ipfsData":{  
      "meta":{  
         "format":{  
            "const":"rnf_invoice"
         },
         "version":{  
            "const":"0.0.2"
         }
      },
      "creationDate":"2018-10-29T13:04:02.748Z",
      "invoiceNumber":"c063e167-f9e9-46cb-932e-86cb855d4425",
      "purchaseOrderId":"fa59e24c-6b8d-40fb-95b8-72c998aebbe5",
      "note":"Donation to https://www.coingecko.com",
      "miscellaneous":{  
         "builderId":"RequestDonations"
      },
      "invoiceItems":"[object Object]"
   },
   "ipfsHash":"QmUAzCJGmLUssZcZz6o8DT47jMPLvKgfqHwEQbnKzKqMUP",
   "method":"broadcastSignedRequestAsPayer",
   "nonce":74,
   "payee":"0x798367b35c3ecd8130c74d9687207e20808de2f6",
   "payer":"0x025a9af6800444559cb9b8d75f9d7b43e0860408",
   "reason":"Donation to https://www.coingecko.com",
   "state":"1",
   "subPayees":"",
   "timeStamp":"1540818289",
   "to":"0x3038045cd883abff0c6eea4b1954843c0fa5a735",
   "value":"50900850000000000",
   "id":"5bfb6db407f694a150ede9e6"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "message": "Request not found"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://mainnet.requestnetworkapi.com" path="/transactions" %}
{% api-method-summary %}
Get all transactions
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to retrieve all the transactions associated to an address \(used to either send or receive\) 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
Either the to or the from address used. 
{% endapi-method-parameter %}

{% api-method-parameter name="endBlock" type="number" required=false %}
End block to select the transactions from
{% endapi-method-parameter %}

{% api-method-parameter name="startBlock" type="number" required=false %}
Start block to select the transactions from
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Used for pagination. Value between 1 and 500 \(default 50\)
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{  
   "docs":[  
      {  
         "operations":[  

         ],
         "contract":null,
         "_id":"0x9b0c3e9bd9a8d200f7f8a422d82ddbf2d8001722d85bf7f2d043059e052a32e0",
         "blockNumber":3060468,
         "timeStamp":"1538033936",
         "nonce":7,
         "from":"0x1cad56fc63830fa0a225daf8d5072496e1990efd",
         "to":"0xd88ab9b1691340e04a5bbf78529c11d592d35f57",
         "value":"116291175000000000",
         "gas":"258452",
         "gasPrice":"9900000000",
         "gasUsed":"244299",
         "input":"0x9dbdd85c00000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000018000000000000000000000000000000000000000000000000000000000000001c00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000001661a2df13a0000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000008c2027c7fd9e48c028e7a927a6def44f4b2e52c7030000000000000000000000000000000000000000012027c7fd9e48c028e7a927a6def44f4b2e52c703000000000000000000000000000000000000000000000000019cbc8c06c7f0002e516d643462654b68506b6a313945785a4e62774d57624d4236664d59464e684a34766b455a50363961416f597252000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000006e347bf41ee62109a76b29e859a58d2bd3374e880000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000019cbc8c06c7f00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000041ca18ca0c9c931c3b7e68bcd310ca1606d6b2315d5d46672ef02989c7f302d16b771d8ee0ae8aadc1040de5e456235b862cab932ce4c1a233ab020c76931a3edc1c00000000000000000000000000000000000000000000000000000000000000",
         "error":"",
         "id":"0x9b0c3e9bd9a8d200f7f8a422d82ddbf2d8001722d85bf7f2d043059e052a32e0"
      },
      {  
         "operations":[  

         ],
         "contract":null,
         "_id":"0xa5f47f2369ea9ee780f6aff630153774eafdda9cbbc06087cf6ea8a1f2e2a634",
         "blockNumber":3055159,
         "timeStamp":"1537954205",
         "nonce":37,
         "from":"0x6e347bf41ee62109a76b29e859a58d2bd3374e88",
         "to":"0x1cad56fc63830fa0a225daf8d5072496e1990efd",
         "value":"100000000000000000",
         "gas":"21000",
         "gasPrice":"1000000000",
         "gasUsed":"21000",
         "input":"0x",
         "error":"",
         "id":"0xa5f47f2369ea9ee780f6aff630153774eafdda9cbbc06087cf6ea8a1f2e2a634"
      },
      {...},
   ],
   "total":17,
   "limit":25,
   "page":1,
   "pages":1
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "docs": [],
    "total": 0,
    "limit": 25,
    "page": 1,
    "pages": 1
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://mainnet.requestnetworkapi.com" path="/transactions/:transactionId" %}
{% api-method-summary %}
Get transaction by transaction ID
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to find information about a transaction based on the TXID
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "addresses": [
        "0x1cad56fc63830fa0a225daf8d5072496e1990efd",
        "0xd88ab9b1691340e04a5bbf78529c11d592d35f57"
    ],
    "operations": [],
    "contract": null,
    "_id": "0x9b0c3e9bd9a8d200f7f8a422d82ddbf2d8001722d85bf7f2d043059e052a32e0",
    "blockNumber": 3060468,
    "timeStamp": "1538033936",
    "nonce": 7,
    "from": "0x1cad56fc63830fa0a225daf8d5072496e1990efd",
    "to": "0xd88ab9b1691340e04a5bbf78529c11d592d35f57",
    "value": "116291175000000000",
    "gas": "258452",
    "gasPrice": "9900000000",
    "gasUsed": "244299",
    "input": "0x9dbdd85c00000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000018000000000000000000000000000000000000000000000000000000000000001c00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000001661a2df13a0000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000008c2027c7fd9e48c028e7a927a6def44f4b2e52c7030000000000000000000000000000000000000000012027c7fd9e48c028e7a927a6def44f4b2e52c703000000000000000000000000000000000000000000000000019cbc8c06c7f0002e516d643462654b68506b6a313945785a4e62774d57624d4236664d59464e684a34766b455a50363961416f597252000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000006e347bf41ee62109a76b29e859a58d2bd3374e880000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000019cbc8c06c7f00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000041ca18ca0c9c931c3b7e68bcd310ca1606d6b2315d5d46672ef02989c7f302d16b771d8ee0ae8aadc1040de5e456235b862cab932ce4c1a233ab020c76931a3edc1c00000000000000000000000000000000000000000000000000000000000000",
    "error": "",
    "id": "0x9b0c3e9bd9a8d200f7f8a422d82ddbf2d8001722d85bf7f2d043059e052a32e0"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "message": "transaction ID not found"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

There are also endpoints available for push notifications, list of all tokens on the blockchain and prices based on [CMC](https://coinmarketcap.com/) which is used for the mobile wallet - if you need access to these endpoints you can reach out on [Reddit](https://www.reddit.com/user/AdmREQ/), [Discord ](https://discordapp.com/invite/6aGhs6v)or [Telegram](https://t.me/REQ_Official). 

## Use cas**e**s

### Find Requests based on a users address

The most basic use-case for CacheDB is to find any associated Requests by a users wallet address, as an example if we wanted to see all Requests paid by 0x1CAD56fc63830fa0a225daf8d5072496e1990efd we can use the following endpoint: [https://rinkeby.requestnetworkapi.com/requests?payer=0x1CAD56fc63830fa0a225daf8d5072496e1990efd](https://rinkeby.requestnetworkapi.com/requests?payer=0x1CAD56fc63830fa0a225daf8d5072496e1990efd)

Similarly, we can use the 'payee' endpoint to see all the Requests received by that address.  

### Finding requests by dApp

Each dApp should conform to the [Request Network Data Format](https://github.com/RequestNetwork/requestNetwork-private/tree/master/packages/requestNetworkDataFormat) - using this format each Request is assigned a 'builderId', using CacheDB we can filter Requests by this builderID to see information on all of the Requests they have made. 

For example, we can see all the Requests made on Rinkeby by the builder 'RequestDonations' here: [https://rinkeby.requestnetworkapi.com/requests?builderId=RequestDonations](https://rinkeby.requestnetworkapi.com/requests?builderId=RequestDonations)

Using this data we can create tracking tools to accurately track Requests and filter them in various ways. 

### Mobile wallet / misc wallet payment detection

1\) UUID is generated by the dApp - signed request is created, IPFS data contains a field \(cbUUID\) with value that has been generated by the app like so: [https://ipfs.io/ipfs/Qme3auPnQoZagUMEx3XtYha59MUkuRpqJWRyDWZxuLByrA](https://ipfs.io/ipfs/Qme3auPnQoZagUMEx3XtYha59MUkuRpqJWRyDWZxuLByrA) 

2\) Payment is made using any wallet.  

3\) CacheDB picks up the new block and parses the Request, it checks IPFS data for various fields like builderID, cbUUID etc. Stored like so: [https://rinkeby.requestnetworkapi.com/requests?cbUUID=42bb4d75-3afe-4aa1-8109-1c2d001e6cf3](https://rinkeby.requestnetworkapi.com/requests?cbUUID=42bb4d75-3afe-4aa1-8109-1c2d001e6cf3) 

4\) From the dApp we can long-poll the URL [https://rinkeby.requestnetworkapi.com/requests?cbUUID=\[generatedUUID](https://rinkeby.requestnetworkapi.com/requests?cbUUID=[generatedUUID)\] in this instance it would be: [https://rinkeby.requestnetworkapi.com/requests?cbUUID=42bb4d75-3afe-4aa1-8109-1c2d001e6cf3](https://rinkeby.requestnetworkapi.com/requests?cbUUID=42bb4d75-3afe-4aa1-8109-1c2d001e6cf3) 

5\) Transaction is then detected on CacheDB and we now have all the transactions information we need to then process the payment in the app



