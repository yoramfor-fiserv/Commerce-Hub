# Create a payment form

## Overview

The first step for adding payments capabilities to your website is to implement a HTML form that you can use to accept a payment.

This type of integration manages the customer checkout process and supports many payment methods or authentication mechanisms.

This gives you the option to use our secure hosted pages or an embeddable form which can reduce the burden of compliance with the Data Security Standard of the Payment Card Industry (PCI DSS).

With the transaction type Pre-Authorization (preauth) you can reserve funds on a customer's card account. This transaction does not charge the card until you perform a completion transaction. In order to complete a transaction and initiate the settlement process, you will need to send a Post-Authorization (postauth) via our API or initiate the completion manually using our Virtual Terminal.

Alternatively you can use the transaction type Sale which immediately charges a customer’s card or bank account over night with no further action required from you.

## Getting started

When using the embedded form or hosted payment page, you'll need the following:

| Item | Description | Sandbox Value |
|----|-------|--------|
| Store Name |	This is the ID of the store that was given to you by First Data. For example: 10123456789 |	80004160 |
| Shared Secret	| This is the shared secret provided to you by First Data. This is used when constructing the hash value.	| sharedsecret |
| URL for Test Transactions |	Sandbox URL |	https://test.ipg-online.com/connect/gateway/processing|

You will get the production URL with your production account credentials from our integration team.

### Building the Request

Independently of the payment method, there are some mandatory fields that need to be included in your transaction request:

| Field |	Description |
| ----- | -------- |
| txntype | The type of the transaction: preauth, sale or payer_auth |
| timezone | 	The timezone of the transaction in Area/Location format, e.g. America/New_York or Europe/Berlin |
| txndatetime | 	The exact time of the transaction in format YYYY:MM:DD-hh:mm:ss |
| hash_algorithm | 	This is to indicate the algorithm that you use for hash calculation. Valid values are: HMACSHA256 or HMACSHA384 or HMACSHA512 |
| hashExtended | 	The extended hash needs to be calculated using all request parameters in ascending order of the parameter names. When you are using Direct Post, there is also an option where you do not need to know the card details (PAN, CVV and Expiry Date) for the hash calculation. This will be managed with a specific setting performed on your store. Please contact your local support team if you want to enable this feature. |
| storename | 	The ID of the store provided to you by First Data, e.g. 541234567 |
| chargetotal | 	The total amount of the transaction using a dot or comma as decimal separator, e.g. 12.34 for an amount of 12 Dollar and 34 Cents. Group separators like 1,000.01 / 1.000,01 are not allowed. |
| currency | 	The numeric ISO code of the transaction currency, e.g. 840 for US Dollar or 978 for Euro |

>**Generating the hash**
>Please refer to the next page on how to generate the hash

## Sample Code

Example of a form with the minimum number of fields:

```html
<form method="post" action="https://test.ipg-online.com/connect/gateway/processing">
  <input type="hidden" name="txntype" value="preauth">
  <input type="hidden" name="timezone" value="Europe/Berlin">
  <input type="hidden" name="txndatetime" value="<% getDateTime() %>"/>
  <input type="hidden" name="hash_algorithm" value="HMACSHA256"/>
  <input type="hidden" name="hashExtended" value="<% call createExtendedHash( "13.00","978" ) %>"/>
  <input type="hidden" name="storename" value="10123456789">
  <input type="text" name="chargetotal" value="13.00">
  <input type="hidden" name="currency" value="978">
  <input type="submit" value="Submit">
</form>

```

## Additional Security Settings

The following recommendations are to limit potential for fraudulent activity on your hosted payment page.

**Recommendations**

- Enable Re-Captcha
- Authentication/Login requirement to access the payment page
- Limit response back to the browser/customer
- Follow [fraud best practices](?path=docs/Resources/Guides/Fraud/Fraud-Settings.md) for the business type or payment flow

## See Also
- Available Fields
- Customize Payment Page
- [Online Wallets](?path=docs/Getting-Started/Getting-Started-Wallets.md)

---