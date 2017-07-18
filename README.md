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

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| paymentScheme | Body | String(35) | Required | Means the type of mandate you want to generate. You may have 2 choices : 'sddCore' or 'sddB2b' |
| amount | Body | Object([Amount Object](../objects/objects.md#customer_object)) | Required | The amount that will be debited for each interval. |
| intervalUnit | Body | String(35) | Required | The interval of time of each debit. You may have 3 choices : 'monthly', 'quarterly', 'annualy'. |
| dayOfMonth | Body | Numérical | Required | The interval of time of each debit. |
| customer | Body | Object([Customer Object](../objects/objects.md#customer_object)) | Required | Details on the subscriber of the mandate. (i.e. the end customer) |

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
    
