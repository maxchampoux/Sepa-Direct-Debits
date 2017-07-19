# Sepa-Direct-Debits

## API Routes ##

| Route | Description |
|-------|-------------|
| [`POST /mandates/}`](#post_mandates) | Create a new mandate and get your (UMR) Unique Mandate Reference. |
| [`GET /mandates/`](#getMandates_list) | Retrieve the list of Mandates. |
| [`GET /mandates/-{id}/`](#getMandates_details) | Retrieve the details of a Mandate. |
| [`DELETE /mandates/-{id}/`](#deleteMandates_details) | Delete a Mandate. |
| [`POST /refunds/}`](#post_mandates) | Create a refund on an existing mandate. |

## <a id="post_mandates"></a> Create a new mandate and get your (UMR) Unique Mandate Reference. ##

```
Method: POST 
URL: /mandates/
```
You can use this API service to Create a new mandate.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| paymentScheme | Body | String(35) | Required | Means the type of mandate you want to generate. You may have 2 choices : 'sddCore' or 'sddB2b' |
| amount | Body | Object([Amount Object](../objects/objects.md#customer_object)) | Required | The amount that will be debited for each interval. |
| intervalUnit | Body | String(35) | Required | The interval of time of each debit. You may have 3 choices : 'monthly', 'quarterly', 'annualy'. |
| dayOfMonth | Body | Numérical | Required | The interval of time of each debit. |
| customer | Body | Object([Customer Object](../objects/objects.md#customer_object)) | Required | Details on the subscriber of the mandate. (i.e. the end customer) |
| tag | Body | String (150) | Required | Customized reference - Free format. |

**Example of a Call:**
```js
POST /mandates/
{
  "paymentScheme": "sddCore",
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
 
**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| mandate | Object([Mandate Object](../objects/objects.md#mandate_object)) | The details of the mandate created. |

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

## <a id="post_mandates"></a> Create a refund on an existing mandate. ##

```
Method: POST 
URL: /refunds/-{id}/financialMovements/-{idFinancialMovement}/
```

You can use this API service to refund a financial movement (credit) linked to one of your mandate

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | Query | String | Required | A mandate ID to specify to retrieve only a specific and extensive mandate detais. |
| idFinancialMovement | Query | String | Required | A financial movement ID to specify in order to be refunded. Credit will be reversed attention to the end customer. |
| tag | Body | String (60) | Optional | Customized reference - Free format. |
| communication | Body | String (140) | Optional | An optional refund communication, displayed on the end-customer's bank statement. This can be up to 140 characters for SEPA payments. |

**Fails:** 

| Error | Description |
| refundPaymentInvalidStatus | when the linked payment isn't either 'confirmed' or 'paidOut'. |

Question :
- possibilité de faire un refund d'un montant partiel?
- Le refund doit se faire sur un objet paiement ou sur un objet FM?




