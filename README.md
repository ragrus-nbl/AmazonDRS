# AmazonDRS #

This library allows your agent code to work with the [Amazon Dash Replenishment Service](https://developer.amazon.com/dash-replenishment-service) via the [RESTful API](https://developer.amazon.com/docs/dash/replenish-endpoint.html).

This version of the library supports the following functionality:

- Device authentication on the Dash Replenishment Service
- Placing test orders for Amazon goods
- Canceling test orders
- Placing real orders for Amazon goods

TODO: should we write a description of the auth process used? Or is it enough to make a link to the example's readme?

**To add this library to your project, add the following lines to the top of your agent code:**

```squirrel
#require "rocky.class.nut:2.0.1" (TODO: Should we place this line into the library's code?)
#require "AmazonDRS.agent.lib.nut:1.0.0"
```

## Library Usage ##

### Prerequisites ###

Before using the library you need to have:

- An Amazon Developer account
- [Client ID and Secret of your LWA Security Profile](https://developer.amazon.com/docs/dash/create-a-security-profile.html)
- [A DRS device](https://developer.amazon.com/dash-replenishment/drs_console.html)

### Callbacks ###

All requests that are made to the Amazon platform occur asynchronously. Every method that sends a request has an optional parameter which takes a callback function that will be executed when the operation is completed, whether successfully or not. The callback’s parameters are listed in the corresponding method description, but every callback has at least one parameter, *error*. If *error* is `0`, the operation has been executed successfully. Otherwise, *error* is a code of an error.

## AmazonDRS Class ##

### Constructor: AmazonDRS(*deviceModel, deviceSerial, clientId, clientSecret*) ###

This method returns a new AmazonDRS instance.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *deviceModel* | String | Yes | `Device Model`. For information, please see [here](https://developer.amazon.com/docs/dash/lwa-web-api.html#integrate-with-the-lwa-sdk-for-javascript). |
| *deviceSerial* | String | Yes | `Device Serial`. For information, please see [here](https://developer.amazon.com/docs/dash/lwa-web-api.html#integrate-with-the-lwa-sdk-for-javascript). |
| *clientId* | String | Yes | `Client ID` of your LWA Security Profile. |
| *clientSecret* | String | Yes | `Client Secret` of your LWA Security Profile. |

#### Example ####

```
#require "AmazonDRS.agent.lib.nut:1.0.0"
TODO
```

### startAuthentication(*[testDevice[, onAuthenticated]]*) ###

This method starts the authentication process using the internal mechanisms. The authentication process is described [here](TODO). Either this method or [*setRefreshToken*](TODO) should be called and authentication should be done before making any DRS-related requests.

The internal authentication mechanisms become disabled once authentication succeeds. They can be restarted if needed.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *testDevice* | Boolean | Optional | `True` if it is a test device making test orders. For more information, please see [here](https://developer.amazon.com/docs/dash/test-device-purchases.html). |
| *onAuthenticated* | Function | Optional | Callback called when any result of authentication is got. |

The method returns nothing. A result of the operation may be obtained via the [onAuthenticated](TODO) callback, if specified in this method.

#### Callback: onAuthenticated(*error, details*) ####

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the authentication is successful, an error code otherwise. |
| *details* | Table | Key-value table with the error's details provided by Amazon server. May be empty. This parameter should be ignored if *error* is `0`. [More about authentication errors descriptions](https://developer.amazon.com/docs/login-with-amazon/authorization-code-grant.html#authorization-errors). |

TODO: could be some exceptions

#### Example ####

```
TODO
```

### setRefreshToken(*refreshToken*) ###

This method allows to set a `Refresh Token` manually. It is used with an external authentication process handler. Either this method or [*startAuthentication*](TODO) should be called before making any DRS-related requests.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *refreshToken* | String | Yes | An authentication `Refresh Token` used to acquire an `Access Token` and refresh it when expired. |

The method returns nothing.

#### Example ####

```
TODO
```

### replenish(*slotId[, onReplenished]*) ###

This method places an order for a device/slot combination. For more information, please see [the Amazon DRS documentation](https://developer.amazon.com/docs/dash/replenish-endpoint.html).

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *slotId* | String | Yes | ID of a slot to place an order for it. |
| *onReplenished* | Function | Optional | Callback called when the operation is completed or an error happens. |

The method returns nothing. A result of the operation may be obtained via the onReplenished callback, if specified in this method.

#### Callback: onReplenished(*error, response*) ####

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the operation is completed successfully, an error code otherwise. |
| *response* | Table | Key-value table with the response provided by Amazon server. May be empty. |

#### Example ####

```
TODO
```

### cancelTestOrder(*slotId[, onCanceled]*) ###

This method cancels test orders for one or all slots in the device. For more information, please see [the Amazon DRS documentation](https://developer.amazon.com/docs/dash/canceltestorder-endpoint.html).

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *slotId* | String | Yes | ID of a slot to cancel a test order for it. |
| *onReplenished* | Function | Optional | Callback called when the operation is completed or an error happens. |

The method returns nothing. A result of the operation may be obtained via the onCanceled callback, if specified in this method.

#### Callback: onCanceled(*error, response*) ####

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the operation is completed successfully, an error code otherwise. |
| *response* | Table | Key-value table with the response provided by Amazon server. May be empty. |

#### Example ####

```
TODO
```

### setDebug(*value*) ###

This method enables (*value* is `true`) or disables (*value* is `false`) the library debug output (including error logging). It is disabled by default. The method returns nothing.

### Error codes ###

An *Integer* error code which specifies a concrete error (if any) happened during an operation.

| Error Code | Description |
| --- | --- |
| 0 | No error. |
| 1-99 | [Internal errors of the HTTP API](https://developer.electricimp.com/api/httprequest/sendasync). |
| 100-999 | HTTP error codes from Amazon server. |
| 1000 | The client is not authenticated. |
| 1001 | The authentication process is already started. |
| 1002 | The operation is not allowed now. Eg. the same operation is already in process. |
| 1003 | The operation is timed out. |
| 1010 | General error. |

## Examples ##

Working examples are provided in the [examples](./examples) directory and described [here](./examples/README.md).

## Testing ##

Tests for the library are provided in the [tests](./tests) directory and described [here](./tests/README.md).

## License ##

The AmazonDRS library is licensed under the [MIT License](./LICENSE)
