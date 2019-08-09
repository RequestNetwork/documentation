---
description: 'This API Is currently in Beta, its specification may change in the future.'
---

# Getting Started

## Getting Started with the Request API

The Request API is a REST API that enables you to create requests, list requests, and find a specific request by its ID. Its purpose is to simplify interaction with the Request Protocol, abstracting all Blockchain-related aspects.‌‌

It is currently an alpha version, running on the [Rinkeby](https://www.rinkeby.io/) Ethereum test network. **It should not be used for production yet.** Please check our [roadmap](https://request.network/en/technology#roadmap) to know more about our Protocol release to the Ethereum Mainnet.‌‌

Our API accepts [JSON-encoded](http://www.json.org/) request bodies, returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes and Bearer authentication.‌‌

Before using the API you MUST create an account [here](https://dashboard.request.network/), and retrieve your API Key.‌‌

{% hint style="info" %}
Please note, if you would like to run a completely decentralized version of the Request network you can [deploy your own node](../request-protocol/getting-started-1/deploy-a-request-node.md) and interact with the network using the [Request Client.](../request-protocol/getting-started-1/)​‌
{% endhint %}

## API Specs and Structure <a id="api-specs-and-structure"></a>

You can view the full API spec [here](https://api-docs.request.network/).‌‌

### API Requests <a id="api-requests"></a>

To construct a REST API request, combine these components:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">HTTP method</td>
      <td style="text-align:left">
        <p>&#x200B;</p>
        <p><code>GET</code>. Requests data from a resource.</p>
        <p><code>POST</code>. Submits data to a resource to process.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">API URL</td>
      <td style="text-align:left">
        <p>&#x200B;</p>
        <p>Mainnet. <code>https://api.request.network</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">URI Path</td>
      <td style="text-align:left">The resource to query, submit data to, update, or delete. For example, <code>requests/${requestId}</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x200B;<a href="https://en.wikipedia.org/wiki/Query_string">Query Parameters</a>&#x200B;</td>
      <td
      style="text-align:left">Optional. Controls which data appears in the response. Use to filter,
        limit the size of, and sort the data in an API response.</td>
    </tr>
    <tr>
      <td style="text-align:left">HTTP Request Headers</td>
      <td style="text-align:left">Includes the <code>Authorization</code> header with the access token. See
        the authorization section for more details.</td>
    </tr>
    <tr>
      <td style="text-align:left">JSON request body</td>
      <td style="text-align:left">Required for some endpoints, and details in the specs.</td>
    </tr>
  </tbody>
</table>## Authentication <a id="authentication"></a>

The Request API uses API keys to authenticate requests. You can view and manage your API keys using your [Request Account.](http://baguette-dashboard.request.network/)‌‌

Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.‌‌

All API requests must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). Calls made over plain HTTP will fail. API requests without authentication will also fail.‌‌

### HTTP Status Code Summary <a id="http-status-code-summary"></a>

| Status Code | Summary |
| :--- | :--- |
| `200 - OK` | Everything worked as expected. |
| `401 - Unauthorized` | No valid API key provided. |
| `403 - Forbidden` | You cannot access this resource. |
| `404 - Not Found` | The requested resource doesn't exist. |

‌

## Errors <a id="errors"></a>

The Request API uses conventional HTTP response codes to indicate the success or failure of an API request. In general: The `200` code indicates a successful request. Codes in the `4xx` range indicate an error that failed given the information provided \(e.g., a required parameter was omitted, authentication issue, etc.\).‌‌

## **Pagination** <a id="pagination"></a>

When fetching multiple Requests you will need to utilize cursor-based pagination via the `skip` and `take` parameters. Both parameters take an integer value and return a list of Requests ordered by the Request ID. The `skip` parameter bypasses a specified number of search results. The `take` parameter specifies the number of search results to return. Both of these parameters are optional, the defaults can be found [here](https://api-docs.request.network/).‌

As an example, if you wanted to retrieve the first 50 Requests, `skip` would equal 0, and the `take`value would be 50. If you then wanted to retrieve the subsequent 50 Requests `skip` would equal 50, and `take`would equal 50.‌

## Basic Usage <a id="basic-usage"></a>

Here is a basic example of creating a Request using the API via curl. Here, we are creating a basic BTC request.

```bash
curl -H "Authorization: [API-KEY]" \
     -H "Content-Type: application/json" \
     -X POST \
     -d '{"currency": "BTC","expectedAmount": "100000000", "payment": { "type": "bitcoin-testnet", "value": "mqdT2zrDfr6kp69hHLBM8CKLMtRzRbT2o9" }}' \ 
     https://api.request.network/requests
```

Don't forget, you can get your API key from your [Request Dashboard](http://baguette-dashboard.request.network/).‌‌

## Examples <a id="examples"></a>

### Creating an Invoice <a id="creating-a-request"></a>

To create an invoice, you must create a basic `Request` object which outlines some information about the Request such as the receiving payment address, which payment network is being used, the currency and the amount expected. You can retrieve individual requests as well as list all requests. Requests are identified by a unique UUID.‌‌

{% embed url="https://runkit.com/adamdowson/create-a-request/5.0.0" %}

You will notice that the API returns both an `_id` and and a `requestId`, which is empty at first. This is to speed up the response time, as the `requestId` will only get computed when persisted on the blockchain. You can get the `requestId` by fetching the invoice data a few minutes after you've created it, using the `_id` field. This should be removed in a next release.

### Fetching an Invoice <a id="fetching-a-request"></a>

All invoices have a unique ID, with this ID you can retrieve all the details of an invoice that has previously been created. By supplying the ID that was returned when creating the invoice you can query the endpoint as seen below, the API will then return the corresponding invoice information.‌

{% embed url="https://runkit.com/adamdowson/find-a-request/6.0.0" %}

