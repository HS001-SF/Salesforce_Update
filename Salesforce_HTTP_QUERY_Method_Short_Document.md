# Salesforce HTTP QUERY Method (RFC 10008) - Short Overview

## Objective
Understand and test the new HTTP `QUERY` method introduced by Salesforce for REST API integrations.

## What is the QUERY Method?

The HTTP `QUERY` method allows SOQL queries to be sent in the request body instead of the URL, helping avoid URL length limitations while keeping the request read-only.

## Benefits

- Supports long SOQL queries
- Avoids URL length limitations
- Cleaner REST requests
- Read-only like HTTP GET

## Example

**Endpoint**
`/services/data/v65.0/query`

**Method**
`QUERY`

**Body**

```json
{
  "q": "SELECT Id, Name FROM Account LIMIT 5"
}
```

## Testing

**Workbench**
- Not supported (supports only GET, POST, PUT, PATCH, DELETE)

**Recommended Tools**
- Postman
- Insomnia
- cURL
- VS Code REST Client


## Conclusion

Use the HTTP `QUERY` method for external integrations requiring large SOQL queries. Use Postman or another REST client for testing.
