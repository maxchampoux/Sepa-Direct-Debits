## API Routes SDD receiving ##

| Route | Description |
|-------|-------------|
| [`POST /mandates/authorizationDebit}`](#postMandates_ReceiptB2B) | Ask for authorization on a received B2B SDD mandate. |
| [`GET /mandates/`](#getMandates_list) | Retrieve the list of Mandates. |
| [`DELETE /mandates/-{id}/`](#deleteMandates_details) | Delete a Mandate. |

## <a id="postMandates_ReceiptB2B"></a> Ask for authorization on a received B2B SDD mandate. ##

```
Method: POST 
URL: /mandates/authorizationDebit/
```

**Description**
You can use this API Service to authorize the debit on you iBanFirst account coming from a new B2B SDD Mandate. All B2B SDD debit are per default automatically rejected by iBanFirst and must be validated by iBanFirst Compliance Dpt before debit.

**Parameters**

| Field | In | Type | tag depth | Required | Description |
|-------|------|------|------|----------|-------------|
| counterpartyName | Body | String(60) | + | Required | Name of the counterparty that has issued the mandate. It can only be a corporate. |
| sci | Body | String(35) | + | Required | Sepa Credit Identifier (SCI). This is a structured format: `FR-47-EDF-001007`. |
| urm | Body | String(35) | + | Required | Unique Reference of Mandate (URM). `++MM500710136744790`. |
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

When a mandate is created, the Unique Mandate reference (URM) is specified in the ([Mandate Object](../objects/objects.md#mandate_object)) returned.



