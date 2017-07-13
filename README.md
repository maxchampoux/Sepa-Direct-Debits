# Sepa-Direct-Debits

Create the Checkout transaction

First, you need to prepare:
* your creditor reference
* the reference f you subscriber
* the payer's firstName, lastName and billingAddress

You also need to know the payment scheme you intend to use for this customer. if you don't know what that is, you probably need to use SEPA Direct Debit CORE.

Make sure the data you provide match our API constraints.
Here are the aprameters, mandatory or optional, which needs your attention:

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| paymentScheme | body | String(35) | Required | Means the type of mandate you want to generate. You may have 2 choices : 'sddCore' or 'sddB2b' |
| client | body | Object | Required | Details on the subscriber of the mandate. (i.e. your client) |
| reference | body | Required | The subscriber reference. |

**Example of a Call:**
```js
POST /XXX/
{
  "paymentScheme": "sddCore",
  "reference": "idMandate",
  "client": {
    "type": "individual",
    "contactDetails": {
      "civility": "M",
      "firstName": "Maxime",
      "lastName": "Champoux",
      "language": "FR",
      "phone": "+33671738257",
     }
     "billingAddress": {address}
     "email": "test@ibanfirst.com",
 ```    
    
