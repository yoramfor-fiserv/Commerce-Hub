# Security Code

## Overview
**Security Code Check** - A service where cardholder is prompt to enter the 3-digit Card Verification Value(CVV) manually in order to get the card verify by the issuing system. Security code check is used as a Fraud Prevention measure for the transaction where card holder is not present.

## Perform Security Check

For the transactions where security check id required, merchant needs to pass the appropriate values for securityCode and  securityCodeIndicator in card object.


## Security Check using Charge Request

<!-- theme: success -->
>##### Endpoint
>**POST** `/payments/v1/charges`

## Payload Example


<!--
type: tab
title: Request
-->

##### Example of Security Check Using Charge Request

```json
{
  "amount": {
    "total": "12.04",
    "currency": "USD"
  },
  "source": {
    "sourceType": "PaymentCard",
    "card": {
      "cardData": "4005550000000019",
      "expirationMonth": "02",
      "expirationYear": "2035",
      "securityCode": "123",
      "securityCodeIndicator": "PROVIDED"
    }
  },
  "transactionDetails": {
    "captureFlag": true
  }
  "billingAddress": {
    "name": "Jane Smith",
    "address": {
      "street": "Main Street",
      "houseNumberOrName": "123",
      "city": "Sandy Springs",
      "stateOrProvince": "GA",
      "postalCode": "30303",
      "country": "US"
    }
  }
}

```
<!--
type: tab
title: Response
-->

##### Charge Response having Security Check Response Fields

```json
{
  "gatewayResponse": {
    "orderId": "R-3b83fca8-2f9c-4364-86ae-12c91f1fcf16",
    "transactionType": "token",
    "transactionState": "authorized",
    "transactionOrigin": "ecom",
    "transactionProcessingDetails": {
      "transactionDate": "2016-04-16",
      "transactionTime": "2016-04-16T16:06:05Z",
      "apiTraceId": "rrt-0bd552c12342d3448-b-ea-1142-12938318-7",
      "clientRequestId": "30dd879c-ee2f-11db-8314-0800200c9a66",
      "transactionId": "838916029301"
    }
  },
  "paymentSource": {
    "sourceType": "PaymentCard",
    "tokenData": "1234123412340019",
    "PARId": "string",
    "declineDuplicates": "FALSE",
    "tokenSource": "string",
    "card": {
      "nameOnCard": "Jane Smith",
      "expirationMonth": "05",
      "expirationYear": "2025",
      "bin": "400555",
      "last4": "0019",
      "scheme": "VISA"
    }
  },
  "processorResponseDetails": {
    "approvalStatus": "APPROVED",
    "approvalCode": "OK3483",
    "referenceNumber": "845366457890-TODO",
    "schemeTransactionId": "019078743804756",
    "feeProgramIndicator": "string",
    "processor": "fiserv",
    "responseCode": "00",
    "responseMessage": "APPROVAL",
    "hostResponseCode": "54022",
    "hostResponseMessage": "",
    "localTimestamp": "2016-04-16T16:06:05Z",
    "bankAssociationDetails": {
      "associationResponseCode": "000",
      "transactionTimestamp": "2016-04-16T16:06:05Z",
      "avsSecurityCodeResponse": {
        "streetMatch": "MATCH",
        "postalCodeMatch": "MATCH",
        "securityCodeMatch": "MATCH",
          "association": {
            "avsCode": "BOTH_MATCH",
            "securityCodeResponse": "MATCH",
            "cardHolderNameResponse": "NAME_MATCH",
          }
      }
    }
  },
}
```
<!-- type: tab-end -->

## Security Check using Verification Request

<!-- theme: success -->
>##### Endpoint
>**POST** `/payments-vas/v1/accounts/verification`

## Payload Example


<!--
type: tab
title: Request
-->

##### Example of Account Verification Request

```json
{
  "source": {
    "sourceType": "PaymentCard",
    "cardData": "4005550000000019",
    "expirationMonth": "02",
    "expirationYear": "2035",
    "securityCode": "123",
    "securityCodeIndicator": "PROVIDED"
  },
  "billingAddress": {
    "name": "Jane Smith",
    "address": {
      "street": "Main Street",
      "houseNumberOrName": "123",
      "city": "Sandy Springs",
      "stateOrProvince": "GA",
      "postalCode": "30303",
      "country": "US"
    }
  }
}

```
<!--
type: tab
title: Response
-->

##### Example of a Account Verification Response

```json
{
  "gatewayResponse": {
    "orderId": "R-3b83fca8-2f9c-4364-86ae-12c91f1fcf16",
    "transactionType": "token",
    "transactionState": "authorized",
    "transactionOrigin": "ecom",
    "transactionProcessingDetails": {
      "transactionDate": "2016-04-16",
      "transactionTime": "2016-04-16T16:06:05Z",
      "apiTraceId": "rrt-0bd552c12342d3448-b-ea-1142-12938318-7",
      "clientRequestId": "30dd879c-ee2f-11db-8314-0800200c9a66",
      "transactionId": "838916029301"
    }
  },
  "paymentSource": {
    "sourceType": "PaymentCard",
    "tokenData": "1234123412340019",
    "PARId": "string",
    "declineDuplicates": "FALSE",
    "tokenSource": "string",
    "card": {
      "nameOnCard": "Jane Smith",
      "expirationMonth": "05",
      "expirationYear": "2025",
      "bin": "400555",
      "last4": "0019",
      "scheme": "VISA"
    }
  },
  "processorResponseDetails": {
    "approvalStatus": "APPROVED",
    "approvalCode": "OK3483",
    "referenceNumber": "845366457890-TODO",
    "schemeTransactionId": "019078743804756",
    "feeProgramIndicator": "string",
    "processor": "fiserv",
    "responseCode": "00",
    "responseMessage": "APPROVAL",
    "hostResponseCode": "54022",
    "hostResponseMessage": "",
    "localTimestamp": "2016-04-16T16:06:05Z",
    "bankAssociationDetails": {
      "associationResponseCode": "000",
      "transactionTimestamp": "2016-04-16T16:06:05Z",
      "avsSecurityCodeResponse": {
        "streetMatch": "MATCH",
        "postalCodeMatch": "MATCH",
        "securityCodeMatch": "MATCH",
          "association": {
            "avsCode": "BOTH_MATCH",
            "securityCodeResponse": "MATCH",
            "cardHolderNameResponse": "NAME_MATCH",
          }
      }
    }
  },
}
```
<!-- type: tab-end -->

## Security Check Result Codes

The result of checking the cardholder’s entered security code against the Issuer’s system of record is is received in the response. The object `avsSecurityCodeResponse` contains `securityCodeResponse` field which contains the result of the security code check. The Valid Values are

| Value | Description |
| ------- | ------- |
| MATCH | Security Code Value matched |
| NO_MATCH | Security Code Value not matched |
| NOT_PROVIDED | Security Code Value not provided |
| NOT_PROCESSED | Security Code check not processed |
| NO_PARTICIPANT | Issuer not participate in Security Check |
| UNKNOWN | Unknown |
