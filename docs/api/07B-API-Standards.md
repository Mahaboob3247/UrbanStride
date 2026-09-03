# API Standards

## Naming

Use:

```text
kebab-case
```

Examples:

```text
refresh-token
trusted-contacts
push-notifications
```

---

## Pagination

Request

GET /activities?page=1&limit=20

Response

{
  "data": [],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}

---

## Sorting

GET /activities?sort=created_at&order=desc

---

## Filtering

GET /activities?type=run

GET /activities?status=completed

---

## API Versioning

Current:

