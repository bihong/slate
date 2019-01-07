# Response Format

> Success Response

```json
{
  "status":{
    "code": "200",
    "msg":"success messages"
  },
  "result":{
    ...
  }
}
```

> Error Response

```json
{
  "status":{
    "code": "400",
    "msg":"error messages"
  },
  "result":{
    ...
  }
}
```

Response are delivered in JSON, and may have the following fields:

- `status`: Response metadata. Always present.
  - `code`: error code.
  - `msg`: error messages.
- `result`: result that return from the endpoints.