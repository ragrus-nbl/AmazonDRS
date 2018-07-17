# AmazonDRS #

This library allows your agent code to work with the [Amazon Dash Replenishment Service](https://developer.amazon.com/dash-replenishment-service) via the [RESTful API](https://developer.amazon.com/docs/dash/replenish-endpoint.html).

This version of the library supports the following functionality:

- Device authentication on the Dash Replenishment Service
- Place test orders for Amazon goods
- Cancel test orders
- Place real orders for Amazon goods

TODO: write a description of the auth process used

**To add this library to your project, add** `#require "AmazonDRS.agent.lib.nut:1.0.0"` **to the top of your agent code**

## Library Usage ##

### Prerequisites ###

Before using the library you need to have:

- An Amazon Developer account
- [Client ID and Secret of your LWA Security Profile](https://developer.amazon.com/docs/dash/create-a-security-profile.html)
- [A DRS device](https://developer.amazon.com/dash-replenishment/drs_console.html)

### Callbacks ###

All requests that are made to the Amazon platform occur asynchronously. Every method that sends a request has an optional parameter which takes a callback function that will be executed when the operation is completed, whether successfully or not. The callback’s parameters are listed in the corresponding method description, but every callback has at least one parameter, *error*. If *error* is `0`, the operation has been executed successfully. Otherwise, *error* is a code of an error.

Some methods require callbacks to be specified, others need only be passed a callback if you wish.

## AmazonDRS Class ##

### Constructor: AmazonDRS(*clientId, clientSecret[, onAuthenticated[, refreshToken]]*) ###

This method returns a new AmazonDRS instance.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *clientId* | String | Yes | `Client ID` of your LWA Security Profile. |
| *clientSecret* | String | Yes | `Client Secret` of your LWA Security Profile. |
| *onAuthenticated* | Function | Optional | Callback called when the client is authenticated. Never called if the *refreshToken* parameter is passed. |
| *refreshToken* | String | Optional | An authentication `Refresh Token` used to acquire an `Access Token` and refresh it when expired. Pass it only if you want to use an external authentication process handler. It disables the internal authentication mechanisms. |

#### Example ####

```
#require "AmazonDRS.agent.lib.nut:1.0.0"

const MY_PLATFORM_ENDPOINT = "<YOUR_PLATFORM_ENDPOINT>";
const MY_APP_KEY = "<YOUR_THINGWORX_APP_KEY>";

tw <- ThingWorx(MY_PLATFORM_ENDPOINT, MY_APP_KEY);
```

### startAuthentication(*[testDevice[, onAuthenticated]]*) ###

TODO: can produce human-readable error messages! Maybe it is worth to make an additional field for error descriptions

This method ...

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *testDevice* | Boolean | Optional | `True` if it is a test device making test orders. For more information, please see [docs](https://developer.amazon.com/docs/dash/test-device-purchases.html). |
| *onAuthenticated* | Function | Optional | Callback called when any result of authentication is got. |

This method returns nothing. The result of the operation may be obtained via the *callback* function, which has the following parameter:

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | Integer | `0` if the authentication is successful, an error code otherwise. |

TODO: could be some exceptions

### replenish(*slotId[, onReplenished]*) ###

This method places an order for a device/slot combination. For more information, please see [the Amazon DRS documentation](https://developer.amazon.com/docs/dash/replenish-endpoint.html).

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *slotId* | String | Yes |  |
| *onReplenished* | Function | Optional |  |

This method returns nothing. The result of the operation may be obtained via the *callback* function, which has the following parameter:

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | [ThingWorx.Error](#thingworxerror-class) | Error details, or `null` if the operation succeeds |

### existThing(*thingName, callback*) ###

This method checks if Thing with the specified name exists.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *thingName* | String | Yes | Name of the Thing |
| *callback* | Function | Yes | Executed once the operation is completed |

This method returns nothing. The result of the operation may be obtained via the *callback* function, which has the following parameters:

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | [ThingWorx.Error](#thingworxerror-class) | Error details, or `null` if the operation succeeds |
| *exist* | Boolean | `true` if the Thing exists, or `false` if the Thing does not exist or the operation fails |

### deleteThing(*thingName[, callback]*) ###

This method deletes Thing with the specified name.

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *thingName* | String | Yes | Name of the Thing |
| *callback* | Function | Optional | Executed once the operation is completed |

This method returns nothing. The result of the operation may be obtained via the *callback* function, which has the following parameter:

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | [ThingWorx.Error](#thingworxerror-class) | Error details, or `null` if the operation succeeds |

### createThingProperty(*thingName, propertyName, propertyType[, callback]*) ###

This method creates a new Property of the specified Thing and restarts the Thing. For more information, please see [the ThingWorx documentation](https://developer.thingworx.com/resources/guides/thingworx-rest-api-quickstart/use-rest-api-add-property-thing).

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *thingName* | String | Yes | Name of the Thing |
| *propertyName* | String | Yes | Name of the new Property. Must be unique across the specified Thing |
| *propertyType* | String | Yes | Type of the new Property. Must be one of the types described in [the ThingWorx documentation](https://support.ptc.com/cs/help/thingworx_hc/thingworx_7.0_hc/index.jspx?id=ThingProperties&action=show) |
| *callback* | Function | Optional | Executed once the operation is completed |

This method returns nothing. The result of the operation may be obtained via the *callback* function, which has the following parameter:

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | [ThingWorx.Error](#thingworxerror-class) | Error details, or `null` if the operation succeeds |

### setPropertyValue(*thingName, propertyName, propertyValue[, callback]*) ###

This method sets a new value of the specified Property. For more information, please see [the ThingWorx documentation](https://developer.thingworx.com/resources/guides/thingworx-rest-api-quickstart/use-rest-api-set-property-value).

| Parameter | Data Type | Required? | Description |
| --- | --- | --- | --- |
| *thingName* | String | Yes | Name of the Thing |
| *propertyName* | String | Yes | Name of the Property |
| *propertyValue* | Boolean, Integer, Float,<br>String, Key-Value Table,<br>Blob, Null | Yes | New value of the Property |
| *callback* | Function | Optional | Executed once the operation is completed |

This method returns nothing. The result of the operation may be obtained via the *callback* function, which has the following parameter:

| Parameter | Data Type | Description |
| --- | --- | --- |
| *error* | [ThingWorx.Error](#thingworxerror-class) | Error details, or `null` if the operation succeeds |

### setDebug(*value*) ###

This method enables (*value* is `true`) or disables (*value* is `false`) the library debug output (including error logging). It is disabled by default. The method returns nothing.

## ThingWorx.Error Class ##

This class represents an error returned by the library and has the following public properties:

- *type* &mdash; The error type, which is one of the following *THING_WORX_ERROR* enum values:
    - *LIBRARY_ERROR* &mdash; The library is wrongly initialized, a method is called with invalid argument(s), or an internal error has occurred. The error details can be found in the *details* property. Usually this indicates an issue during application development which should be fixed during debugging and therefore should not occur after the application has been deployed.
    - *REQUEST_FAILED* &mdash; An HTTP request to the ThingWorx platform failed. The error details can be found in the *details*, *httpStatus* and *httpResponse* properties. This error may occur during the normal execution of an application. The application logic should process this error.
   - *UNEXPECTED_RESPONSE* &mdash; An unexpected response from the ThingWorx platform. The error details can be found in the *details* and *httpResponse* properties.
- *details* &mdash; A string containing a human readable description of the error.
- *httpStatus* &mdash; An integer indicating the HTTP status code, or `null` if the *type* property is *LIBRARY_ERROR*
- *httpResponse* &mdash; A table of key-value strings holding the response body of the failed request, or `null` if the *type* property is *LIBRARY_ERROR*.

## Examples ##

Working examples are provided in the [Examples](./Examples) directory and described [here](./Examples/README.md).

## Testing ##

Tests for the library are provided in the [tests](./tests) directory and described [here](./tests/README.md).

## License ##

The ThingWorx library is licensed under the [MIT License](./LICENSE)
