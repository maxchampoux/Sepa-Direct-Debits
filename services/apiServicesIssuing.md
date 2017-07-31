## API Routes SDD receiving ##

| Route | Description |
|-------|-------------|
| [`POST /mandates/-{URM}/authorizationDebit/`](#postMandates_ReceiptB2B) | Ask for authorization on a received B2B SDD mandate. |
| [`GET /mandates/`](#getMandates_list) | Retrieve the list of Mandates. |
| [`GET /mandates/-{URM}/`](#getMandates_list) | Retrieve the list of Mandates. |
| [`DELETE /mandates/-{URM}/`](#deleteMandates_details) | Delete a Mandate. |

## <a id="postMandates_ReceiptB2B"></a> Authorize a B2B SDD mandate. ##

```
Method: POST 
URL: /mandates/-{URM}/authorizationDebit/
```

**Description**

You can use this API Service to authorize the debit on your iBanFirst account coming from a new B2B SDD Mandate. All B2B SDD debit are per default automatically rejected by iBanFirst and must be validated by iBanFirst Compliance Dpt before debit.

**Parameters**

| Field | In | Type | tag depth | Required | Description |
|-------|------|------|------|----------|-------------|
| urm | Query | String(35) | + | Required | Unique Reference of Mandate (URM). `++MM500710136744790`. |
| counterpartyName | Body | String(60) | + | Required | Name of the counterparty that has issued the mandate. It can only be a corporate. |
| sci | Body | String(35) | + | Required | Sepa Credit Identifier (SCI). This is a structured format: `FR-47-EDF-001007`. |
| document | Body | [Document Object](../objects/objects.md#document_object) | + | Required | Empty file. |
| file | Body | String | ++ | Required | The binary content of the mandate file, encoded with a base64 algorithm. |
| name | Body | String(60) | ++ | Required | The mandate name of the file you want to upload. Including the document format. |
| type | Body | String(60) | ++ | Required | The type of mandate to reference. Here: `mandateSddB2b` |

**Example:**

```js
POST /mandates/authorizationDebit/
{
    "counterpartyName": "Mandate SDD B2B",
    "sci": "FR-47-EDF-001007",
    "urm": "++MM500710136744790",
    "document": {
        "type": "mandateSddB2b",
        "name": "TrésorPublicMandate.pdf",
        "file": "DPcCUEQs1hrqSxB3giGY620kQJ1BujlG4rOh+jjvEeW79GpT.....5Oj8dj1wQiKoqyaNGi4cOH51LYvn37k08+WVpaah4"
    }
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

**Description**

If you have not implemented our Webhook, you can use this API service to retrieve at any time status and information on mandates you are currently managing. For SDD mandates on debit of your account, please use the filter in the API call.

**Parameters**

| Field | In | Type | tag depth | Required | Description |
|-------|------|------|------|----------|-------------|
| side | Query | String | + | Optional | Means the quality (debtor or creditor) you are on the mandate. You may have 2 choices : `debtor` or `creditor`.  |
| scheme | Query | String | + | Optional | Means the type of mandate related. You may have 2 choices: `core` or `b2b`. |
| page | Query | String | + | Optional | Index of the page. |
| perPage | Query | String | + | Optional | Number of items returned. |
| fromDate | Query | String | + | Optional | The starting date to search the list of mandates. |
| endDate | Query | String | + | Optional | The end date to search the list of mandates. |
| sort | Query | String | + | Optional | A code representing the order of rendering objects. Values should be: "ASC", "DESC". |



