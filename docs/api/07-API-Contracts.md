# UrbanStride API Contracts

Version: 1.0

Status: Draft

Base URL

/api/v1

---

# API Principles

- RESTful APIs
- JSON request/response
- JWT Authentication
- Versioned APIs
- Consistent response format

---

# Standard Success Response

{
  "success": true,
  "data": {},
  "message": "Operation successful"
}

---

# Standard Error Response

{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Requested resource was not found"
  }
}

---

# Authentication Header

Authorization: Bearer <JWT_TOKEN>