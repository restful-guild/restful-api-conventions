# restful-api-conventions

A short, opinionated set of conventions for designing consistent REST APIs and OpenAPI documents.
Take what is useful, ignore the rest. Issues and pull requests are welcome.

## Contents
1. [Resource naming](#1-resource-naming)
2. [HTTP methods and status codes](#2-http-methods-and-status-codes)
3. [Error format](#3-error-format)
4. [Request and response bodies](#4-request-and-response-bodies)
5. [Pagination, filtering and sorting](#5-pagination-filtering-and-sorting)
6. [Versioning](#6-versioning)
7. [OpenAPI document](#7-openapi-document)

## 1. Resource naming
- Use plural nouns for collections: `/api/tickets`, `/api/files`, `/api/users`.
- Use lowercase kebab-case for multi-word paths: `/api/project-requests`.
- Use sub-resources for related data: `/api/tickets/{id}/files`.
- Keep verbs out of paths; let the HTTP method express the action (`POST /api/tickets`, not `/api/create-ticket`).

## 2. HTTP methods and status codes

| Method | Use | Success |
|---|---|---|
| `GET` | Read a resource or a collection | `200` |
| `POST` | Create a resource | `201` with a `Location` header |
| `PUT` | Replace a resource | `200` |
| `PATCH` | Partially update a resource | `200` |
| `DELETE` | Remove a resource | `204` |

Client errors: `400` invalid input, `401` not authenticated, `403` authenticated but not allowed, `404` not found, `409` conflict. Use `500` only for unexpected server errors.

## 3. Error format
Return a consistent JSON body for API errors:

```json
{ "error": { "code": "not_found", "message": "Resource not found", "request_id": "..." } }
```

## 4. Request and response bodies
- Use ISO 8601 UTC timestamps, for example `2026-11-05T09:30:00Z`.
- Use consistent `snake_case` field names across all resources.

## 5. Pagination, filtering and sorting
- Pagination: `?page=1&page_size=50`; return the total count in the response.
- Filtering by field: `?status=open&priority=high`.
- Sorting: `?sort=created_at` (ascending) or `?sort=-created_at` (descending).
- Free-text search: `?q=printer`.

## 6. Versioning
- Put the major version in the path: `/api/v1/tickets`.
- Additive changes (new fields, new endpoints) are not breaking; removing or renaming fields is.

## 7. OpenAPI document
- Provide an OpenAPI 3.x document (`openapi.yaml` or `openapi.json`) in the repository root or in `docs/`.
- Fill in `info` and `servers` (the base URL).
- Give every operation a `summary`, an `operationId` and a `tags` entry.
- Define the shared error body once under `components/schemas` and reference it from the API error responses.
- Add at least one request example and one response example per operation.

## Contributing
Open an issue to discuss a change, or send a pull request with a short rationale.

## Changelog
- **0.3.0** Added pagination and versioning.
- **0.2.0** Added the status code table.
- **0.1.0** Initial version.

## License
[MIT](LICENSE)
