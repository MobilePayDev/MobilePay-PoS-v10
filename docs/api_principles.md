
# <a name="api_principles"></a> API Principles

This is a preliminary list of architectural principles.

### Backend Has the Truth

One of the architectural principles in MobilePay PoS is that the backend has the truth. This means that in a situation where certainty is required, for instance whether the payment has been approved, the app and client will have to wait for confirmation from the backend.

### API Versus Implementation

The API does not describe the abstractions of the underlying backend or client implementations. Instead the API serves as the joint interface for backend and frontend. The ramification for this is that both frontend and backend can move independently from the API. However, they must always support the API specification and not only support perceived details of current implementations. Any valid HTTP request must be handled appropriately by the backend and produce a useful informative HTTP response. Any valid HTTP response received in the client must be handled appropriately by the client.

### RESTful API

The API is defined using the RESTful principles.

## <a name="security_model"></a> Security Model

### Communication Security

The MobilePay PoS V10 API uses TLS for communication security and data integrity (secure channel between the client and the 
backend). The API currently uses TLS 1.2. It is the integrator's responsibility to plan for an upgrade to TLS 1.3, when
TLS 1.2 is deprecated. 

### Integrator Authorization

The MobilePay API Gateway is ensuring the authentication of all PoS API requests. 
In order to be granted access to the MobilePay PoS API each integrator/vendor will have to enroll their clients as a client (called App) in our API Gateway Developer Portal (see [Client Identification](api_principles#client_identification) for more information).

Creating an app in MobilePay Developer Portal will create a client ID that should be used in all calls to the MobilePay PoS API in the following way:

````
--header 'X-IBM-Client-Id: 80e0075c-d0b5-4b74-a466-ecaca65234b0'
````

## <a name="client_identification"></a> Client Identification

All calls to the MobilePay PoS V10 API must include the following two headers `X-MobilePay-Client-System-Name` and
`X-MobilePay-Client-System-Version` to identify the client system and verify that the given system has been [certified](api_principles#self_certification).
The Client Name (`X-MobilePay-Client-System-Name`) is a suitable name used for the 
client, preferably the name that the integrator uses in their own communication. This way support communication 
between merchant, integrator and MobilePay uses the same name which should aid in removing confusion in the support 
situation. The Client Version (`X-MobilePay-Client-System-Version`) is a 3 dimensional number Major.Minor.Build. It 
is recommended that when the client software is updated, the client version is updated accordingly. 
The client version will be used by MobilePay to block versions of clients that are not certified 
and/or are misbehaving. An example of misbehavior is spamming irrelevant HTTP calls that endanger fast 
response times for other clients.

The three parts of the Client Version is defined as described below.
* Major version represents major changes to the client version, perhaps representing breaking changes on the clients interfaces or representing major changes communicated to merchants. A major change requires recertification.
* Minor version represents minor changes to the client version, changes that introduces new features or a change in the way internal logic is handled. Minor version changes are perhaps not communicated to merchants. A minor change requires recertification.
* Build version represents a new build of the client, including minor bug-fixes and changes of the lowest magnitude. A new build version does not require recertification.

Certification requirements in regard to changes to Client Name and Client Version are the following

* Changes in Client Name, major version or minor version require a new Certification.
* Changes in Build version do not require a new Certification.

The Client Name and Client Version should be added in all calls as shown below.

````
--header 'X-MobilePay-Client-System-Name: MobilePay Pos Client Reference Implementation'
--header 'X-MobilePay-Client-System-Version: 2.1.1'
````

## <a name="api_responses"></a> API Responses

The MobilePay PoS V10 API uses HTTP 2XX status codes for successful requests, HTTP 4XX
status codes for failed requests that failed due to a client error and HTTP 5XX status 
codes for failed requests that failed due to a server error. An overview of error codes
used in the V10 API is given below.

| Status code                 | Description                                                                               |
|-----------------------------|-------------------------------------------------------------------------------------------|
| `200 OK`                    | The request succeeded.                                                                     |
| `204 No Content`            | The request succeeded but no response was returned.                                        |
| `400 Bad Request`           | The request is syntactically ill-formed or violates [validation rules](input_formats).  |
| `401 Unauthorized`          | [Authentication](api_principles#security_model) of the caller failed.                      |
| `403 Forbidden`             | The call was rejected due to insufficient permissions of the caller.                       |
| `404 Not Found`             | The specified resource does not exist.                                                     |
| `409 Conflict`              | The request was rejected due to the state of the underlying resource.                      |
| `500 Internal Server Error` | An unrecoverable internal server error occurred.                                            |

For most errors, the V10 API returns an error response body that includes an error code and an error
description. The error response has the following structure:

```javascript
{
  "code": "19999",
  "message": "Tiny elves have invaded V10; we surrender",
  "CorrelationId": "197b2e31-787d-423f-ba00-0bd1f19291df"
}
```

### <a name="error_codes"></a> Error codes

#### Entity not found error codes

| Error code | Description |
|------------|-------------|
| 1000       | The ``MerchantPosId`` did not correspond to a point of sale in our database |
| 1001       | The ``PaymentId`` did not correspond to a payment in our database |
| 1002       | The ``StoreId`` did not correspond to a store in our database |
| 1003       | The ``BrandId`` did not correspond to a brand in our database |

#### Bad request error codes

| Error code | Description |
|------------|-------------|
| 1100       | ``BeaconId`` must be a valid guid or a 15 digit string |
| 1101       | One or more BeaconTypes are not supported. |
| 1102       | The ``amount`` needs to be a positive number |
| 1103       | The ``amount`` cannot have more than two decimal places |
| 1104       | ``CurrencyCode`` not supported. |
| 1105       | The ``userMinimumAge`` needs to be a positive number |
| 1109       | Payment filter not specific enough |
| 1110       | Refund filter not specific enough |
| 1111       | Invalid ``PosId`` |
| 1112       | Invalid ``PosName`` |
| 1113       | Invalid ``PaymentOrderId`` |
| 1114       | Invalid ``RefundOrderId`` |
| 1117       | Invalid ``MerchantPaymentLabel`` |
| 1118       | Invalid ``CalibrationType`` |
| 1119       | Invalid ``MerchantBrandId`` |
| 1120       | Invalid ``MerchantLocationId`` |

#### Point of sale conflict error codes

| Error code | Description |
|------------|-------------|
| 1200       | A point of sale with the given ``merchantPosId`` already exist |
| 1201       | The checkin cannot be cancelled when there is an ongoing payment |
| 1202       | The ``BeaconId`` is assigned to another point of sale |
| 1203       | Point of sale filter not specific enough |

#### Payment conflict error codes

| Error code | Description |
|------------|-------------|
| 1300       | The payment cannot be cancelled in the current state |
| 1301       | A payment is already active. Cancel it before starting a new one |
| 1303       | Payment needs to be prepared before it can be marked as ready |
| 1304       | Cannot capture payment when payment is not reserved |
| 1305       | Capture amount cannot exceed the reserved amount |
| 1306       | Idempotency Key has to be unique per request unless the request is a retry of a previous request |

## <a name="error_handling"></a> Error Handling

Clients integrating against the MobilePay PoS V10 API should expect intermittent errors and **must** implement suitable
error handling. Errors can generally be classified into three categories: *network errors*, *server errors* and
*client errors*. Network and server errors should be handled by retrying requests, while client errors should be
handled by fixing the client request. 

### Network and server errors

Network errors typically present themselves as timeouts or connections that are closed prematurely. 
Network errors and server errors (HTTP 5XX responses) should initially be handled by retrying requests.
If errors persist despite retries, the flow should be aborted. The PoS V10 API uses *idempotency* to
ensure that requests can always be safely retried. Idempotency ensures that performing the same call 
multiple times will not cause additional state changes beyond those caused by the first call. 

For instance, if a capture call on a payment is successful on the backend, but the connection to the client is
closed before the client receives the response, then it is safe for the client to retry the capture 
call. The second capture call will immediately return with a `200 OK` response as the capture was 
already completed on the first capture call.

### Idempotency keys
In the case of `POST` endpoints that create new resources (e.g., initiating a payment or a refund) the backend
cannot determine on its own whether two requests with identical request bodies is due to a retry or a request to
create two resources. The PoS V10 API thus requires the client to set an *idempotency-key* header 
(`X-MobilePay-Idempotency-Key`) on each request when calling the following endpoints, to allow the 
backend to identify retried calls:
````
POST /api/v10/payments
POST /api/v10/payments/prepare
POST /api/v10/refunds
POST /api/v10/pointofsales
````
For each call to the endpoints above, the client should generate a unique idempotency key for the
given call. In case the client decides to retry a call due to a failure, the client **must** use the
same idempotency key, to allow the backend to identify it as a retried call. We recommend using a
client-generated *GUID* as the idempotency key.

For instance, if a client calls to initiate a payment with a unique idempotency key, *key1*, and the 
initiate call is successful but the client never receives the response due to an intermittent network 
issue, then it is safe to retry the initiate payment call with the same idempotency key, *key1*.
Because the idempotency key is the same, the second call will not initiate a new payment, but rather 
return the `paymentId` of the already initiated payment. 

All other endpoints in the PoS V10 API are naturally idempotent and do not require explicit idempotency keys
to be set by the client. 

The PoS V10 API stores idempotency keys for at least 24 hours. If a call is retried with the same idempotency
key more than 24 hours after the original call, then the PoS V10 API does not guarantee that it will be handled
as a retried call. 

### Retrying requests
We recommend retrying failed requests due to network and server errors using one of these strategies:
* Retrying requests up to a fixed number of times with a constant delay between each call. 
* Retrying requests until a proper response is received, using an exponential backoff with jitter strategy (i.e.
doubling the delay between each retried call and adding some randomness to the delay to avoid overloading the backend).

### Client errors

Client errors (HTTP 4XX) indicate a problem with the client request and can typically not be resolved by retrying
the request. HTTP 409 errors typically indicate that the client and the PoS backend is out-of-sync about
the state of a given resource (e.g. trying to capture a reservation that is not yet reserved or initiating
a payment on a PoS that already has an active payment). If possible, the client should try to query the given
resource to fix any inconsistencies between the client and the PoS backend.

## <a name="call_throttling"></a> Call Throttling

Several flows in the PoS V10 API require the client to poll the PoS backend for state changes. To help 
protect against excessive polling, all endpoints used for polling in the PoS V10 API include a poll delay
field to allow the backend to throttle polling calls from clients. The following polling endpoints include
a `pollDelayInMs` field in the response body:

````
GET /api/v10/payments/{paymentId}
GET /api/v10/refunds/{refundId}
GET /api/v10/pointofsales/{posId}/checkin
````

If a response includes a `pollDelayInMs` of 1000, the client **must** wait at least 1000ms (i.e. 1 second)
before polling the same endpoint. In case no response is received when querying one of the above polling endpoints,
then clients should either:

* Use the `pollDelayInMs` from the last successful call to the given endpoint.
* Continue polling using an exponential backoff with jitter strategy (i.e. doubling the delay between each retried 
call and adding some randomness to the delay to avoid overloading the backend).
