# Apps analytics with the "builderId" field

To track the origin of the requests, a field in the request data called `builderId` is used. You can filter on this field to compute analytics on the requests created by your app.

Following the [Request Network format](https://docs.request.network/development/guides/using-request-network-data-format), the field should be in  `miscellaneous`:

```javascript
const requestData = {
  "miscellaneous": {
    "builderId": "myRequestDapp.com"
  }
}
```

 The value of the field is decided by the builder, make sure to use something unique that identifies your app, a domain name for example.

