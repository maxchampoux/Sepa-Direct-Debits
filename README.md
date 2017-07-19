# Sepa-Direct-Debits

## API Routes ##

| Route | Description |
|-------|-------------|
| [`POST /mandates/}`](#post_mandates) | Create a new mandate and get your (UMR) Unique Mandate Reference. |
| [`GET /mandates/`](#getMandates_list) | Retrieve the list of Mandates. |
| [`GET /mandates/-{id}/`](#getMandates_details) | Retrieve the details of a Mandate. |
| [`DELETE /mandates/-{id}/`](#deleteMandates_details) | Delete a Mandate. |
| [`POST /refunds/-{id}/financialMovements/-{idFinancialMovement}/}`](#post_refunds) | Create a refund on an existing mandate. |
| [`GET /refunds/}`](#get_refundsList) | Retrieve a list of refunds |
| [`GET /refunds/-{id}/financialMovements/-{idFinancialMovement}/`](#get_RefundsDetails) | Retrieve the details of a Refund. |

## <a id="post_mandates"></a> Create a new mandate and get your (UMR) Unique Mandate Reference. ##

```
Method: POST 
URL: /mandates/
```
You can use this API service to Create a new mandate.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| paymentScheme | Body | String(35) | Required | Means the type of mandate you want to generate. You may have 2 choices : `sddCore` or `sddB2b`. |
| amount | Body | Object([Amount Object](../objects/objects.md#customer_object)) | Required | The amount that will be debited for each interval. |
| intervalUnit | Body | String(35) | Required | The interval of time of each debit. You may have 4 choices : `oneOff`, `monthly`, `quarterly`, `yearly`. |
| dayOfMonth | Body | Numérical | Optional | The interval of time of each debit. As per ([RFC 2445](https://www.ietf.org/rfc/rfc2445.txt)). The day of the month to charge customers on. `1`-`28` or `-1` to indicate the last day of the month. |
| startDate | Body | Numérical | Optional | The date on which the first payment should be charged. Must be within one year of creation. When blank, this will be set as the mandate’s next possible charge date. |
| endDate | Body | Numérical | Optional | Date on or after which no further payments should be created. If this field is blank and `count` is not specified, the subscription will continue forever. |
| count | Body | Numérical | Optional | An alternative way to set `endDate`. The total number of payments that should be taken by this subscription. This will set `endDate` automatically. |
| customer | Body | Object([Customer Object](../objects/objects.md#customer_object)) | Required | Details on the subscriber of the mandate. (i.e. the end customer) |
| tag | Body | String (150) | Required | Customized reference - Free format. |

**Example of a Call:**
```js
POST /mandates/
{
  "paymentScheme": "sddCore",
  "intervalUnit": "oneOff",
  "amount": {
    "value": "100.00",
    "currency": "EUR",
  }
  "customer": {
    "type": "individual",
    "contactDetails": {
      "civility": "M",
      "firstName": "Maxime",
      "lastName": "Champoux",
      "language": "FR",
      "phone": "+33671738257",
      "nationality": "BE",
     }
     "billingAddress": {address},
     "email": "test@ibanfirst.com",
     "iban": "FR76 3000 3009 9500 0503 4076 086",
  }
  "tag": "XXX",
} 
  
 ```   

**Fails:** 

| Error | Description |
|----------|-------------|
| invalidCurrency | Currency must be `EUR`. ([ISO 4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)) currency code.|
| invaliIban | IBAN is not part of ([SEPA Zone](https://en.wikipedia.org/wiki/Single_Euro_Payments_Area)).|

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| mandate | Object([Mandate Object](../objects/objects.md#mandate_object)) | The details of the mandate created. |

When a mandate is created, the Unique Mandate reference (URM) is specified in the ([Mandate Object](../objects/objects.md#mandate_object)) returned.

### <a id="getMandates_list"></a> Retrieve the list of Mandates. ###

```
Method: GET 
URL: /mandates/
```

If you have not implemented our Webhook, you can use this API service to retrieve at any time status and information on mandates you are currently managing.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| page | Query | String | Optional | Index of the page. |
| perPage | Query | String | Optional | Number of items returned. |
| fromDate | Query | String | Optional | The starting date to search the list of mandates. |
| endDate | Query | String | Optional | The end date to search the list of mandates. |
| sort | Query | String | Optional | A code representing the order of rendering objects. Values should be: "ASC", "DESC". |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| XX | XX | XX |

### <a id="getMandates_details"></a> Retrieve a Mandate details. ###

```
Method: GET 
URL: /mandates/-{id}/
```

You can use this API service to retrieve specific information on a mandate.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | Query | String | required | A mandate ID to specify to retrieve only a specific and extensive mandate detais. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| mandate | Object([Mandate Object](../objects/objects.md#mandate_object)) | The details of the mandate created. |

## <a id="deleteMandates_details"></a> Delete a Mandate. ##

```
Method: POST 
URL: /mandates/-{id}/
```

You can use this API service to delete a mandate.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | Query | String | Required | A mandate ID to specify to retrieve only a specific and extensive mandate detais. |

## <a id="post_refunds"></a> Create a refund on an existing mandate. ##

```
Method: POST 
URL: /refunds/-{id}/financialMovements/-{idFinancialMovement}/
```

You can use this API service to refund a financial movement (credit) linked to one of your mandate.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | Query | String | Required | A mandate ID to specify to retrieve only a specific and extensive mandate detais. |
| idFinancialMovement | Query | String | Required | A financial movement ID to specify in order to be refunded. Credit will be reversed attention to the end customer. |
| tag | Body | String (60) | Optional | Customized reference - Free format. |
| communication | Body | String (140) | Optional | An optional refund communication (Free format), displayed on the end-customer's bank statement. This can be up to 140 characters for SEPA payments. |

**Fails:** 

| Error | Description |
|----------|-------------|
| refundPaymentInvalidStatus | when the linked payment isn't either 'confirmed' or 'paidOut'. |

**Question:** 
* Possibilité de faire un refund d'un montant partiel?
* Le refund doit se faire sur un objet paiement ou sur un objet FM?
* Est-ce qu'il faut indiquer la raison du refund?

## <a id="get_refundsList"></a> Retrieve a list of refunds. ##

```
Method: GET 
URL: /refunds/
```

You can use this API service to retrieve a list of refunds.

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | Query | String | Required | A mandate ID to specify to retrieve only a specific and/or extensive refunds detais. |
| page | Query | String | Optional | Index of the page. |
| perPage | Query | String | Optional | Number of items returned. |
| fromDate | Query | String | Optional | The starting date to search the list of refunds. |
| endDate | Query | String | Optional | The end date to search the list of refunds. |
| sort | Query | String | Optional | A code representing the order of rendering objects. Values should be: `ASC`, `DESC`. |

## <a id="get_refundsDetails"></a> Retrieve the details of a Refund. ##

```
Method: GET 
URL: /refunds/-{id}/financialMovements/-{idFinancialMovement}/
```

You can use this API service to retrieve a specific refund details.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | Query | String | Required | A mandate ID to specify to retrieve only a specific and extensive mandate detais. |
| idFinancialMovement | Query | String | Required | A financial movement ID to specify in order to be refunded. Credit will be reversed attention to the end customer. |


