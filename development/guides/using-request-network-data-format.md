# Using Request Network Data Format

### Introduction

Request Network Data Format is a Json Schema library providing standards for the data of the Request Network protocol.

It also provide a Javascript library to play with. For now, only a validation function is available.

### Available JSON Schema

| Name | Last version | Last version | Description | Example |
| --- | --- |
| [Invoice](https://github.com/RequestNetwork/requestNetwork-private/blob/development/packages/requestNetworkDataFormat/src/format/rnf_invoice) | rnf\_invoice | 0.0.1 | Format to create an invoice | [example-valid.json](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/requestNetworkDataFormat/test/data/example-valid.json) |

### Javascript library

#### Install with NPM

`npm install requestnetwork-data-format`

#### Install with Yarn

`yarn add requestnetwork-data-format`

#### Using

```javascript
import RequestNetworkDataFormat from 'requestnetwork-data-format';

let result = RequestNetworkDataFormat.validate(A_JSON_OBJECT);

if (!result.valid) {
    // use the errors from result.errors
}
```



