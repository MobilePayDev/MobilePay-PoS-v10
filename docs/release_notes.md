# <a name="release_notes"></a>PoS API Release Notes

The Point of Sale API V10 is now in production.

## Changelog
### 2022-08-12
- Introduced error code 1368 to [API Errors](endpoint_errors.md) and edited description on refundflow in [Refund Flow](payment_flows#refunds)

### 2022-07-28
- Introduced error code 1204 to [API Errors](endpoint_errors.md)

---
### 2022-05-25
- Made it clear that directly using the `pollDelayInMs` response can improve payment flow completion time in [API Principles](api_principles).

---
### 2022-05-03
- Removed information regarding a now decomissioned migration endpoint.

---
### 2022-04-08
- Created optimal flow section under [Best practices](best_practices).

---
### 2021-08-11
- Made it clear that it is the `beaconId` that should be used in [QR code generation](create_qr_codes) instead of `posId`.

---
### 2021-08-04
- Made it clear that Integrator's notification endpoint must return 200 on success in [Notification Service](notification_service#notification_endpoint)

---
### 2021-05-25
- Expanded list of valid characters in [Input Formats](input_formats#valid_characters)

---
### 2021-03-23
- Added `requirePaymentBeforeCheckin` documentation to [PoS management](pos_management#preventing-payments-before-checkin)

---
### 2021-03-22
- Added explicit retry suggestions and rules in [Retrying requests](api_principles#retrying_requests)

---
### 2021-03-16
- Removed CalibrationType property from [Input Formats](input_formats#poses) since it has been removed from the V10 API.
- Made the [Detecting MobilePay by notification service](detecting_mobilepay#notification) section more clear by adding the call ``GET /pointofsales/{posid}/checkin`` to the flow diagram

---
### 2021-03-09
- Re-wrote the [Onboarding](pos_management#onboarding) section of the Point-of-Sale Management page. The section now contains a description on how to acquire store information needed for integrators when onboarding a merchant, which does not have a MobilePay PoS solution using an older version of the MobilePay PoS API. 

---
### 2021-01-29
- Added guide to [Create QR Codes](create_qr_codes)

---
### 2020-12-15
- Added section to [Refund Flow](payment_flows#refunds) regarding how to get a `paymentId` for a refund if the `paymentId` is not known.

---
### 2020-10-23
- Clarified language in the network and server errors paragraph of [API principles](api_principles#error_handling).
- Added desciption of why it is important to either capture or cancel a refund in error situations in [Cancelling refunds](payment_flows#cancel).

---
### 2020-10-23
- Removed age validation from documentation since this is not possible to do.

---
### 2020-10-20
- Added error codes 1406, 1407, 1408, 1409, 1410, and 1411 to [API Errors](endpoint_errors.md).

---
### 2020-10-13
- Refund functionality now ready and live in prod. See [Refund Flow](payment_flows#refunds) for documentation on how to implement the functionality. An integrator needs to be certified in the new Refund section on the self certification website, to use the refund endpoints in the production environment. Also see the MobilePay developer portal to check out the API documentation.

---
### 2020-09-18
- Removed the following supportedBeaconTypes: `BluetoothOther`, `BluetoothMP1`, `BluetoothMP2`, `BluetoothMP3`, `BluetoothMP4` and replaced them with one `Bluetooth` in [Input Formats](input_formats). During certification when calling ``POST /pointofsales`` the request will now fail if supportedBeaconTypes contains any of the removed mentioned values above. If you already have a client that is certified and is using the old values, they will continue to work in production.

---
### 2020-09-14
- Added 1365, 1366, 1367 error codes to [Api Errors](endpoint_errors.md)

---
### 2020-09-08
- Removed Integrator ID from [Self Certification](self_certification)
- Updated screenshots in [Self Certification](self_certification)

---
### 2020-09-02
- Changed and updated how Refunds is planned to work. See description in [Payment Flows](payment_flows#refunds). Release of Refunds are planned to Ultimo September.

---
### 2020-09-02
- Clarified how to choose a beaconId in [Best Practices](best_practices#choosing_a_beaconid)  

---
### 2020-08-19
- Clarified the requirements for using long lived access tokens in [Authentication](pos_integratorauthentication)  
- Reduced the number of test cases in Self Certification by e.g. having more than one error or timeout scenario in one test case. Categories remain the same and this does not require a re-certification since this has not changed what is being tested.
- Updated picture of Onboarding cases in [Self Certification](self_certification)
- Removed x-mobilepay-client-system-name header from table in [Input Formats](input_formats)
- Please do not hesitate to provide feedback

---
### 2020-08-07

- Added a section on [Best Practices](best_practices) regarding Merchant onboarding.
- Moved information regarding Partial Capture from [Best Practices](best_practices) to [Payment Flows](payment_flows#partial_capture) and added some diagrams describing the flow.
- Removed the 403 forbidden statuscode when calling ``POST /payments/{paymentId}/capture`` from [API ERRORS](endpoint_errors)
- Added a 409 conflict statuscode when calling ``POST /payments/{paymentId}/capture`` from [API ERRORS](endpoint_errors) that descibes if a partial capture is attempted on a payment where that is not possible.

---
### 2020-07-13

- Added a 403 error code for payments/{payid}/capture in [API ERRORS](endpoint_errors) which is returned when trying to make a partial capture which is still not supported. When it will be possible to do partial captures it will be listed here in the release notes.

---
### 2020-07-10

- Clarify handling timeout for POST/DELETE/PUT in [API principles](api_principles#error_handling)

---
### 2020-06-29

- Updated payment lifetime in [Payment Flow](payment_flows#payment_flow)

---
### 2020-06-24

- Updated description and added configuration details in [Self Certification](self_certification)

---
### 2020-05-14

- Added error 1182 in [API ERRORS](endpoint_errors)

---
### 2020-05-13

- Added a note on partial capture handling in [Best Practices](best_practices)

---
### 2020-04-28

- Added a note on the possibility of long-lived access tokens for PoS devices

---
### 2020-04-21

- Added 403 in the API Errors section as a possible non-successful status-code when creating a Point of Sale.
- Added a conflict case to the capture payment endpoint in the API Errors section when trying to capture an already captured payment with a different amount.

---

### 2020-03-27

- Dropped the `x-ibm-client-system-name` header and modified the [API principles](api_principles) page to explain how the `x-ibm-client-system-version` header will be used for client versioning.

- Renamed all `X-IBM-*` and `X-MobilePay-*` headers to be lowercase in the documentation (the actual headers are case-insensitive).

---

### 2020-03-26

- Renamed `vat_number` parameter to `merchant_vat` for [Authentication](pos_integratorauthentication) endpoint and changed the format to remove the dash between country code and VAT (e.g., "DK12345678" instead of "DK-12345678"). 

- Removed 'api/' as part of the endpoint route for all endpoints.

---

### 2020-03-25

- Added a *RejectedByMobilePayDueToAgeRestrictions* [payment state](payment_flows#payment_flow_states), to indicate that a payment could not be completed because of age restrictions on the payment. 

- Added section Authentication  [Authentication](pos_integratorauthentication) section so Integrators can use MobilePay APIs, as they'll have to obtain an access_token from the Integrator Authentication service. This way, MobilePay knows who is calling our service.


---

### 2020-03-04

- Added section Handling Timeouts in [API Principles](api_principles) section.

- Removed a 409 errorcode from POST /api/v10/refunds regarding refund amount in the [API Errors](endpoint_errors) section and added an extra one regarding call to POST /api/v10/refunds if payment is not captured.

---

### 2020-03-03

- Renamed header from X-MobilePay-MerchantVatNumber to X-MobilePay-Merchant-VAT-Number in the [Input Formats](input_formats) section.

- Rewrote parameters to camelCase instead of PascalCase in the [Input Formats](input_formats) section to reflect the right casing in the API

---
### 2020-02-27

- Added HTTP StatusCodes and ErrorCodes pr. endpoint in the API. Response codes have also slightly changed in the API on the basis of this documentation so be aware of minor changes to the API as well. See more at [API Errors](endpoint_errors).

- Added header to all endpoints in the API called X-MobilePay-MerchantVatNumber. For more information see [Input Formats](input_formats#HTTP_Headers)

- Fixed wrong header name from X-IBM-ClientId to X-IBM-Client-Id. See [Input Formats](input_formats#HTTP_Headers)

---
### 2020-02-07

- Added documentation on payment restrictions under [Input Formats](input_formats#payment_restrictions) and expanded on best practices regarding payment restrictions [Best Practices](best_practices)

---
### 2020-02-04

- Specified expiration length of Refund reservations to 24 hours. [Refunds](payment_flows#refunds)

---
### 2020-01-31

- Added a description of Self-Certification to the documention under [Self Certification](self_certification)

---

### 2019-12-23

- Changed the month that news will come out regarding self-certification from December 2019 to January 2020

---

### 2019-12-10

- Added `BrandName` to the subpage **Input and Output Formats** in the Brand section

---

### 2019-12-10

- Renamed subpage **Input Formats** to **Input and Output Formats**.
- Adjusted content of subpage **Input and Output Formats**
  - **HTTP Headers**
    - Renamed from `X-MP-\*` to `X-MobilePay-\*`.
    - Removed `X-MP-IntegratorId`.
  - **Payments**
    - Added `PlannedCaptureDelay`, `CustomerToken` and `CustomerReceiptToken`.
