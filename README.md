# AmazonDRS #

This library allows your agent code to work with the [Amazon Dash Replenishment Service](https://developer.amazon.com/dash-replenishment-service) via the [RESTful API](https://developer.amazon.com/docs/dash/replenish-endpoint.html).

This version of the library supports the following functionality:

- Device authentication on the Dash Replenishment Service
- Placing test orders for Amazon goods
- Canceling test orders
- Placing real orders for Amazon goods

**To add this library to your project, add** `#require "AmazonDRS.agent.lib.nut:1.0.0"` **to the top of your agent code**

## Library Usage ##

### Prerequisites ###

Before using the library you need to have:

- An Amazon Developer account
- [Client ID and Secret of your LWA Security Profile](https://developer.amazon.com/docs/dash/create-a-security-profile.html)
- [A DRS device](https://developer.amazon.com/dash-replenishment/drs_console.html)

### Authentication ###

This library needs a `Refresh Token` to be able to call the Amazon's DRS API.
The `Refresh Token` can be acquired with the [login()](TODO) method or any other application-specific way.
Then this token should be passed in with the [setRefreshToken()](TODO) method.

The [login()](TODO) method provides the following authentication flow:
1. a user opens the agent's URL in a browser
1. the library handles this request and redirects the user to the Amazon login page or to the Amazon device's settings page if the user is already logged in
1. the user sets up the device in the Amazon's UI
1. the Amazon's LWA redirects the user back to the agent's URL with an authorization code
1. the agent receives this code and acquires security tokens (`Refresh Token` and `Access Token`)

**Note**: After each restart of the agent the `Refresh Token` should be passed in to the library. So if you don't want to go through the authentication steps again, you may save the token in the agent's persistent storage and set it with the [setRefreshToken()](TODO) method after each restart of the agent.

### Callbacks ###

All requests that are made to the Amazon platform occur asynchronously. Every method that sends a request has an optional parameter which takes a callback function that will be executed when the operation is completed, whether successfully or not. The callback’s parameters are listed in the corresponding method description, but every callback has at least one parameter, *error*. If *error* is `0`, the operation has been executed successfully. Otherwise, *error* is a code of an error.

## AmazonDRS Class ##

### Constructor: AmazonDRS(*deviceModel, deviceSerial, clientId, clientSecret*) ###

This method returns a new AmazonDRS instance.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *deviceModel* | String | Yes | `Device Model`. For information, please see [here](https://developer.amazon.com/docs/dash/lwa-web-api.html#integrate-with-the-lwa-sdk-for-javascript). |
| *deviceSerial* | String | Yes | `Device Serial`. For information, please see [here](https://developer.amazon.com/docs/dash/lwa-web-api.html#integrate-with-the-lwa-sdk-for-javascript). |
| *clientId* | String | Yes | `Client ID` of your LWA Security Profile. For information, please see [here](https://developer.amazon.com/docs/login-with-amazon/glossary.html#client_identifier). |
| *clientSecret* | String | Yes | `Client Secret` of your LWA Security Profile. For information, please see [here](https://developer.amazon.com/docs/login-with-amazon/glossary.html#client_secret). |

#### Example ####

```
#require "AmazonDRS.agent.lib.nut:1.0.0"
TODO
```

### login(*[onAuthenticated[, testDevice]]*) ###

This method starts the [internal authentication](#internal-authentication). Either this method or [*setRefreshToken*](TODO) should be called and authentication should be done before making any DRS-related requests.

**If you are going to use this method, please add the following line to the top of your agent code:**

```squirrel
#require "Rocky.class.nut:2.0.1"
```

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *onAuthenticated* | Function | Optional | Callback called when the operation is completed or an error happens. |
| *testDevice* | Boolean | Optional | `True` if it is a test device making test orders. `False` by default. For more information, please see [here](https://developer.amazon.com/docs/dash/test-device-purchases.html). |

The method returns nothing. A result of the operation may be obtained via the [onAuthenticated](TODO) callback, if specified in this method.

#### Callback: onAuthenticated(*error, details*) ####

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the authentication is successful, an error code otherwise. |
| *details* | Table | Key-value table with the error's details provided by Amazon server. May be empty. This parameter should be ignored if *error* is `0`. [See "Error Parameters" for more information](https://developer.amazon.com/docs/login-with-amazon/authorization-code-grant.html#authorization-errors). |

TODO: could be some exceptions

#### Example ####

```
TODO
```

### setRefreshToken(*refreshToken*) ###

This method allows to set a `Refresh Token` manually. It is used for the [external authentication](#external-authentication). Either this method or [*login*](TODO) should be called before making any DRS-related requests.

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

The method returns nothing. A result of the operation may be obtained via the [onReplenished](TODO) callback, if specified in this method.

#### Callback: onReplenished(*error, response*) ####

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the operation is completed successfully, an error code otherwise. |
| *response* | Table | Key-value table with the response provided by Amazon server. May be empty. [See response example](https://developer.amazon.com/docs/dash/replenish-endpoint.html#response-example). Also may contain an [error details](https://developer.amazon.com/docs/dash/replenish-endpoint.html#error-responses). |

#### Example ####

```
TODO
```

### cancelTestOrder(*[slotId[, onCanceled]]*) ###

This method cancels test orders for one or all slots in the device. For more information, please see [the Amazon DRS documentation](https://developer.amazon.com/docs/dash/canceltestorder-endpoint.html).

The method can only be used for the orders made by a test device ([a device authenticated as a test one](https://developer.amazon.com/docs/dash/test-device-purchases.html)). The library does not check if your device authenticated as a test one or not, so you are responsible for this check.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *slotId* | String | Yes | ID of a slot to be canceled. If is `null` or not specified test orders for all slots in the device will be canceled. |
| *onCanceled* | Function | Optional | Callback called when the operation is completed or an error happens. |

The method returns nothing. A result of the operation may be obtained via the [onCanceled](TODO) callback, if specified in this method.

#### Callback: onCanceled(*error, response*) ####

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the operation is completed successfully, an error code otherwise. |
| *response* | Table | Key-value table with the response provided by Amazon server. May be empty. [See response example](https://developer.amazon.com/docs/dash/canceltestorder-endpoint.html#response-example). Also may contain an [error details](https://developer.amazon.com/docs/dash/canceltestorder-endpoint.html#error-responses). |

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
| 100-999 | HTTP error codes from Amazon server. See [here](https://developer.amazon.com/docs/dash/replenish-endpoint.html#error-responses) for [replenish](TODO) method and [here](https://developer.amazon.com/docs/dash/canceltestorder-endpoint.html#error-responses) for [cancelTestOrder](TODO) method.  |
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
