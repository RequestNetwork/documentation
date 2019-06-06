# Adding support for a new currency

To understand this chapter you should read [protocol](https://docs.request.network/development/protocol).

For ERC20 tokens, please go to the end of this chapter.

If you wish to add a new currency in the Request Network Protocol, you are very welcome! You will need to follow the following process to do so: 

* **Step 1:** Define the payment process of your new currency
* **Step 2:** Develop in Solidity a new currency contract compatible with the Request Network Core
* **Step 3:** Update the Request Network JS library to interact with your fresh new contract
* **Step 4:** Submit your code with some explanation through a PR on the Request Network GitHub

### Step 1: Define the payment process

**You will need to define how to identify a payment on the new blockchain**. E.g: for bitcoin, we link a request with a Bitcoin address. A mechanism in the library checks if a payment has been done on this address. You can also rely on data included in the transaction.

From this point, you will be able to define:

* the state variable you need in step 2
* the payment identification process you need in step 3

### Step 2: How to develop a new currency contract

#### **Step 2.a**: Understand the Request architecture

First of all, let's see how the Request Network contracts are architectured: \(you will find the implementation on our GitHub: [packages/requestNetworkSmartContracts](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/requestNetworkSmartContracts)\)

```text
                        +--------------------------+
                        |   R. currency interface  |
                        +------------+-------------+
                                     |
 +-------------+        +------------+-------------+
 |   R. Core   +--------+   R. currency contract   |
 +-------------+        +--------------------------+
```

The Request Core stores the basic information about the requests \(payee, payer, amounts, state, etc...\) and handles the basic logic. Each Request currency contract handles the logic and the specificity of their currency. Every currency contract inherits from the Request currency interface which contains the base logic that is common for all the requests, which is four public functions:

* `acceptAction`
* `cancelAction`
* `additionalAction`
* `subtractAction`

  and one internal function to create a request in the core: `createCoreRequestInternal`.

#### **Step 2.b**: Create the constructor and the request creation functions

From all of this, you will just have to create your own currency contract implementing functions of creation, such as:

* `createRequestAsPayeeAction`
* `broadcastSignedRequestAsPayerAction`
* `constructor`

This functions will affect values to specific state variables \(e.g: payment address in the currency\).

Below, you can find a generic currency contract. Everywhere you find the mention `YOUR CODE HERE` is a space where you can fill it up with the specific need of your currency:

```javascript
pragma solidity ^0.4.23;

import "./CurrencyContract.sol";
import "../core/RequestCore.sol";
import "../utils/Bytes.sol";
import "../utils/Signature.sol";

contract RequestNewCurrency is CurrencyContract {

    // ---------------------------------------------------------------------------------
    // YOUR CODE HERE: Specific parameter for this currency, such as payment address and refund address
    //  e.g: 
    //    mapping(bytes32 => string[256]) public payeesPaymentAddress;
    // ---------------------------------------------------------------------------------

    /**
     * @param _requestCoreAddress Request Core address
     * @param _requestBurnerAddress Request Burner contract address
     *
     * add your specific parameters here
     *
     */
    constructor (address _requestCoreAddress, address _requestBurnerAddress /*, YOUR CODE HERE: add specific parameters here */) 
        CurrencyContract(_requestCoreAddress, _requestBurnerAddress)
        public
    {
        // YOUR CODE HERE: use your specific parameters here
    }

    /**
     * @notice Function to create a request as payee.
     *
     * @dev msg.sender must be the main payee.
     *
     * @param _payeesIdAddress array of payees address (the index 0 will be the payee - must be msg.sender - the others are subPayees)
     * @param _expectedAmounts array of Expected amount to be received by each payees
     * @param _payer Entity expected to pay
     * @param _data Hash linking to additional data on the Request stored on IPFS
     *
     * add your specific parameters here
     *
     * @return Returns the id of the request
     */
    function createRequestAsPayeeAction(
        address[]    _payeesIdAddress,
        int256[]     _expectedAmounts,
        address      _payer,
        string       _data,
        // YOUR CODE HERE: Add your specific parameters here
        )
        external
        payable
        whenNotPaused
        returns(bytes32 requestId)
    {
        // YOUR CODE HERE: add you check here
        require(
            msg.sender == _payeesIdAddress[0] && msg.sender != _payer && _payer != 0,
            "caller should be the payee"
        );

        // Function to create request in the core and give the fees required from CurrencyContract interface
        uint256 collectedFees;
        (requestId, collectedFees) = createCoreRequestInternal(
            _payer,
            _payeesIdAddress,
            _expectedAmounts,
            _data
        );

        // Additional check on the fees: they should be equal to the about of ETH sent
        require(collectedFees == msg.value, "fees should be the correct amout");

        // ---------------------------------------------------------------------------------
        // YOUR CODE HERE: use you specific parameters here
        // ---------------------------------------------------------------------------------

        return requestId;
    }

    /**
     * @notice Function to broadcast and accept an offchain signed request (the broadcaster can also pay and makes additionals).
     *
     * @dev msg.sender will be the _payer.
     * @dev only the _payer can additionals.
     *
     * @param _requestData nested bytes containing : creator, payer, payees|expectedAmounts, data

     * add your specific parameters here

     * @param _additionals array to increase the ExpectedAmount for payees
     * @param _expirationDate timestamp after that the signed request cannot be broadcasted
     * @param _signature ECDSA signature in bytes
     *
     * @return Returns the id of the request
     */
    function broadcastSignedRequestAsPayerAction(
        bytes         _requestData, // gather data to avoid "stack too deep"
        // YOUR CODE HERE: add your specific parameters here
        uint256[]     _additionals,
        uint256       _expirationDate,
        bytes         _signature)
        external
        payable
        whenNotPaused
        returns(bytes32 requestId)
    {
        // YOUR CODE HERE: add you check here
        require(_expirationDate >= block.timestamp, "expiration should be after current time");

        // YOUR CODE HERE: modify this part to check the signature
        require(
            Signature.checkRequestSignature(
                _requestData,
                _expirationDate,
                _signature
            ),
            "signature should be correct"
        );

        // compute and send fees
        uint256 fees = collectEstimation(totalExpectedAmounts);
        require(fees == msg.value, "fees should be the correct amout");
        collectForREQBurning(fees);

        // insert the msg.sender as the payer in the bytes
        Bytes.updateBytes20inBytes(_requestData, 20, bytes20(msg.sender));

        // Function to create request in the core
        requestId = requestCore.createRequestFromBytes(_requestData);

        // ---------------------------------------------------------------------------------
        // YOUR CODE HERE: use you specific parameters here
        // ---------------------------------------------------------------------------------

        // Function from CurrencyContract contract to accept the request
        acceptAction(_requestId);

        // Function from CurrencyContract contract to add additionals 
        additionalAction(_requestId, _additionals);

        return requestId;
    }

    // YOUR CODE HERE: you can also add more future here
}
```

N.B: You can find an example of this implementation for bitcoin: [packages/requestNetworkSmartContracts](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/requestNetworkSmartContracts)

#### Step 2.c: Implement unit test

Don't forget to include tests in the proper directory: `test/currencyContracts/newCurrency`.

### Step 3: Update the Request Network library

Now, the contract is ready, you will need to modify the library to handle the new currency. You can view  the Request Network library here: [packages/requestNetwork.js](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/requestNetwork.js)

#### Step 3.a: Create the service contracts

You will have to create a new file in the directory `src/servicesContracts` with the formatted name `requestNewCurrency-service.ts`. It will contain a class with all the entry point of the smartcontracts you have just created before. Basically the actions, creations and information getters:

* `createRequestAsPayeeAction`
* `signRequestAsPayee`
* `broadcastSignedRequestAsPayerAction`
* `acceptAction`
* `cancelAction`
* `additionalAction`
* `subtractAction`
* `getRequestCurrencyContractInfo`
* `getRequestEventsCurrencyContractInfo`

#### Step 3.b: Implement the request creation functions and the basic action

You will have first to implements the creation functions: 

* `createRequestAsPayeeAction`
* `signRequestAsPayee`
* `broadcastSignedRequestAsPayerAction`

And the basic actions:

* `acceptAction`
* `cancelAction`
* `additionalAction`
* `subtractAction`

All this function will be called directly by the user \(or through the API, we will see it later\)

#### Step 3.c: Implement the payment and refund actions

Because the payments are in another blockchain as the Request Network, most likely, the function to handle the funds in this new currency won't need to be implemented \(`paymentAction` and `refundAction`\). However, you will have to write the code to watch and check the transfers of funds.

#### Step 3.d: Implement the payment and refund identification process

For example in BTC, we use an API connected to BTC nodes. You will have to find a good way to do the say thing in the new currency, regarding its own specificity. If you need a library to request information from external actors \(e.g: nodes of the new currency\), please create it in the directory `src/servicesExternal` N.B: You can modify `src/config-json.json` to include configuration information you need for the new currency.

#### Step 3.e: Implement the generic getters

Note also that the two getters:

* `getRequestCurrencyContractInfo` 
* `getRequestEventsCurrencyContractInfo`

Will be called by the class `src/servicesCore/requestCore-service.ts` to get the right information regarding a request regardless of the currency. That's why they have a fixed signature.

#### Step 3.f: Register your new currency service

When the new currency service is ready, you will need to register it in `src/servicesContracts.ts`:

```javascript
        import RequestNewCurrencyService from '../servicesContracts/requestNewCurrency-service';
        [...]
        export const getServiceFromAddress = (_networkName: string, _address: string): any => {
            [...]
                case 'RequestNewCurrency':
                    return RequestNewCurrencyService.getInstance();
            [...]
        };
```

#### Step 3.e: Allow your new currency service to be called through the API layer

You can now add the new currency to the API layer of the library. First, go to `src/types.ts` and add your new currency. You will probably also need to modify others interfaces here to match the new currency specific needs.

Go to `src/api/request-network.ts`, you will have to modify:

* the constructor to add your new service \(N.B: you will have to add also your external services\)

  ```javascript
        import RequestNewCurrencyService from '../servicesContracts/requestNewCurrency-service';
        [...]
        public requestNewCurrencyService: RequestNewCurrencyService;
        [...]
        constructor( ... ) {
            [...]
            RequestNewCurrency.destroy();
            this.requestNewCurrencyService = RequestNewCurrencyService.getInstance();
        }
  ```

* the functions `createRequest`, `createSignedRequest` and `broadcastSignedRequest`

  ```javascript
        public createRequest( ... ) {
            [...]
            if (currency === Types.Currency.NEWCURRENCY) {
                const requestEthereumService: RequestEthereumService = currencyUtils.serviceForCurrency(currency);
                if (as === Types.Role.Payee) {
                    // Create an ETH Request as Payee
                    promise = requestNewCurrencyService.createRequestAsPayee( ... );
                }
                [...]
               }
        }
  ```

#### Step 3.f: Implement unit test

Don't forget to include tests: `test/unit/newCurrencyServices`.

### Step 4: Submit your code

When everything has been developed and tested thoroughly, you can then submit the code to us through a PR on our GitHub: [https://github.com/RequestNetwork/requestNetwork](https://github.com/RequestNetwork/requestNetwork).

Please, explain what your code does and how it works. A well-commented PR will help us merge your work faster!

## Adding a new ERC20 token

Note: Because adding a new ERC20 requires little changes in the code but mostly auditing, the addition will be only performed by the Request team.

### Step 1: Audit the token

First of all, we have to audit the token to be sure it is trustworthy.

### Step 2: Deploy ERC20 request contract

Deploy the [ERC20 request contract](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/requestNetworkSmartContracts/contracts/synchrone/RequestERC20.sol) with the address of the token as the third parameter of the constructor. 

### Step 3: Update the artifacts

Add the deployed contract to [packages/requestNetworkArtifacts/artifacts.json](https://github.com/RequestNetwork/requestNetwork/blob/d7496b42445498b9d9e5b4855a237423064158e7/packages/requestNetworkArtifacts/artifacts.json). 

### Step 4: Update the js library

Update the type Currency in [packages/requestNetwork.js/src/types.ts](https://github.com/RequestNetwork/requestNetwork/blob/d7496b42445498b9d9e5b4855a237423064158e7/packages/requestNetwork.js/src/types.ts) to add the new token.

Add your new token in [packages/requestNetwork.js/src/utils/currency.ts](https://github.com/RequestNetwork/requestNetwork/blob/f88abf8b3312369518ff9d6735c68770af3c4010/packages/requestNetwork.js/src/utils/currency.ts) to define specific information such as: decimal, service and token address.

